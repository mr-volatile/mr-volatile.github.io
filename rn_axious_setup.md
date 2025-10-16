# Axios Client Setup 
```
const axiosClient: AxiosInstance = axios.create({
  baseURL: Config.BASE_URL,
  timeout: 15000,
  // signal: newAbortSignal(),
  headers: {
    'Content-Type': 'application/json',
  },
});

axiosClient.interceptors.request.use(
  async function (config) {
    const token = await getStorageData('ACCESS_TOKEN');
    const skipToken = config.headers['skip-token'];
    const isSkip = skipToken === 'true';

    if (token && !isSkip) {
      config.headers.Authorization = `Bearer ${token}`;
    }
    return config;
  },
  function (error) {
    return Promise.reject(error);
  },
);

let isRefreshing = false;
let failedQueue: Array<{
  resolve: (token?: string | null) => void;
  reject: (error: any) => void;
}> = [];

const processQueue = (error: any, token: string | null = null): void => {
  failedQueue.forEach(prom => {
    if (error) {
      prom.reject(error);
    } else {
      prom.resolve(token);
    }
  });

  failedQueue = [];
};

axiosClient.interceptors.response.use(
  response => {
    return response;
  },
  async error => {
    const originalRequest = error.config;
    const skipToken = originalRequest.headers['skip-token'];
    const isSkipToken = skipToken === 'true';

    if (
      error?.response?.status === HttpStatusCode.UNAUTHORIZED &&
      !originalRequest._retry &&
      !isSkipToken
    ) {
      if (isRefreshing) {
        return new Promise((resolve, reject) => {
          failedQueue.push({ resolve, reject });
        })
          .then(token => {
            originalRequest.headers.Authorization = `Bearer ${token}`;
            return axiosClient(originalRequest);
          })
          .catch(err => {
            return Promise.reject(err);
          });
      }

      originalRequest._retry = true;
      isRefreshing = true;

      const refreshToken = await getStorageData('REFRESH_TOKEN');

      if (!refreshToken) {
        showLoginModal();
        return Promise.reject(new Error('No refresh token available'));
      }

      try {
        const result = await refreshAccessToken(refreshToken);
        const accessToken = result?.data?.accessToken;

        if (!accessToken) {
          showLoginModal();
          return Promise.reject(
            new Error('Failed to retrieve new access token'),
          );
        }

        await setStorageData('ACCESS_TOKEN', accessToken);
        await setStorageData('REFRESH_TOKEN', result?.data?.refreshToken);

        axiosClient.defaults.headers.common.Authorization = `Bearer ${accessToken}`;

        processQueue(null, accessToken);

        originalRequest.headers.Authorization = `Bearer ${accessToken}`;
        return axiosClient(originalRequest);
      } catch (err) {
        processQueue(err, null);
        showLoginModal();
        return Promise.reject(err);
      } finally {
        isRefreshing = false;
      }
    }

    return Promise.reject({
      data: error?.response?.data,
      status: error?.response?.status,
    });
  },
);

export default axiosClient;
```

# Network Request Setup

```
import axiosClient from './apiClient';

const NetworkRequest = (
  method: string,
  url: string,
  body?: any,
  config?: any,
) => {
  switch (method) {
    case GET:
      return axiosClient({
        method: 'get',
        url,
        ...config,
      });

    case POST:
      return axiosClient({
        method: 'post',
        url,
        data: body,
        ...config,
      });

    case PUT:
      return axiosClient({
        method: 'put',
        url,
        data: body,
        ...config,
      });

    case DELETE:
      return axiosClient({
        method: 'delete',
        url,
        ...config,
      });
    default:
      throw new Error('Incorrect Network Request');
  }
};
```

# Interview Questions

| #                                                                 | Topic – Question                                                                                                                                                                                                                                                                                                                                                                                     |
| ----------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **1. Axios Setup – Create and configure a custom Axios instance** | Use `axios.create()` to define a reusable Axios instance with default settings such as `baseURL`, `timeout`, and default headers. This centralizes API configuration and avoids duplication across services. Keep it in a dedicated module like `apiClient.js` and import it where needed. <br><br> `js const api = axios.create({ baseURL: Config.API_URL, timeout: 15000 }); export default api; ` |
| **2. Base URL – Manage multiple base URLs (dev, staging, prod)**  | Use environment variables managed by `react-native-config` or `.env` files. The `Config.API_URL` variable can change automatically based on the build type or environment. <br><br> `js const api = axios.create({ baseURL: Config.API_URL }); `                                                                                                                                                     |
| **3. Interceptors – Centralized request/response handling**       | Axios interceptors allow you to modify requests before they are sent and responses before they are returned. They’re ideal for adding tokens, logging, or handling global errors. <br><br> `js api.interceptors.request.use(cfg => { cfg.headers.Authorization = token; return cfg; }); `                                                                                                            |
| **4. Auth Header – Attach Authorization header dynamically**      | Fetch the token from secure storage (e.g., EncryptedStorage or Keychain) and attach it to the request header in the request interceptor. This keeps authorization consistent and automatic. <br><br> ``js cfg.headers.Authorization = `Bearer ${token}`; ``                                                                                                                                          |
| **5. 401 Handling – Handle Unauthorized errors globally**         | Add a response interceptor that detects 401 responses and triggers re-authentication or a token refresh flow. If refresh fails, direct the user to login. <br><br> `js if (error.response.status === 401) refreshToken(); `                                                                                                                                                                          |
| **6. Token Refresh – Refresh tokens and retry failed requests**   | When an access token expires, pause all failing requests, refresh the token once, store the new token, update headers, and replay all queued requests with the new token. <br><br> `js const newToken = await refreshToken(); processQueue(null, newToken); `                                                                                                                                        |
| **7. Race Conditions – Prevent multiple refresh requests**        | Use an `isRefreshing` flag and a queue for pending requests. If multiple requests return 401, only trigger one refresh call and make others wait until it completes. <br><br> `js if (isRefreshing) queue.push(resolve); `                                                                                                                                                                           |
| **8. Request Queue – Queue and replay during token refresh**      | Maintain a `failedQueue` array of Promises for requests that failed due to token expiry. Once the token refresh succeeds, resolve all queued Promises with the new token and retry them. <br><br> `js processQueue(null, newToken); `                                                                                                                                                                |
| **9. Infinite Loop – Prevent repeated 401 retries**               | Use a `_retry` flag on the original request to ensure that a request that fails again after token refresh doesn’t trigger another refresh cycle. <br><br> `js if (!req._retry) req._retry = true; `                                                                                                                                                                                                  |
| **10. Skip Auth – Skip token for specific endpoints**             | Add a custom flag or header like `'skip-token': 'true'`. In the interceptor, check for this flag and skip adding the `Authorization` header when true. <br><br> `js if (!cfg.headers['skip-token']) addAuth(cfg); `                                                                                                                                                                                  |
| **11. Timeout – Handle request timeouts**                         | Set a timeout value when creating the Axios instance. If the request exceeds this time, Axios throws an `ECONNABORTED` error, which can be handled gracefully in UI. <br><br> `js const api = axios.create({ timeout: 15000 }); `                                                                                                                                                                    |
| **12. Cancellation – Cancel requests on unmount**                 | Use `AbortController` (modern) or `CancelToken` (legacy) to cancel API requests when a screen unmounts or a user navigates away, preventing memory leaks or unwanted state updates. <br><br> `js const c = new AbortController(); api.get(url, { signal: c.signal }); c.abort(); `                                                                                                                   |
| **13. Retry Logic – Retry with exponential backoff**              | Implement automatic retry logic for transient network errors using `axios-retry` or a custom retry mechanism that increases delay exponentially on each retry. <br><br> `js axiosRetry(api, { retries: 3, retryDelay: axiosRetry.exponentialDelay }); `                                                                                                                                              |
| **14. Global Error Handling – Unified error management**          | A global interceptor can catch all failed requests, normalize error structures, and dispatch appropriate UI feedback (e.g., toast messages or modals). <br><br> `js api.interceptors.response.use(r => r, e => Promise.reject(formatError(e))); `                                                                                                                                                    |
| **15. Logging – Log requests and responses safely**               | Use interceptors or custom loggers to log only non-sensitive data in development builds. Never log tokens or private user info in production. <br><br> `js console.log(cfg.url, cfg.method, cfg.data); `                                                                                                                                                                                             |
| **16. Error Normalization – Consistent error object**             | Standardize errors into a common shape (e.g., `{ status, message, code }`) so that your UI or Redux store can handle them predictably. <br><br> `js throw { status: e.response.status, message: e.message }; `                                                                                                                                                                                       |
| **17. Response Transformation – Format API responses**            | Modify or unwrap responses globally to simplify data access in the UI layer. For example, if APIs always wrap results in a `data.result` key, unwrap it automatically. <br><br> `js res => res.data?.result ?? res.data; `                                                                                                                                                                           |
| **18. Caching – Improve performance with caching**                | Integrate `axios-cache-adapter` or a custom memory cache to store responses temporarily. This helps reduce redundant API calls and improves perceived performance. <br><br> `js import { setupCache } from 'axios-cache-adapter'; `                                                                                                                                                                  |
| **19. Concurrent Requests – Execute parallel calls**              | Use `axios.all()` or `Promise.all()` to perform multiple API calls in parallel and process all results together. <br><br> `js const [user, posts] = await axios.all([getUser(), getPosts()]); `                                                                                                                                                                                                      |
| **20. Upload/Download – Track file transfer progress**            | Use `onUploadProgress` and `onDownloadProgress` to track and update progress bars for file transfers. <br><br> `js onUploadProgress: e => setProgress(Math.round((e.loaded * 100) / e.total)); `                                                                                                                                                                                                     |
| **21. SSL Pinning – Secure API connections**                      | Implement SSL pinning using libraries like `react-native-ssl-pinning` to prevent man-in-the-middle attacks by validating server certificates. <br><br> `js fetch('https://api', { sslPinning: { certs: ['cert.pem'] } }); `                                                                                                                                                                          |
| **22. Token Storage – Secure token management**                   | Store tokens securely using native Keychain (iOS) or Keystore (Android) via `react-native-encrypted-storage`. Avoid AsyncStorage for sensitive data. <br><br> `js await EncryptedStorage.setItem('ACCESS_TOKEN', token); `                                                                                                                                                                           |
| **23. Testing – Mock Axios in tests**                             | Use Jest mocks or `axios-mock-adapter` to simulate API responses during testing. This ensures predictable test behavior without hitting real endpoints. <br><br> `js mock.onGet('/user').reply(200, { id: 1 }); `                                                                                                                                                                                    |
| **24. Error Types – Distinguish network vs. API errors**          | Check for `error.request` to detect network or timeout issues, and `error.response` for HTTP errors. This distinction helps in accurate error messages. <br><br> `js if (!err.response) alert('Network error'); `                                                                                                                                                                                    |
| **25. App Lifecycle – Manage requests on app state change**       | Use `AppState` listeners to cancel or pause API requests when the app goes to background, and resume them when it becomes active again. <br><br> `js AppState.addEventListener('change', s => s === 'background' && controller.abort()); `                                                                                                                                                           |
| **26. Request Deduplication – Prevent duplicate calls**           | Maintain a map of active requests and cancel duplicates targeting the same endpoint. This avoids redundant network load and UI flicker. <br><br> `js if (pending[url]) cancel(); pending[url] = true; `                                                                                                                                                                                              |
| **27. Memory Leaks – Avoid duplicate interceptors**               | Always eject interceptors before adding new ones to prevent stacking, which can cause duplicate requests or memory issues. <br><br> `js api.interceptors.request.eject(interceptorId); `                                                                                                                                                                                                             |
| **28. Dynamic Headers – Send device/app metadata**                | Dynamically append headers such as app version, device model, or locale in the request interceptor to support analytics and backend tracing. <br><br> `js cfg.headers['x-app-version'] = VersionNumber.appVersion; `                                                                                                                                                                                 |
| **29. File Upload Retry – Retry failed multipart uploads**        | Always recreate `FormData` objects before retrying uploads, as they can’t be reused after the first request. <br><br> `js const fd = new FormData(); fd.append('file', file); `                                                                                                                                                                                                                      |
| **30. GraphQL Integration – Use Axios for GraphQL APIs**          | Send GraphQL queries as `POST` requests with `{ query, variables }` in the request body. This works seamlessly with Axios and supports error interceptors. <br><br> `js api.post('/graphql', { query, variables }); `                                                                                                                                                                                |

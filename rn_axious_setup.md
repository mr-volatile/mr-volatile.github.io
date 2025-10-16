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

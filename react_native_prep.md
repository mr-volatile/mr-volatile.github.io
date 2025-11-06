# React Native + Advanced JavaScript Interview Questions

### JavaScript Fundamentals

| Question | Answer |
|-----------|---------|
| What is the difference between `==` and `===`? | `==` performs type coercion, `===` checks both type and value strictly. |
| Difference between `null` and `undefined` | `null` = assigned empty value, `undefined` = declared but not assigned. |
| What is Hoisting? | JS moves variable and function declarations to the top before execution. |
| What is the Temporal Dead Zone (TDZ)? | Period between block start and initialization of `let`/`const` where access throws error. |
| Can you redeclare `let` and `const` variables? | No — redeclaring them in the same scope throws a `SyntaxError`. |
| Does `const` make values immutable? | No — only prevents reassignment; internal mutation allowed. |
| What is Strict Mode? | `"use strict"` enforces safer, more predictable code execution. |
| What is Scope? | Defines where variables are accessible during runtime. |
| What is the `void` operator? | Evaluates an expression but always returns `undefined`. |
| Key features of ES6 | `let`, `const`, arrow functions, modules, destructuring, classes, promises, etc. |


### Functions & Closures

| Question | Answer |
|-----------|---------|
| What is a Closure? | Function that retains access to its outer scope after it executes. |
| What is an Anonymous Function? | Function without a name, often used as callbacks. |
| What is a First-Class Function? | Functions are treated as values — can be passed or returned. |
| What is a Higher-Order Function? | Takes or returns other functions; used in functional programming. |
| What is Currying? | Converts multi-arg function into nested single-arg functions. |
| Example of Currying in JS | `const add = a => b => c => a + b + c;` |
| What are Arrow Functions? | Lexically bind `this`; cannot be constructors or generators. |
| What is `this` keyword? | Refers to current context of execution; call-site dependent. |
| Difference between `call`, `apply`, and `bind` | `call()` and `apply()` invoke immediately; `bind()` returns new bound function. |


### Objects & Operators

| Question | Answer |
|-----------|---------|
| What is a Proxy object? | Intercepts object operations (get/set/has) for custom behavior. |
| How to copy properties between objects? | `Object.assign(target, source)` merges properties. |
| How to get object keys? | `Object.keys(obj)` returns array of property names. |
| What is `Object.freeze()` used for? | Makes object immutable by preventing modifications. |



### Arrays & Iteration

| Question | Answer |
|-----------|---------|
| What is `Array.slice()`? | Returns shallow copy of portion of array (non-mutating). |
| What is `Array.splice()`? | Adds/removes elements and mutates the original array. |
| What is Rest Parameter? | Collects remaining args into an array using `...args`. |
| What happens if rest parameter isn’t last? | Syntax error — it must be last parameter. |
| What is Spread Operator? | Expands iterable values into function arguments or array literals. |
| Purpose of `compareFunction` in sort() | Defines custom sort order; `(a, b) => b - a` for descending. |
| How does synchronous iteration work? | Uses `Symbol.iterator` and `next()` for controlled iteration. |


### Async JavaScript

| Question | Answer |
|-----------|---------|
| What is the Event Loop? | Continuously checks call stack and queues for pending tasks. |
| What is the Call Stack? | Tracks function execution order (LIFO). |
| What is the Event Queue / Macrotask Queue? | Stores async callbacks waiting for execution. |
| What is a Promise? | Represents future completion or failure of an async task. |
| Why do we need Promises? | Handles async flow cleanly; avoids callback hell. |
| What is Callback Hell? | Nested callbacks making code unreadable. |
| What is Promise Chaining? | Sequential `.then()` calls to execute async tasks in order. |
| What is `Promise.all()`? | Resolves when all promises succeed or rejects if one fails. |
| What is `Promise.race()`? | Resolves/rejects when first promise settles. |
| What is Memoization? | Caching function results to optimize performance. |


### Functional Concepts

| Question | Answer |
|-----------|---------|
| What is a Pure Function? | Always returns same output for same input, no side effects. |
| Difference between Debounce and Throttle | Debounce waits until idle; Throttle limits execution frequency. |
| What is Currying? | Breaks function with multiple args into chained single-arg calls. |


### React Native Architecture

| Question | Answer |
|-----------|---------|
| How does reconciliation differ from React DOM? | RN maps components to native views, not HTML DOM. |
| How does the bridge affect render performance? | Async JSON bridge adds latency; JSI removes overhead. |
| What’s new in RN architecture (Fabric, JSI, TurboModules)? | Direct JS-native access, concurrent UI, lazy native modules. |
| What is Yoga? | RN’s Flexbox layout engine written in C++. |
| What are TurboModules? | Next-gen native modules using JSI, type-safe and lazy-loaded. |
| What is Hermes Engine? | Optimized JS engine improving startup and memory. |
| What is Fabric? | Modern rendering system improving concurrency and layout. |
| What is Flipper? | Debugging tool for network, layout, and performance. |
| What is Watchman? | File watcher used by Metro bundler for rebuilds. |


### Performance, Debugging & Security

| Question | Answer |
|-----------|---------|
| How to detect memory leaks in RN? | Use Flipper Memory, Android Profiler, cleanup listeners/timers. |
| How to optimize startup time? | Lazy imports, Hermes, inline requires, smaller bundles. |
| How to optimize bundle size? | Tree-shaking, dynamic imports, compress assets. |
| App crashes only in Android release build — what to check? | Minification issues, Hermes mismatch, native errors. |
| What is SSL Pinning? | Embeds server cert to prevent MITM attacks. |
| How to secure OTA updates? | Code signing, encrypted storage, avoid JS secrets. |
| How to handle offline-first design? | Cache data, queue requests, sync when online. |


### React Native APIs & Features

| Question | Answer |
|-----------|---------|
| What is Deep Linking? | Opens app via custom URLs (e.g., `myapp://profile/1`). |
| How to implement background tasks? | Use Headless JS or native background-fetch modules. |
| How to implement accessibility? | Use `accessible`, `accessibilityLabel`, `accessibilityRole`. |
| What is SafeAreaView? | Keeps content away from notches/status bars. |


### Components, State & Hooks

| Question | Answer |
|-----------|---------|
| Stateful vs Stateless Components | Stateful have internal state; Stateless rely on props only. |
| PureComponent vs React.memo | Both skip re-renders via shallow comparisons. |
| Controlled vs Uncontrolled Components | Controlled managed by React; Uncontrolled by native UI. |
| useLayoutEffect vs useEffect | `useLayoutEffect` runs sync after layout, `useEffect` async after paint. |
| How to persist app state? | AsyncStorage, Redux Persist, Recoil Persist, MMKV. |
| What is forwardRef? | Passes ref to child component or native view. |


### Native Modules & Background Behavior

| Question | Answer |
|-----------|---------|
| Example of a Native Module | Use `@ReactMethod` in Java/Kotlin and access via `NativeModules` in JS. |
| What are TurboModules? | JSI-based modules — faster, lazy-loaded, type-safe. |
| How does `setTimeout`/`setInterval` behave in background? | Suspended or delayed; use native background services for reliability. |

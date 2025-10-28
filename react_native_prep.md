<style>
table {
  width: 100%;
  border-collapse: collapse;
  display: block;
  overflow-x: auto;
  white-space: normal;
  margin-top: 16px;
}
th, td {
  border: 1px solid #ccc;
  padding: 8px 12px;
  text-align: left;
  vertical-align: top;
}
tr:nth-child(even) {
  background: #f9f9f9;
}
code {
  background: #f3f3f3;
  padding: 2px 4px;
  border-radius: 4px;
}
</style>

# React Native + Advanced JavaScript Interview Questions

| # | Question | Key Idea / Concept |
|---|-----------|--------------------|
| | **JavaScript Fundamentals** | |
| **1** | What is the difference between `==` and `===`? | `==` performs type coercion; `===` checks value and type strictly. |
| **2** | Difference between `null` and `undefined` | `null`: assigned empty value; `undefined`: uninitialized variable. |
| **3** | What is Hoisting? | Variable and function declarations moved to top of scope before execution. |
| **4** | What is the Temporal Dead Zone (TDZ)? | Period between block start and variable initialization with `let`/`const`. |
| **5** | Can you redeclare `let` and `const` variables? | No — redeclaring in same scope throws `SyntaxError`. |
| **6** | Does `const` make values immutable? | No — it prevents reassignment but allows internal mutation. |
| **7** | What is Strict Mode? | `"use strict"` enforces stricter parsing and error handling. |
| **8** | What is Scope? | Determines variable accessibility in code at runtime. |
| **9** | What is the `void` operator? | Evaluates an expression and returns `undefined`. |
| **10** | Features of ES6 | Includes `let`, `const`, arrow functions, modules, promises, destructuring, etc. |
| | **Functions & Closures** | |
| **11** | What is a Closure? | A function that remembers variables from its lexical scope. |
| **12** | What is an Anonymous Function? | Function without a name, often used as callback. |
| **13** | What is a First-Class Function? | Functions are treated as values — can be passed, returned, or stored. |
| **14** | What is a Higher-Order Function? | Accepts or returns other functions; enables composition. |
| **15** | What is Currying? | Breaking multi-arg function into nested single-arg functions. |
| **16** | Example of Currying in JS | `const add = a => b => c => a + b + c;` |
| **17** | What are Arrow Functions? | Lexically bind `this`, cannot be constructors or generators. |
| **18** | What is `this` keyword? | Refers to context of current execution; depends on call-site. |
| **19** | Difference between call, apply, and bind | `call()` and `apply()` invoke immediately; `bind()` returns a bound function. |
| | **Objects & Operators** | |
| **20** | What is a Proxy object? | Intercepts operations like get/set on objects. |
| **21** | How to copy properties from one object to another? | Use `Object.assign(target, source)`. |
| **22** | How to get object keys? | `Object.keys(obj)` returns array of keys. |
| **23** | What is `Object.freeze()` used for? | Prevents object mutation (used for immutable state). |
| | **Arrays & Iteration** | |
| **24** | What is Array.slice()? | Returns shallow copy of selected portion (non-mutating). |
| **25** | What is Array.splice()? | Adds/removes items, mutates original array. |
| **26** | What is Rest Parameter? | Collects all remaining args into an array. |
| **27** | What happens if rest parameter isn’t last? | Syntax error — must be last argument. |
| **28** | What is Spread Operator? | Expands iterable into elements. |
| **29** | Purpose of compareFunction in sort() | Defines custom sorting order logic. |
| **30** | How synchronous iteration works | Uses `Symbol.iterator` and `next()` to produce iterator results. |
| | **Async JavaScript** | |
| **31** | What is the Event Loop? | Continuously checks call stack and queues for tasks. |
| **32** | What is the Call Stack? | Tracks active function calls (LIFO). |
| **33** | What is the Event Queue / Macrotask Queue? | Holds async callbacks to be pushed to call stack. |
| **34** | What is a Promise? | Object representing eventual completion of async task. |
| **35** | Why do we need Promises? | To handle async operations cleanly (avoid callback hell). |
| **36** | What is Callback Hell? | Nested callbacks making async code hard to read. |
| **37** | What is Promise Chaining? | Sequential execution using `.then()`. |
| **38** | What is Promise.all()? | Resolves when all promises succeed; rejects on any failure. |
| **39** | What is Promise.race()? | Resolves/rejects as soon as one promise settles. |
| **40** | What is Memoization? | Caching function results to avoid redundant computation. |
| | **Functional Programming** | |
| **41** | What is a Pure Function? | Deterministic, no side effects — ideal for React rendering. |
| **42** | What is Debounce vs Throttle? | Debounce delays until idle; Throttle limits rate of calls. |
| **43** | What is Currying? | Transform multi-arg function into chained single-arg functions. |
| | **React Native Architecture** | |
| **44** | Difference between old and new RN architecture | Old: bridge-based; New: Fabric + JSI + TurboModules (faster). |
| **45** | How does reconciliation differ from React DOM? | RN maps components to native UI, not the DOM. |
| **46** | How does the Bridge affect performance? | Async JSON serialization adds latency; JSI removes it. |
| **47** | What is Fabric? | New rendering system improving UI concurrency and performance. |
| **48** | What is Yoga? | C++ Flexbox layout engine powering RN layouts. |
| **49** | What are TurboModules? | Next-gen native modules via JSI, lazy-loaded and type-safe. |
| **50** | What is Hermes Engine? | Optimized JS engine for RN improving startup & memory. |
| **51** | What is Flipper? | RN debugging suite for network, layout, logs, memory. |
| **52** | What is Watchman? | File watcher for Metro bundler’s fast rebuilds. |
| | **React Native Performance & Debugging** | |
| **53** | How to detect memory leaks in RN? | Use Flipper Memory, Android Profiler, clean up listeners/timers. |
| **54** | How to optimize startup time? | Lazy imports, Hermes, inline requires, smaller bundles. |
| **55** | How to optimize bundle size? | Tree shaking, dynamic imports, asset compression. |
| **56** | Crash only on Android release — what to check? | ProGuard/R8 config, Hermes mismatch, native deps. |
| **57** | Useful Flipper plugin example | Network plugin for debugging API calls. |
| | **React Native Security & Networking** | |
| **58** | What is SSL Pinning? | Embeds cert to prevent MITM attacks. |
| **59** | How to secure OTA updates? | Code signing, avoid secrets, validate runtime integrity. |
| **60** | How to handle offline-first design? | Cache data, queue requests, background sync. |
| | **React Native Features & APIs** | |
| **61** | What is Deep Linking? | Opens app via custom URLs or universal links. |
| **62** | How to implement background tasks? | Headless JS, background-fetch, native services. |
| **63** | How to implement accessibility? | Use `accessible`, `accessibilityLabel`, and roles. |
| **64** | What is SafeAreaView? | Prevents content overlap with device notches. |
| | **React Native Components & State** | |
| **65** | Stateful vs Stateless components | Stateful has internal state; Stateless depends on props. |
| **66** | PureComponent vs React.memo | Both skip re-renders via shallow prop/state check. |
| **67** | Controlled vs Uncontrolled components | Controlled managed by React; Uncontrolled by native. |
| **68** | useLayoutEffect vs useEffect | LayoutEffect runs sync after layout, Effect async after paint. |
| **69** | How to persist app state? | AsyncStorage, Redux Persist, Recoil Persist, MMKV. |
| **70** | What is forwardRef? | Passes ref through components to child native elements. |
| | **React Native Native Modules** | |
| **71** | Example of a Native Module | Expose native method via `@ReactMethod` and `NativeModules`. |
| **72** | How to write TurboModule? | Use Codegen for type-safe native bindings via JSI. |
| **73** | How does `setTimeout`/`setInterval` behave in background? | Suspended or delayed; use native schedulers for reliability. |
---

# ⚡ React Native + Advanced JavaScript Interview Questions

| # | Category | Question | Key Idea / Concept |
|---|-----------|-----------|--------------------|
| **1** | **JavaScript Fundamentals** | What is the difference between `==` and `===`? | `==` performs type coercion; `===` checks value and type strictly. |
| **2** | JavaScript Fundamentals | Difference between `null` and `undefined` | `null`: assigned empty value; `undefined`: uninitialized variable. |
| **3** | JavaScript Fundamentals | What is Hoisting? | Variable and function declarations moved to top of scope before execution. |
| **4** | JavaScript Fundamentals | What is the Temporal Dead Zone (TDZ)? | Period between block start and variable initialization with `let`/`const`. |
| **5** | JavaScript Fundamentals | Can you redeclare `let` and `const` variables? | No — redeclaring in same scope throws `SyntaxError`. |
| **6** | JavaScript Fundamentals | Does `const` make values immutable? | No — it prevents reassignment but allows internal mutation. |
| **7** | JavaScript Fundamentals | What is Strict Mode? | `"use strict"` enforces stricter parsing and error handling. |
| **8** | JavaScript Fundamentals | What is Scope? | Determines variable accessibility in code at runtime. |
| **9** | JavaScript Fundamentals | What is the `void` operator? | Evaluates an expression and returns `undefined`. |
| **10** | JavaScript Fundamentals | Features of ES6 | Includes `let`, `const`, arrow functions, modules, promises, destructuring, etc. |

| **11** | **Functions & Closures** | What is a Closure? | A function that remembers variables from its lexical scope. |
| **12** | Functions & Closures | What is an Anonymous Function? | Function without a name, often used as callback. |
| **13** | Functions & Closures | What is a First-Class Function? | Functions are treated as values — can be passed, returned, or stored. |
| **14** | Functions & Closures | What is a Higher-Order Function? | Accepts or returns other functions; enables composition. |
| **15** | Functions & Closures | What is Currying? | Breaking multi-arg function into nested single-arg functions. |
| **16** | Functions & Closures | Example of Currying in JS | `const add = a => b => c => a + b + c;` |
| **17** | Functions & Closures | What are Arrow Functions? | Lexically bind `this`, cannot be constructors or generators. |
| **18** | Functions & Closures | What is `this` keyword? | Refers to context of current execution; depends on call-site. |
| **19** | Functions & Closures | Difference between call, apply, and bind | `call()` and `apply()` invoke immediately; `bind()` returns a bound function. |

| **20** | **Objects & Operators** | What is a Proxy object? | Intercepts operations like get/set on objects. |
| **21** | Objects & Operators | How to copy properties from one object to another? | Use `Object.assign(target, source)`. |
| **22** | Objects & Operators | How to get object keys? | `Object.keys(obj)` returns array of keys. |
| **23** | Objects & Operators | What is `Object.freeze()` used for? | Prevents object mutation (used for immutable state). |

| **24** | **Arrays & Iteration** | What is Array.slice()? | Returns shallow copy of selected portion (non-mutating). |
| **25** | Arrays & Iteration | What is Array.splice()? | Adds/removes items, mutates original array. |
| **26** | Arrays & Iteration | What is Rest Parameter? | Collects all remaining args into an array. |
| **27** | Arrays & Iteration | What happens if rest parameter isn’t last? | Syntax error — must be last argument. |
| **28** | Arrays & Iteration | What is Spread Operator? | Expands iterable into elements. |
| **29** | Arrays & Iteration | Purpose of compareFunction in sort() | Defines custom sorting order logic. |
| **30** | Arrays & Iteration | How synchronous iteration works | Uses `Symbol.iterator` and `next()` to produce iterator results. |

| **31** | **Async JavaScript** | What is the Event Loop? | Continuously checks call stack and queues for tasks. |
| **32** | Async JavaScript | What is the Call Stack? | Tracks active function calls (LIFO). |
| **33** | Async JavaScript | What is the Event Queue / Macrotask Queue? | Holds async callbacks to be pushed to call stack. |
| **34** | Async JavaScript | What is a Promise? | Object representing eventual completion of async task. |
| **35** | Async JavaScript | Why do we need Promises? | To handle async operations cleanly (avoid callback hell). |
| **36** | Async JavaScript | What is Callback Hell? | Nested callbacks making async code hard to read. |
| **37** | Async JavaScript | What is Promise Chaining? | Sequential execution using `.then()`. |
| **38** | Async JavaScript | What is Promise.all()? | Resolves when all promises succeed; rejects on any failure. |
| **39** | Async JavaScript | What is Promise.race()? | Resolves/rejects as soon as one promise settles. |
| **40** | Async JavaScript | What is Memoization? | Caching function results to avoid redundant computation. |

| **41** | **Functional Programming** | What is a Pure Function? | Deterministic, no side effects — ideal for React rendering. |
| **42** | Functional Programming | What is Debounce vs Throttle? | Debounce delays until idle; Throttle limits rate of calls. |
| **43** | Functional Programming | What is Currying? | Transform multi-arg function into chained single-arg functions. |

| **44** | **React Native Architecture** | Difference between old and new RN architecture | Old: bridge-based; New: Fabric + JSI + TurboModules (faster). |
| **45** | Architecture | How does reconciliation differ from React DOM? | RN maps components to native UI, not the DOM. |
| **46** | Architecture | How does the Bridge affect performance? | Async JSON serialization adds latency; JSI removes it. |
| **47** | Architecture | What is Fabric? | New rendering system improving UI concurrency and performance. |
| **48** | Architecture | What is Yoga? | C++ Flexbox layout engine powering RN layouts. |
| **49** | Architecture | What are TurboModules? | Next-gen native modules via JSI, lazy-loaded and type-safe. |
| **50** | Architecture | What is Hermes Engine? | Optimized JS engine for RN improving startup & memory. |
| **51** | Architecture | What is Flipper? | RN debugging suite for network, layout, logs, memory. |
| **52** | Architecture | What is Watchman? | File watcher for Metro bundler’s fast rebuilds. |

| **53** | **React Native Performance & Debugging** | How to detect memory leaks in RN? | Use Flipper Memory, Android Profiler, clean up listeners/timers. |
| **54** | Performance | How to optimize startup time? | Lazy imports, Hermes, inline requires, smaller bundles. |
| **55** | Performance | How to optimize bundle size? | Tree shaking, dynamic imports, asset compression. |
| **56** | Debugging | Crash only on Android release — what to check? | ProGuard/R8 config, Hermes mismatch, native deps. |
| **57** | Debugging | Useful Flipper plugin example | Network plugin for debugging API calls. |

| **58** | **React Native Security & Networking** | What is SSL Pinning? | Embeds cert to prevent MITM attacks. |
| **59** | Security | How to secure OTA updates? | Code signing, avoid secrets, validate runtime integrity. |
| **60** | Networking | How to handle offline-first design? | Cache data, queue requests, background sync. |

| **61** | **React Native Features & APIs** | What is Deep Linking? | Opens app via custom URLs or universal links. |
| **62** | Background | How to implement background tasks? | Headless JS, background-fetch, native services. |
| **63** | Accessibility | How to implement accessibility? | Use `accessible`, `accessibilityLabel`, and roles. |
| **64** | UI | What is SafeAreaView? | Prevents content overlap with device notches. |

| **65** | **React Native Components & State** | Stateful vs Stateless components | Stateful has internal state; Stateless depends on props. |
| **66** | Components | PureComponent vs React.memo | Both skip re-renders via shallow prop/state check. |
| **67** | Components | Controlled vs Uncontrolled components | Controlled managed by React; Uncontrolled by native. |
| **68** | Hooks | useLayoutEffect vs useEffect | LayoutEffect runs sync after layout, Effect async after paint. |
| **69** | Hooks | How to persist app state? | AsyncStorage, Redux Persist, Recoil Persist, MMKV. |
| **70** | Components | What is forwardRef? | Passes ref through components to child native elements. |

| **71** | **React Native Native Modules** | Example of a Native Module | Expose native method via `@ReactMethod` and `NativeModules`. |
| **72** | Native | How to write TurboModule? | Use Codegen for type-safe native bindings via JSI. |
| **73** | Background | How does `setTimeout`/`setInterval` behave in background? | Suspended or delayed; use native schedulers for reliability. |

---

# frontend-basic

## upload

## basic

### sage-get

```javascript

const safeGet = (obj, path) => path.reduce((acc, key) => acc?.[key], obj);
safeGet({a: {b: {c: 1}}}, ['a', 'b', 'c']); // 1
safeGet({a: {b: {c: 1}}}, ['a', 'b', 'd']); // undefined
safeGet({a: {b: {c: 1}}}, ['a', 'b', 'd', 'e']); // undefined
safeGet({a: {b: {c: 1}}}, ['a', 'b', 'd', 'e', 'f']); // undefined

```

## throttle

```javascript

const throttle = (fn, delay) => {
  let lastCall = 0;
  return (...args) => {
    const now = new Date().getTime();
    if (now - lastCall < delay) {
      return;
    }
    lastCall = now;
    return fn(...args);
  };
};
const fn = (a, b) => a + b;
const throttledFn = throttle(fn, 1000);
throttledFn(1, 2); // 3
throttledFn(1, 2); // undefined

```

## debounce

```javascript

const debounce = (fn, delay) => {
  let timer;
  return (...args) => {
    clearTimeout(timer);
    timer = setTimeout(() => {
      fn(...args);
    }, delay);
  };
};
const fn = (a, b) => a + b;
const debouncedFn = debounce(fn, 1000);
debouncedFn(1, 2); // undefined
debouncedFn(1, 2); // undefined

```

### curry

```javascript

const curry = (fn) => {
  return function curried(...args) {
    if (args.length >= fn.length) {
      return fn.apply(this, args);
    } else {
      return function (...args2) {
        return curried.apply(this, args.concat(args2));
      };
    }
  };
};
const add = (a, b, c) => a + b + c;
const curriedAdd = curry(add);
curriedAdd(1, 2, 3); // 6
curriedAdd(1)(2, 3); // 6
curriedAdd(1, 2)(3); // 6
curriedAdd(1)(2)(3); // 6

```


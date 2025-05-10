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

### proxy

```javascript

const proxy = (obj, handler) => {
  return new Proxy(obj, handler);
};
const obj = {a: 1, b: 2};
const handler = {
  get(target, prop) {
    return target[prop];
  },
  set(target, prop, value) {
    target[prop] = value;
    return true;
  },
};
const proxiedObj = proxy(obj, handler);
proxiedObj.a; // 1
proxiedObj.b; // 2
proxiedObj.c = 3; // true
proxiedObj.c; // 3

```

### promise

```javascript

const promise = (fn) => {
  return new Promise((resolve, reject) => {
    fn(resolve, reject);
  });
};
const fn = (resolve, reject) => {
  
}
const p = promise(fn);
p.then((res) => {
  console.log(res);
});
p.catch((err) => {
  
})

```

### promiseify

```javascript

const promiseify = (fn) => {
  return function (...args) {
    return new Promise((resolve, reject) => {
      fn(...args, (err, res) => {
        if (err) {
          reject(err);
        } else {
          resolve(res);
        }
      });
    });
  };
};
const fs = require('fs');
const readFile = promiseify(fs.readFile);
readFile('file.txt', 'utf8').then((res) => {
  console.log(res);
});
readFile('file.txt', 'utf8').catch((err) => {
  
})

```

### queue promise

```javascript

const queuePromise = (promises, limit) => {
  return new Promise((resolve, reject) => {
    let results = [];
    let count = 0;
    let index = 0;
    const next = () => {
      if (index === promises.length) {
        resolve(results);
        return;
      }
      const promise = promises[index++];
      promise.then((res) => {
        results.push(res);
        count++;
        if (count === promises.length) {
          resolve(results); 
        } 
      }) 
    } 
  })  
}

const p1 = new Promise((resolve, reject) => {
  resolve(1); 
})
const p2 = new Promise((resolve, reject) => {
  resolve(2); 
})
const p3 = new Promise((resolve, reject) => {
  resolve(3); 
})
const p4 = new Promise((resolve, reject) => {
  resolve(4); 
})
const p5 = new Promise((resolve, reject) => {
  resolve(5); 
})
queuePromise([p1, p2, p3, p4, p5], 2).then((res) => {
  console.log(res); 
})

```

### promise.all

```javascript

const promiseAll = (promises) => {
  return new Promise((resolve, reject) => {
    let results = [];
    let count = 0; 
    for (let i = 0; i < promises.length; i++) {
      promises[i].then((res) => {
        results[i] = res;
        count++;
        if (count === promises.length) {
          resolve(results);
        } 
      }) 
    }
  })  
}
const p1 = new Promise((resolve, reject) => {
  resolve(1);
})
const p2 = new Promise((resolve, reject) => {
  resolve(2); 
})
const p3 = new Promise((resolve, reject) => {
  resolve(3); 
})
const p4 = new Promise((resolve, reject) => {
  resolve(4); 
})
const p5 = new Promise((resolve, reject) => {
  resolve(5); 
})
PromiseAll([p1, p2, p3, p4, p5]).then((res) => {
  console.log(res); 
})

```

### promise.race

```javascript

const promiseRace = (promises) => {
  return new Promise((resolve, reject) => {
    for (let i = 0; i < promises.length; i++) {
      promises[i].then((res) => {
        resolve(res);
      })
    }
  })
}

const p1 = new Promise((resolve, reject) => {
  resolve(1); 
})
const p2 = new Promise((resolve, reject) => {
  resolve(2); 
})
const p3 = new Promise((resolve, reject) => {
  resolve(3); 
})
const p4 = new Promise((resolve, reject) => {
  resolve(4);
})
const p5 = new Promise((resolve, reject) => {
  resolve(5); 
})
PromiseRace([p1, p2, p3, p4, p5]).then((res) => {
  console.log(res); 
})
```

### promise.any

```javascript
const promiseAny = (promises) => {
  return new Promise((resolve, reject) => {
    let count = 0;
    for (let i = 0; i < promises.length; i++) {
      promises[i].then((res) => {
        resolve(res); 
      }) 
    } 
  }) 
}
const p1 = new Promise((resolve, reject) => {
  resolve(1); 
})
const p2 = new Promise((resolve, reject) => {
  resolve(2); 
})
const p3 = new Promise((resolve, reject) => {
  resolve(3); 
})
const p4 = new Promise((resolve, reject) => {
  resolve(4); 
})
const p5 = new Promise((resolve, reject) => {
  resolve(5); 
})
PromiseAny([p1, p2, p3, p4, p5]).then((res) => {
  console.log(res); 
})
```

### promise.allSettled

```javascript
const promiseAllSettled = (promises) => {
  return new Promise((resolve, reject) => {
    let results = [];
    let count = 0;
    for (let i = 0; i < promises.length; i++) {
      promises[i].then((res) => {
        results[i] = {status: 'fulfilled', value: res};
        count++;
        if (count === promises.length) {
          resolve(results);
        }
      })
    } 
  }) 
}
const p1 = new Promise((resolve, reject) => {
  resolve(1); 
})
const p2 = new Promise((resolve, reject) => {
  resolve(2); 
})
const p3 = new Promise((resolve, reject) => {
  resolve(3); 
})
const p4 = new Promise((resolve, reject) => {
  resolve(4); 
})
const p5 = new Promise((resolve, reject) => {
  resolve(5); 
})
PromiseAllSettled([p1, p2, p3, p4, p5]).then((res) => {
  console.log(res); 
})

```

### promise.finally

```javascript
const promiseFinally = (promise, fn) => {
  return promise.then((res) => {
    fn();
    return res;
  })
}
const p1 = new Promise((resolve, reject) => {
  resolve(1); 
})
PromiseFinally(p1, () => {
  console.log('finally'); 
}).then((res) => {
  console.log(res);
})

```

### promise.catch

```javascript
const promiseCatch = (promise, fn) => {
  return promise.then((res) => {
    return res;
  }, (err) => {
    fn(err);
  })
}
const p1 = new Promise((resolve, reject) => {
  reject(1); 
})
PromiseCatch(p1, (err) => {
  console.log(err); 
})

```

### promise.resolve

```javascript
const promiseResolve = (value) => {
  return new Promise((resolve, reject) => {
    resolve(value);
  })
}
PromiseResolve(1).then((res) => {
  console.log(res); 
})

```

### promise.reject

```javascript
const promiseReject = (value) => {
  return new Promise((resolve, reject) => {
    reject(value); 
  })  
}
PromiseReject(1).then((res) => {
  console.log(res); 
})

```

### promise.try

```javascript
const promiseTry = (fn) => {
  return new Promise((resolve, reject) => {
    try {
      resolve(fn());
    } catch (err) {
      reject(err);
    }
  })
}
PromiseTry(() => {
  throw new Error('error'); 
})
.then((res) => {
  console.log(res); 
})
.catch((err) => {
  console.log(err); 
})

```

### async-await

```javascript

const asyncAwait = async () => {
  
}
asyncAwait();

```

### generator

```javascript

function* generator() {
  
}
const g = generator();
g.next(); // {value: undefined, done: true}

```

### iterator

```javascript

const iterator = {
  next() {
    return {value: undefined, done: true};
  }
}
const i = iterator[Symbol.iterator]();
i.next(); // {value: undefined, done: true}

```

### class

```javascript

class MyClass {
  constructor() {
    this.a = 1;
    this.b = 2;
  }
  method() {
    return this.a + this.b;
  }
}
const myClass = new MyClass();
myClass.method(); // 3

```

### inheritance

```javascript

class MyClass extends MyClass {
  constructor() {
    super();
    this.c = 3;
  }
  method() {
    return super.method() + this.c;
  }
}
const myClass = new MyClass();
myClass.method(); // 6

```

### mixin

```javascript

const mixin = (obj1, obj2) => {
  return Object.assign({}, obj1, obj2);
}
const obj1 = {a: 1, b: 2};
const obj2 = {c: 3, d: 4};
const obj3 = mixin(obj1, obj2);
obj3; // {a: 1, b: 2, c: 3, d: 4}

```

### closure

```javascript

const closure = () => {
  
}
const c = closure();
c(); // undefined

```

### module

```javascript

const module = () => {
  
}
module(); // undefined

```

### singleton

```javascript

const singleton = () => {
  
}
const s = singleton();
s(); // undefined

```

### factory

```javascript

const factory = () => {
  
}
const f = factory();
f(); // undefined

```

### builder

```javascript

const builder = () => {
  
}
const b = builder();
b(); // undefined

```

### facade

```javascript

const facade = () => {
  
}
const f = facade();
f(); // undefined

```

### decorator

```javascript

const decorator = (fn) => {
  return function (...args) {
    return fn.apply(this, args);
  };
}
const fn = (a, b) => a + b;
const decoratedFn = decorator(fn);
decoratedFn(1, 2); // 3

```

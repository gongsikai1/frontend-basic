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
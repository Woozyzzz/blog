# 刁钻

## 1 `【1,2,3】.map(parseInt)`

- 展开
  - `parseInt` 第二个参数为进制
  - `parseInt(1,0,arr)` => parseInt(1) => 1
    - 0 进制不存在
  - `parseInt(2,1,arr)` => NaN
    - 2 在 1 进制中不存在
  - `parseInt(3,2,arr)`
    - 3 在 2 进制中不存在
- 答案
  - `[1, NaN, NaN]`
  - 正确写法
    - `[1,2,3].map(number. index, array)=>{pareInt(number)}

## 2 `a.x = a = {}`

```javascript
// JavaScript
// 要点：用地址表达
var a = { x: 1 };
var b = a;
a.x = a = { x: 2 };

console.log(a.x);
console.log(b.x);
```

## 3 `if true / function a / a = 2`

```javascript
// JavaScript
// 未定义行为 答案不唯一
var a = 0;
if (true) {
  a = 1;
  function a() {
    return 3;
  }
  a = 2;
  console.log(a);
}
console.log(a);
```

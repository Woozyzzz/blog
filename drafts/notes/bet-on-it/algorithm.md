# 算法

## 1 大数相加

- 背景：JS 中数字接近 16 位时，无法有效存储

```javascript
// JavaScript
// 背景：JS 中数字接近 16 位时，无法有效存储
// 题目
// 接收两个内容为数字的字符串，输出内容为数字的字符串

const add = (a, b) => {
  // ...
  return sum;
};

console.log(add("7777777777123456789", "11111111111987654321"));
console.log(7777777777123456789n + 11111111111987654321n);
```

```javascript
// JavaScript
// 思路：字符串竖式计算，从右往左，倒序遍历，超过9要进位；
// 也可以写15位加速版，前15位先按数字计算；
// 网上多为先转化为数组后join的方法；
// 队列也可以。
const add = (a, b) => {
  const [lengthA, lengthB] = [a.length, b.length];
  const maxLength = Math.max(lengthA, lengthB);
  const diffLengthA = maxLength - lengthA;
  const diffLengthB = maxLength - lengthB;
  const filledA = "0".repeat(diffLengthA) + a;
  const filledB = "0".repeat(diffLengthB) + b;

  let sum = "";
  let overflow = false;
  for (let index = maxLength - 1; index >= 0; index--) {
    const [currentA, currentB] = [
      parseInt(filledA[index]),
      parseInt(filledB[index]),
    ];
    const currentSumNumber = currentA + currentB + (overflow ? 1 : 0);
    const currentSumString = `${currentSumNumber}`.slice(-1);
    overflow = currentSumNumber > 9;
    sum = currentSumString + sum;
  }

  return sum;
};

console.log(add("7777777777123456789", "11111111111987654321"));
console.log(7777777777123456789n + 11111111111987654321n);
```

## 2 两数之和

```javascript
// JavaScript
// 题目
// 要求返回 numbers 中 相加等于 target 的元素索引
// 出题者保证：
// 1. numbers 中的数字不会重复
// 2. 有且只有一个有效答案
const numbers = [2, 7, 11, 15];
const target = 9;
const twoSum = () => {
  // ...
};
console.log(twoSum(numbers, target));
```

```javascript
// JavaScript
// 思路：常规解法两层for循环；标准解法用hashMap遍历一次
const numbers = [2, 7, 11, 15];
const target = 9;

const twoSum = () => {
  const hashMap = new Map();
  for (let index = 0; index < numbers.length; index++) {
    const currentNumber = numbers[index];
    const diff = target - currentNumber;
    if (!hashMap.has(diff)) {
      hashMap.set(currentNumber, index);
    } else {
      return [hashMap.get(diff), index];
    }
  }
};
console.log(twoSum(numbers, target));
```

## 3 无重复最长子串的长度

```javascript
// JavaScript
// 题目
// 编写一个函数接收一个字符串，返回这个字符串中最长字符串的长度，要求字符串不能有重复的字符
const lengthOfLongestSubstring = (s) => {
  // ...
  return max;
};
console.log(lengthOfLongestSubstring("abcabcbb"));
```

```javascript
// JavaScript
// 标准解法：滑动窗口法 两根手指
const lengthOfLongestSubstring = (s) => {
  let max = s ? 1 : 0;
  for (let leftIndex = 0; leftIndex < s.length; leftIndex++) {
    for (let rightIndex = leftIndex + 1; rightIndex < s.length; rightIndex++) {
      const currentString = s.slice(leftIndex, rightIndex);
      if (currentString.includes(s[rightIndex])) {
        break;
      } else {
        max = Math.max(max, `${currentString}${s[rightIndex]}`.length);
      }
    }
  }
  return max;
};

console.log(lengthOfLongestSubstring("abcabcbb"));
```

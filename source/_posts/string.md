#### 1.替换空格

请实现一个函数，把字符串 s 中的每个空格替换成"%20"。

```js
// JS 中字符串无法被修改，一旦给字符串变量重新赋值，就要花费时间和空间去重新新建一个字符串，
var replaceSpace = function (s) {
  s = s.split(""); // 将字符串转为数组
  let oldLen = s.length;
  let spaceCount = 0;
  for (let i = 0; i < oldLen; i++) {
    // 统计空格的数量
    if (s[i] === " ") spaceCount++;
  }
  s.length += spaceCount * 2; //计算新的字符串的长度
  for (let i = oldLen - 1, j = s.length - 1; i >= 0; i--, j--) {
    if (s[i] !== " ") s[j] = s[i];
    //从后向前赋值
    else {
      s[j - 2] = "%";
      s[j - 1] = "2";
      s[j] = "0";
      j -= 2;
    }
  }
  return s.join("");
};
// 原生api
var replaceSpace = function (s) {
  if (s.length === 0) {
    return s;
  }
  let array = s.split(" ");
  return array.join("%20");
};
```

#### 2. 第一个只出现一次的字符

在字符串 s 中找出第一个只出现一次的字符。如果没有，返回一个单空格。 s 只包含小写字母。

```js
// 原生API
var firstUniqChar = function (s) {
  for (let x of s) {
    if (s.indexOf(x) === s.lastIndexOf(x)) return x;
  }
  return " ";
};
// map
var firstUniqChar = function (s) {
  if (s.length === 0) {
    return " ";
  }
  let map = new Map();
  for (let x of s) {
    if (map.has(x)) {
      let count = map.get(x);
      map.set(x, count + 1);
    } else {
      map.set(x, 1);
    }
  }
  for (let x of s) {
    if (map.get(x) === 1) {
      return x;
    }
  }
  return " ";
};
// 队列
var firstUniqChar = function (s) {
  const map = new Map();
  const q = [];
  const n = s.length;
  // Array.from(): 将一个类数组转为一个数组实例，
  //entries() 方法返回一个新的Array Iterator对象，该对象包含数组中每个索引的键/值对
  for (let [i, ch] of Array.from(s).entries()) {
    if (!map.has(ch)) {
      // i 是下标，ch是值
      //map中不存在就保存在map和队列中
      map.set(ch, i);
      q.push([s[i], i]); //二维数组
    } else {
      position.set(ch, -1); //存在为-1
      while (q.length && position.get(q[0][0]) === -1) {
        //如果在队列最前面，出队列
        q.shift();
      }
    }
  }
  return q.length ? q[0][0] : " ";
};
```

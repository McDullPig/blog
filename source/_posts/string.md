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

#### 3. 把字符串转换成整数

写一个函数 StrToInt，实现把字符串转换成整数这个功能。不能使用 atoi 或者其他类似的库函数。

首先，该函数会根据需要丢弃无用的开头空格字符，直到寻找到第一个非空格的字符为止。

当我们寻找到的第一个非空字符为正或者负号时，则将该符号与之后面尽可能多的连续数字组合起来，作为该整数的正负号；假如第一个非空字符是数字，则直接将其与之后连续的数字字符组合起来，形成整数。

该字符串除了有效的整数部分之后也可能会存在多余的字符，这些字符可以被忽略，它们对于函数不应该造成影响。

注意：假如该字符串中的第一个非空格字符不是一个有效整数字符、字符串为空或字符串仅包含空白字符时，则你的函数不需要进行转换。

在任何情况下，若函数不能进行有效的转换时，请返回 0。

说明：假设我们的环境只能存储 32 位大小的有符号整数，那么其数值范围为 [−2^31, 2^31 − 1]。如果数值超过这个范围，请返回 INT_MAX (2^31 − 1) 或 INT_MIN (−2^31) 。

```
输入: "-91283472332"
输出: -2147483648
解释: 数字 "-91283472332" 超过 32 位有符号整数范围。 因此返回 INT_MIN (−2^31) 。
```

```js
// 1.正则表达式
//match: 一个在字符串中执行查找匹配的String方法，它返回一个数组，在未匹配到时会返回 null。
// 例子：['-12', index: 0, input: '-12i3dedw', groups: undefined]
var strToInt = function (str) {
  const num = str.trim().match(/^[+-]?\d+/);
  if (!num) return 0;
  let MIN_VALUE = Math.pow(-2, 31);
  let MAX_VALUE = Math.pow(2, 31) - 1;
  return num > MAX_VALUE ? MAX_VALUE : num < MIN_VALUE ? MIN_VALUE : num;
};
// 循环
var strToInt = function (str) {
  str = str.trim(); //清除两边的空格
  let MIN_VALUE = Math.pow(-2, 31);
  let MAX_VALUE = Math.pow(2, 31) - 1;
  let res = 0; //最后的值
  for (let i = 0; i < str.length; i++) {
    if (str[i] >= "0" && str[i] <= "9") {
      //如果是数字，就计算在res中
      res = res * 10 + str[i] * 1;
    } else if (i === 0 && (str[i] === "+" || str[i] === "-")) {
      //如果在开头是+-，就忽略
      continue;
    } else {
      //如果不是数字，就中断
      break;
    }
  }
  if (str[0] === "-") res = -res; //是负数
  return res < MIN_VALUE ? MIN_VALUE : res > MAX_VALUE ? MAX_VALUE : res;
};
```

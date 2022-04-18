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

#### 4. 左旋转字符串

字符串的左旋转操作是把字符串前面的若干个字符转移到字符串的尾部。请定义一个函数实现字符串左旋转操作的功能。比如，输入字符串"abcdefg"和数字 2，该函数将返回左旋转两位得到的结果"cdefgab"。

```js
var reverseLeftWords = function (s, n) {
  if (s.length === 0 || s.length <= n) {
    return s;
  }
  return s.slice(n) + s.slice(0, n);
};
```

#### 5. 翻转单词顺序

输入一个英文句子，翻转句子中单词的顺序，但单词内字符的顺序不变。为简单起见，标点符号和普通字母一样处理。例如输入字符串"I am a student. "，则输出"student. a am I"。

输入字符串可以在前面或者后面包含多余的空格，但是反转后的字符不能包括。如果两个单词间有多余的空格，将反转后单词间的空格减少到只含一个。

时间复杂度和空间复杂度都是 O(n)

```js
// 使用API
var reverseWords = function (s) {
  s = s.trim();
  if (s.length <= 1) {
    return s;
  }
  let array = s.split(" ").filter((item) => item.trim() !== "");
  return array.reverse().join(" ");
};
//正则：\s+ 表示多个空格
var reverseWords = function (s) {
  return s.trim().split(/\s+/).reverse().join(" ");
};
```

#### 6. 表示数值的字符串

请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。

数值（按顺序）可以分成以下几个部分：

1、若干空格
2、一个 小数 或者 整数
3、（可选）一个 'e' 或 'E' ，后面跟着一个 整数
4、若干空格
小数（按顺序）可以分成以下几个部分：

1、（可选）一个符号字符（'+' 或 '-'）
2、下述格式之一：
1、至少一位数字，后面跟着一个点 '.'
2、至少一位数字，后面跟着一个点 '.' ，后面再跟着至少一位数字
3、一个点 '.' ，后面跟着至少一位数字
整数（按顺序）可以分成以下几个部分：

1、（可选）一个符号字符（'+' 或 '-'）
2、至少一位数字

```js
// 使用isNaN
var isNumber = function (s) {
  s = s.trim();
  if (!s) return false;
  return !isNaN(s);
};
//使用循环去做
var isNumber = function (s) {
  s = s.trim();
  if (s === "") {
    return false;
  }
  let len = s.length;
  let numFlag = false;
  let dotFlag = false; //有没有点
  let eFlag = false; //有没有eE
  for (let i = 0; i < len; i++) {
    //判定为数字，则标记numFlag
    if (s[i] >= "0" && s[i] <= "9") {
      numFlag = true;
      //判定为.  需要没出现过.并且没出现过e
    } else if (s[i] == "." && !dotFlag && !eFlag) {
      dotFlag = true;
      //判定为e，需要没出现过e，并且出过数字了
    } else if ((s[i] == "e" || s[i] == "E") && !eFlag && numFlag) {
      eFlag = true;
      numFlag = false; //为了避免123e这种请求，出现e之后就标志为false
      //判定为+-符号，只能出现在第一位或者紧接e后面
    } else if (
      (s[i] == "+" || s[i] == "-") &&
      (i == 0 || s[i - 1] == "e" || s[i - 1] == "E")
    ) {
      continue;
      //其他情况，都是非法的
    } else {
      return false;
    }
  }
  return numFlag;
};
```

#### 7. 正则表达式匹配

请实现一个函数用来匹配包含'. '和'_'的正则表达式。模式中的字符'.'表示任意一个字符，而'_'表示它前面的字符可以出现任意次（含 0 次）。在本题中，匹配是指字符串的所有字符匹配整个模式。例如，字符串"aaa"与模式"a.a"和"ab*ac*a"匹配，但与"aa.a"和"ab\*a"均不匹配。

```js
var isMatch = function (s, p) {
  if (s == null || p == null) {
    return false;
  }
  let sIndex = 0,
    pIndex = 0;
  return matchCore(s, sIndex, p, pIndex);
};

const matchCore = (s, sIndex, p, pIndex) => {
  // 如果都走到结尾了，返回成功
  if (sIndex === s.length && pIndex === p.length) {
    return true;
  }
  if (sIndex !== s.length && pIndex === p.length) {
    return false;
  }
  // 如果第二个字符是*
  if (pIndex + 1 !== p.length && p[pIndex + 1] === "*") {
    if (
      (sIndex !== s.length && p[pIndex] === s[sIndex]) ||
      (p[pIndex] === "." && sIndex !== s.length)
    ) {
      return (
        matchCore(s, sIndex + 1, p, pIndex + 2) || // 如果第一个字符匹配，将比对值匹配了一个字符
        matchCore(s, sIndex + 1, p, pIndex) || //如果第一个字符是匹配，比对匹配多个字符
        matchCore(s, sIndex, p, pIndex + 2)
      ); // 匹配了0个字符,
    } else {
      //如果第一个字符就不匹配,连走两个
      return matchCore(s, sIndex, p, pIndex + 2);
    }
  }
  // 如果第二个字符不是*，
  if (
    (sIndex !== s.length && p[pIndex] === s[sIndex]) ||
    (p[pIndex] === "." && sIndex !== s.length)
  ) {
    //只有当第一个字符相等或者是.的时候继续遍历
    return matchCore(s, sIndex + 1, p, pIndex + 1);
  }
  return false;
};
```

#### 8. 最长不含重复字符的子字符串

请从字符串中找出一个最长的不包含重复字符的子字符串，计算该最长子字符串的长度。

> 输入: "abcabcbb"
> 输出: 3
> 解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。

```js
var lengthOfLongestSubstring = function (s) {
  let len = s.length;
  if (len <= 1) {
    return len;
  }
  let max = 0;
  let map = new Map(); //key=值，value=下标
  let slow = -1;
  let fast = 0; //快慢指针
  for (; fast < len; fast++) {
    if (map.has(s[fast]) && map.get(s[fast]) > slow) {
      //如果在map中存在，而且存在的这个下标在slow后面才赋值，在slow前面证明没有计算到这个值，可以忽略
      slow = map.get(s[fast]); //替换，
    } else {
      // 没重复：更新 max
      max = Math.max(max, fast - slow);
    }
    map.set(s[fast], fast); //放到map中
  }
  return max;
};
```

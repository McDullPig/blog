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
// 正则表达式
var replaceSpace = function (s) {
  if (s.length < 1) {
    return "";
  }
  return s.replace(/\s/g, "%20");
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

// 第二种：用数组去做
var lengthOfLongestSubstring = function (s) {
  const len = s.length;
  if (len <= 1) {
    return len;
  }
  let count = 0;
  let arr = [];
  for (let i = 0; i < len; i++) {
    if (!arr.includes(s[i])) {
      arr.push(s[i]);
      count = Math.max(count, arr.length);
    } else {
      while (arr[0] !== s[i]) {
        arr.shift();
      }
      arr.push(arr.shift());
    }
  }
  return count;
};
```

#### 9. 反转字符串

编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 s 的形式给出。

不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。

```js
// 双指针
var reverseString = function (s) {
  let len = s.length;
  if (len <= 1) {
    return;
  }
  let left = 0;
  let right = len - 1;
  while (left < right) {
    [s[left], s[right]] = [s[right], s[left]];
    left++;
    right--;
  }
};
```

#### 9-1. 反转字符串中的单词 III

给定一个字符串 s ，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。

```js
//输入：s = "Let's take LeetCode contest"
//输出："s'teL ekat edoCteeL tsetnoc"
var reverseWords = function (s) {
  if (s.trim() === "") {
    return s;
  }
  let arr = reverse(s); //先整体反转
  return arr.split(" ").reverse().join(" "); //然后按单词拆分数组，再反转拼接
};

var reverse = function (s) {
  if (s.trim() === "") {
    return s;
  }
  let arr = s.split("");
  let left = 0;
  let right = arr.length - 1;
  while (left < right) {
    [arr[left], arr[right]] = [arr[right], arr[left]];
    left++;
    right--;
  }
  return arr.join("");
};

// 第二种：简洁的方式
var reverseWords = function (s) {
  if (s.trim() === "") {
    return s;
  }
  // 字符串按空格进行分隔, 每一个单词作为数组一个值
  return s
    .split(" ")
    .map((item) => item.split("").reverse().join(""))
    .join(" ");
};
```

#### 10. 回文数

给你一个整数 x ，如果 x 是一个回文整数，返回 true ；否则，返回 false

回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

```js
var isPalindrome = function (x) {
  if (x < 0) {
    return false;
  }
  let s = x.toString();
  let left = 0;
  let right = s.length - 1;
  while (left < right) {
    if (s[left] !== s[right]) {
      return false;
    }
    left++;
    right--;
  }
  return true;
};
```

#### 11. 复原 IP 地址

有效 IP 地址 正好由四个整数（每个整数位于 0 到 255 之间组成，且不能含有前导 0），整数之间用 '.' 分隔。

例如："0.1.2.201" 和 "192.168.1.1" 是 有效 IP 地址，但是 "0.011.255.245"、"192.168.1.312" 和 "192.168@1.1" 是 无效 IP 地址。
给定一个只包含数字的字符串 s ，用以表示一个 IP 地址，返回所有可能的有效 IP 地址，这些地址可以通过在 s 中插入  '.' 来形成。
你不能重新排序或删除 s 中的任何数字。你可以按任何顺序返回答案。

```js
// 回溯，一般就是dfs
var restoreIpAddresses = function (s) {
  let res = [];
  const len = s.length;
  if (len < 4) {
    return res;
  }
  let segArray = new Array(4); // 存IP地址的
  const dfs = (s, segId, segStart) => {
    // 刚好4段，且长度为字符串的长度就加入数组，返回
    if (segId === 4) {
      if (segStart === len) {
        res.push(segArray.join("."));
      }
      return;
    }
    // 如果还没有找到 4 段 IP 地址就已经遍历完了字符串，那么提前回溯
    if (segStart === s.length) {
      return;
    }
    // 没有达到4段，但是当前的数字是0，需要额外判断
    if (s[segStart] === "0") {
      segArray[segId] = 0;
      dfs(s, segId + 1, segStart + 1);
    }
    let val = 0;
    for (let i = segStart; i < len; i++) {
      val = val * 10 + +s[i];
      // 当前存的值一定是大于0 ，小于255的
      if (val > 0 && val <= 255) {
        segArray[segId] = val;
        dfs(s, segId + 1, i + 1);
      } else {
        break;
      }
    }
  };
  dfs(s, 0, 0);
  return res;
};
```

#### 12. 最长回文子串

给你一个字符串 s，找到 s 中最长的回文子串。

```js
// 1、中心扩展：以一个字符为中心，先向左右找到相同的字符，因为无论单数还是双数的相同字符都是回文，在对比边界以内相同的字符
// 这个比较耗时
var longestPalindrome = function (s) {
  const len = s.length;
  if (len <= 1) {
    return s;
  }
  let max = 0; //当期最大回文串的长度
  let maxStr = ""; //当前最大的回文串
  for (let i = 0; i < len; i++) {
    let str = s[i]; //当前作为回文串的中心
    let l = i - 1;
    let r = i + 1; // 右侧遍历开始索引
    while (s[r] === s[i]) {
      //找到当前字符后连接的所有一样的字符,获取连续的字符
      str += s[r];
      r++;
    }
    while (s[l] === s[i]) {
      //找到当前字符前连接的所有一样的字符,获取连续的字符
      str += s[l];
      l--;
    }
    while (l >= 0 && r < len && s[l] === s[r] && s[l] !== undefined) {
      // 从连续字符两端开始像两侧扩展,直到越界或者不一致,一致的直接拼到 str 中
      str = s[l] + str + s[r];
      l--;
      r++;
    }
    if (str.length > max) {
      //判断和之前最大的比较
      max = str.length;
      maxStr = str;
    }
  }
  return maxStr;
};
```

```js
// 使用了快慢指针的思想 + 中心扩展
var longestPalindrome = function (s) {
  let max = 0; // 当前最大回文串的长度
  let start = -1; // 当前最大回文串的起始索引
  const l = s.length; // s 的长度
  for (let i = 0; i < l; i++) {
    // 遍历 s
    let now = 1; // 当前回文串的长度
    let l = i - 1; // 左侧开始遍历的指针
    while (s[i + 1] === s[i]) {
      // 如果当前字符后边的字符都一样, 当前长度 + 1,  s遍历指针向后推
      now++;
      i++;
    }
    let r = i + 1; // 获取右侧开始遍历的指针
    while (s[l] === s[r] && s[l] !== undefined) {
      // 从连续字符两端开始像两侧扩展,直到越界或者不一致,一致的直接累积到当前长度中,修改左右指针
      now += 2;
      l--;
      r++;
    }
    if (now > max) {
      // 判断与之前最大的对比,更新当前最大回文串的起始索引
      max = now;
      start = l + 1;
    }
  }
  return s.slice(start, start + max); // 通过最大长度和起始索引,获取需要的字符串
};
```

#### 13. Z 字形变换

将一个给定字符串 s 根据给定的行数 numRows ，以从上往下、从左到右进行  Z 字形排列。

比如输入字符串为 "PAYPALISHIRING"  行数为 3 时，排列如下：

```js
P   A   H   N
A P L S I I G
Y   I   R
```

之后，你的输出需要从左往右逐行读取，产生出一个新的字符串，比如："PAHNAPLSIIGYIR"。

```js
var convert = function (s, numRows) {
  // 如果只有一行，或者s的长度没有长过行数「只有一列」，就直接返回s即可
  if (numRows === 1 || s.length <= numRows) {
    return s;
  }
  let arr = new Array(numRows).fill(""); //不需要二维数组，用一维数组，每个值都是字符串
  let index = 0;
  let down = false; //等于false，因为下面for循环中index ===0,改了方向
  for (let val of s) {
    arr[index] += val;
    if (index === 0 || index === numRows - 1) {
      down = !down;
    }
    index += down ? 1 : -1; //每次向下走一个，或者向上走一个
  }
  return arr.join("");
};
```

#### 14. 最长公共前缀

编写一个函数来查找字符串数组中的最长公共前缀。

如果不存在公共前缀，返回空字符串 ""。

```js
var longestCommonPrefix = function (strs) {
  const len = strs.length;
  if (len < 1) {
    return "";
  }
  if (len === 1) {
    return strs[0];
  }
  let res = strs[0];
  for (let i = 1; i < len; i++) {
    let j = 0; //为啥要放在外面呢，因为需要在跳出来for循环之后才能截取,例如res =ab, strs[i] =a
    for (; j < res.length && j < strs[i].length; j++) {
      if (res[j] !== strs[i][j]) {
        break;
      }
    }
    res = res.substr(0, j);
  }
  return res;
};
```

#### 15. 字符串的排列

给你两个字符串 s1  和  s2 ，写一个函数来判断 s2 是否包含 s1  的排列。如果是，返回 true ；否则，返回 false 。

换句话说，s1 的排列之一是 s2 的子串。

```
输入：s1 = "ab" s2 = "eidbaooo"
输出：true
解释：s2 包含 s1 的排列之一 ("ba").
```

```js
var checkInclusion = function (s1, s2) {
  let len1 = s1.length;
  let len2 = s2.length;
  if (len1 > len2) {
    return false;
  }
  const cnt = new Array(26).fill(0); //因为英文字母就26个，所以使用一个长度是26的数组保存
  for (let i = 0; i < len1; i++) {
    --cnt[s1[i].charCodeAt() - "a".charCodeAt()]; //将存在的值都转成-1
  }
  // s1中的字符减1，s2中的字符加1，如果s2中的字符连续加1以后的值等于s1的长度就返回true
  let left = 0; // 计算一个起始值
  for (let right = 0; right < len2; right++) {
    const x = s2[right].charCodeAt() - "a".charCodeAt();
    ++cnt[x];
    while (cnt[x] > 0) {
      --cnt[s2[left].charCodeAt() - "a".charCodeAt()];
      ++left;
    }
    if (right - left + 1 === len1) {
      return true;
    }
  }
  return false;
};
```

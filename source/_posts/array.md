## 记录自己做过的算法和解析

### 1、数组

#### 1、有序数组的平方

给你一个按非递减顺序排序的整数数组 nums，返回 每个数字的平方 组成的新数组，要求也按 非递减顺序 排序。

```js
var sortedSquares = function (nums) {
  const len = nums.length;
  if (len < 1) {
    return [];
  }
  let res = nums.map((item) => {
    return item * item;
  });
  return res.sort((a, b) => a - b);
};
```

#### 2、轮转数组

给你一个数组，将数组中的元素向右轮转 k 个位置，其中 k 是非负数。

```js
// 1、在数组后面拿出一个，放在数组的最前面，unshift的时间复杂度应该是o(n)
var rotate = function (nums, k) {
  let len = nums.length;
  if (len < 1 || k <= 0) {
    return;
  }
  if (k > len) {
    k = k % len;
  }
  for (let i = 1; i <= k; i++) {
    nums.unshift(nums.pop());
  }
};

// 2、先翻转，然后分别去翻转
/**
 * @param {number[]} nums
 * @param {number} k
 * @return {void} Do not return anything, modify nums in-place instead.
 */
var rotate = function (nums, k) {
  let len = nums.length;
  if (len < 1 || k <= 0) {
    return;
  }
  if (k > len) {
    k = k % len;
  }
  nums = nums.reverse();
  reverse(nums, 0, k - 1);
  reverse(nums, k, len - 1);
};

function reverse(nums, left, right) {
  while (left < right) {
    [nums[left], nums[right]] = [nums[right], nums[left]];
    left++;
    right--;
  }
}
```

### 2、二分查找

#### 1、二分查找

给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索  nums  中的 target，如果目标值存在返回下标，否则返回 -1。

```js
var search = function (nums, target) {
  const len = nums.length;
  if (len < 1 || nums[0] > target || nums[len - 1] < target) {
    return -1;
  }
  let left = 0;
  let right = len - 1;
  while (left <= right) {
    const mid = left + ((right - left) >> 1);
    if (nums[mid] === target) {
      return mid;
    } else if (nums[mid] > target) {
      right = mid - 1;
    } else {
      left = mid + 1;
    }
  }
  return -1;
};
```

#### 2、第一个错误的版本

你是产品经理，目前正在带领一个团队开发新的产品。不幸的是，你的产品的最新版本没有通过质量检测。由于每个版本都是基于之前的版本开发的，所以错误的版本之后的所有版本都是错的。

假设你有 n 个版本 [1, 2, ..., n]，你想找出导致之后所有版本出错的第一个错误的版本。

你可以通过调用  bool isBadVersion(version)  接口来判断版本号 version 是否在单元测试中出错。实现一个函数来查找第一个错误的版本。你应该尽量减少对调用 API 的次数。

```js
/**
 * Definition for isBadVersion()
 *
 * @param {integer} version number
 * @return {boolean} whether the version is bad
 * isBadVersion = function(version) {
 *     ...
 * };
 */

/**
 * @param {function} isBadVersion()
 * @return {function}
 */
var solution = function (isBadVersion) {
  /**
   * @param {integer} n Total versions
   * @return {integer} The first bad version
   */
  return function (n) {
    let left = 1;
    let right = n;
    while (left <= right) {
      let mid = left + ((right - left) >> 1);
      //如果中间的值是错误的版本，就向前面的区间中找，
      if (isBadVersion(mid)) {
        right = mid - 1;
      } else {
        left = mid + 1;
      }
    }
    return left;
  };
};
```

#### 3、搜索插入位置

给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。

请必须使用时间复杂度为 O(log n) 的算法。

```js
var searchInsert = function (nums, target) {
  let len = nums.length;
  // 空数组或者比数组中第一个值小，就返回0
  if (len < 1 || nums[0] > target) {
    return 0;
  }
  // 比数组中最大的数都大，就返回len
  if (nums[len - 1] < target) {
    return len;
  }
  let left = 0;
  let right = len - 1;
  while (left <= right) {
    let mid = left + ((right - left) >> 1);
    if (nums[mid] > target) {
      right = mid - 1;
    } else if (nums[mid] < target) {
      left = mid + 1;
    } else {
      return mid;
    }
  }
  return left;
};
```

### 剑指 offer 中的算法

#### 1. 数组中重复的数字

找出数组中重复的数字。
在一个长度为 n 的数组 nums 里的所有数字都在 0 ～ n-1 的范围内。数组中某些数字是重复的，但不知道有几个数字重复了，也不知道每个数字重复了几次。请找出数组中任意一个重复的数字。

答案：

1. 使用哈希表

```js
// Map判断是否存在时：has(),  获取某个位置上的是get() ,   保存某个数据是set()
var findRepeatNumber = function (nums) {
  let map = new Map();
  for (let i of nums) {
    if (map.has(i)) return i;
    map.set(i, 1);
  }
  return null;
};
```

2. 利用长度为 n 的所有数字都是 0~n-1 范围内，遍历数组并交换位置，使元素的索引和值一一对应。

```js
/**
 * @param {number[]} nums
 * @return {number}
 */
//从头到尾依次扫描,如果当前项和下标不相等，则比较当前项和以当前值对应的下标的项是否相等，不想等则交换位置
var findRepeatNumber = function (nums) {
  let len = nums.length;
  if (len <= 1) {
    return 0;
  }
  let i = 0;
  while (i < len) {
    if (nums[i] === i) {
      i++;
      continue;
    }
    if (nums[i] === nums[nums[i]]) return nums[i];
    // [nums[i], nums[nums[i]]]=[nums[nums[i]], nums[i]]: 这样写会超时,因为nums[i]会变
    let temp = nums[i];
    nums[i] = nums[temp];
    nums[temp] = temp;
    // let temp = nums[i];  这两行也可以
    //[nums[i], nums[temp]] = [nums[temp], nums[i]];
  }
};
```

#### 2. 二维数据中的查找

在一个 n \* m 的二维数组中，每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个高效的函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

```js
/**
 * @param {number[][]} matrix
 * @param {number} target
 * @return {boolean}
 * 以右上角或左下角为原点，右上角为例，小于坐标上的值，往左⬅️走，大于坐标上的值，向下⬇️走，超过长度返回false
 */
var findNumberIn2DArray = function (matrix, target) {
  if (!matrix || !matrix[0]) {
    return false;
  }
  let rows = matrix.length;
  let cols = matrix[0].length - 1;
  let row = 0;
  let col = cols;
  while (row < rows && col >= 0) {
    if (matrix[row][col] > target) {
      col--;
    } else if (matrix[row][col] < target) {
      row++;
    } else {
      return true;
    }
  }
  return false;
};
```

#### 6. 顺时针打印矩阵

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字

```js
// 以回字路push到数组。
var spiralOrder = function (matrix) {
  if (matrix.length === 0 || matrix[0].length === 0) {
    return [];
  }
  let res = [];
  let rows = matrix.length;
  let cols = matrix[0].length;
  let count = Math.ceil(Math.min(rows, cols) / 2);
  for (let i = 0; i < count; i++) {
    for (let key = i; key < cols - i; key++) {
      res.push(matrix[i][key]);
    }
    for (let key = i + 1; key < rows - i; key++) {
      res.push(matrix[key][cols - 1 - i]);
    }
    for (let key = cols - 2 - i; key >= i && rows - 1 - i !== i; key--) {
      res.push(matrix[rows - 1 - i][key]);
    }
    for (let key = rows - 2 - i; key > i && cols - 1 - i !== i; key--) {
      res.push(matrix[key][i]);
    }
  }
  return res;
};
```

#### 7. 打印从 1 到最大的 n 位数

输入数字 n，按顺序打印出从 1 到最大的 n 位十进制数。比如输入 3，则打印出 1、2、3 一直到最大的 3 位数 999.

```js
输入: n = 1;
输出: [1, 2, 3, 4, 5, 6, 7, 8, 9];
var printNumbers = function (n) {
  if (n < 1) {
    return [];
  }
  let tail = Math.pow(10, n) - 1;
  let res = new Array(tail);
  //必须写fill(1),否则打印出来的是[,,,,,]
  return res.fill(1).map((i, index) => {
    return index + 1;
  });
};
```

#### 9. 数组中出现次数超过一半的数字

数组中有一个数字出现的次数超过数组长度的一半，请找出这个数字。
你可以假设数组是非空的，并且给定的数组总是存在多数元素

```js
var majorityElement = function (nums) {
  let len = nums.length;
  if (len === 0) {
    return 0;
  }
  if (len === 1) {
    return nums[0];
  }
  let count = 1;
  let num = nums[0];
  for (let i = 1; i < len; i++) {
    if (num === nums[i]) {
      count++;
    } else {
      count--;
      if (count === 0) {
        num = nums[i];
        count++;
      }
    }
  }
  return num;
};
```

#### 10. 青蛙跳台阶问题

一只青蛙一次可以跳上 1 级台阶，也可以跳上 2 级台阶。求该青蛙跳上一个 n 级的台阶总共有多少种跳法

```js
/**
 * @param {number} n
 * @return {number}
 */
var numWays = function (n) {
  if (n <= 1) {
    return 1;
  }
  let pre = 1;
  let prepre = 1;
  let res = 1;
  for (let i = 2; i <= n; i++) {
    res = (pre + prepre) % 1000000007;
    prepre = pre;
    pre = res;
  }
  return res;
};
```

#### 11. 斐波那契数列

写一个函数，输入 n ，求斐波那契（Fibonacci）数列的第 n 项（即 F(N)）。斐波那契数列的定义如下：

F(0) = 0,   F(1) = 1
F(N) = F(N - 1) + F(N - 2), 其中 N > 1.
斐波那契数列由 0 和 1 开始，之后的斐波那契数就是由之前的两数相加而得出。

答案需要取模 1e9+7（1000000007），如计算初始结果为：1000000008，请返回 1。

```js
/**
 * @param {number} n
 * @return {number}
 */
var fib = function (n) {
  if (n <= 0) {
    return 0;
  }
  if (n === 1 || n === 2) {
    return 1;
  }
  let pre = 1;
  let prepre = 1;
  let res = 1;
  for (let i = 3; i <= n; i++) {
    res = (pre + prepre) % 1000000007;
    prepre = pre;
    pre = res;
  }
  return res;
};
```

#### 12. 数值的整数次方

实现 pow(x, n) ，即计算 x 的 n 次幂函数（即，x^n）。不得使用库函数，同时不需要考虑大数问题。

```js
/**
 * @param {number} x
 * @param {number} n
 * @return {number}
 */
// 二分法
var myPow = function (x, n) {
  if (n === 0) return 1;
  if (n === 1) return x;
  if (n === -1) return 1 / x;
  if (n % 2 === 0) {
    let a = myPow(x, n / 2);
    return a * a;
  } else {
    let b = myPow(x, (n - 1) / 2);
    return b * b * x;
  }
};

// 位运算
// >>> 无符号右移, 除以2中的操作
// >>  右移
var myPow = function (x, n) {
  if (n === 0) return 1;
  if (n === 1) return x;
  let isNegative = n < 0;
  let res = 1;
  let y = Math.abs(n);
  while (y) {
    if (y & 1) {
      res = res * x;
    }
    x = x * x;
    y = y >>> 1;
  }
  return isNegative ? 1 / res : res;
};
```

#### 13. 旋转数组的最小数字

把一个数组最开始的若干个元素搬到数组的末尾，我们称之为数组的旋转。输入一个递增排序的数组的一个旋转，输出旋转数组的最小元素。例如，数组 [3,4,5,1,2] 为 [1,2,3,4,5] 的一个旋转，该数组的最小值为 1。

```js
// for循环从后往前遍历，找到比前面的值小的就返回，最好是O(1),最坏是O(n)
/**
 * @param {number[]} numbers
 * @return {number}
 */
var minArray = function (numbers) {
  if (numbers.length === 0) {
    return -1;
  }
  let len = numbers.length;
  for (let i = len - 1; i > 0; i--) {
    if (numbers[i] < numbers[i - 1]) {
      return numbers[i];
    }
  }
  return numbers[0];
};
// 二分法
var minArray = function (numbers) {
  if (numbers.length === 0) {
    return -1;
  }
  let len = numbers.length;
  let right = len - 1;
  let left = 0;
  while (left <= right) {
    //每次和中间的比，只要中间大于right，就代表小的数在后面，
    let mid = Math.floor((left + right) / 2);
    // let mid = left + ((right - left) >> 1) 这个也是查找中间值
    if (numbers[mid] > numbers[right]) {
      left = mid + 1;
    } else if (numbers[mid] < numbers[right]) {
      //中间的数小于或等于后面的数，不确定，例如：135，或22202
      right = mid;
    } else {
      right--;
    }
  }
  return numbers[left];
};
```

#### 14. 调整数组顺序使奇数位于偶数前面

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有奇数在数组的前半部分，所有偶数在数组的后半部分。

```js
// 双指针： 位运算的优先级最低，可以优化判断奇偶 (nums[i] & 1) == 1 ： 奇数的最低位是1， & 1之后会是1
var exchange = function (nums) {
  if (nums.length <= 1) {
    return nums;
  }
  let odd = 0;
  let even = nums.length - 1;
  while (odd < even) {
    if ((nums[odd] & 1) === 1) {
      odd++;
    } else if ((nums[even] & 1) === 0) {
      even--;
    } else {
      [nums[odd], nums[even]] = [nums[even], nums[odd]];
    }
  }
  return nums;
};
```

#### 15. 圆圈中最后剩下的数字

0,1,···,n-1 这 n 个数字排成一个圆圈，从数字 0 开始，每次从这个圆圈里删除第 m 个数字（删除后从下一个数字开始计数）。求出这个圆圈里剩下的最后一个数字。

例如，0、1、2、3、4 这 5 个数字组成一个圆圈，从数字 0 开始每次删除第 3 个数字，则删除的前 4 个数字依次是 2、0、4、1，因此最后剩下的数字是 3。

```js
/**
 * @param {number} n = 5 
 * @param {number} m = 3
 * @return {number}
0 1 2 3 4  2 
3 4 0 1    0
1 3 4      4
1 3 1      1 
3          3 
 */
// n个人时，第一个删掉的人是(m-1)%n, 对于n-1个人来说，m%n是n-1的开始，n-1个人时编号为i的人就是n个人时(m+i)%n
// 出现一个公式f(n)=(m+f(n-1))%n
//将人数从2反推到n：
var lastRemaining = function (n, m) {
  let pos = 0;
  for (let i = 2; i <= n; i++) {
    pos = (pos + m) % i;
  }
  return pos;
};

//递归  return n > 0 ? (lastRemaining(n - 1, m) + m) % n : 0
```

#### 16、二进制中 1 的个数

编写一个函数，输入是一个无符号整数（以二进制串的形式），返回其二进制表达式中数字位数为 '1' 的个数（也被称为 汉明重量).）。

```js
// 根据 n & n-1 ,可以消除最低位的1。比如3是0011，2是0010， 2&3 = 0010。

var hammingWeight = function (n) {
  if (n === 1) {
    return 1;
  }
  let count = 0;
  while (n) {
    count++;
    n = n & (n - 1);
  }
  return count;
};
```

#### 17、最小的 k 个数

输入整数数组 arr ，找出其中最小的 k 个数。例如，输入 4、5、1、6、2、7、3、8 这 8 个数字，则最小的 4 个数字是 1、2、3、4。

#### 18、和为 s 的连续正数序列

输入一个正整数 target ，输出所有和为 target 的连续正整数序列（至少含有两个数）。

序列内的数字由小到大排列，不同序列按照首个数字从小到大排列。

```js
/**
 * @param {number} target
 * @return {number[][]}
 */
var findContinuousSequence = function (target) {
  if (target <= 2) {
    //小于1,正数序列就没有，返回[]
    return [];
  }
  let left = 1;
  let right = 1; //左右指针都是在1开始
  let res = [];
  let sum = 1; //连续的和
  while (left <= target >>> 1) {
    //至少包含两个连续的数，所以当大于中间值时，两个数相加一定大于target
    sum = ((left + right) * (right - left + 1)) >> 1; //和是Sn=(a1+an)n / 2
    if (sum < target) {
      right++;
    } else if (sum > target) {
      left++;
    } else {
      let item = [];
      for (let i = left; i <= right; i++) {
        item.push(i);
      }
      res.push(item);
      left++;
    }
  }
  return res;
};
```

#### 19、求 1+2+……+n

求 `1+2+...+n` ，要求不能使用乘除法、for、while、if、else、switch、case 等关键字及条件判断语句（A?B:C）

```js
// 递归
var sumNums = function (n) {
  if (n <= 1) {
    return n;
  }
  return n + sumNums(n - 1);
};
// 数列求和   [n(a1+an)]/2
var sumNums = function (n) {
  if (n <= 1) {
    return n;
  }
  return (Math.pow(n, 2) + n) >> 1;
};
```

#### 20、0 ～ n-1 中缺失的数字

一个长度为 n-1 的递增排序数组中的所有数字都是唯一的，并且每个数字都在范围 0 ～ n-1 之内。在范围 0 ～ n-1 内的 n 个数字中有且只有一个数字不在该数组中，请找出这个数字。

```js
var missingNumber = function (nums) {
  let len = nums.length;
  if (!len) {
    return 0;
  }
  let low = 0;
  let high = len - 1;
  while (low <= high) {
    let mid = low + ((high - low) >> 1);
    if (nums[mid] === mid) {
      low = mid + 1;
    } else {
      high = mid - 1;
    }
  }
  return low;
};
```

#### 21. 矩阵中的路径

给一个二维网格和一个字符串，在网格中找到水平或垂直相邻单元格可以构成这个字符串的返回 true。

```js
// 使用DFS的方式，遍历数组
var exist = function (board, word) {
  let rows = board.length;
  let cols = board[0].length;
  if (rows === 0 || cols === 0 || word.length === 0) {
    return false;
  }
  if (rows * cols < word.length) {
    //如果二维数组的数量没有字符串的数量多，直接返回false
    return false;
  }
  for (let i = 0; i < rows; i++) {
    for (let j = 0; j < cols; j++) {
      if (compare(board, word, i, j, 0)) {
        //从第一个字符开始比较，如果有字符串的字符返回true
        return true;
      }
    }
  }
  return false; //最后没有返回false
};
var compare = (board, word, i, j, k) => {
  if (
    i < 0 ||
    j < 0 ||
    j >= board[0].length ||
    i >= board.length ||
    board[i][j] !== word[k]
  ) {
    // 下标超出范围，或者值不相等，直接返回false
    return false;
  }
  if (k === word.length - 1) {
    //如果已经走到了字符串末尾，就返回true
    return true;
  }
  board[i][j] = ""; // 将走过的字符清空，避免重复，因为深度遍历，所以如果返回false，再将每个字符重新填回去
  let res =
    compare(board, word, i + 1, j, k + 1) ||
    compare(board, word, i, j + 1, k + 1) ||
    compare(board, word, i - 1, j, k + 1) ||
    compare(board, word, i, j - 1, k + 1); // 遍历前后左右
  board[i][j] = word[k];
  return res; //返回res
};
```

#### 22. 不用加减乘除做加法

写一个函数，求两个整数之和，要求在函数体内不得使用 “+”、“-”、“\*”、“/” 四则运算符号。

![e29b6738c98c7ddca478baa3b36c0fbd6df61db8cb16c4c4ea9b7522f0a4a2c7-image.png](https://pic.leetcode-cn.com/1608042254-oSQqco-e29b6738c98c7ddca478baa3b36c0fbd6df61db8cb16c4c4ea9b7522f0a4a2c7-image.png)

```js
// 异或：同为0，异为1，异或表示不进位的。
// & :都为1才是1，&表示需要进位的。
// 递归
var add = function (a, b) {
  if (a === 0) {
    return b;
  }
  if (b === 0) {
    return a;
  }
  const c = (a & b) << 1;
  const d = a ^ b;
  return add(c, d);
};
// 循环
var add = function (a, b) {
  while (b) {
    let c = (a & b) << 1; // 进位
    a ^= b; // 非进位和
    b = c; // 进位
  }
  return a;
};
```

#### 23. 扑克牌中的顺子

从扑克牌中随机抽 5 张牌，判断是不是一个顺子，即这 5 张牌是不是连续的。2 ～ 10 为数字本身，A 为 1，J 为 11，Q 为 12，K 为 13，而大、小王为 0 ，可以看成任意数字。A 不能视为 14。

```js
//先从小到大排序，然后找到0的个数，判断是否重复，以及0的个数是否满足空缺位
var isStraight = function (nums) {
  let len = nums.length;
  if (len < 5) {
    return false;
  }
  nums = nums.sort((a, b) => a - b);
  let i = 0;
  let count = 0;
  for (; i < len; i++) {
    if (nums[i] === 0) {
      count++;
    } else {
      break;
    }
  }
  i++;
  for (; i < len; i++) {
    let temp = nums[i] - nums[i - 1];
    if (temp > 1) {
      if (count - temp + 1 >= 0) {
        count -= temp - 1;
      } else {
        return false;
      }
    } else if (temp !== 1) {
      return false;
    }
  }
  return true;
};

/* 
  分治思想 五张牌构成顺子的充分条件需要满足
  1. 不重复 使用Set去重
  2. max - min < 5 最大牌值 减去 最小牌值 小于5 且跳过大小王
*/
var isStraight = function (nums) {
  const set = new Set();
  let min = 14,
    max = 0; // min和max的初始值是两个边界值[0, 13]
  for (const num of nums) {
    // 遇到大小王 跳过
    if (!num) continue;
    // 遇到重复则直接 返回false
    if (set.has(num)) return false;
    set.add(num);
    // 迭代更新 min和max 以及set
    min = Math.min(min, num);
    max = Math.max(max, num);
  }

  return max - min < 5;
};
```

#### 24. 队列的最大值：使用 o(1)的时间复杂度获取队列中最大的值。

若队列为空，pop_front 和 max_value 需要返回 -1

```js
// 如果只用一个队列，那么只能和最大值或者最小值对比，每一次比对插入都需要移动数组中的内容，时间复杂度是O(n)。
//使用单调队列：使用一个队列保存进出的顺序，另一个队列去保存最大值，要求最大值的队列是单调递减的,每次去除数组中前面比这个进来的数 A 小的数，因为在这个数 A 出去之前，前面最大的数就是这个进来的数 A。

var MaxQueue = function () {
  this.queue = [];
  this.maxQueue = [];
};

/**
 * @return {number}
 */
MaxQueue.prototype.max_value = function () {
  return this.maxQueue.length === 0 ? -1 : this.maxQueue[0];
};
/**
 * @param {number} value
 * @return {void}
 */
MaxQueue.prototype.push_back = function (value) {
  this.queue.push(value);
  while (
    this.maxQueue.length !== 0 &&
    value > this.maxQueue[this.maxQueue.length - 1]
  ) {
    this.maxQueue.pop();
  }
  this.maxQueue.push(value);
};

/**
 * @return {number}
 */
MaxQueue.prototype.pop_front = function () {
  if (this.queue.length === 0) return -1;
  let value = this.queue.shift();
  if (value === this.maxQueue[0]) this.maxQueue.shift();
  return value;
};
```

#### 25. 滑动窗口的最大值

给定一个数组 `nums` 和滑动窗口的大小 `k`，请找出所有滑动窗口里的最大值。

```
输入: nums = [1,3,-1,-3,5,3,6,7], 和 k = 3
输出: [3,3,5,5,6,7]
解释:
  滑动窗口的位置                最大值
---------------               -----
[1  3  -1] -3  5  3  6  7       3
 1 [3  -1  -3] 5  3  6  7       3
 1  3 [-1  -3  5] 3  6  7       5
 1  3  -1 [-3  5  3] 6  7       5
 1  3  -1  -3 [5  3  6] 7       6
 1  3  -1  -3  5 [3  6  7]      7
```

```js
var maxSlidingWindow = function (nums, k) {
  if (nums.length < k || k < 1) {
    return [];
  }
  if (k === 1) {
    return nums;
  }
  let queue = []; //单调队列,单调递减队列.如果把最大值放在单调队列中，这样无法区分最大值的位置.
  let res = []; //存储结果
  let i = 0;
  for (; i < k; i++) {
    //找到第一个值应该放什么
    while (queue.length && nums[i] > nums[queue[queue.length - 1]]) {
      queue.pop();
    }
    queue.push(i);
  }
  res[0] = nums[queue[0]];
  for (; i < nums.length; i++) {
    if (i - queue[0] >= k) {
      //超过范围的，就去掉这个值
      queue.shift();
    }
    while (queue.length && nums[i] > nums[queue[queue.length - 1]]) {
      queue.pop();
    }
    queue.push(i);
    res.push(nums[queue[0]]);
  }
  return res;
};
```

#### 26. 数组中数字出现的次数

一个整型数组 `nums` 里除两个数字之外，其他数字都出现了两次。请写程序找出这两个只出现一次的数字。要求时间复杂度是 O(n)，空间复杂度是 O(1)。

```js
var singleNumbers = function (nums) {
  if (nums.length < 2) {
    return [];
  }
  // 先异或一次，最后的值是两个不同的数字的异或值
  let num1 = 0;
  for (let num of nums) {
    num1 = num1 ^ num;
  }
  // 找到某一位上是1的位置
  let count = 1;
  while ((count & num1) === 0) {
    count = count << 1;
  }
  let num2 = 0;
  let num3 = 0;
  // 再异或一次，异或这位上是0在一组中，是1在一组中，将这两个不同的值分在两组中，相同的值必然在一组
  for (let num of nums) {
    if ((count & num) === 0) {
      //count & num的值不是0或1，不一定为1
      num2 = num2 ^ num;
    } else {
      num3 = num3 ^ num;
    }
  }
  return [num2, num3];
};
```

#### 27. 数组中出现的次数 2

在一个数组 `nums` 中除一个数字只出现一次之外，其他数字都出现了三次。请找出那个只出现一次的数字。

```js
// 1. 使用对象存储
var singleNumber = function (nums) {
  const hash = {};
  for (const num of nums) hash[num] ? hash[num]++ : (hash[num] = 1);
  for (const k in hash) if (hash[k] === 1) return k;
};
// 位运算：计算每一位上的数是否是1，因为只有一个数是一次，其他都是3次，对3取余，如果整除说明不是1，否则是1
var singleNumber = function (nums) {
  let res = 0;
  for (let bit = 0; bit < 32; ++bit) {
    let mask = 1 << bit;
    let count = 0; //统计该位1的出现次数情况
    for (let num of nums) {
      if (num & mask) ++count; // 该位是1, num & mask 不一定是1
    }
    if (count % 3) {
      res = res | mask;
    }
  }
  return res;
};
// 位运算：^是相同为0，不同为1，ones保存的是每次第一次出现的数的和，twos是第二次出现的数的和，第三次出现的数会让ones和twos中都减去这个数，让这个数为0
var singleNumber = function (nums) {
  let ones = 0,
    twos = 0;
  for (const num of nums) {
    ones = ones ^ (num & ~twos);
    twos = twos ^ (num & ~ones);
  }
  return ones;
};
// [3,4,3,3]
// 3 ones====
// 0 twos====
// 7 ones====
// 0 twos====
// 4 ones====
// 3 twos====
// 4 ones====
// 0 twos====
```

#### 28. 在排序数组中查找数字

统计一个数字在排序数组中出现的次数。

```js
// 双指针
var search = function (nums, target) {
  // 2. 双端指针队列 向中心收敛 利用有序这一条件 Time: O(n) Space: O(1)
  let l = 0,
    r = nums.length - 1;
  while (nums[l] < target) l++;
  while (nums[r] > target) r--;
  return r - l >= 0 ? r - l + 1 : 0;
};
// 二分递归
var search = function (nums, target) {
  let len = nums.length;
  if (len === 0 || target < nums[0] || target > nums[len]) {
    return 0;
  }
  let middle = Math.floor((len - 1) / 2);
  let count = 0;
  if (nums[middle] > target) {
    count = search(nums.slice(0, middle), target);
  } else if (nums[middle] < target) {
    count = search(nums.slice(middle + 1), target);
  } else {
    count =
      search(nums.slice(0, middle), target) +
      search(nums.slice(middle + 1), target) +
      1;
  }
  return count;
};
```

#### 30. 机器人的运动范围

地上有一个 m 行 n 列的方格，从坐标 [0,0] 到坐标 [m-1,n-1] 。一个机器人从坐标 [0, 0] 的格子开始移动，它每次可以向左、右、上、下移动一格（不能移动到方格外），也不能进入行坐标和列坐标的数位之和大于 k 的格子。例如，当 k 为 18 时，机器人能够进入方格 [35, 37] ，因为 3+5+3+7=18。但它不能进入方格 [35, 38]，因为 3+5+3+8=19。请问该机器人能够到达多少个格子？

```js
// 应该使用深度优先或广度优先，如果使用双重循环，有可能导致中间断开，没有连续
var movingCount = function (m, n, k) {
  if (k < 0) {
    return -1;
  }
  // visited 用来记录走过的格子，避免重复
  const visited = Array(m)
    .fill(0)
    .map((_) => Array(n).fill(false));
  //计算位数和
  function sum(n) {
    let res = 0;
    while (n) {
      res += n % 10;
      n = Math.floor(n / 10);
    }
    return res;
  }
  function dfs(x, y) {
    // 对应三个终止条件，超过k值、到达边界、走过的格子
    if (
      x >= m ||
      y >= n ||
      x < 0 ||
      y < 0 ||
      sum(x) + sun(y) > k ||
      visited[x][y]
    ) {
      return 0;
    }
    // 记录当前格子已经走过，返回当前计数 1 + 后续其他两个方向的总和
    visited[x][y] = true;
    return 1 + dfs(x + 1, y) + dfs(x, y + 1);
  }
  return dfs(0, 0);
};
```

#### 31. 把数组排成最小的数

输入一个非负整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。

```js
// sort()排序
var minNumber = function (nums) {
  return nums
    .sort((a, b) => {
      return "" + a + b - ("" + b + a);
    })
    .join("");
};
//主要考察快排， i， j， 一次能确定一个值放在哪个位置，
var minNumber = function (nums) {
  let len = nums.length;
  if (len < 1) {
    return "";
  }
  let pivot = nums[0];
  let left = nums.filter((item) => `${item}${pivot}` < `${pivot}${item}`);
  let eq = nums.filter((item) => `${item}${pivot}` === `${pivot}${item}`);
  let right = nums.filter((item) => `${item}${pivot}` > `${pivot}${item}`);
  let array = [...minNumber(left), ...eq, ...minNumber(right)];
  return array.join("");
};
```

### 数组

#### 1、和为 s 的两个数字

输入一个递增排序的数组和一个数字 s，在数组中查找两个数，使得它们的和正好是 s。如果有多对数字的和等于 s，则输出任意一对即可。

```js
/**
 * @param {number[]} nums
 * @param {number} target
 * @return {number[]}
 */
var twoSum = function (nums, target) {
  let len = nums.length;
  if (len < 2 || target < nums[0]) {
    return [];
  }
  let left = 0;
  let right = len - 1;
  while (left < right) {
    if (nums[left] + nums[right] === target) {
      return [nums[left], nums[right]];
    } else if (nums[left] + nums[right] < target) {
      left++;
    } else {
      right--;
    }
  }
  return [];
};
```

#### 2、三数之和

给你一个包含 n 个整数的数组  nums，判断  nums  中是否存在三个元素 a，b，c ，使得  a + b + c = 0 ？请你找出所有和为 0 且不重复的三元组。

注意：答案中不可以包含重复的三元组。

```js
var threeSum = function (nums) {
  nums.sort((a, b) => a - b); //排序，排序之后循环找
  const len = nums.length;
  if (len < 3 || nums[0] > 0) {
    return [];
  }
  let res = [];
  for (let i = 0; i < len - 2; i++) {
    // 只对比当前数字后面的数，所以排序数组如果大于0，后面的和一定大于0
    if (nums[i] > 0) {
      break;
    }
    // 如果当前的值和前面的一样，会出现重复的数，所以忽略掉这个值
    if (i > 0 && nums[i] === nums[i - 1]) {
      continue;
    }
    let left = i + 1;
    let right = len - 1;
    while (left < right) {
      const sum = nums[i] + nums[left] + nums[right];
      if (sum === 0) {
        res.push([nums[i], nums[left], nums[right]]);
        // 下面两个while循环是去重的，[-2,0,0,2,2]
        while (left < right && nums[left] === nums[left + 1]) {
          left++;
        }
        while (left < right && nums[right] === nums[right - 1]) {
          right--;
        }
        left++;
        right--;
      } else if (sum < 0) {
        left++;
      } else if (sum > 0) {
        right--;
      }
    }
  }
  return res;
};
```

### 栈

#### 1、有效的括号

给定一个只包括 '('，')'，'{'，'}'，'['，']'  的字符串 s ，判断字符串是否有效。

```js
var isValid = function (s) {
  if (s.length < 1) {
    return true;
  }
  let map = {
    ")": "(",
    "}": "{",
    "]": "[",
  };
  let stack = []; //使用栈，将左边括号进栈
  for (let i = 0; i < s.length; i++) {
    if (s[i] === "(" || s[i] === "{" || s[i] === "[") {
      stack.push(s[i]);
    } else {
      if (stack.length !== 0 && stack[stack.length - 1] === map[s[i]]) {
        stack.pop();
      } else {
        return false;
      }
    }
  }
  return stack.length === 0;
};
```

#### 1-1、有效的括号字符串

给定一个只包含三种字符的字符串：（ ，）  和 \*，写一个函数来检验这个字符串是否为有效字符串。有效字符串具有如下规则：

任何左括号 (  必须有相应的右括号 )。
任何右括号 )  必须有相应的左括号 ( 。
左括号 ( 必须在对应的右括号之前 )。
\*  可以被视为单个右括号 ) ，或单个左括号 ( ，或一个空字符串。
一个空字符串也被视为有效字符串。

```js
// 两个栈，一个存放左括号的下标，一个存放*号的下标
var checkValidString = function (s) {
  let len = s.length;
  if (len === 0) {
    return true;
  }
  let stack = [];
  let stack1 = [];
  for (let i = 0; i < len; i++) {
    if (s[i] === "(") {
      stack.push(i);
    }
    if (s[i] === "*") {
      stack1.push(i);
    }
    if (s[i] === ")") {
      if (stack.length !== 0) {
        stack.pop();
      } else if (stack1.length !== 0) {
        stack1.pop();
      } else {
        return false;
      }
    }
  }
  while (stack.length && stack1.length) {
    let leftValue = stack.pop();
    let starValue = stack1.pop();
    if (leftValue > starValue) {
      return false;
    }
  }
  return stack.length === 0;
};
```

#### 2、基本计算器

给你一个字符串表达式 s ，请你实现一个基本计算器来计算并返回它的值。

```js

```

#### 3、包含 min 函数的栈

定义栈的数据结构，请在该类型中实现一个能够得到栈的最小元素的 min 函数在该栈中，调用 min、push 及 pop 的时间复杂度都是 O(1)。

```js
/**
 * initialize your data structure here.
 */
var MinStack = function () {
  this.stack = []; //构造函数中使用this
  this.minStack = [];
};

/**
 * @param {number} x
 * @return {void}
 */
MinStack.prototype.push = function (x) {
  if (
    this.minStack.length === 0 ||
    this.minStack[this.minStack.length - 1] >= x
  ) {
    this.minStack.push(x);
  }
  this.stack.push(x);
};

/**
 * @return {void}
 */
MinStack.prototype.pop = function () {
  if (this.stack.length !== 0) {
    let pop = this.stack.pop();
    if (pop === this.minStack[this.minStack.length - 1]) {
      this.minStack.pop();
    }
  }
};

/**
 * @return {number}
 */
MinStack.prototype.top = function () {
  return this.stack[this.stack.length - 1];
};

/**
 * @return {number}
 */
MinStack.prototype.min = function () {
  if (this.minStack.length) {
    return this.minStack[this.minStack.length - 1];
  }
  return -1;
};
```

#### 4、栈的压入、弹出序列

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如，序列 {1,2,3,4,5} 是某栈的压栈序列，序列 {4,5,3,2,1} 是该压栈序列对应的一个弹出序列，但 {4,3,5,1,2} 就不可能是该压栈序列的弹出序列。

```js
/**
 * @param {number[]} pushed
 * @param {number[]} popped
 * @return {boolean}
 */
var validateStackSequences = function (pushed, popped) {
  let pushLen = pushed.length;
  let popLen = popped.length;
  if (pushLen !== popLen) {
    return false;
  }
  if (pushLen === 0 && popLen === 0) {
    return true;
  }
  let stack = [];
  let j = 0;
  for (let i = 0; i < pushLen; i++) {
    stack.push(pushed[i]);
    while (stack.length !== 0 && stack[stack.length - 1] === popped[j]) {
      j++;
      stack.pop();
    }
  }
  return stack.length === 0 && j === popLen;
};

//栈和双指针
var validateStackSequences = function (pushed, popped) {
  let stack = [];
  let i = 0;
  let j = 0;
  while (i < pushed.length) {
    if (stack.length && popped[j] == stack[stack.length - 1]) {
      stack.pop();
      j++;
    } else {
      stack.push(pushed[i++]);
    }
  }
  while (j < popped.length) {
    if (popped[j++] == stack[stack.length - 1]) {
      stack.pop();
    } else {
      return false;
    }
  }
  return true;
};
```

#### 5、每日温度

给定一个整数数组  temperatures ，表示每天的温度，返回一个数组  answer ，其中  answer[i]  是指在第 i 天之后，才会有更高的温度。如果气温在这之后都不会升高，请在该位置用  0 来代替。

```js
// 单调栈：通常是一维数组，向一个方向(左或者右)比较比自己大（或者小）的元素。

//   栈内元素从下往上，依次减小，新元素快速从小到大比较，剔除小项的位置，让新的元素进行补充
//     时间复杂度要求在 O(n)

var dailyTemperatures = function (temperatures) {
  if (temperatures < 1) {
    return [];
  }
  let len = temperatures.length;
  let stack = [];
  let res = new Array(len).fill(0);
  for (let i = 0; i < len; i++) {
    while (
      stack.length &&
      temperatures[i] > temperatures[stack[stack.length - 1]]
    ) {
      res[stack[stack.length - 1]] = i - stack[stack.length - 1];
      stack.pop();
    }
    stack.push(i);
  }
  return res;
};
```

### 队列

#### 1、用栈实现队列

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead  操作返回 -1 )

```js
//js数组单独可以当队列使用，如果一定要使用栈改成队列的话，使用两个栈，一个 保存数据，把栈出来进另一个栈，就是出来的顺序。

// 不是每次都在一个栈进另一个栈，只有当B栈没有数据的时候，再在A栈进B栈，如果进完还是没有数据，返回-1。

var MyQueue = function () {
  this.pushed = [];
  this.popped = [];
};

/**
 * @param {number} x
 * @return {void}
 */
MyQueue.prototype.push = function (x) {
  this.pushed.push(x);
};

/**
 * @return {number}
 * 只有每次出栈的栈中没有数据，才会把入栈的数据放到出栈的栈中
 */
MyQueue.prototype.pop = function () {
  if (this.popped.length !== 0) {
    return this.popped.pop();
  } else {
    while (this.pushed.length !== 0) {
      this.popped.push(this.pushed.pop());
    }
    return this.popped.length !== 0 ? this.popped.pop() : -1;
  }
};

/**
 * @return {number}
 */
MyQueue.prototype.peek = function () {
  if (this.popped.length !== 0) {
    return this.popped[this.popped.length - 1];
  } else {
    if (this.pushed.length !== 0) {
      return this.pushed[0];
    } else {
      return -1;
    }
  }
};

/**
 * @return {boolean}
 */
MyQueue.prototype.empty = function () {
  if (this.pushed.length === 0 && this.popped.length === 0) {
    return true;
  }
  return false;
};
```

### 排序

#### 1、合并两个有序数组

给你两个按 非递减顺序 排列的整数数组  nums1 和 nums2，另有两个整数 m 和 n ，分别表示 nums1 和 nums2 中的元素数目。

请你 合并 nums2 到 nums1 中，使合并后的数组同样按 非递减顺序 排列。

注意：最终，合并后数组不应由函数返回，而是存储在数组 nums1 中。为了应对这种情况，nums1 的初始长度为 m + n，其中前 m 个元素表示应合并的元素，后 n 个元素为 0 ，应忽略。nums2 的长度为 n 。

```js
var merge = function (nums1, m, nums2, n) {
  let len1 = nums1.length;
  if (len1 !== m + n || m < 0 || n < 0) {
    return;
  }
  if (n === 0) {
    return nums1;
  }
  let p1 = m - 1,
    p2 = n - 1;
  let tail = m + n - 1;
  let cur;
  while (p1 >= 0 || p2 >= 0) {
    if (p1 === -1) {
      cur = nums2[p2--];
    } else if (p2 === -1) {
      cur = nums1[p1--];
    } else if (nums1[p1] > nums2[p2]) {
      cur = nums1[p1--];
    } else {
      cur = nums2[p2--];
    }
    nums1[tail--] = cur;
  }
};
```

#### 2、颜色分类

给定一个包含红色、白色和蓝色、共  n 个元素的数组  nums ，原地对它们进行排序，使得相同颜色的元素相邻，并按照红色、白色、蓝色顺序排列。

我们使用整数 0、 1 和 2 分别表示红色、白色和蓝色。

必须在不使用库的 sort 函数的情况下解决这个问题。

```js
var sortColors = function (nums) {
  let len = nums.length;
  if (len <= 1) {
    return;
  }
  let p = -1; // 前面放0
  let q = nums.length; //后面放2
  let i = 0;
  while (i < q) {
    if (nums[i] == 0) {
      [nums[p + 1], nums[i]] = [nums[i], nums[p + 1]];
      p = p + 1;
      i++;
    } else if (nums[i] == 2) {
      [nums[i], nums[q - 1]] = [nums[q - 1], nums[i]];
      q = q - 1;
    } else if (nums[i] == 1) {
      i++;
    }
  }
};
```

#### 3、两数之和

给定一个整数数组 nums  和一个整数目标值 target，请你在该数组中找出 和为目标值 target  的那   两个   整数，并返回它们的数组下标。

你可以假设每种输入只会对应一个答案。但是，数组中同一个元素在答案里不能重复出现。

你可以按任意顺序返回答案。

```js
// 不一定有顺序, 使用map去保存
var twoSum = function (nums, target) {
  let map = new Map();
  for (let i = 0; i < nums.length; i++) {
    if (map.has(target - nums[i])) {
      return [map.get(target - nums[i]), i];
    } else {
      map.set(nums[i], i);
    }
  }
  return [];
};
```

#### 4、合并区间

以数组 intervals 表示若干个区间的集合，其中单个区间为 intervals[i] = [starti, endi] 。
请你合并所有重叠的区间，并返回一个不重叠的区间数组，该数组需恰好覆盖输入中的所有区间。

```js
var merge = function (intervals) {
  const len = intervals.length;
  if (len <= 1) {
    return intervals;
  }
  const res = [];
  intervals.sort((a, b) => a[0] - b[0]); //先排序，这样按数组左边界排序
  let prev = intervals[0];
  for (let i = 1; i < len; i++) {
    let cur = intervals[i];
    if (prev[1] >= cur[0]) {
      //前一个的右边界大于等于下一个的左边界
      prev[1] = Math.max(prev[1], cur[1]);
    } else {
      //两个数组没有重叠，就推到结果数组中
      res.push(prev);
      prev = cur;
    }
  }
  res.push(prev); //最后要再加一次，因为最后一个肯定没有放进去
  return res;
};
```

#### 5、数组中的逆序对

在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组，求出这个数组中的逆序对的总数。

```js
// 归并，俩俩比较，然后44比较
var reversePairs = function (nums) {
  // 归并排序
  let sum = 0;
  mergeSort(nums);
  return sum;

  function mergeSort(nums) {
    if (nums.length < 2) return nums;
    let mid = (0 + nums.length - 1) >> 1;
    let left = nums.slice(0, mid + 1); // 递归左边数组
    let right = nums.slice(mid + 1); // 递归右边数组
    return merge(mergeSort(left), mergeSort(right));
  }

  function merge(left, right) {
    //合并左右数组
    let res = [];
    let leftLen = left.length;
    let rightLen = right.length;
    let len = leftLen + rightLen;
    for (let index = 0, i = 0, j = 0; index < len; index++) {
      if (i >= leftLen) res[index] = right[j++];
      else if (j >= rightLen) res[index] = left[i++];
      else if (left[i] <= right[j]) res[index] = left[i++];
      else {
        res[index] = right[j++]; //左边的值大于右边的，左边值后面的数都大于右边的数
        sum += leftLen - i; //在归并排序中唯一加的一行代码
      }
    }
    return res;
  }
};
```

#### 6、数据流中的中位数

如何得到一个数据流中的中位数？如果从数据流中读出奇数个数值，那么中位数就是所有数值排序之后位于中间的数值。如果从数据流中读出偶数个数值，那么中位数就是所有数值排序之后中间两个数的平均值。

例如，

[2,3,4]  的中位数是 3

[2,3] 的中位数是 (2 + 3) / 2 = 2.5

设计一个支持以下两种操作的数据结构：

void addNum(int num) - 从数据流中添加一个整数到数据结构中。
double findMedian() - 返回目前所有元素的中位数。

```js
/**
 * initialize your data structure here.
 */
var MedianFinder = function () {
  // 维护2个优先队列
  // 小数放左边最大优先队列
  this.maxPriorityQueue = new MaxPriorityQueue({
    compare: (e1, e2) => e2 - e1,
  });
  // 大数放右边最小优先队列
  this.minPriorityQueue = new MinPriorityQueue({
    compare: (e1, e2) => e1 - e2,
  });
};

/**
 * @param {number} num
 * @return {void}
 */
MedianFinder.prototype.addNum = function (num) {
  // 如果最大优先队列为空，直接进队
  if (this.maxPriorityQueue.isEmpty()) {
    this.maxPriorityQueue.enqueue(num);
  } else {
    const value1 = this.maxPriorityQueue.front();
    const value2 = this.minPriorityQueue.front();
    if (num <= value1 || (num > value1 && num < value2)) {
      this.maxPriorityQueue.enqueue(num);
      // 调整队列
      if (this.maxPriorityQueue.size() - this.minPriorityQueue.size() > 1) {
        this.minPriorityQueue.enqueue(this.maxPriorityQueue.dequeue());
      }
    } else {
      this.minPriorityQueue.enqueue(num);
      if (this.minPriorityQueue.size() - this.maxPriorityQueue.size() > 1) {
        this.maxPriorityQueue.enqueue(this.minPriorityQueue.dequeue());
      }
    }
  }
};

/**
 * @return {number}
 */
MedianFinder.prototype.findMedian = function () {
  // 奇数
  if (this.maxPriorityQueue.size() > this.minPriorityQueue.size()) {
    return this.maxPriorityQueue.front();
  } else if (this.maxPriorityQueue.size() < this.minPriorityQueue.size()) {
    return this.minPriorityQueue.front();
  }
  // 偶数
  const value1 = this.maxPriorityQueue.front();
  const value2 = this.minPriorityQueue.front();
  return (value1 + value2) / 2;
};

// 第二种: 排序
/**
 * initialize your data structure here.
 */
var MedianFinder = function () {
  this.queue = [];
};

/**
 * @param {number} num
 * @return {void}
 */
const getIndex = (arr, left, right, num) => {
  for (let i = 0; i < arr.length - 1; i++) {
    if (num > arr[i] && num <= arr[i + 1]) {
      return i + 1;
    }
  }
  return -1;
};

MedianFinder.prototype.addNum = function (num) {
  if (this.queue.length === 0) {
    this.queue.push(num);
  } else {
    if (num <= this.queue[0]) {
      this.queue.unshift(num);
    } else if (num >= this.queue[this.queue.length - 1]) {
      this.queue.push(num);
    } else {
      let index = getIndex(this.queue, 0, this.queue.length - 1, num);
      for (let i = this.queue.length - 1; i >= index; i--) {
        this.queue[i + 1] = this.queue[i];
      }
      this.queue[index] = num;
    }
  }
};

/**
 * @return {number}
 */
MedianFinder.prototype.findMedian = function () {
  let mid = Math.floor((this.queue.length - 1) / 2);
  if (this.queue.length % 2 === 0) {
    return (this.queue[mid] + this.queue[mid + 1]) / 2;
  } else {
    return this.queue[mid];
  }
};

/**
 * Your MedianFinder object will be instantiated and called as such:
 * var obj = new MedianFinder()
 * obj.addNum(num)
 * var param_2 = obj.findMedian()
 */
```

#### 计算右侧小于当前元素的个数

给你一个整数数组 nums ，按要求返回一个新数组  counts 。数组 counts 有该性质： counts[i] 的值是   nums[i] 右侧小于  nums[i] 的元素的数量。

### 二分查找

#### 3、在排序数组中查找元素的第一个和最后一个位置

给定一个按照升序排列的整数数组 nums，和一个目标值 target。找出给定目标值在数组中的开始位置和结束位置。

如果数组中不存在目标值 target，返回  [-1, -1]。

```js
// 双指针的方式，走过 n/2, 时间复杂度还是O(n)
var searchRange = function (nums, target) {
  let len = nums.length;
  if (len < 1 || target < nums[0] || nums[len - 1] < target) {
    return [-1, -1];
  }
  let left = 0;
  let right = len - 1;
  while (left <= right) {
    if (nums[left] === target && nums[right] === target) {
      return [left, right];
    }
    if (nums[left] > target || nums[right] < target) {
      return [-1, -1];
    }
    if (nums[left] < target) {
      left++;
    }
    if (nums[right] > target) {
      right--;
    }
  }
  return [-1, -1];
};

// 二分查找
var searchRange = function (nums, target) {
  let l = 0,
    r = nums.length - 1,
    lr,
    rl;
  // 先找到目标值有没有
  while (l <= r) {
    let mid = l + ((r - l) >> 1);
    if (target === nums[mid]) {
      lr = rl = mid; // 先找到目标值索引
      break;
    }
    if (target > nums[mid]) l = mid + 1;
    else r = mid - 1;
  }
  // 如果没有直接返回-1
  if (lr === undefined) return [-1, -1];

  (l = 0), (r = nums.length - 1);
  // 向前搜索，往下取整,第一个值
  while (l < lr) {
    let mid = l + ((lr - l) >> 1);
    if (nums[mid] === target) lr = mid;
    else l = mid + 1;
  }
  // // 向后搜索，往上取整
  while (rl < r) {
    let mid = rl + Math.ceil((r - rl) / 2);
    if (nums[mid] === target) rl = mid;
    else r = mid - 1;
  }
  return [lr, rl];
};
```

#### 4、搜索旋转排序数组

整数数组 nums 按升序排列，数组中的值 互不相同 。

在传递给函数之前，nums 在预先未知的某个下标 k（0 <= k < nums.length）上进行了 旋转，使数组变为 [nums[k], nums[k+1], ..., nums[n-1], nums[0], nums[1], ..., nums[k-1]]（下标 从 0 开始 计数）。例如， [0,1,2,4,5,6,7] 在下标 3 处经旋转后可能变为  [4,5,6,7,0,1,2] 。

给你 旋转后 的数组 nums 和一个整数 target ，如果 nums 中存在这个目标值 target ，则返回它的下标，否则返回  -1 。

```js
// 第一种方式递归
var search = function (nums, target) {
  const len = nums.length;
  if (len < 1) {
    return -1;
  }
  if (len === 1) {
    return nums[0] === target ? 0 : -1;
  }
  const find = (left, right) => {
    //先找中间的值，相等就返回，不相等就递归找前后的内容
    if (left >= right) {
      return nums[left] === target ? left : -1;
    }
    let mid = left + ((right - left) >> 1);
    if (nums[mid] === target) {
      return mid;
    }
    let leftIndex = find(left, mid - 1);
    if (leftIndex !== -1) {
      return leftIndex;
    }
    return find(mid + 1, right);
  };
  return find(0, len - 1);
};

// 第二种方式
var search = function (nums, target) {
  let len = nums.length;
  if (len < 1) {
    return -1;
  }
  if (len === 1) {
    return nums[0] === target ? 0 : -1;
  }
  let start = 0;
  let end = nums.length - 1;

  while (start <= end) {
    const mid = start + ((end - start) >> 1);
    if (nums[mid] === target) return mid;
    // [start, mid]有序
    if (nums[mid] >= nums[start]) {
      //target 在 [start, mid] 之间
      if (target >= nums[start] && target <= nums[mid]) {
        end = mid - 1;
      } else {
        start = mid + 1;
      }
    } else {
      // [mid, end]有序
      if (target >= nums[mid] && target <= nums[end]) {
        start = mid + 1;
      } else {
        end = mid - 1;
      }
    }
  }
  return -1;
};
```

### 位运算

#### 1. 整数反转

给你一个 32 位的有符号整数 x ，返回将 x 中的数字部分反转后的结果。

如果反转后整数超过 32 位的有符号整数的范围  [−231,  231 − 1] ，就返回 0。

假设环境不允许存储 64 位整数（有符号或无符号）。

```js
// 固定API完成
var reverse = function (x) {
  let flag = x >= 0 ? true : false; //记录是正数还是负数
  let s = Math.abs(x).toString(); //只讲数字转为字符串
  let reverseS = s.split("").reverse().join(""); //反转
  let MAX_VALUE = Math.pow(2, 31) - 1;
  let MIN_VALUE = Math.pow(-2, 31);
  let num = flag ? +reverseS : -reverseS;
  return num > MAX_VALUE || num < MIN_VALUE ? 0 : num;
};

// 每次 / % 比较
var reverse = function (x) {
  let res = 0;
  let MAX_VALUE = Math.pow(2, 31) - 1;
  let MIN_VALUE = Math.pow(-2, 31);
  while (x) {
    res = res * 10 + (x % 10);
    if (res > MAX_VALUE || res < MIN_VALUE) {
      return 0;
    }
    x = ~~(x / 10); // ~x = -(x+1), ~~x = -(-(x+1)+1) = x
  }
  return res;
};
```

### 双指针

#### 1、盛最多水的容器 https://leetcode.cn/problems/container-with-most-water/

给定一个长度为 n 的整数数组  height 。有  n  条垂线，第 i 条线的两个端点是  (i, 0)  和  (i, height[i]) 。

找出其中的两条线，使得它们与  x  轴共同构成的容器可以容纳最多的水。

返回容器可以储存的最大水量。

说明：你不能倾斜容器。

```js
var maxArea = function (height) {
  let len = height.length;
  // 确定边界
  if (len <= 1) {
    return 0;
  }
  let max = 0; //保存最大的区域值
  // 使用双指针的形式，两边开始向中间走
  for (let i = 0, j = len - 1; i < j; ) {
    //i，j较小的那个先向内移动 如果高的指针先移动，那肯定不如当前的面积大
    const minHeight = height[i] < height[j] ? height[i++] : height[j--];
    const area = (j - i + 1) * minHeight;
    max = Math.max(area, max);
  }
  return max;
};
```

### 数学运算

#### 1、整数转罗马数字

罗马数字包含以下七种字符： I， V， X， L，C，D  和  M。

```js
字符          数值
I             1
V             5
X             10
L             50
C             100
D             500
M             1000
```

例如， 罗马数字 2 写做  II ，即为两个并列的 1。12 写做  XII ，即为  X + II 。 27 写做   XXVII, 即为  XX + V + II 。

通常情况下，罗马数字中小的数字在大的数字的右边。但也存在特例，例如 4 不写做  IIII，而是  IV。数字 1 在数字 5 的左边，所表示的数等于大数 5 减小数 1 得到的数值 4 。同样地，数字 9 表示为  IX。这个特殊的规则只适用于以下六种情况：

I  可以放在  V (5) 和  X (10) 的左边，来表示 4 和 9。
X  可以放在  L (50) 和  C (100) 的左边，来表示 40 和  90。 
C  可以放在  D (500) 和  M (1000) 的左边，来表示  400 和  900。
给你一个整数，将其转为罗马数字。

```js
var intToRoman = function (num) {
  if (num < 1) {
    return "";
  }
  // 先记录所有的可能性
  const valueSymbols = [
    [1000, "M"],
    [900, "CM"],
    [500, "D"],
    [400, "CD"],
    [100, "C"],
    [90, "XC"],
    [50, "L"],
    [40, "XL"],
    [10, "X"],
    [9, "IX"],
    [5, "V"],
    [4, "IV"],
    [1, "I"],
  ];
  const res = [];
  for (const [key, value] of valueSymbols) {
    while (num >= key) {
      num -= key;
      res.push(value);
    }
    if (num === 0) {
      break;
    }
  }
  return res.join("");
};
```

#### 2、罗马数字转整数

```js
var romanToInt = function (s) {
  const len = s.length;
  if (len < 1) {
    return 0;
  }
  // 保留所有的字母的值
  const map = {
    M: 1000,
    D: 500,
    C: 100,
    L: 50,
    X: 10,
    V: 5,
    I: 1,
  };
  let res = 0;
  for (let i = 0; i < len; i++) {
    const value = map[s[i]];
    // 如果后面的数比前一个大，说明要么是4，要么是9，需要后面的那个减去前面的这个值「先减后加是一样的」
    if (i < len - 1 && value < map[s[i + 1]]) {
      res -= value;
    } else {
      res += value;
    }
  }
  return res;
};
```

### 数组中的算法

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
    // [nums[i], nums[nums[i]]]=[nums[nums[i]], nums[i]]: 这样写会超时
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

#### 3. 用两个栈实现队列

用两个栈实现一个队列。队列的声明如下，请实现它的两个函数 appendTail 和 deleteHead ，分别完成在队列尾部插入整数和在队列头部删除整数的功能。(若队列中没有元素，deleteHead  操作返回 -1 )

```js
//js数组单独可以当队列使用，如果一定要使用栈改成队列的话，使用两个栈，一个 保存数据，把栈出来进另一个栈，就是出来的顺序。

// 不是每次都在一个栈进另一个栈，只有当B栈没有数据的时候，再在A栈进B栈，如果进完还是没有数据，返回-1。
var CQueue = function () {
  this.stack = [];
  this.stack1 = [];
};

/**
 * @param {number} value
 * @return {void}
 */
CQueue.prototype.appendTail = function (value) {
  this.stack.push(value);
};

/**
 * @return {number}
 * 只有每次出栈的栈中没有数据，才会把入栈的数据放到出栈的栈中
 */
CQueue.prototype.deleteHead = function () {
  if (this.stack.length === 0 && this.stack1.length === 0) {
    return -1;
  }
  if (this.stack1.length !== 0) {
    return this.stack1.pop();
  }
  while (this.stack.length) {
    this.stack1.push(this.stack.pop());
  }
  return this.stack1.pop();
};

/**
 * Your CQueue object will be instantiated and called as such:
 * var obj = new CQueue()
 * obj.appendTail(value)
 * var param_2 = obj.deleteHead()
 */
```

#### 4. 栈的压入、弹出序列

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

#### 5. 包含 min 函数的栈

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
  let poppedValue = this.stack.pop();
  if (poppedValue === this.minStack[this.minStack.length - 1]) {
    this.minStack.pop();
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

#### 8. 和为 s 的两个数字

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
// >>> 无符号右移
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
  while (left <= target >> 1) {
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

#### 21. 构建乘积数组

给定一个数组 A[0,1,…,n-1]，请构建一个数组 B[0,1,…,n-1]，其中 B[i] 的值是数组 A 中除了下标 i 以外的元素的积, 即 B[i]=A[0]×A[1]×…×A[i-1]×A[i+1]×…×A[n-1]。不能使用除法。

```js
// 动态规划： 两个数组，一个保存左边的乘积，一个保存右边的乘积，最后相乘
var constructArr = function (a) {
  let len = a.length;
  if (len <= 1) {
    return a;
  }
  let left = new Array(len).fill(1);
  for (let i = 1; i < len; i++) {
    left[i] = left[i - 1] * a[i - 1];
  }
  let right = new Array(len).fill(1);
  for (let i = len - 2; i >= 0; i--) {
    right[i] = right[i + 1] * a[i + 1];
  }
  let res = [];
  for (let i = 0; i < len; i++) {
    res[i] = left[i] * right[i];
  }
  return res;
};
```

#### 22. 矩阵中的路径

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

#### 23. 不用加减乘除做加法

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

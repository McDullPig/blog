#### 1-1. 删除链表中的节点:`避免节点断链`

给你一个链表的头节点 head 和一个整数 val ，请你删除链表中所有满足 Node.val == val 的节点，并返回 新的头节点 。

```js
/**
 * @param {ListNode} head
 * @param {number} val
 * @return {ListNode}
 */
var removeElements = function (head, val) {
  if (!head) {
    return head;
  }
  let newNode = new ListNode(-1);
  newNode.next = head;
  let pre = newNode;
  while (pre && pre.next) {
    if (pre.next.val !== val) {
      pre = pre.next;
    } else {
      pre.next = pre.next.next;
    }
  }
  return newNode.next;
};
```

#### 1-2. 删除链表的倒数第 N 个结点 「双指针」

给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点

```js
var removeNthFromEnd = function (head, n) {
  if (!head || n < 1) {
    return head;
  }
  let slow = head,
    fast = head;
  // 先让 fast 往后移 n 位
  while (n--) {
    fast = fast.next;
  }

  // 如果 n 和 链表中总结点个数相同，即要删除的是链表头结点时，fast 经过上一步已经到外面了
  if (!fast) {
    return head.next;
  }

  // 然后 快慢指针 一起往后遍历，当 fast 是链表最后一个结点时，此时 slow 下一个就是要删除的结点
  while (fast.next) {
    slow = slow.next;
    fast = fast.next;
  }
  slow.next = slow.next.next;
  return head;
};
```

#### 1-3. 移除链表元素

给你一个链表的头节点 head 和一个整数 val ，请你删除链表中所有满足 Node.val == val 的节点，并返回 新的头节点 。

```js
/**
 * @param {ListNode} head
 * @param {number} val
 * @return {ListNode}
 */
var removeElements = function (head, val) {
  if (!head || (head.val === val && !head.next)) {
    return null;
  }
  let node = new ListNode(-1);
  node.next = head;
  let p = node;
  while (p && p.next) {
    if (p.next.val === val) {
      p.next = p.next.next;
    } else {
      p = p.next;
    }
  }
  return node.next;
};
```

#### 1-4. 删除排序列表中的重复元素

给定一个已排序的链表的头 head ， 删除所有重复的元素，使每个元素只出现一次 。返回 已排序的链表 。

```js
var deleteDuplicates = function (head) {
  if (!head || !head.next) {
    return head;
  }
  let node = new ListNode(-1);
  let p = node;
  p.next = head;
  while (p.next && p.next.next) {
    if (p.next.val === p.next.next.val) {
      p.next = p.next.next;
    } else {
      p = p.next;
    }
  }
  return node.next;
};
```

#### 2. 链表中倒数第 K 个节点

输入一个链表，输出该链表中倒数第 k 个节点。为了符合大多数人的习惯，本题从 1 开始计数，即链表的尾节点是倒数第 1 个节点。

例如，一个链表有 6 个节点，从头节点开始，它们的值依次是 1、2、3、4、5、6。这个链表的倒数第 3 个节点是值为 4 的节点。

```js
// 使用快慢指针方法，快指针先走k个，然后一起走，快指针走到最后，慢指针走到倒数第k个
var getKthFromEnd = function (head, k) {
  if (!head || k <= 0) {
    return null;
  }
  let p = head;
  let q = head;
  while (k !== 0) {
    if (p == null) {
      return null;
    }
    p = p.next;
    k--;
  }
  while (p !== null) {
    p = p.next;
    q = q.next;
  }
  return q;
};
```

#### 3. 反转链表

定义一个函数，输入一个链表的头节点，反转该链表并输出反转后链表的头节点。

```js
/**
 * @param {ListNode} head
 * @return {ListNode}
 */
var reverseList = function (head) {
  if (!head || head.next === null) {
    return head;
  }
  let pre = new ListNode(-1);
  pre.next = null;
  let p = head;
  while (p !== null) {
    let q = p.next;
    p.next = pre.next;
    pre.next = p;
    p = q;
  }
  return pre.next;
};
```

#### 4. 合并两个排序的链表

输入两个递增排序的链表，合并这两个链表并使新链表中的节点仍然是递增排序的。

```js
/**
 * @param {ListNode} l1
 * @param {ListNode} l2
 * @return {ListNode}
 */
var mergeTwoLists = function (l1, l2) {
  if (l1 === null) return l2;
  if (l2 === null) return l1;
  let pre = new ListNode(-1);
  let head = pre;
  let p1 = l1;
  let p2 = l2;
  while (p1 !== null && p2 !== null) {
    if (p1.val <= p2.val) {
      head.next = p1;
      p1 = p1.next;
    } else {
      head.next = p2;
      p2 = p2.next;
    }
    head = head.next;
  }
  if (p1) head.next = p1;
  if (p2) head.next = p2;
  return pre.next;
};
```

#### 5. 从尾到头打印链表

输入一个链表的头节点，从尾到头反过来返回每个节点的值（用数组返回）

```js
/**
 * @param {ListNode} head
 * @return {number[]}
 */
var reversePrint = function (head) {
  if (head === null) {
    return [];
  }
  let array = [];
  while (head) {
    array.unshift(head.val);
    head = head.next;
  }
  return array;
};
// 可以使用栈的先进后出，我尝试过unshift()方法，但是执行时间会稍长一点。
// 确定长度，比使用stack.length 用时短一些。
var reversePrint = function (head) {
  const stack = [],
    res = [];
  let p = head,
    t = 0;
  while (p) {
    stack.push(p.val);
    t++;
    p = p.next;
  }
  for (let i = 0; i < t; i++) {
    res.push(stack.pop());
  }
  return res;
};
```

#### 6. 两个链表的第一个公共节点,相交链表

输入两个链表，找出它们的第一个公共节点。

```js
// 第一种方法：快慢指针：时间复杂度时o(n), 空间负责度o(1)
var getIntersectionNode = function (headA, headB) {
  let node = headA;
  let lengthA = 0;
  while (node) {
    ++lengthA;
    node = node.next;
  }
  if (!lengthA) return null;
  node = headB;
  let lengthB = 0;
  while (node) {
    ++lengthB;
    node = node.next;
  }
  if (!lengthB) return null;

  let diff = 0,
    slow,
    fast;
  if (lengthA > lengthB) {
    slow = headA;
    fast = headB;
    diff = lengthA - lengthB;
  } else {
    slow = headB;
    fast = headA;
    diff = lengthB - lengthA;
  }

  while (diff--) {
    slow = slow.next;
  }

  while (slow !== fast) {
    slow = slow.next;
    fast = fast.next;
  }
  return slow;
};

// 第二种：哈希表: 时间复杂度o(n), 空间复杂度o(n)
var getIntersectionNode = function (headA, headB) {
  if (!headA || !headB) {
    return null;
  }
  let map = new Map();
  while (headA) {
    map.set(headA, true);
    headA = headA.next;
  }
  while (headB) {
    if (map.has(headB)) {
      return headB;
    }
    map.set(headB, true);
    headB = headB.next;
  }
  return null;
};
```

#### 7. 复杂链表的复制

请实现 copyRandomList 函数，复制一个复杂链表。在复杂链表中，每个节点除了有一个 next 指针指向下一个节点，还有一个 random 指针指向链表中的任意节点或者 null。

```js
// 原地复制：把一个节点放在原节点的后面，然后再拆分
var copyRandomList = function (head) {
  if (head === null) {
    return null;
  }
  let node = head;
  let copy = null;
  //将拷贝节点放到原节点后面，例如1->2->3这样的链表就变成了这样1->1'->2'->3->3'
  while (node !== null) {
    copy = new Node(node.val);
    copy.next = node.next;
    node.next = copy;
    node = node.next.next;
  }
  //把拷贝节点的random指针安排上
  for (node = head; node !== null; node = node.next.next) {
    if (node.random !== null) {
      node.next.random = node.random.next;
    }
  }
  node = head;
  let newHead = head.next;
  //分离拷贝节点和原节点，变成1->2->3和1'->2'->3'两个链表，后者就是答案
  while (node !== null && node.next !== null) {
    copy = node.next;
    node.next = copy.next;
    node = copy;
  }
  return newHead;
};

//map 映射
var copyRandomList = function (head) {
  if (!head) {
    return null;
  }
  const map = new Map();
  let node = head; // 当前节点
  const newHead = new Node(node.val);
  let newNode = newHead; // 当前节点的copy
  map.set(node, newNode);
  // 复制节点，并链接next
  while (node.next) {
    newNode.next = new Node(node.next.val);
    node = node.next;
    newNode = newNode.next;
    map.set(node, newNode);
  }
  // 将random指针完成
  newNode = newHead;
  node = head;
  while (newNode) {
    newNode.random = map.get(node.random);
    newNode = newNode.next;
    node = node.next;
  }

  return newHead;
};
```

#### 8. 分隔链表

给你一个链表的头节点 head 和一个特定值 x ，请你对链表进行分隔，使得所有 小于 x 的节点都出现在 大于或等于 x 的节点之前。

你应当 保留 两个分区中每个节点的初始相对位置。

```js
// 使用两个链表small和large,small维护小于x的链表，最后将small的尾结点指向large的头结点
var partition = function (head, x) {
  let small = new ListNode(0);
  const smallHead = small;
  let large = new ListNode(0);
  const largeHead = large;
  while (head !== null) {
    if (head.val < x) {
      small.next = head;
      small = small.next;
    } else {
      large.next = head;
      large = large.next;
    }
    head = head.next;
  }
  large.next = null;
  small.next = largeHead.next;
  return smallHead.next;
};
```

#### 9. 反转链表 2

给你单链表的头指针 head 和两个整数  left 和 right ，其中  left <= right 。请你反转从位置 left 到位置 right 的链表节点，返回 反转后的链表

```js
var reverseBetween = function (head, left, right) {
  if (!head || !head.next) {
    //如果只有一个就返回
    return head;
  }
  let preHead = new ListNode(-1); // 加一个虚拟的头结点，头插法会更好写
  let pre = preHead;
  pre.next = head;
  for (let i = 1; i < left; i++) {
    // 先走到需要反转的前一个节点
    pre = pre.next;
  }
  let cur = pre.next;
  for (let i = 1; i <= right - left; i++) {
    //反转需要反转的节点，链表别断，就是让当前节点指向next.next
    const next = cur.next;
    cur.next = next.next;
    next.next = pre.next;
    pre.next = next;
  }
  return preHead.next;
};
```

#### 10-1. 环形链表

给你一个链表的头节点 head ，判断链表中是否有环。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。注意：pos 不作为参数进行传递  。仅仅是为了标识链表的实际情况。

如果链表中存在环  ，则返回 true 。 否则，返回 false 。

```js
/**
 * @param {ListNode} head
 * @return {boolean}
 */
var hasCycle = function (head) {
  if (!head || !head.next) {
    return false;
  }
  let slow = head;
  let fast = head;
  while (fast && fast.next) {
    fast = fast.next.next;
    slow = slow.next;
    if (fast === slow) {
      return true;
    }
  }
  return false;
};
```

#### 10-2.环形链表 2

给定一个链表的头节点  head ，返回链表开始入环的第一个节点。  如果链表无环，则返回  null。

如果链表中有某个节点，可以通过连续跟踪 next 指针再次到达，则链表中存在环。 为了表示给定链表中的环，评测系统内部使用整数 pos 来表示链表尾连接到链表中的位置（索引从 0 开始）。如果 pos 是 -1，则在该链表中没有环。注意：pos 不作为参数进行传递，仅仅是为了标识链表的实际情况。

```js
//哈希表: 遍历节点，如果有环的话，map中会存在当前节点。
var detectCycle = function (head) {
  if (!head || !head.next) {
    return null;
  }
  let map = new Map();
  let node = head;
  while (node) {
    if (map.get(node)) {
      return node;
    } else {
      map.set(node, true);
      node = node.next;
    }
  }
  return null;
};
// 快慢指针: 在有环的前提下，slow 走过 x+y, fast 走过 x+y+n(y+z)
//fast走的节点是slow的2倍：2(x+y) = x+y+n(y+z)
//x = n(y+z) -y = (n-1)(y+z) +z   n =1 =>x = z
var detectCycle = function (head) {
  if (!head || !head.next) {
    return null;
  }
  let fast = (slow = head);
  // 第一次相遇
  while (fast && fast.next) {
    fast = fast.next.next;
    slow = slow.next;
    if (slow === fast) {
      break;
    }
  }
  // 判断fast是否为空
  if (!fast || !fast.next) {
    return null;
  }
  // 让slow重新指向head
  slow = head;
  while (slow !== fast) {
    fast = fast.next;
    slow = slow.next;
  }
  return slow;
};
```

#### 11. 回文链表

给你一个单链表的头节点 head ，请你判断该链表是否为回文链表。如果是，返回 true ；否则，返回 false 。

```js
// 1、可以使用数组保存值，然后使用双指针的方式去判断是否回文

// 2、快慢指针
var isPalindrome = function (head) {
  if (!head || !head.next) {
    return true;
  }
  let slow = head;
  let fast = head;
  // 找到前半部分链表的尾节点
  while (fast.next && fast.next.next) {
    fast = fast.next.next;
    slow = slow.next;
  }
  // 反转后半部分链表
  let node = slow.next;
  slow.next = null;
  while (node) {
    let q = node.next;
    node.next = slow;
    slow = node;
    node = q;
  }
  // 判断是否回文
  fast = head;
  while (fast) {
    if (slow.val !== fast.val) {
      return false;
    }
    slow = slow.next;
    fast = fast.next;
  }
  return true;
};
```

#### 12. 奇偶链表

给定单链表的头节点 head ，将所有索引为奇数的节点和索引为偶数的节点分别组合在一起，然后返回重新排序的列表。

第一个节点的索引被认为是奇数 ， 第二个节点的索引为偶数 ，以此类推。

请注意，偶数组和奇数组内部的相对顺序应该与输入时保持一致。

必须在 O(1) 的额外空间复杂度和 O(n) 的时间复杂度下解决这个问题。

```js
var oddEvenList = function (head) {
  if (!head || !head.next) {
    return head;
  }
  let odd = head;
  let even = head.next;
  let o = odd;
  let e = even;
  while (e !== null && e.next !== null) {
    o.next = e.next;
    o = o.next;
    e.next = o.next;
    e = e.next;
  }
  o.next = even;
  return odd;
};
```

#### 13. 合并 K 个升序链表

给你一个链表数组，每个链表都已经按升序排列。

请你将所有链表合并到一个升序链表中，返回合并后的链表。

```js
// 使用归并排序+链表
var mergeKLists = function (lists) {
  if (lists.length <= 0) {
    return null;
  }
  if (lists.length === 1) {
    return lists[0];
  }
  return mergeList(lists, 0, lists.length - 1);
};
let merge = (left, right) => {
  let node = new ListNode(-1);
  let head = node;
  while (left && right) {
    if (left.val <= right.val) {
      head.next = left;
      left = left.next;
    } else {
      head.next = right;
      right = right.next;
    }
    head = head.next;
  }
  head.next = left ? left : right;
  return node.next;
};
let mergeList = (lists, left, right) => {
  if (left >= right) {
    return lists[left];
  }
  let mid = left + ((right - left) >> 1);
  let leftArr = mergeList(lists, left, mid);
  let rightArr = mergeList(lists, mid + 1, right);
  return merge(leftArr, rightArr);
};
```

#### 14. 两数相加

给你两个非空的链表，表示两个非负的整数。它们每位数字都是按照逆序的方式存储的，并且每个节点只能存储一位数字。

请你将两个数相加，并以相同形式返回一个表示和的链表。

你可以假设除了数字 0 之外，这两个数都不会以 0  开头。

```js
var addTwoNumbers = function (l1, l2) {
  if (!l1 || (!l1.next && l1.val === 0)) {
    return l2;
  }
  if (!l2 || (!l2.next && l2.val === 0)) {
    return l1;
  }
  let flag = 0; //记录进位
  let l = new ListNode(-1); // 返回结果的头结点
  let head = l;
  while (l1 || l2) {
    let value = (l1 ? l1.val : 0) + (l2 ? l2.val : 0) + flag; //有可能l1或l2为null
    flag = Math.floor(value / 10); //进位
    value = value % 10; //个数
    let node = new ListNode(value);
    head.next = node;
    head = head.next;
    l1 = l1 && l1.next;
    l2 = l2 && l2.next;
  }
  if (flag !== 0) {
    let node = new ListNode(flag);
    head.next = node;
  }
  return l.next;
};
```

#### 15. 链表的中间结点 「双指针」

给定一个头结点为 head 的非空单链表，返回链表的中间结点。

如果有两个中间结点，则返回第二个中间结点。

```js
var middleNode = function (head) {
  if (!head || !head.next) {
    return head;
  }
  let slow = head;
  let fast = head;
  while (fast && fast.next) {
    fast = fast.next.next;
    slow = slow.next;
  }
  return slow;
};
```

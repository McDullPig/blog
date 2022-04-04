#### 1. 删除链表中的节点:`避免节点断链`

给定单向链表的头指针和一个要删除的节点的值，定义一个函数删除该节点。

返回删除后的链表的头节点。

```js
/**
 * @param {ListNode} head
 * @param {number} val
 * @return {ListNode}
 */
//不带头链表
var deleteNode = function (head, val) {
  if (head === null) {
    return head;
  }
  if (head.val === val) {
    return head.next;
  }
  let p = head;
  while (p.next !== null) {
    if (p.next.val === val) {
      p.next = p.next.next;
      return head;
    } else {
      p = p.next;
    }
  }
  return head;
};
//带头链表
var deleteNode = function (head, val) {
  let pre = new ListNode(-1); // 哨兵节点
  pre.next = head;

  let node = pre;
  while (node.next) {
    if (node.next.val === val) {
      node.next = node.next.next;
      break;
    }
    node = node.next;
  }
  return pre.next;
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
  if (!head) {
    return null;
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

#### 6. 两个链表的第一个公共节点

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

#### 8. 

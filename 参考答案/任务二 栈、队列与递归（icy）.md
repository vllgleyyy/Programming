# 任务二
栈

- 用数组实现一个顺序栈
- 用链表实现一个链式栈
- 编程模拟实现一个浏览器的前进、后退功能

队列

- 用数组实现一个顺序队列
- 用链表实现一个链式队列
- 实现一个循环队列

递归

- 编程实现斐波那契数列求值 f(n)=f(n-1)+f(n-2)
- 编程实现求阶乘 n!
- 编程实现一组数据集合的全排列

对应的 LeetCode 练习题

- 栈
    - Valid Parentheses（有效的括号）

    - Longest Valid Parentheses（最长有效的括号）

    - Evaluate Reverse Polish Notatio（逆波兰表达式求值）

- 队列
    - Design Circular Deque（设计一个双端队列）

    - Sliding Window Maximum（滑动窗口最大值）

- 递归
    - Climbing Stairs（爬楼梯）



# 解答：栈

## 用数组实现一个顺序栈

假定数组的结尾作为栈顶，后端插入和删除是 O(1) 操作。

栈有如下基本操作：
- Stack() 创建一个空的新栈。 返回一个空栈。
- push(item)将一个新项添加到栈的顶部。它需要 item 做参数并不返回任何内容。
- pop() 从栈中删除顶部项。它返回 item 。栈被修改。
- top() 从栈返回顶部项，但不会删除它。不修改栈。
- isEmpty() 测试栈是否为空。返回布尔值。
- size() 返回栈中的 item 数量。返回一个整数。


```python
class SStack:
    def __init__(self):
        self.items = []
    def isEmpty(self):
        return self.items == []
        # return not self.items
    def push(self,item):
        self.items.append(item)
    def pop(self):
        return self.items.pop()
    def top(self):
        if self.items:
            return self.items[-1]
        else:
            return None
    def size(self):
        return len(self.items)
    
    
if __name__ == "__main__":
    s = SStack()
    s.push('a')
    print(s.top())
    s.push('b')
    s.push('c')
    print(s.pop())
```
输出：
```
a
c
```
数组实现的缺点：
- 需要完整的大块存储空间
- 扩大存储的操作代价高

而用链表实现就没有以上的问题。

## 用链表实现一个链式栈
对于链接表，将表头作为栈顶，在前端插入和删除都是 O(1) 操作。
```python
class Node: # 链表的结点
    def __init__(self, x, next_=None):
        self.val = x
        self.next = next_

class LStack: #栈的链接表实现
    def __init__(self):
        self._top = None
    def isEmpty(self):
        return self._top is None
    def push(self,item):
        self._top = Node(item, self._top)
    def pop(self):
        p = self._top
        self._top = p.next
        return p.val
    def top(self):
        if self._top:
            return self._top.val
        else:
            return None
    
    
if __name__ == "__main__":
    s = LStack()
    s.push('a')
    print(s.top())
    s.push('b')
    s.push('c')
    print(s.pop())
```
输出：
```
a
c
```
## 编程模拟实现一个浏览器的前进、后退功能
每个 web 浏览器都有一个返回按钮。当你浏览网页时，这些网页被放置在一个栈中（实际是网页的网址）。你现在查看的网页在顶部，你第一个查看的网页在底部。如果按‘返回’按钮，将按相反的顺序浏览刚才的页面。
```python
class Page: # 一个网页
    def __init__(self, url, prev_=None, next_=None):
        self.val = url
        self.next = next_
        self.prev = prev_

class History: 
    def __init__(self): 
        self._cur = Page(None)
    
    def loadPage(self, newPage): #加载新的页面
        p = Page(newPage,self._cur,None)
        self._cur.next = p
        self._cur = self._cur.next
        return self._cur.val
        
    def back(self): #返回上一页面
        if self._cur.prev is None:
            return
        self._cur = self._cur.prev
        return self._cur.val
    
    def forward(self): #前进页面
        if self._cur.next is None:
            return
        self._cur = self._cur.next
        return self._cur.val
    
if __name__ == '__main__':
    h = History()
    print('加载页面'+ h.loadPage('a')) 
    print('加载页面'+ h.loadPage('b'))
    print('返回到'+ h.back())       
    print('加载页面'+ h.loadPage('c')) 
    print('返回到'+ h.back())
    print('前进到'+ h.forward())    
```
输出：
```
加载页面a
加载页面b
返回到a
加载页面c
返回到a
前进到c
```
## LeetCode 20. 有效的括号
[Valid Parentheses](https://leetcode-cn.com/problems/valid-parentheses/)

给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串，判断字符串是否有效。

有效字符串需满足：

- 左括号必须用相同类型的右括号闭合。
- 左括号必须以正确的顺序闭合。
- 注意空字符串可被认为是有效字符串。

示例 1:

>输入: "()[]{}"

输出: true

示例 2:

> 输入: "([)]"

> 输出: false

示例 3:

> 输入: "{[]}"

> 输出: true 

## 方法
- 先建立一个map
- 遍历字符串，对输入的字符串入栈操作（如果入栈的元素是key的话）
- 依次比较，直到出现不匹配或者栈里所有元素都比较结束(栈空)。
- 还要注意这样的问题：如果最后多余了’key‘，比如()[]{}(，所以最后还要判断一下`len(stack)==0`

```python
class Solution:
    def isValid(self, s):
        """
        :type s: str
        :rtype: bool
        """
        stack = list()
        match = {'{':'}', '[':']', '(':')'}
        for i in s:
            if i in match:
                stack.append(i)
            else:
                if len(stack) == 0:
                    return False

                if match[stack[-1]] != i:
                    return False
                
                stack.pop()

        if len(stack) != 0:
            return False
        return True
```

## LeetCode 32. 最长有效的括号
[Longest Valid Parentheses](https://leetcode-cn.com/problems/longest-valid-parentheses/)

给定一个只包含 '(' 和 ')' 的字符串，找出最长的包含有效括号的子串的长度。

示例 1:
```
输入: "(()"
输出: 2
解释: 最长有效括号子串为 "()"
```
示例 2:
```
输入: ")()())"
输出: 4
解释: 最长有效括号子串为 "()()"
```

### 方法
- 用一个栈来存储左括号的索引.
- 遇到正确匹配的括号则弹出匹配的索引，所以栈中存储的是未匹配上的左括号。
- 新匹配上的括号位置减去前一段未匹配到的括号的索引的差，是当前有效子串的大小。

```python
class Solution:
    def longestValidParentheses(self, s: str) -> int:
        maxlen = 0  # 最长的字串长度
        last = -1   # 上一段有效子串的结尾位置
        stack = []  # 栈里存放左括号的位置序号
        
        for i in range(len(s)):
            if s[i] == '(':
                stack.append(i) # 存入左括号的位置序号
            else:# 右括号
                if stack == []: # 栈空
                    last = i # 配对失败，一段有效子串结尾
                else:
                    stack.pop() #取出配对左括号
                    if stack == []: # 配对完，栈空
                    #当前位置i减去上一段结尾last，是这一段有效子串的长度
                        maxlen = max(maxlen, i-last) 
                    else:
                        # stack剩余的左括号未必能配对成功，先比较当前的有效子串
                        maxlen= max(maxlen, i - stack[-1])
        return maxlen
```

## LeetCode 150. 逆波兰表达式求值
[Evaluate Reverse Polish Notatio](https://leetcode-cn.com/problems/evaluate-reverse-polish-notation/)

根据[逆波兰表示法](https://baike.baidu.com/item/%E9%80%86%E6%B3%A2%E5%85%B0%E5%BC%8F/128437)，求表达式的值。

有效的运算符包括 +, -, *, / 。每个运算对象可以是整数，也可以是另一个逆波兰表达式。

说明：

- 整数除法只保留整数部分。
- 给定逆波兰表达式总是有效的。换句话说，表达式总会得出有效数值且不存在除数为 0 的情况。

示例 1：
```
输入: ["2", "1", "+", "3", "*"]
输出: 9
解释: ((2 + 1) * 3) = 9
```

示例 2：
```
输入: ["4", "13", "5", "/", "+"]
输出: 6
解释: (4 + (13 / 5)) = 6
```

### 方法
- 逆波兰式（Reverse Polish notation，RPN，或逆波兰记法），也叫后缀表达式（将运算符写在操作数之后）
- python 有个函数eval()，可以给它一个运算表达式，直接给你求值。中缀表达式转正常表达式很简单了，直接用栈就行。
- 但需要注意的是，python中的’/’负数除法和c语言不太一样。在python中，(-1)/2=-1，而在c语言中，(-1)/2=0。也就是c语言中，除法是向零取整，即舍弃小数点后的数。而在python中，是向下取整的。而这道题的oj是默认的c语言中的语法，所以需要在遇到’/’的时候注意一下。
- 可采用的方式是使用operator.truediv(int(a), int(b))变成和c相同的方式。

```python
class Solution:
    def evalRPN(self, tokens: List[str]) -> int:
        stack = []
        operators = ['+', '-','*','/']
        
        for token in tokens:
            if token not in operators:
                stack.append(token)
            else:
                b = stack.pop()
                a = stack.pop()
                if token == '/':
                    res = int(operator.truediv(int(a), int(b)))
                else:
                    res = eval(a+token+b)
                stack.append(str(res))
        return int(stack.pop())
```

# 解答：队列
## 用数组实现一个顺序队列
list表头入队，表尾出队。反过来也可以。

实现的基本操作有：
- enqueue 入队，要把表中其余元素全部后移，再插入首元素，需要 $O(n)$时间
- dequeue 出队,$O(1)$

```python
class Queue:
    def __init__(self):
        self.items = []
    def enqueue(self, item): # 入队
        self.items.insert(0,item)
    def dequeue(self): # 出队
        return self.items.pop()
    
if __name__ == '__main__':
    q = Queue()
    q.enqueue(4)
    q.enqueue(5)
    q.enqueue(6)
    print(q.dequeue())
```
输出：
```
4
```

## 用链表实现一个链式队列
等同于用带表尾指针的单链表。链尾入队，链首出队。

实现的基本操作有：
- enqueue 入队   $O(1)$
- dequeue 出队   $O(1)$
- peek 查看队首元素 $O(1)$

```python
class Node: # 链表的结点
    def __init__(self, x, next_=None):
        self.val = x
        self.next = next_

class LQueue: # 
    def __init__(self): # 空表
        self._head = None
        self._rear = None # 尾结点
    
    def enqueue(self, x): # 入队
        if self._head is None:
            self._head = Node(x, self._head)
            self._rear = self._head
        else:
            self._rear.next = Node(x)
            self._rear = self._rear.next
    def dequeue(self): # 出队
        if self._head is None:
            return 
        x = self._head.val
        self._head = self._head.next
        return x
    def peek(self): # 查看最早入队的元素
        if self._head is None:
            return
        return self._head.val 
    
if __name__ == '__main__':
    l = LQueue()
    l.enqueue(4)
    l.enqueue(5)
    print(l.peek())
    l.enqueue(6)
    l.dequeue()
    print(l.peek())
```
输出：
```
4
5
```

## 实现一个循环队列
[LeetCode 622](https://leetcode-cn.com/problems/design-circular-queue/)

队头变量`_head`记录当前队列的第一个元素位置
```python
class QueueUnderflow(ValueError):# 空队列无法 dequeue的异常错误
    pass

class SQueue:
    def __init__(self, init_len=8):
        self._elems = [0]*init_len  # 队列元素
        self._head = 0 # 队列首元素位置的下标
        self._num = 0  # 表中元素个数
        self._len = init_len   # 当前表的长度，表满了的话，要换一个存储表
    def peek(self):
        if self._num == 0: # 队空状态
            raise QueueUnderflow
        return self._elems[self._head]
    def dequeue(self):
        if self._num == 0:  # 队空状态
            raise QueueUnderflow
        e = self._elems[self._head]
        self._head = (self._head+1) % self._len 
        self._num -= 1
        return e
    def enqueue(self, e):
        if self._num == self._len: # 队满状态
            self.__extend() # 扩大存储区
        # 新元素的入队位置
        self._elems[(self._head + self._num) % self._len]=e
        self._num += 1
    def __extend(self):  # 扩容
        old_len = self._len
        self._len *= 2
        new_elems = [0] * self._len
        for i in range(old_len):
            new_elems[i] = self._elems[(self._head + i)%old_len]
        self._elems, self._head = new_elems, 0
        
if __name__ == '__main__':
    l = SQueue()
    for i in range(10):
        l.enqueue(i+1)
    print(l.peek())
    l.dequeue()
    print(l.peek())
```

## LeetCode 641. 设计循环双端队列
[Design Circular Deque](https://leetcode-cn.com/problems/design-circular-deque/)

设计实现双端队列。
你的实现需要支持以下操作：

-   MyCircularDeque(k)：构造函数,双端队列的大小为k。
-   insertFront()：将一个元素添加到双端队列头部。 如果操作成功返回 true。
-   insertLast()：将一个元素添加到双端队列尾部。如果操作成功返回 true。
-   deleteFront()：从双端队列头部删除一个元素。 如果操作成功返回 true。
-   deleteLast()：从双端队列尾部删除一个元素。如果操作成功返回 true。
-   getFront()：从双端队列头部获得一个元素。如果双端队列为空，返回 -1。
-   getRear()：获得双端队列的最后一个元素。 如果双端队列为空，返回 -1。
-   isEmpty()：检查双端队列是否为空。
-   isFull()：检查双端队列是否满了。

示例：
```
MyCircularDeque circularDeque = new MycircularDeque(3); // 设置容量大小为3
circularDeque.insertLast(1);			        // 返回 true
circularDeque.insertLast(2);			        // 返回 true
circularDeque.insertFront(3);			        // 返回 true
circularDeque.insertFront(4);			        // 已经满了，返回 false
circularDeque.getRear();  				// 返回 2
circularDeque.isFull();				        // 返回 true
circularDeque.deleteLast();			        // 返回 true
circularDeque.insertFront(4);			        // 返回 true
circularDeque.getFront();				// 返回 4
 ```
 

提示：

-   所有值的范围为 [1, 1000]
-   操作次数的范围为 [1, 1000]
-   请不要使用内置的双端队列库。

### 方法
和上一题思路相同，用一个list实现类似的环形列表。
双向链表可以从头尾插入元素，对应了list的insert和append方法。注意，无论是在头尾插入，要移动的指针都是rear。

```python
class MyCircularDeque:

    def __init__(self, k: int):
        """
        Initialize your data structure here. Set the size of the deque to be k.
        """
        self._elems = [0]*k  # 队列元素
        self._head = 0 # 队列首元素位置的下标
        self._num = 0  # 表中元素个数
        self._len = k   # 表的长度

    def insertFront(self, value: int) -> bool:
        """
        Adds an item at the front of Deque. Return true if the operation is successful.
        """
        if self._num == self._len: # 队满状态
            return False
        # 新元素的入队位置
        self._head = (self._head + self._len - 1) % self._len
        self._elems[self._head]= value
        self._num += 1
        return True

    def insertLast(self, value: int) -> bool:
        """
        Adds an item at the rear of Deque. Return true if the operation is successful.
        """
        if self._num == self._len: # 队满状态
            return False
        # 新元素的入队位置
        self._elems[(self._head + self._num) % self._len]= value
        self._num += 1
        return True
        

    def deleteFront(self) -> bool:
        """
        Deletes an item from the front of Deque. Return true if the operation is successful.
        """
        if self._num == 0:  # 队空状态
            return False
        # e = self._elems[self._head]
        self._head = (self._head+1) % self._len 
        self._num -= 1
        return True

    def deleteLast(self) -> bool:
        """
        Deletes an item from the rear of Deque. Return true if the operation is successful.
        """
        if self._num == 0:  # 队空状态
            return False
        # e = self._elems[(self._head + self._num-1) % self._len]
        self._num -= 1
        return True

    def getFront(self) -> int:
        """
        Get the front item from the deque.
        """
        if self._num == 0: # 队空状态
            return -1
        return self._elems[self._head]

    def getRear(self) -> int:
        """
        Get the last item from the deque.
        """
        if self._num == 0: # 队空状态
            return -1
        return self._elems[(self._head + self._num-1) % self._len]

    def isEmpty(self) -> bool:
        """
        Checks whether the circular deque is empty or not.
        """
        return  self._num == 0

    def isFull(self) -> bool:
        """
        Checks whether the circular deque is full or not.
        """
        return  self._len == self._num


# Your MyCircularDeque object will be instantiated and called as such:
# obj = MyCircularDeque(k)
# param_1 = obj.insertFront(value)
# param_2 = obj.insertLast(value)
# param_3 = obj.deleteFront()
# param_4 = obj.deleteLast()
# param_5 = obj.getFront()
# param_6 = obj.getRear()
# param_7 = obj.isEmpty()
# param_8 = obj.isFull()
```


## LeetCode 239. 滑动窗口最大值
[Sliding Window Maximum](https://leetcode-cn.com/problems/sliding-window-maximum/)


### 方法：deque（双端队列）

遍历数组nums，使用双端队列deque维护滑动窗口内有可能成为最大值元素的数组下标。

当下标i从队尾入队时，顺次弹出队列尾值`<=nums[i]`的数组下标。

当前下标为i，则滑动窗口的有效下标范围为[i - (k - 1), i] ,所以当队头元素`dq[0] == i-k`，就要从队头出队。

```python
class Solution:
    def maxSlidingWindow(self, nums: List[int], k: int) -> List[int]:
        dq = collections.deque() # 队列
        res = []
        for i in range(len(nums)):
            while dq and nums[dq[-1]] <= nums[i]:#(循环)当dq队尾值小于新元素nums[i]
                dq.pop() #队尾值出队(直到dq中的值都大于新元素nums[i])
            dq.append(i) #新元素入队
            if dq[0] == i-k: # 窗口滑出，队头元素不在窗口区域内
                dq.popleft() # 队头值出队
            if i >= k-1: # i遍历到一个窗口大小之后，每轮都能执行此行。
                res.append(nums[dq[0]]) #窗口最大元素
        return res
```

# 解答：递归
## 斐波那契数列求值 f(n)=f(n-1)+f(n-2)
斐波那契数列由 0 和 1 开始，后面的每一项数字都是前面两项数字的和。

[0,1,1,2,3,5,8,13,...]  给定`n`,计算 `f(N)`

对应LeetCode习题：[509. Fibonacci Number](https://leetcode-cn.com/problems/fibonacci-number/)

### 方法
```python
class Solution:
    def fib(self, N: int) -> int:
        if N <= 1:
            return N
        return self.fib(N-1) + self.fib(N-2)
```

## 求阶乘 n!
```python
def factorial(n):
    if n == 0 or n == 1:
        return 1
    else:
        return (n*factorial(n-1))

a = factorial(3)
print(a)
```
输出：6

## LeetCode 46. 全排列
[Permutations](https://leetcode-cn.com/problems/permutations/)

给定一个**没有重复**数字的序列，返回其所有可能的全排列。

**示例**

> 输入: [1,2,3]

> 输出:
[
  [1,2,3],
  [1,3,2],
  [2,1,3],
  [2,3,1],
  [3,1,2],
  [3,2,1]
]


### 方法
每次选择一个数出来，然后把剩下的数，再选择一个出来，依次类推，选到头，就回溯到上一层。
```python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        res = []
        
        def dfs(nums = nums, path = []):
            if not nums:
                res.append(path)
            for i in range(len(nums)):
                dfs(nums[:i]+nums[i+1:], path+[nums[i]])
        dfs()
        return res
```
## LeetCode 47. 全排列 II
[Permutations II](https://leetcode-cn.com/problems/permutations-ii/)

给定一个可包含重复数字的序列，返回所有不重复的全排列。

**示例:**

> 输入: [1,1,2]

> 输出:
[
  [1, 1, 2], 
  [1, 2, 1], 
  [2, 1, 1] 
]


### 方法
相比上一题，多了排序和内部的重复判断。
```python
class Solution:
    def permuteUnique(self, nums: List[int]) -> List[List[int]]:
        nums.sort()
        res = []
        
        def dfs(nums = nums, path=[]):
            if not nums:
                res.append(path)
            for i in range(len(nums)):
                if i>0 and nums[i] == nums[i-1]:
                    continue
                dfs(nums[:i]+nums[i+1:], path+[nums[i]])
        dfs()
        return res
```

## LeetCode 70. 爬楼梯
[Climbing Stairs](https://leetcode-cn.com/problems/climbing-stairs/)


假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

注意：给定 n 是一个正整数。


**示例 ：**
```
输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶
```

### 方法：~~递归~~ 
当有n个台阶时，可供选择的走法可以分两类：1，先跨一阶再跨完剩下n-1阶；2，先跨2阶再跨完剩下n-2阶。所以n阶的不同走法的数目是n-1阶和n-2阶的走法数的和。 这和斐波那契数列的规律相同，可以用这个思路。
```python
class Solution:
    def climbStairs(self, n):
        """
        :type n: int
        :rtype: int
        """
        if n<=2:
            return n 
        return self.climbStairs(n-1) + self.climbStairs(n-2)
```
这个方法运行正常，但是因为超出时间限制，未能通过。

### 方法：动态规划
动态规划来记录历史数据。
```python
class Solution:
    def climbStairs(self, n):
        """
        :type n: int
        :rtype: int
        """
        prev, current = 0, 1
        for i in range(n):
            prev, current = current, prev + current
        return current
```



# 参考
-   《我的第一本算法书》
- [problem-solving-with-algorithms-and-data-structure-using-python](https://facert.gitbooks.io/python-data-structure-cn/content/)
- [How does the Back button in a web browser work?
](https://stackoverflow.com/questions/1313788/how-does-the-back-button-in-a-web-browser-work)
- [【LeetCode】150. Evaluate Reverse Polish Notation 解题报告（Python）](https://blog.csdn.net/fuxuemingzhu/article/details/79559703)
-   [[LeetCode]Sliding Window Maximum ](http://bookshadow.com/weblog/2015/07/18/leetcode-sliding-window-maximum/)
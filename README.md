# leetcode
#### 141. Linked List Cycle
Given head, the head of a linked list, determine if the linked list has a cycle in it. There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to. Note that pos is not passed as a parameter. Return true if there is a cycle in the linked list. Otherwise, return false.

- we initialize two variables, `slow` and `fast`
- we loop each node in the linked list untill the fast node is `None`
- in each iteration, we increment the slow variable by 1 node and the fast variable by 2 nodes
- if there is a cycle, the `slow` and `fast` variables will eventually be equals
- we return `True` if they equals
- if there is no cycle, the loop will come to an end and return `False`

```python
def hasCycle(self, head: Optional[ListNode]) -> bool:
  slow, fast = head, head
  while fast and fast.next:
      fast = fast.next.next
      slow = slow.next
  
      if fast == slow:
          return True
  
  return False
```

---

#### 21. Merge Two Sorted Lists
You are given the heads of two sorted linked lists `list1` and `list2`.
Merge the two lists into one **sorted** list. The list should be made by splicing together the nodes of the first two lists.
Return the head of the merged linked list.

- we initialize two variables `l3` and `head`
- the `l3` is the new linked list
- the `head` is the first node in the `l3` list
- we loop both lists together
- first we check if we have both `list1` and `list2` nodes
- if we do, we take the the node with the smaller value (this condition used for the sorting part)
- if we dont, we check which list have a node for us, and we take it
- of course in every condition, we increment all lists `list1`, `list2` and `l3`
- at the end we return `head.next` since the first node is empty

```python
def mergeTwoLists(self, list1: Optional[ListNode], list2: Optional[ListNode]) -> Optional[ListNode]:
    l3 = ListNode()
    head = l3
    while list1 or list2:
        if list1 and list2:
            if list1.val < list2.val:
                l3.next = list1
                l3 = l3.next
                list1 = list1.next
            else:
                l3.next = list2
                l3 = l3.next
                list2 = list2.next

        elif list1:
            l3.next = list1
            l3 = l3.next
            list1 = list1.next

        elif list2:
            l3.next = list2
            l3 = l3.next
            list2 = list2.next

    return head.next
```

---

#### 160. Intersection of Two Linked Lists (with len approach)
Given the heads of two singly linked-lists headA and headB, return the node at which the two lists intersect. If the two linked lists have no intersection at all, return null.

- we first initialize 4 variables, `a_len` and `b_len` which holds the length of the lists
- and `h1` and `h2` which holds ref to `headA` and `headB`
- we loop both `h1` and `h2` to measure the length
- then we choose the longer list and skip `n-nodes`
- lastly we loop both heads to find the intersecting node
- return the intersecting node if found
- otherise, we return None
    
```python
def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> Optional[ListNode]:
    a_len, b_len = 0, 0
    h1, h2 = headA, headB

    while h1 or h2:
        if h1:
            a_len += 1
            h1 = h1.next
        if h2:
            h2 = h2.next
            b_len += 1

    if a_len > b_len:
        for _ in range(a_len - b_len):
            headA = headA.next
    else:
        for _ in range(b_len - a_len):
            headB = headB.next

    while headA and headB:
        if headA == headB:
            return headA
        
        headA = headA.next
        headB = headB.next

    return None
```

---

#### 160. Intersection of Two Linked Lists (with hashmap)
Given the heads of two singly linked-lists headA and headB, return the node at which the two lists intersect. If the two linked lists have no intersection at all, return null.

- we initialize `hashmap` which stores nodes we already traversed
- we loop `headA` and `headB` together
- each iteration we check if the node already exists in the hashmap
- if it does, we return the node
- otherwise, we insert the node into the `hashmap`
- lastly, if we dont have intersection, return None
    
```python
def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> Optional[ListNode]:
    hashmap = {}
    while headA or headB:
        if headA:
            if hashmap.get(headA):
                return headA
            else:
                hashmap[headA] = True
            headA = headA.next

        if headB:
            if hashmap.get(headB):
                return headB
            else:
                hashmap[headB] = True
            headB = headB.next

    return None
```

---

#### 817. Linked List Components
You are given the head of a linked list containing unique integer values and an integer array nums that is a subset of the linked list values.
Return the number of connected components in nums where two values are connected if they appear consecutively in the linked list.

- we initialize two variables `count` and `total`
- we loop each node in the linked list
- we check if the node's value exist in the `nums` list
- if it exist, we increment the count by one
- if it does not exist and our `count` is bigger then 0, we basicaly reset the count and increment total
- we do it this way because when `count` is not 0, we had previous match in `nums`
- at the end we simply return the `total` of consecutive matchs

**Time & Space complexity: o(n*m), o(1)**

```python
def numComponents(self, head: Optional[ListNode], nums: List[int]) -> int:
    count, total = 0, 0
    while head:
        if head.val in nums:
            count += 1
        
        elif count > 0:
            count = 0
            total += 1
            
        head = head.next

        if head == None and count > 0:
            total += 1

    return total
```

---

#### 58. Length of Last Word
Given a string `s` consisting of words and spaces, return *the length of the **last** word in the string.*
A **word** is a maximal *substring* consisting of non-space characters only.

- initialize two variables `i` and `l` which holds current index and length of word
- we start to loop from the end of the string
- we check to see if the current character is not space, and increment the `l` length of the word
- then we check if we have length and the current char is space
- if its the case, then we found the whole length of the last word.
- we exit the loop and return the length
  
**Time & Space complexity: o(n), o(1)**

```python
def lengthOfLastWord(self, s: str) -> int:
    i = len(s) -1
    l = 0
    while i >= 0:
        if s[i] != ' ':
            l += 1
        elif s[i] == ' ' and l > 0:
            break
        i -= 1
    return l
```

---

#### 142. Linked List Cycle II (with hashmap)
Given the `head` of a linked list, *return the node where the cycle begins. If there is no cycle, return* `null`.
There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the `next` pointer. 
Internally, `pos` is used to denote the index of the node that tail's `next` pointer is connected to (**0-indexed**). It is `-1` if there is no cycle. **Note that** `pos` **is not passed as a parameter**.

**Do not modify** the linked list.

- initialize `hashmap` to store traversed nodes
- initialize `node` which points to head to prevent traversing directly on `head`
- loop each node
- check if node exists in `hashmap`
- if it does, we found the node starting cycle, `return` the node
- otherwise, no cycle yet, continue
- eventually, if no cycle found, return None
  
**Time & Space complexity: o(n), o(n)**

```python
def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
    node, hashmap = head, {}
    while node:
        if hashmap.get(node):
            return node

        hashmap[node] = True
        node = node.next
    return None
  ```

---

#### 83. Remove Duplicates from Sorted List (using dummy)
Given the head of a sorted linked list, delete all duplicates such that each element appears only once. Return the linked list sorted as well.

- initialize `dummy` and `prev` pointers
- the `dummy` is just a ref to the head which we will interate over
- the `prev` pointer as the name suggest, will hold the previous node of dummy
- when we see that `prev`.val equals to current node, we know there is duplication, so we "skip" the node by updating the previous node to the next dummy node
- each iteratiom (if not uplicated), we update the prev node to the current dummy node and continue.
- at the end, we return the `head`
-   
**Time & Space complexity: o(n), o(1)**

```python
def deleteDuplicates(self, head: Optional[ListNode]) -> Optional[ListNode]:
    dummy, prev = head, None
    while dummy:
    
        if prev and dummy.val == prev.val:
            prev.next = dummy.next
    
        else:
            prev = dummy
        
        dummy = dummy.next
    
    return head
  ```
---

#### 1313. Decompress Run-Length Encoded List
We are given a list nums of integers representing a list compressed with run-length encoding.
Consider each adjacent pair of elements [freq, val] = [nums[2*i], nums[2*i+1]] (with i >= 0).  For each such pair, there are freq elements with value val concatenated in a sublist. Concatenate all the sublists from left to right to generate the decompressed list.
Return the decompressed list.

**Time & Space complexity: o(n), o(n)**

```python
def decompressRLElist(self, nums: List[int]) -> List[int]:
    arr = []
    i = 0
    while i < len(nums):
        freq = nums[i]
        val = nums[i+1]
        for _ in range(freq):
            arr += [val]
        i += 2
    return arr
  ```

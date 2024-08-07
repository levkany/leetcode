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

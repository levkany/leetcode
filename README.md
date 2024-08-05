# leetcode
#### 141. Linked List Cycle
Given head, the head of a linked list, determine if the linked list has a cycle in it. There is a cycle in a linked list if there is some node in the list that can be reached again by continuously following the next pointer. Internally, pos is used to denote the index of the node that tail's next pointer is connected to. Note that pos is not passed as a parameter. Return true if there is a cycle in the linked list. Otherwise, return false.

we initialize two variables, `slow` and `fast`
we loop each node in the linked list untill the fast node is `None`
in each iteration, we increment the slow variable by 1 node and the fast variable by 2 nodes
if there is a cycle, the `slow` and `fast` variables will eventually be equals

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

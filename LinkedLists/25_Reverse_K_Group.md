
# 🎯 **LeetCode 25 - Reverse Nodes in k-Group**

---

## 📝 **Problem Statement**

Given a linked list, reverse the nodes of the list `k` at a time and return the modified list.

* Only nodes in groups of size `k` are reversed.
* If the number of nodes is not a multiple of `k`, the remaining nodes stay as is.
* You must solve the problem in-place with constant extra space.

---

## 🔧 **Example**

**Input:**
`1 → 2 → 3 → 4 → 5 → 6 → 7 → 8 → 9 → 10`

**k = 3**

**Output:**
`3 → 2 → 1 → 6 → 5 → 4 → 9 → 8 → 7 → 10`

---

## 💡 **Approach**

✅ Use a **dummy node** to handle edge cases.

✅ Iterate the list, reversing groups of size `k`.

✅ Connect the reversed groups properly.

✅ Stop when fewer than `k` nodes remain.

---

## 🛠️ **Visual Step-by-Step**

```
Original List:
1 → 2 → 3 → 4 → 5 → 6 → 7 → 8 → 9 → 10

Step 1: Reverse first k = 3 nodes
3 → 2 → 1 → 4 → 5 → 6 → 7 → 8 → 9 → 10

Step 2: Reverse next k = 3 nodes
3 → 2 → 1 → 6 → 5 → 4 → 7 → 8 → 9 → 10

Step 3: Reverse next k = 3 nodes
3 → 2 → 1 → 6 → 5 → 4 → 9 → 8 → 7 → 10
```

---

# 🧩 **Modular Python Solution**

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next

def reverseKGroup(head, k):
    dummy = ListNode(0)
    dummy.next = head
    group_prev = dummy

    while True:
        kth = getKthNode(group_prev, k)
        if not kth:
            break
        
        group_next = kth.next
        
        # Reverse the group using helper function
        new_group_head = reverseLinkedList(group_prev.next, group_next)
        
        tmp = group_prev.next  # Tail of reversed group
        group_prev.next = new_group_head
        group_prev = tmp
    
    return dummy.next

def getKthNode(curr, k):
    while curr and k > 0:
        curr = curr.next
        k -= 1
    return curr

def reverseLinkedList(start, end):
    prev = end
    curr = start

    while curr != end:
        tmp = curr.next
        curr.next = prev
        prev = curr
        curr = tmp
    
    return prev
```

---

## ⏱️ **Complexity Analysis**

* **Time Complexity:** `O(N)` — Each node is visited once
* **Space Complexity:** `O(1)` — In-place reversal with constant space

---

## 🚀 **Key Takeaways**

✅ Clean modular design with helper functions
✅ Efficient, in-place reversal
✅ Easy to extend for similar problems
✅ Shows good coding practice in interviews

---

# ⚡ **Pro Tip for Interviews**

* Always mention the dummy node simplifies head changes
* Explain pointer movement clearly
* Modular functions like `reverseLinkedList` show design thinking

---



---

## **Setup**

**Linked List:**
`1 → 2 → 3 → 4 → 5 → 6 → 7 → 8 → 9 → 10`

**Group Size (`k`) = 3**

---

## **Initial Pointers**

* `dummy → 1 → 2 → 3 → 4 → 5 → 6 → 7 → 8 → 9 → 10`
* `group_prev = dummy`

---

## **First Iteration: Reverse First Group (1 → 2 → 3)**

1. Find `kth` node → Node `3`
2. `group_next = 4`

### Reverse:

```
Start: 1 → 2 → 3 → 4
After Reverse: 3 → 2 → 1 → 4
```

### Connect:

```
dummy → 3 → 2 → 1 → 4 → 5 → 6 → 7 → 8 → 9 → 10
```

Update:

* `group_prev = 1` (tail of reversed group)

---

## **Second Iteration: Reverse Next Group (4 → 5 → 6)**

1. Find `kth` node → Node `6`
2. `group_next = 7`

### Reverse:

```
Start: 4 → 5 → 6 → 7
After Reverse: 6 → 5 → 4 → 7
```

### Connect:

```
dummy → 3 → 2 → 1 → 6 → 5 → 4 → 7 → 8 → 9 → 10
```

Update:

* `group_prev = 4`

---

## **Third Iteration: Reverse Next Group (7 → 8 → 9)**

1. Find `kth` node → Node `9`
2. `group_next = 10`

### Reverse:

```
Start: 7 → 8 → 9 → 10
After Reverse: 9 → 8 → 7 → 10
```

### Connect:

```
dummy → 3 → 2 → 1 → 6 → 5 → 4 → 9 → 8 → 7 → 10
```

Update:

* `group_prev = 7`

---

## **Fourth Iteration: Remaining Nodes (only Node 10)**

* Fewer than `k` nodes → stop

---

## **Final Linked List**

```
3 → 2 → 1 → 6 → 5 → 4 → 9 → 8 → 7 → 10
```

---

# **Summary Table**

| Iteration | Reversed Group  | Linked List After Iteration               |
| --------- | --------------- | ----------------------------------------- |
| 1         | 1 → 2 → 3       | 3 → 2 → 1 → 4 → 5 → 6 → 7 → 8 → 9 → 10    |
| 2         | 4 → 5 → 6       | 3 → 2 → 1 → 6 → 5 → 4 → 7 → 8 → 9 → 10    |
| 3         | 7 → 8 → 9       | 3 → 2 → 1 → 6 → 5 → 4 → 9 → 8 → 7 → 10    |
| 4         | - (1 node left) | Stop; Final Linked List remains unchanged |

---

## **Visual Snapshot of Each Step**

**Before Any Operation:**
`1 → 2 → 3 → 4 → 5 → 6 → 7 → 8 → 9 → 10`

**After 1st Group Reversal:**
`3 → 2 → 1 → 4 → 5 → 6 → 7 → 8 → 9 → 10`

**After 2nd Group Reversal:**
`3 → 2 → 1 → 6 → 5 → 4 → 7 → 8 → 9 → 10`

**After 3rd Group Reversal:**
`3 → 2 → 1 → 6 → 5 → 4 → 9 → 8 → 7 → 10`

---

# **Conclusion**

✅ Each iteration:

* Find k nodes
* Reverse them
* Connect back to the list

✅ Stops when fewer than `k` nodes remain

---
Sourced from https://takeuforward.org/data-structure/reverse-linked-list-in-groups-of-size-k/
![image](https://github.com/user-attachments/assets/af63f00f-1199-446f-81b8-81b985f0aff2)

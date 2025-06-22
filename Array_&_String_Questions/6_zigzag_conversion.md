
# 🧩 Zigzag Conversion – LeetCode #6

## 📝 Problem Descriptions

Given a string `s` and an integer `numRows`, write the string in a zigzag pattern on a given number of rows.

**Example:**

Input:  
`s = "PAYPALISHIRING"`  
`numRows = 3`

Visual Zigzag Pattern:
```

P   A   H   N
A P L S I I G
Y   I   R

````

Output: `"PAHNAPLSIIGYIR"`

---

## ✅ Constraints

- `1 <= s.length <= 1000`
- `1 <= numRows <= 1000`
- `s` consists of English letters (upper/lowercase), `,`, and `.`

---

## 🧠 Approach 1: Matrix-Based Zigzag Simulation

We simulate a zigzag fill using a 2D matrix of size `numRows x len(s)`.

### 📦 Time and Space

- **Time:** O(n)
- **Space:** O(numRows × len(s))

### 🧪 Python Code

```python
class Solution:
    def convert(self, s: str, numRows: int) -> str:
        if numRows == 1 or numRows >= len(s):
            return s

        n = len(s)
        matrix = [['' for _ in range(n)] for _ in range(numRows)]

        i, j = 0, 0  # i: row, j: column
        x = 0        # x: index in original string
        going_down = True

        while x < len(s):
            matrix[i][j] = s[x]
            x += 1

            if going_down:
                if i + 1 < numRows:
                    i += 1
                else:
                    going_down = False
                    i -= 1
                    j += 1
            else:
                if i > 0:
                    i -= 1
                    j += 1
                else:
                    going_down = True
                    i += 1

        result = ''
        for row in matrix:
            result += ''.join([c for c in row if c])

        return result
````

---

## 🚀 Optimized Approach: Row-Wise Zigzag Simulation

Instead of simulating a full matrix, we only track characters row by row.

### ✅ Key Insight

Each character belongs to a specific row based on **zigzag row traversal**: down to the bottom, then up to the top, and so on.

### ✨ Visualization

Input: `"PAYPALISHIRING"`
`numRows = 4`

Zigzag pattern formation:

```
Row 0: P     I     N
Row 1: A   L S   I G
Row 2: Y A   H R
Row 3: P     I
```

Step-by-step row fill trace:

| Step | Char | Row | Direction | Rows State                      |
| ---- | ---- | --- | --------- | ------------------------------- |
| 1    | P    | 0   | ↓         | \['P', '', '', '']              |
| 2    | A    | 1   | ↓         | \['P', 'A', '', '']             |
| 3    | Y    | 2   | ↓         | \['P', 'A', 'Y', '']            |
| 4    | P    | 3   | ↓→↑       | \['P', 'A', 'Y', 'P']           |
| 5    | A    | 2   | ↑         | \['P', 'A', 'YA', 'P']          |
| 6    | L    | 1   | ↑         | \['P', 'AL', 'YA', 'P']         |
| 7    | I    | 0   | ↑→↓       | \['PI', 'AL', 'YA', 'P']        |
| 8    | S    | 1   | ↓         | \['PI', 'ALS', 'YA', 'P']       |
| 9    | H    | 2   | ↓         | \['PI', 'ALS', 'YAH', 'P']      |
| 10   | I    | 3   | ↓→↑       | \['PI', 'ALS', 'YAH', 'PI']     |
| 11   | R    | 2   | ↑         | \['PI', 'ALS', 'YAHR', 'PI']    |
| 12   | I    | 1   | ↑         | \['PI', 'ALSI', 'YAHR', 'PI']   |
| 13   | N    | 0   | ↑→↓       | \['PIN', 'ALSI', 'YAHR', 'PI']  |
| 14   | G    | 1   | ↓         | \['PIN', 'ALSIG', 'YAHR', 'PI'] |

Final Output (join all rows):
**`PINALSIGYAHRPI`**

---
Let’s explain and **visualize the optimized zigzag conversion algorithm** step-by-step for:

1. `s = "PAYPALISHIRING"` and `numRows = 3`
2. `s = "PAYPALISHIRING"` and `numRows = 4`

We'll walk through the logic of this code:

---

### ✅ Optimized Zigzag Code (Recap)

```python
class Solution:
    def convert(self, s: str, numRows: int) -> str:
        if numRows == 1 or numRows >= len(s):
            return s
        
        rows = [''] * numRows
        current_row = 0
        going_down = False

        for char in s:
            rows[current_row] += char
            if current_row == 0 or current_row == numRows - 1:
                going_down = not going_down
            current_row += 1 if going_down else -1

        return ''.join(rows)
```



Let’s explain and **visualize the optimized zigzag conversion algorithm** step-by-step for:

1. `s = "PAYPALISHIRING"` and `numRows = 3`
2. `s = "PAYPALISHIRING"` and `numRows = 4`

We'll walk through the logic of this code:

---
---

### 🔁 Algorithm Explanation

* We maintain a list `rows[]` of size `numRows`, each to hold characters of that row.
* Traverse the input string `s` character by character.
* We use a flag `going_down` to determine whether we are:

  * Moving down from row 0 to `numRows-1`
  * Or going diagonally up from `numRows-1` to 0
* When we reach the top or bottom, we flip the direction.
* Finally, we concatenate all the rows to get the result.

---

## 🔍 Case 1: `s = "PAYPALISHIRING"`, `numRows = 3`

### 💡 Visual Zigzag Pattern

We arrange characters as:

```
Row 0: P   A   H   N  
Row 1: A P L S I I G  
Row 2: Y   I   R
```

### 📊 Step-by-Step Row Assignment

| Char | Row | Rows State                  |
| ---- | --- | --------------------------- |
| P    | 0   | \['P', '', '']              |
| A    | 1   | \['P', 'A', '']             |
| Y    | 2   | \['P', 'A', 'Y']            |
| P    | 1   | \['P', 'AP', 'Y']           |
| A    | 0   | \['PA', 'AP', 'Y']          |
| L    | 1   | \['PA', 'APL', 'Y']         |
| I    | 2   | \['PA', 'APL', 'YI']        |
| S    | 1   | \['PA', 'APLS', 'YI']       |
| H    | 0   | \['PAH', 'APLS', 'YI']      |
| I    | 1   | \['PAH', 'APLSI', 'YI']     |
| R    | 2   | \['PAH', 'APLSI', 'YIR']    |
| I    | 1   | \['PAH', 'APLSII', 'YIR']   |
| N    | 0   | \['PAHN', 'APLSII', 'YIR']  |
| G    | 1   | \['PAHN', 'APLSIIG', 'YIR'] |

### ✅ Final Output:

`"PAHNAPLSIIGYIR"`

---

## 🔍 Case 2: `s = "PAYPALISHIRING"`, `numRows = 4`

### 💡 Visual Zigzag Pattern

```
Row 0: P     I    N  
Row 1: A   L S  I G  
Row 2: Y A   H R  
Row 3: P     I
```

### 📊 Step-by-Step Row Assignment

| Char | Row | Rows State                      |
| ---- | --- | ------------------------------- |
| P    | 0   | \['P', '', '', '']              |
| A    | 1   | \['P', 'A', '', '']             |
| Y    | 2   | \['P', 'A', 'Y', '']            |
| P    | 3   | \['P', 'A', 'Y', 'P']           |
| A    | 2   | \['P', 'A', 'YA', 'P']          |
| L    | 1   | \['P', 'AL', 'YA', 'P']         |
| I    | 0   | \['PI', 'AL', 'YA', 'P']        |
| S    | 1   | \['PI', 'ALS', 'YA', 'P']       |
| H    | 2   | \['PI', 'ALS', 'YAH', 'P']      |
| I    | 3   | \['PI', 'ALS', 'YAH', 'PI']     |
| R    | 2   | \['PI', 'ALS', 'YAHR', 'PI']    |
| I    | 1   | \['PI', 'ALSI', 'YAHR', 'PI']   |
| N    | 0   | \['PIN', 'ALSI', 'YAHR', 'PI']  |
| G    | 1   | \['PIN', 'ALSIG', 'YAHR', 'PI'] |

### ✅ Final Output:

`"PINALSIGYAHRPI"`

---

## 📦 Summary Table

| Input              | numRows | Output             |
| ------------------ | ------- | ------------------ |
| `"PAYPALISHIRING"` | 3       | `"PAHNAPLSIIGYIR"` |
| `"PAYPALISHIRING"` | 4       | `"PINALSIGYAHRPI"` |

---

---

### 🧪 Optimized Python Code

```python
class Solution:
    def convert(self, s: str, numRows: int) -> str:
        # Early return for edge cases
        if numRows == 1 or numRows >= len(s):
            return s

        # Initialize list to hold strings for each row
        rows = ['' for _ in range(numRows)]
        current_row = 0
        going_down = False

        for char in s:
            rows[current_row] += char

            # Flip direction when we reach top or bottom
            if current_row == 0 or current_row == numRows - 1:
                going_down = not going_down

            # Move to next row depending on direction
            current_row += 1 if going_down else -1

        # Concatenate all rows to get the final string
        return ''.join(rows)


```

---

## 💬 Interview Tips

* ✅ Explain how characters move down and then diagonally up
* ✅ State that this avoids building a full matrix
* ✅ Discuss edge cases (`numRows = 1`)
* ✅ Complexity:

  * Time: O(n)
  * Space: O(n)
* 💡 Mention space optimization when compared to matrix solution

---

## 📌 Final Comparison

| Approach           | Time Complexity | Space Complexity | Notes                      |
| ------------------ | --------------- | ---------------- | -------------------------- |
| Matrix Simulation  | O(n)            | O(numRows × n)   | Good for learning          |
| Row-wise Optimized | ✅ O(n)          | ✅ O(n)           | Preferred in interviews 💡 |

---


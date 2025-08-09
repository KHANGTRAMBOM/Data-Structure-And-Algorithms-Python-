# **OVERVIEW**

#### **1. Định nghĩa**

`Stack` (ngăn xếp) là một cấu trúc dữ liệu tuyến tính, hoạt động theo nguyên tắc **LIFO (Last-In, First-Out)**, có nghĩa là "vào sau, ra trước". Phần tử được thêm vào sau cùng sẽ là phần tử được lấy ra đầu tiên.

Bạn có thể hình dung Stack giống như một chồng sách: bạn chỉ có thể đặt thêm một cuốn sách mới lên trên cùng và khi lấy, bạn cũng chỉ có thể lấy quyển sách ở trên cùng ra.


#### **2. Cấu trúc & Các cách cài đặt**

Stack có thể được cài đặt bằng hai phương pháp chính: đó là sử dụng mảng và Link List. Nhưng trong bài `notebook` này tôi sẽ chú tâm vào cách cài đặt bằng `Link List`

*   **Sử dụng Danh sách liên kết (Linked List):**
    *   Mỗi phần tử là một `node` trong danh sách liên kết.
    *   Thao tác `Push` tương ứng với việc thêm một node vào đầu danh sách.
    *   Thao tác `Pop` tương ứng với việc xóa node ở đầu danh sách.
    *   Thao tác `Peek` để lấy giá trị đầu tiên ra tương tự như `get(0)`

#### **3. Ứng dụng**

Stack có rất nhiều ứng dụng quan trọng và quen thuộc trong khoa học máy tính:

* **Gọi hàm đệ quy (Call Stack)** ta sẽ thường thấy thông báo *Stack Overflow* từ IDE khi chúng ta lập vô tận hay đệ quy không điểm dừng.

*   **Các thuật toán duyệt đồ thị và cây:** Thuật toán Tìm kiếm theo chiều sâu (Depth-First Search - DFS) sử dụng stack để lưu trữ các đỉnh sẽ được duyệt.


> 📌 **Lưu ý về "Stack Overflow"**:
> Tên gọi "Stack Overflow" không chỉ là một lỗi lập trình mà còn là tên của một trang web hỏi đáp cực kỳ nổi tiếng dành cho lập trình viên. Khi bạn gặp lỗi này, rất có thể bạn sẽ tìm thấy câu trả lời trên chính trang web cùng tên.


[Video tham khảo](https://www.youtube.com/watch?v=KcT3aVgrrpU)

---

# **CODE**
Trong bài `notebook` này tôi cũng sẽ xây dựng lại cấu trúc `Stack` dựa vào `Link List` bằng `Python` mà không sử dụng bất kì thư viện hổ trợ nào. Do `notebook` chỉ mang tính học tập và tham khảo nếu có sai sót mong các bạn góp ý và thông cảm bỏ qua 😌😌😌


```python
class Node:
  def __init__(self, val: int, next = None):
    self.val = val
    self.next = next
```


```python
class Stack:
  def __init__(self):
    self.head = None

  def isEmpty(self) -> bool
    return head == None

  def push(self ,val : int) -> None:
    new_node = Node(val, self.head)
    self.head = new_node

  def pop(self) -> None:
    if not self.isEmpty():
      self.head = self.head.next

  def peek(self) -> int:
    if not self.isEmpty():
      return self.head.val

  def toString(self):
    head = self.head
    while head is not None:
      print(f"{head.val} ", end ="")
      head = head.next
    print()
```


```python
# Test case chức năng push
myStack = Stack()
myStack.push(1)
myStack.push(2)
myStack.push(3)
myStack.push(4)
myStack.push(5)
myStack.toString()
```

    5 4 3 2 1 
    


```python
# Test case chức năng pop
myStack.pop()
myStack.pop()
myStack.toString()
```

    3 2 1 
    


```python
# Test case chức năng peek
print(f"Giá trị hiện tại top hiện tại của Stack là: {myStack.peek()}")
```

    Giá trị hiện tại top hiện tại của Stack là: 3
    

# **MỘT SỐ BÀI TẬP RÈN LUYỆN**
Cúng giống với các bài `notebook` trước về `DSA` thì trong kì này cũng sẽ có một số bài tập mà có thể nói là `đỉnh cao` các bài này cũng được lấy từ nguồn`Leetcode`. Ngoài ra do thuật toán này cũng được sử dụng trong `Theory Graph` (Lý thuyết đồ thị) mà sẽ có ở những series về chúng sau nên tôi cũng không đưa nó vào bài `notebook` này mong các bạn thông cảm 😜😜😜😜 .

## [LEETCODE 20. Valid Parentheses](https://leetcode.com/problems/valid-parentheses/description/) **`EASY`**

Given a string s containing just the characters '(', ')', '{', '}', '[' and ']', determine if the input string is valid.

An input string is valid if:

1. Open brackets must be closed by the same type of brackets.
2. Open brackets must be closed in the correct order.
3. Every close bracket has a corresponding open bracket of the same type.


```python
### Đối với bài tập này chúng ta chỉ tiến hành kiễm có thể thể tạo thành một cặp () {} [] khi gặp dấu đóng ) } ]
### lúc đó ta sẽ lấy thằng top hiện tại trong stack rồi tiến hành so sánh nếu nó khác nhau thì trả về False, còn
### nếu gặp dấu mở ( { [ thì ta chỉ đơn giản là đưa vào stack

class Solution:
    def isValid(self, s: str) -> bool:
        stack = []
        output = s
        # Duyệt
        for s in output:
            # Nếu gặp các dấu đóng thì tiến hành kiểm tra
            if s == ')' or s == '}' or s == "]":
                # Nếu bắt đâu đã là đóng thì trả về False
                if not stack:
                    return False
                else:
                # Lấy thằng top hiện tại của stack nếu nó không trùng thì return False
                    cur = stack.pop()
                    if s == ')' and cur != '(':
                        return False
                    elif s == '}' and cur != '{':
                        return False
                    elif s == ']' and cur != '[':
                        return False
            else:
               # Nếu là dấu mở thì đưa vào stack
                stack.append(s)
        # Nếu stack chưa rỗng có nghĩa là dấu mở nào đó không có dấu đóng ví dụ
        # str = "{([])"
        return not stack

```

## [LEETCODE 844. Backspace String Compare](https://leetcode.com/problems/backspace-string-compare/description/) **`EASY`**
Given two strings s and t, return true if they are equal when both are typed into empty text editors. '#' means a backspace character.

Note that after backspacing an empty text, the text will continue empty.


```python
### Đối với bài tập này cũng tương tự với bài trên ta cũng sẽ dùng stack làm chủ đạo
### khi gặp kí hiệu backspace '#' thì ta sẽ pop đi phần từ top hiện tại trong stack
### nếu gặp các kí tự khác thì đơn giản chỉ thêm kí tự đó vào stack thôi

class Solution:
    # Hàm hổ trợ
    def helper(self, s: str)-> str:
        stack = []
        # Duyệt toàn bộ chuổi
        for c in s:
            # Gặp kí hiệu '#' thì pop khỏi stack
            if c == '#':
                if len(stack) > 0:
                    stack.pop()
            # Nếu không thì thêm vào stack
            else:
                stack.append(c)

        return "".join(stack)
    def backspaceCompare(self, s: str, t: str) -> bool:
        return self.helper(s) ==  self.helper(t)

```

## [LEETCODE 682. Baseball Game](https://leetcode.com/problems/baseball-game/description/) **`EASY`**
You are keeping the scores for a baseball game with strange rules. At the beginning of the game, you start with an empty record.

You are given a list of strings operations, where operations[i] is the ith operation you must apply to the record and is one of the following:

1. An integer x.
Record a new score of x.
2. '+'.
Record a new score that is the sum of the previous two scores.
3. 'D'.
Record a new score that is the double of the previous score.
4. 'C'.
Invalidate the previous score, removing it from the record.
Return the sum of all the scores on the record after applying all the operations.

The test cases are generated such that the answer and all intermediate calculations fit in a 32-bit integer and that all operations are valid.


```python
### Bài này có các quy tắc như sau:
###   1. Nếu là số nguyên x thì cộng x điễm
###   2. '+' thì cộng vào tổng 2 của 2 điễm trước đó ví dụ [1,2,3] -> [1,2,3,2+3]
###   3. 'D' sẽ gắp đôi điễm vừa được ghi
###   4. 'C' điễm trước đó không hợp lệ -> xóa nó khỏi stack

### Với đề bài trên ta vẫn cũng sẽ sử dụng stack với logic như 2 bài trên ta cũng sẽ duyệt mảng
### rồi so sánh nó thuộc trường hợp nào rồi xử lý theo logic theo đề bài, sau khi xây dựng được
### stack theo đề bài ta tiến hành tính tổng của stack đó

class Solution:
    def calPoints(self, operations: List[str]) -> int:
        stack = []
        # Duyệt mảng
        for command in operations:
            # Xét theo từng trường hơpk
            if command == 'C':
                stack.pop()
            elif command == 'D':
                # Gấp đôi phần tử mới nhất
                top = stack[-1]
                stack.append(top * 2)
            elif command == '+':
                # Tổng 2 phần tử trước đó
                total = stack[-1] + stack[-2]
                stack.append(total)
            else:
                num = int(command)
                stack.append(num)
        # Tính tổng các phần tử trong stack
        return sum(stack)
```

## [LEETCODE 71. Simplify Path](https://leetcode.com/problems/simplify-path/description/) **`MEDIUM`**

You are given an absolute path for a Unix-style file system, which always begins with a slash '/'. Your task is to transform this absolute path into its simplified canonical path.

The rules of a Unix-style file system are as follows:

A single period '.' represents the current directory.
A double period '..' represents the previous/parent directory.
Multiple consecutive slashes such as '//' and '///' are treated as a single slash '/'.
Any sequence of periods that does not match the rules above should be treated as a valid directory or file name. For example, '...' and '....' are valid directory or file names.
The simplified canonical path should follow these rules:

The path must start with a single slash '/'.
Directories within the path must be separated by exactly one slash '/'.
The path must not end with a slash '/', unless it is the root directory.
The path must not have any single or double periods ('.' and '..') used to denote current or parent directories.
Return the simplified canonical path.


```python
### Bài tập này yêu cầu chung ta đưa ra đường dẫn (path) chuẩn dựa vào query (str).
### Ta sẽ đưa các thư mục cần duyệt trong str vào stack khi đó nếu ta gặp ../
### (về thư mục trước) thì đơn giản chỉ la pop() nó ra khỏi stack thôi


class Solution:
    def simplifyPath(self, path: str) -> str:
        # Hàm hổ trợ để lấy các tên thư mục giả sử str = "/home/user"
        # thì step = ['home','user']
        step = path.split('/')
        stack = []

        # Tiến hành duyệt
        for s in step:
            # Nếu gặp các dấu '.' hay "" thì đơn giản là bước tới lần lặp tiếp theo
            if s == "." or s == "":
                continue
            # Gặp dấu trở về thư mục trước '..' thì pop thằng top hiện tại của stack
            elif s == "..":
                if len(stack) > 0:
                    stack.pop()
            else:
                stack.append(s)

        result = "/"

        return result + result.join(stack)
```

## [LEETCODE 224. Basic Calculator](https://leetcode.com/problems/basic-calculator/description/) **`HARD`**

Given a string s representing a valid expression, implement a basic calculator to evaluate it, and return the result of the evaluation.

Note: You are not allowed to use any built-in function which evaluates strings as mathematical expressions, such as eval().


```python
### Đây là bài toán HARD đầu tiên trong các bài notebook của tôi về cấu trúc dữ liệu và giải thuật
### Lý do tôi chọn nó là vì tính thực tế của nó, ta có thể kết hợp với một số framword app của Java,
### C# hay Python tạo ra phần mềm máy tính cầm tay, một project nho nhỏ, mặc dù chỉ có 2 phép tính '+'
### và '-' nhưng nó sẽ giúp ích bạn sau này rất nhiều, và bạn có thể mở rộng thêm '*' và '/'


### Phương hướng tiếp cận: Bài này có độ khó HARD nên cũng sẽ hơi phức tạp, đầu tiên ta có thể dễ dàng
### đưa các con số dương vào một cách dễ dàng nhưng làm cách nào để lấy ra số âm khi đầu vào là chuỗi
### và sẽ như thế nào khi nó là một chuỗi biểu thức ví dụ: "(1+(4+5+2)-3)+(6+8)".
### - Đầu tiên ta sẽ duyệt mảng, nếu gặp con số ta sẽ lưu nó vào cur_num (ban đầu mặc định dấu của con số
### sẽ là dương).
### - Tiếp theo nếu ta duyệt trúng '+' hoặc '-' thì ta sẽ tiến hành thực hiện phép tính như
### sau total += cur_num * sign (với sign ban đầu là sign = 1). Sau khi gán vào total ta cho cur_num = 0 và
### gắn sign = 1 nếu gặp '+' và sign = '-1' nếu gặp '-'.
### - Cuối cùng là '(' và ')'. Khi gặp '(' ta sẽ tiến hành đệ quy để tính tổng trong ngoặc '()' và khi duyệt
### và gặp ')' (lúc này đang trong đệ quy) ta sẽ return luôn giá trị mà trong ngoặc '()' tính được


class Solution:
    def helper(self, s , i):
        sign = 1
        total = 0
        cur_num = 0
        while i < len(s):
            # Gặp dấu ')' kết thúc đệ quy trong ngoặc trả về kết quả đệ quy
            if s[i] == ')':
                total += cur_num
                # Trả về kèm theo i để duyệt tiếp phần tử sau ')' tránh duyệt
                # lặp lại nhiều lần
                return total, i
            # Gặp dấu '+' cộng cur_num vào total đồng thời gán sign = 1
            elif s[i] == '+':
                total += cur_num
                cur_num = 0
                sign = 1
            # Gặp dấu '-' cộng cur_num vào total đồng thời gán sign = -1
            elif s[i] == '-':
                total += cur_num
                cur_num = 0
                sign = -1
            # Gặp dấu '(' tiến hành đệ quy để tính giá trị trong '()'
            elif s[i] == '(':
                cur_num, i = self.helper(s , i + 1)
                cur_num *= sign
                total += cur_num
                cur_num = 0
            # Gặp số thì gán cho cur_num
            elif s[i] >= '0' and s[i] <= '9':
                cur_num = cur_num * 10 + (int(s[i]) * sign)

            i += 1
        # Giả sử cur_num != 0 có nghĩa là vẫn còn số chưa được cộng
        # ví dụ s = "1 + 1" khi gặp con số 1 cuối nó sẽ gán vào cur_num rồi
        # kết thúc vòng lập luôn nên cần phải cộng vào để tránh bỏ sót
        total += cur_num

        return total, i

    def calculate(self, s: str) -> int:
        total, _ = self.helper(s , 0)
        return total


### Cũng có thể dùng cách này
class Solution:
    def calculate(self, s: str) -> int:
        stack = []
        cur_num = 0
        sign = 1
        total = 0

        for c in s:
            if c >= '0' and c <= '9':
                cur_num = cur_num*10 + int(c)
            elif c == '+':
                total += cur_num * sign
                sign = 1
                cur_num = 0
            elif c == '-':
                total += cur_num * sign
                sign = -1
                cur_num = 0
            elif c == '(':
                stack.append(total)
                stack.append(sign)
                sign = 1
                total = 0
            elif c == ')':
                total += cur_num * sign
                total *= stack.pop()
                total += stack.pop()
                cur_num = 0

        return total + cur_num * sign
```

## [LEETCODE 227. Basic Calculator II](https://leetcode.com/problems/basic-calculator-ii/description/) **`MEDIUM`**

Given a string s which represents an expression, evaluate this expression and return its value.

The integer division should truncate toward zero.

You may assume that the given expression is always valid. All intermediate results will be in the range of [-231, 231 - 1].

Note: You are not allowed to use any built-in function which evaluates strings as mathematical expressions, such as eval().


```python
### Đối với bài này thì hướng tiếp cận cũng giống như bài trên, chỉ có khác là chúng ta sẽ mở rộng sign ra là
### '+' '-' '*' '/' chứ không chỉ có '+' và trừ '-'. Ban đầu ta cho sign = '+' và tiến hành duyệt, nếu gặp số
### thì gán nó vào biến num, còn gặp phép tình thì ta sẽ đưa nó vào stack tương ứng với sign (sign sẽ đại diện
### cho dấu của con số phía trước). Còn đối với trường hợp '*' và '/' sẽ hơi đặc biệt chút xíu, ta không thể
### stack.push(num) như vậy được tại vì nó là phép nhân hoặc chia nên nó cần 2 con số để thực hiện cho nên
### chúng ta sẽ lấy phần tử top ra rồi mới thực hiện phép tính stack.push(stack.top *|/ num). Rồi cuối cùng là
### tính tổng các phần từ có trong stack

class Solution:
    def calculate(self, s: str) -> int:
        # Thêm '+' vào cuối chuỗi để bớt xử lý trường hợp num != 0
        s += "+"
        stack = []
        sign = '+'
        num = 0
        for c in s:
            # Nếu duyệt là số thì gán vao num
            if c.isdigit():
              num = num * 10 + int(c)
             # Nếu duyệt gặp dấu thì tiến hành đưa vào stack theo từng
             # trường hợp
            elif c in ['+', '-' , '*', '/']:
                if sign == '+':
                    stack.append(num)
                elif sign == '-':
                    stack.append(-num)
                elif sign == '*':
                    top = stack.pop()
                    stack.append(top * num)
                else:
                    top = stack.pop()
                    stack.append(int(top / num))
                # Gán lại để tính ở lần tiếp theo
                num = 0
                sign = c
        # Trả về tổng các giá trị trong stack
        return sum(stack)
```

# TADA HẾT RỒI !!! 🥳🥳🥳🥳

Cảm ơn các bạn đã đọc hết bài `notebook` này, mong các bạn góp ý và ủng hộ mình trong các bài `notebook` tiếp theo về `DSA` nhé 😌😌😌

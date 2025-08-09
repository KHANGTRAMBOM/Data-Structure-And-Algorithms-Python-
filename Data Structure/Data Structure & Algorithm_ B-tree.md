# **OVERVIEW**

##  Vấn đề của Binary Search Tree (BST)

- Cây nhị phân tìm kiếm (BST) có thể bị **mất cân bằng** giữa hai nhánh trái và phải.
- Khi số lượng node lớn, cây lệch sẽ khiến các thao tác như:
  - **Tìm kiếm**
  - **Thêm**
  - **Xóa**  
  trở nên **kém hiệu quả** (độ phức tạp có thể là `O(n)` thay vì `O(log n)`).

##  Sự ra đời của B-Tree

- B-Tree (**Balanced Tree**) được đề xuất nhằm **giữ cho cây luôn cân bằng**.
- Đặc điểm nổi bật:
  - Mỗi node có thể chứa **nhiều key** và **nhiều node con**.
  - Khi node đầy, B-Tree sẽ **tự động chia node** và **đẩy key ở giữa lên** node cha.
  - Khi xóa và thiếu key, B-Tree sẽ **tự động mượn hoặc gộp node lân cận** để giữ tính cân bằng.
- Những đặc tính này giúp B-Tree trở thành một trong những **cấu trúc dữ liệu được ứng dụng rộng rãi nhất**, đặc biệt trong các **hệ quản trị cơ sở dữ liệu (CSDL)** và **hệ thống lưu trữ ngoài**.

##  Ứng dụng của B-Tree

- B-Tree được ứng dụng phổ biến trong:
  - **Cơ sở dữ liệu (Database Indexing)**
  - **Hệ thống file (Filesystem)**
  - **Các thuật toán xử lý bộ nhớ ngoài (External Memory Algorithms)**
- Do mỗi node chứa nhiều key, B-Tree giúp **giảm số lần truy xuất I/O**, điều này rất quan trọng trong các hệ thống có dữ liệu lớn không thể lưu hết trong RAM.

[Video tham khảo](https://www.youtube.com/watch?v=K1a2Bk8NrYQ)

<hr>

# **B-TREE**

##  Định nghĩa

B-Tree là một cấu trúc dữ liệu dạng cây được lấy cảm hứng từ **cây nhị phân**, với mục tiêu đảm bảo **sự cân bằng về chiều cao** giữa các nhánh. Cấu trúc này đặc biệt hữu ích trong việc lưu trữ và truy vấn dữ liệu lớn. B-Tree có các đặc điểm nổi bật sau:

- Mỗi node (trừ node gốc) chứa:
  - Tối đa `t - 1` khóa (key)
  - Tối thiểu `ceil(t / 2) - 1` khóa

- Các key trong mỗi node được **sắp xếp tăng dần** từ trái sang phải.

- Với mỗi key trong node:
  - Nhánh **bên trái** (`key.left`) chứa các key **nhỏ hơn**
  - Nhánh **bên phải** (`key.right`) chứa các key **lớn hơn**

- Mỗi node (trừ node gốc) có:
  - Tối đa `t` con (node con)
  - Tối thiểu `ceil(t / 2)` con

>  Chính những đặc điểm trên giúp B-Tree duy trì **độ cao cân bằng** giữa hai phía trái và phải, từ đó **tối ưu thời gian và bộ nhớ** cho các thao tác như:
> - **Thêm (Insert)**
> - **Xóa (Delete)**
> - **Tìm kiếm (Search)**


# **CÁC THAO TÁC TRÊN B-TREE**

## **Thêm (Insert)**

Thao tác thêm vào B-Tree có thể `phức tạp` và `khó hiểu`, đặc biệt là khi cần phải `tách node` hoặc `đẩy giá trị lên trên`. Dưới đây là các bước tổng quát dựa trên lý thuyết và minh họa từ bài viết:

---

- **BƯỚC 1:** Tìm `node lá phù hợp` để chèn `key` vào. Việc chèn luôn theo `thứ tự tăng dần` từ trái sang phải.

- **BƯỚC 2:** Thêm key vào node đó.

- **BƯỚC 3:** Kiểm tra `số lượng key` trong node đó có vượt quá `max_key` không.

    - Nếu chưa vượt thì **BƯỚC 5**.
    - Nếu vượt thì **BƯỚC 4**.

- **BƯỚC 4:** Tiến hành `tách node` (split)

    - **4.1:** Tìm `median` (giá trị chính giữa) của node hiện tại.
    
    - **4.2:** Tạo 2 node mới: `left_node` chứa các key bên trái median, và `right_node` chứa các key bên phải.
    
    - **4.3:** Đưa `median` lên node cha (`parent`). Nếu node hiện tại là root thì tạo node root mới.
    
    - **4.4:** Gán `left_node` và `right_node` làm con của node cha tại vị trí liên quan tới median vừa được đẩy lên.
    
    - **4.5:** Nếu node cha hiện tại cũng vượt quá `max_key`, thực hiện đệ quy việc split lên trên (lặp lại từ bước 3 cho node cha).

- **BƯỚC 5:** Kết thúc quá trình thêm.

> Ghi chú: Một B-Tree luôn đảm bảo tính cân bằng nhờ việc "đẩy median lên" và chia nhỏ khi node đầy.


---

## **Xóa (Delete)**

Thao tác `xóa` là một trong những thao tác `khó nhất` trong B-Tree vì có nhiều `trường hợp đặc biệt`. Để dễ tiếp cận, ta chia ra làm 2 **trường hợp lớn**:

---

### **1. Xóa key trong `node lá`**

#### 1.1 `Node lá` chứa số lượng key > `min_key` (còn dư)

- Chỉ cần xóa key đó trực tiếp, đồng thời giữ thứ tự tăng dần trong node.



#### 1.2 `Node lá` chứa số lượng key == `min_key` (vừa đủ)

- Phải kiểm tra `siblings` (node anh em kế bên – trái/phải):


##### a. Siblings còn dư (`length > min_key`):

- Mượn 1 key từ sibling (thường là key cuối/trước) và đẩy key từ parent xuống thay thế, đảm bảo đúng thứ tự.



##### b. Siblings không còn dư:

- Tiến hành `merge` với sibling và đẩy key từ `parent` xuống giữa 2 node để tạo thành 1 node mới.

    - Nếu node `parent` sau khi mất key còn đủ key thì không cần làm thêm gì.
    
    - Nếu node `parent` cũng chỉ có `min_key` → có thể dẫn đến việc merge tiếp tục lan lên cao → tái cân bằng đệ quy.


---

### **2. Xóa key trong `node không phải lá`**

- Ta cần tìm **key thay thế**:

    - Có thể là **key lớn nhất bên trái** (tiền nhiệm – predecessor) hoặc **key nhỏ nhất bên phải** (kế nhiệm – successor).

- Thay key cần xóa bằng key thay thế.

- Sau đó xóa key thay thế ở `node lá` (vì key thay thế chắc chắn sẽ nằm ở node lá), quay lại xử lý như **Trường hợp 1**.



---


Nguồn tham khảo:  
🔗 [How to Build a B-Tree in JavaScript](https://levelup.gitconnected.com/building-a-b-tree-in-javascript-4482dee083cb)


# **CODE**



Trong bài `notebook` này, tôi sẽ tiến hành **xây dựng lại cấu trúc cây B-Tree từ đầu (from scratch)** — **không sử dụng bất kỳ thư viện hỗ trợ nào** liên quan đến B-Tree hay đoạn mã sinh tự động từ AI. Ngoài trừ hàm liên quan đến `toString()` để có thể in ra cho dễ quan sát hơn

Ngôn ngữ được sử dụng là **Python**, một ngôn ngữ hiện đại, mạnh mẽ với cú pháp đơn giản, dễ hiểu, rất phù hợp để xây dựng các cấu trúc dữ liệu như B-Tree.

>  **Phạm vi hiện tại của chương trình:**
> - Hoàn thiện các chức năng **thêm (insert)** và **xóa (delete)** khi **key nằm ở node lá**.
> - Việc xóa chưa xử lý hết tất cả các trường hợp phức tạp — cụ thể là **chưa xử lý việc mượn/gộp node** khi số lượng key nhỏ hơn `min_key` ở node cha, hay xóa key ở node cha

Do **tài hèn sức mọn 😅**, tôi chỉ mới xây dựng được phần lõi cơ bản. Rất mong nhận được sự **góp ý và phản hồi** từ mọi người để hoàn thiện hơn nữa!



```python
# Khởi tạo node cơ bản
class Node:
  def __init__(self, val):
     self.val = val
     self.left = None
     self.right = None
```


```python
# Khởi tạo B-tree node
class B_tree_node:
  # Hàm khởi tạo
  def __init__(self , b_size):
    self.list_node = [Node(0) for _ in range(b_size)]
    self.length = 0

  def __move_key(self, start, end, step):
    if step > 0:  # Di chuyển sang phải (cho delete)
        for i in range(start, end, step):
            self.list_node[i] = self.list_node[i + step]
    else:  # Di chuyển sang trái (cho insert)
        for i in range(start, end, step):
            self.list_node[i] = self.list_node[i - step]

  def sreach_key(self, val: int) -> int:

    for i in range(self.length):
      if self.list_node[i].val == val:
        return i

    return -1

  # Hàm thêm basic node vào b-tree node
  def add(self, node: Node):
    index = 0

    while index < self.length and self.list_node[index].val < node.val:
        index += 1

    self.length += 1

    # Di chuyển các phần tử
    for i in range(self.length - 1, index, -1):
        self.list_node[i] = self.list_node[i - 1]

    self.list_node[index] = node

    # Chỉ duy chuyển các liên kết khi length > 1
    if self.length > 1:
      # Nếu chèn vào đầu
      if index == 0:
          self.list_node[index + 1].left = self.list_node[index].right

      # Nếu chèn vào cuối
      elif index == self.length - 1:
          self.list_node[index - 1].right = self.list_node[index].left

      # Nếu chèn vào giữa
      else:
          self.list_node[index - 1].right = self.list_node[index].left
          self.list_node[index + 1].left = self.list_node[index].right


  def delete(self, node: Node):

    # Tìm vị trí chứa key cần xóa
    remove_index = self.sreach_key(node.val)

    if remove_index != -1:

        self.__move_key(remove_index, self.length - 1, 1)

        # Tìm vị trí chứa key cần xóa là ở giữa cần phải điều chỉnh liên kết
        if remove_index > 0 and remove_index < self.length - 1:

          self.list_node[remove_index - 1].right = self.list_node[remove_index].left

        self.length -= 1

  def get(self, index):
    if index < self.length:
      return self.list_node[index]


  def isLeaf(self):
    if self.length >= 0 and self.list_node[0].left is None:
      return True
    return False


  def toString(self):
    for i in range(self.length):
      print(self.list_node[i].val, end=" ")

    print()

```


```python
import math
from collections import deque
from graphviz import Digraph

# Tạo class b-tree
class B_tree:
  # Hàm khởi tạo
  def __init__(self, b_size):
    self.b_size = b_size
    self.root = B_tree_node(self.b_size)
    self.max_key = self.b_size - 1
    self.min_key = math.ceil(self.b_size / 2) - 1

    self.isLeaf = True


  # Hàm tách khi b-tree node đã đầy
  def Split(self, parent, child):
    mid_index = child.length // 2
    middle_key = Node(child.get(mid_index).val)  # Tạo Node mới

    left_node = B_tree_node(self.b_size)
    right_node = B_tree_node(self.b_size)

    # Chia các key sang trái/phải
    for i in range(child.length):
        if i < mid_index:
            left_node.add(child.get(i))
        elif i > mid_index:
            right_node.add(child.get(i))

    # Gắn các con cho key giữa
    middle_key.left = left_node
    middle_key.right = right_node

    parent.add(middle_key)

  # Hàm tìm b_tree_node phù hợp để tiến hành thêm
  def find_leaf_node(self, root: B_tree_node, val: int) -> B_tree_node:
    if root is None:
      return None

    # Trường hợp node lá
    if root.isLeaf():
        return root

    for i in range(root.length):
        if val < root.list_node[i].val:
            return self.find_leaf_node(root.list_node[i].left, val)

    return self.find_leaf_node(root.get(root.length - 1).right, val)

  # Hàm tìm node cha
  def find_parent_node(self, start_node, child_node, child_key) -> B_tree_node:

    if start_node is None or start_node.isLeaf():
        return None

    key_index_in_parent = -1
    for i in range(start_node.length):
       # Truy tìm đúng đường đi
       if child_key.val < start_node.get(i).val:
          key_index_in_parent = i
          break

    # Tìm được đường đi ở phần node.left
    if key_index_in_parent != -1:
      if start_node.get(key_index_in_parent).left == child_node:
        return start_node
      return self.find_parent_node(start_node.get(key_index_in_parent).left, child_node, child_key)
    else:
     if start_node.get(start_node.length - 1).right == child_node:
        return start_node

    return self.find_parent_node(start_node.get(start_node.length - 1).right, child_node, child_key)


  # Hàm thêm (cập nhật thêm là tìm chổ phù hợp để add vào)
  def add(self, val):
    new_node = Node(val)
    leaf_node = self.find_leaf_node(self.root, val)
    leaf_node.add(new_node)

    # Nếu node đã đầy key
    while leaf_node.length == self.b_size:
        # Nếu node đầy là gốc
        if leaf_node == self.root:
            new_root = B_tree_node(self.b_size)
            self.Split(new_root, leaf_node)
            self.root = new_root
            break
        else:
            # Tìm cha để thực hiện phân tách
            parent_node = self.find_parent_node(self.root, leaf_node,new_node)
            self.Split(parent_node, leaf_node)
            leaf_node = parent_node

  def find_largest_left(self, root):
    # Trường hợp root là none
    if root is None:
      return None

    # Trường hợp root hiện tại không có node lá
    if root.length > 0 and root.get(0).left is None:
      return root

    left_part = root.get(0).left

    while left_part.get(left_part.length -1).right != None:
      left_part = left_part.get(left_part.length -1).right

    return left_part


  def find_smallest_right(self, root):
     # Trường hợp root là none
    if root is None:
      return None

    # Trường hợp root hiện tại không có node lá
    if root.length > 0 and root.get(0).left is None:
      return root # Đây là b-node chứa giá trị lớn nhất bên phải

    right_part = root.get(root.length - 1).right

    while right_part.get(0).left != None:
      right_part = right_part.get(0).left

    return right_part

  # Hàm merge 2 b-tree node lại với nhau
  def merge(self, parent: B_tree_node, key_index: int, left_node: B_tree_node, right_node: B_tree_node):
    new_node = B_tree_node(self.b_size)

    for i in range(left_node.length):
        new_node.add(left_node.get(i))

    parent_key = parent.get(key_index)
    parent_key_as_node = Node(parent_key.val)

    new_node.add(parent_key_as_node)

    for i in range(right_node.length):
        new_node.add(right_node.get(i))

    # Chỉnh lại con trỏ cho phù hợp
    for i in range(new_node.length - 1):
        new_node.get(i).right = new_node.get(i + 1).left

    return new_node

  # Hàm tìm sibling khi xóa
  def find_siblings_and_key_index(self, parent: B_tree_node, child: Node):

        left_sibling = None

        right_sibling = None

        # Tìm vị trí của child trong các con trỏ của parent
        for i in range(parent.length + 1):

          if i == 0:
            current_b_node = parent.get(i).left
          else:
            current_b_node = parent.get(i - 1).right


          if current_b_node == child:
             if i == 0:
                right_sibling = parent.get(i).right

             elif i == parent.length :
                left_sibling = parent.get(i -1).left

             else:
                left_sibling = parent.get(i - 1).left
                right_sibling = parent.get(i).right

             key_index_in_parent = i if i < parent.length  else i - 1
             return left_sibling, right_sibling , key_index_in_parent

        return None, None, -1

  def borrow_from_left(self, parent, current_node, left_sibling, key_index):

        # Key từ cha đi xuống node hiện tại

        key_from_parent = parent.get(key_index)

        current_node.add(Node(key_from_parent.val))

        # Key lớn nhất từ anh em trái đi lên thay thế cha

        key_from_left = left_sibling.get(left_sibling.length - 1)

        parent.get(key_index).val = key_from_left.val

        # Xóa key đã cho mượn ở anh em trái

        left_sibling.delete(key_from_left)

  def borrow_from_right(self, parent, current_node, right_sibling, key_index):

        # Key từ cha đi xuống node hiện tại
        key_from_parent = parent.get(key_index)

        current_node.add(Node(key_from_parent.val))

        # Key nhỏ nhất từ anh em phải đi lên thay thế cha
        key_from_right = right_sibling.get(0)

        parent.get(key_index).val = key_from_right.val

        # Xóa key đã cho mượn ở anh em phải
        right_sibling.delete(key_from_right)

  # Hàm xóa
  def delete(self, val: int):

    deleted_key = Node(val)

    deleted_node = self.find_leaf_node(self.root, val)

    # Trường hợp xóa node lá
    if deleted_node.isLeaf():

      parent_node = self.find_parent_node(self.root, deleted_node ,deleted_key)

      deleted_node.delete(deleted_key)

      current_node = deleted_node

      # Trường hợp sao khi xóa, không đủ số lượng key tối thiểu
      while current_node.length < self.min_key and current_node != self.root:

        # CASE 1: Có thể mượn key từ sibling
        left_sibling, right_sibling , key_parent_index = self.find_siblings_and_key_index(parent_node, deleted_node)

        # Dựa vào thuật toán của hàm find_siblings_and_key_index thì chỉ đúng với trường hợp chỉ được mượn một phía
        # Còn mượn 2 phía thì left_part_index = key_parent_index - 1

        # Trường hợp mượn 2 phía
        if left_sibling != None and right_sibling != None and (left_sibling.length > self.min_key or right_sibling.length > self.min_key):
          if left_sibling.length > self.min_key:
                self.borrow_from_left(parent_node, deleted_node, left_sibling, key_parent_index - 1)
          elif right_sibling.length > self.min_key:
                self.borrow_from_right(parent_node, deleted_node, right_sibling, key_parent_index)

          # Trường hợp chỉ mượn được bên trái
        elif left_sibling != None and left_sibling.length > self.min_key:
          self.borrow_from_left(parent_node, deleted_node, left_sibling, key_parent_index)

          # Trường hợp chỉ mượn được bên phải
        elif right_sibling != None and right_sibling.length > self.min_key:
          self.borrow_from_right(parent_node, deleted_node, right_sibling, key_parent_index)

        # CASE 2: Không thể mượn từ sibling --> tiến hành merge
        # CASE 2.1: Parent có dư key để merge
        elif parent_node.length > self.min_key:

          if left_sibling != None and right_sibling is None:
              # Trường hợp key xóa là node cuối
              merge_node = self.merge(parent_node, key_parent_index , left_sibling, current_node)

              parent_node.get(key_parent_index - 1).right = merge_node

              deleted_parent_key = parent_node.get(key_parent_index)

              parent_node.delete(deleted_parent_key)

          elif right_sibling != None and left_sibling is None:

                # Trường hợp key xóa là node đầu
              merge_node = self.merge(parent_node, key_parent_index , current_node, right_sibling)

              parent_node.get(key_parent_index + 1).left = merge_node

              deleted_parent_key = parent_node.get(key_parent_index)

              parent_node.delete(deleted_parent_key)

          else:
                # Trường hợp key xóa là node giữa
              merge_node = self.merge(parent_node, key_parent_index , current_node, right_sibling)

              parent_node.get(key_parent_index - 1).right = merge_node

              deleted_parent_key = parent_node.get(key_parent_index)

              parent_node.delete(deleted_parent_key)

        # CASE 2.2: Node parent không đủ key
        elif parent_node.length <= self.min_key:
          # Key xóa là node cuối
          if left_sibling != None and right_sibling is None:
            merge_node = self.merge(parent_node, key_parent_index , left_sibling, current_node)

          # Key xóa là node đầu
          elif right_sibling != None and left_sibling is None:
             merge_node = self.merge(parent_node, key_parent_index , current_node, right_sibling)

          if parent_node == self.root:
            self.root = merge_node
            parent_node = merge_node

        current_node = parent_node
    # Trường hợp xóa node cha
    else:
      None

  def toString(self):

    if not self.root or self.root.length == 0:
        print("Cây rỗng")
        return

    lines, *_ = self._draw_tree(self.root)
    for line in lines:
        print(line)

  def _draw_tree(self, node):

    # Tạo label cho node hiện tại
    values = [str(node.list_node[i].val) for i in range(node.length)]
    label = "[" + ",".join(values) + "]"
    label_width = len(label)

    # Trường hợp node lá - không có node con
    if self._is_leaf_node(node):
        return [label], label_width, 1, label_width // 2

    # Thu thập tất cả node con
    children = self._get_child_nodes(node)

    # Vẽ đệ quy các cây con
    child_drawings = [self._draw_tree(child) for child in children]

    # Chuẩn hóa chiều cao và ghép các cây con
    normalized_lines, total_width, child_centers = self._merge_subtrees(child_drawings)

    # Tạo dòng label cha và dòng kết nối
    parent_line, connection_line, parent_center = self._create_parent_lines(
        label, label_width, total_width, child_centers
    )

    # Ghép tất cả lại
    result_lines = [parent_line, connection_line] + normalized_lines
    return result_lines, total_width, len(result_lines), parent_center

  def _is_leaf_node(self, node):

    return node.list_node[0].left is None

  def _get_child_nodes(self, node):

    children = []

    # Node con đầu tiên (bên trái của key đầu tiên)
    children.append(node.list_node[0].left)

    # Các node con còn lại (bên phải của mỗi key)
    for i in range(node.length):
        children.append(node.list_node[i].right)

    return children

  def _merge_subtrees(self, child_drawings):

    if not child_drawings:
        return [], 0, []

    sub_lines, sub_widths, sub_heights, sub_middles = zip(*child_drawings)
    max_height = max(sub_heights)

    # Chuẩn hóa chiều cao - thêm khoảng trắng cho các cây thấp hơn
    normalized_sub_lines = []
    for lines, width, height in zip(sub_lines, sub_widths, sub_heights):
        padding_needed = max_height - height
        padded_lines = lines + [' ' * width] * padding_needed
        normalized_sub_lines.append(padded_lines)

    # Ghép các dòng theo chiều ngang
    merged_lines = []
    for row in range(max_height):
        row_parts = [normalized_sub_lines[i][row] for i in range(len(child_drawings))]
        merged_lines.append(' '.join(row_parts))

    # Tính tổng chiều rộng và vị trí trung tâm của các node con
    total_width = sum(sub_widths) + len(child_drawings) - 1  # +1 space between each child
    child_centers = []
    current_pos = 0

    for width, middle in zip(sub_widths, sub_middles):
        center = current_pos + middle
        child_centers.append(center)
        current_pos += width + 1  # +1 for space

    return merged_lines, total_width, child_centers

  def _create_parent_lines(self, label, label_width, total_width, child_centers):

    if not child_centers:
        return label, '', label_width // 2

    # Tính vị trí trung tâm cho label cha
    parent_center = (child_centers[0] + child_centers[-1]) // 2

    # Đảm bảo label không bị cắt
    label_start = max(0, parent_center - label_width // 2)
    label_end = label_start + label_width

    if label_end > total_width:
        label_start = total_width - label_width
        label_end = total_width
        parent_center = label_start + label_width // 2

    # Tạo dòng label cha
    parent_line = ' ' * total_width
    parent_line = (parent_line[:label_start] +
                  label +
                  parent_line[label_end:])

    # Tạo dòng kết nối
    connection_chars = [' '] * total_width

    for center in child_centers:
        if center < parent_center:
            connection_chars[center] = '/'
        elif center > parent_center:
            connection_chars[center] = '\\'
        # center == parent_center thì để space (trường hợp đặc biệt)

    connection_line = ''.join(connection_chars)

    return parent_line, connection_line, parent_center

```


```python
#Testcase kiễm tra chức năng thêm

bt = B_tree(3)
bt.add(5)
bt.add(12)
bt.add(10)
bt.add(32)
bt.add(45)
bt.add(40)
bt.add(30)
bt.add(25)
bt.add(50)
bt.toString()
```

           [25]            
       /            \      
     [10]        [32,45]   
     /    \    /         \ 
    [5] [12] [30] [40] [50]
    


```python
#Testcase chức năng xóa (xóa key ở node lá)
bt = B_tree(3)
bt.add(10)
bt.add(20)
bt.add(30)
bt.add(5)
bt.add(7)
bt.add(0)
bt.add(15)
bt.toString()
```

          [7,20]      
      /             \ 
    [0,5] [10,15] [30]
    


```python
#Xóa key ở một node lá đủ key
bt.delete(10)
bt.toString()
```

        [7,20]     
      /     \    \ 
    [0,5] [15] [30]
    


```python
#Xóa key ở một node lá không đủ key và phải mượn sibling của nó
bt.delete(15)
bt.toString()
```

      [5,20]    
     /        \ 
    [0] [7] [30]
    


```python
#Xóa key ở một node lá không đủ key và không thể mượn sibling
#Chỉ có thể merge với node parent có đủ key

bt.delete(30)
bt.toString()
```

       [5]    
     /     \  
    [0] [7,20]
    

Các trường hợp còn lại còn đang trong quá trình nghiên cứu và test, mong các bạn chờ đợi và thông cảm chương trình hoàn thiện 😌😌😌

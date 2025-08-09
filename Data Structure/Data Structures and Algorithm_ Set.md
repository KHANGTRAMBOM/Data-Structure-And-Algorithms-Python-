# **OVERVIEW**

Trước khi tìm hiểu về `Set` và `Dictionary`, hãy điểm qua các cấu trúc dữ liệu phổ biến và lý do chúng ra đời:

- **Cấu trúc tuyến tính**:
  - **Array (Mảng)**: Lưu trữ phần tử, truy cập nhanh O(1) qua chỉ số (index).
  - **Linked List, Stack, Queue**: Liên kết các phần tử rời rạc qua con trỏ, linh hoạt trong quản lý bộ nhớ.

- **Cấu trúc cây**:
  - **Binary Search Tree (BST)**: Thêm, xóa, tìm kiếm với độ phức tạp O(log n), tối ưu cho dữ liệu lớn.
  - **Heap**: Truy cập phần tử lớn nhất/nhỏ nhất với O(1).

> **Từ nhu cầu thực tế**, người ta mong muốn một cấu trúc dữ liệu kết hợp **truy cập nhanh O(1)** như mảng, **thêm/xóa hiệu quả** như cây, và **liên kết dữ liệu linh hoạt**. Do đó, `Set` và `Dictionary` ra đời, sử dụng cơ chế **hashing** để đạt hiệu suất cao.


## Hashing là gì?

**Hashing** là quá trình **chuyển một giá trị (`key`) thành một con số (`hash code`)**, từ đó xác định **vị trí lưu trữ** trong bộ nhớ.  

### Ví dụ:
Trong danh bạ, khi cần tra số điện thoại của `"Khang"`:
- Hàm `hash("Khang")` sẽ tạo ra một số nguyên.
- Con số này được dùng để xác định vị trí lưu `"0909-xxx-xxx"`.
- Nhờ đó, ta có thể tra cứu nhanh chóng trong **O(1)**.


##  Set là gì?

`Set` là một **cấu trúc dữ liệu rời rạc**, lấy cảm hứng từ **tập hợp trong toán học**, với đặc điểm:

- **Không chứa phần tử trùng lặp**
- **Không đảm bảo thứ tự phần tử**
- **Truy cập và kiểm tra phần tử cực nhanh — O(1)** (nhờ hashing)
- **Không thể chứa các giá trị không hashable**

## Một số đặc điểm:
- Không thể có 2 phần tử giống nhau: `{1, 2, 2, 3}` → `{1, 2, 3}`
- Các phần tử bắt buộc phải là **hashable** (băm được): `int`, `str`, `float`, `tuple` (nếu tuple chỉ chứa giá trị bất biến)


[Video tham khảo](https://www.youtube.com/watch?v=T2kk6n7EE3E)

---

# Thao tác trên Set trong Python

`Set` là một tập hợp toán học, lưu trữ các phần tử **duy nhất**, **không thứ tự**. Các thao tác chính trên `Set` bao gồm **giao**, **hợp**, và **trừ**.

## 1. Giao (Intersection)
Lấy các phần tử **có mặt trong cả hai tập hợp** A và B.

**Cú pháp**:
- `A & B` hoặc `A.intersection(B)`

**Ví dụ**:
```python
A = {1, 2, 3, 4}
B = {2, 4, 6, 8}
giao = A & B
print(giao)  # Output: {2, 4}
```

## 2. Hợp (Union)
Lấy tất cả phần tử **thuộc A hoặc B**, không trùng lặp.


```python
A = {1, 2, 3}
B = {3, 4, 5}
hop = A | B
print(hop)  # Output: {1, 2, 3, 4, 5}
```

## 3. Hiệu (Difference)
Lấy các phần tử **chỉ có trong A mà không có trong B**.

```python
A = {1, 2, 3, 4}
B = {3, 4, 5}
hieu = A - B
print(hieu)  # Output: {1, 2}
```

Bên cạnh đó còn các thao tác cơ bản khác như là `add`và `remove`

# **CODE**

Phần này, tôi sẽ viết lại cấu trúc `set` từ đầu mà không dùng thư viện có sẵn. Tuy nhiên, `set` và `dictionary` khá phức tạp, đặc biệt là hàm "hash", nên code chỉ mang tính tham khảo, không hoàn toàn chính xác. Mong các bạn thông cảm! 😊😊😊 Tôi sẽ giải thích hai ý chính về `set` và bổ sung các phần còn thiếu mà chưa đề cập trong phần **OVERVIEW**.

1. **"Hash" hoạt động thế nào?**  
Mỗi kiểu dữ liệu có cách "hash" khác nhau, nên không có cách chung cho mọi trường hợp. Với kiểu `int`, cách đơn giản nhất là dùng phép chia dư. **Ví dụ**: Giả sử giá trị đầu vào là 100 và `hash value` là 1000, khi đó `hash code` sẽ là `100 % 1000 = 100`. Vậy giá trị 100 được lưu ở vị trí 100 trong bảng băm. Với kiểu dữ liệu khác như chuỗi, Python thường dùng hàm hash phức tạp hơn, như kết hợp các ký tự thành số.

2. **Xử lý va chạm (collision) thế nào?**  
Khi hai giá trị cho cùng `hash code`, gọi là va chạm (collision). **Ví dụ**: Với các giá trị 1 và 11, nếu `hash value` là 10, cả hai đều cho `hash code` là `1 % 10 = 1`. Nhưng mỗi vị trí chỉ nên ánh xạ một giá trị duy nhất, vậy làm sao? Một cách dễ hiểu là dùng **Linked List**. Mỗi vị trí trong bảng băm sẽ chứa một Linked List. Khi giá trị mới bị trùng `hash code`, ta tạo một node mới chứa giá trị đó và thêm vào Linked List. **Ví dụ**: Ở vị trí 1, Linked List sẽ chứa cả 1 và 11. Cách này đảm bảo không mất dữ liệu và giữ đặc điểm của `set`: không trùng lặp.


```python
class Set:
  def __init__(self):
    self.HASH_VALUE = 1000
    self.List = [[] for _ in range(self.HASH_VALUE + 1)]


  def hash(self, val: int )-> int:
    return val % self.HASH_VALUE

  def add(self, val: int) -> bool:
    # Tiến hành băm giá trị đầu vào
    index = self.hash(val)

    # Lấy List thuộc vị trí băm
    hash_list = self.List[index]

    # Nếu phần tử cần thêm đã có rồi thì return False
    if val in hash_list:
      return False

    # Nếu giá trị chưa tồn tai hay collision vẫn có thể
    # thêm vào bình thuong
    hash_list.append(val)

    return True

  def remove(self, val: int)-> int:
    # Tiến hành băm giá trị đầu vào
    index = self.hash(val)

    # Lấy List thuộc vị trí băm
    hash_list = self.List[index]

    # Nếu số cần xóa tồn tại
    if val in hash_list:
      hash_list.remove(val)
      return val
    else:
      raise IndexError("Giá trị không tồn tại")


  def __getitem__(self, val: int):
    # Tiến hành băm giá trị đầu vào
    index = self.hash(val)

    # Lấy List thuộc vị trí băm
    hash_list = self.List[index]

    if val in hash_list:
      return val
    else:
      raise IndexError("Giá trị không tồn tại")


  def __str__(self):
    result = []
    for sublist in self.List:
        if sublist:  # Chỉ thêm nếu sublist không rỗng
            result.extend(sublist)
    return str(result)


```


```python
mySet = Set()
mySet.add(1)
mySet.add(100)
mySet.add(234)
mySet.add(1001) #(collision)
print(mySet)           # [1, 100, 234, 1001]

```

    [1, 1001, 100, 234]
    


```python
print(f"Giá trị của i = {1} trong set là: {mySet[1]}")
```

    Giá trị của i = 1 trong set là: 1
    


```python
mySet.remove(100)
print(mySet)           # [1, 234, 1001]
mySet.remove(999)      # IndexError: Giá trị không tồn tại
```

    [1, 1001, 234]
    


    ---------------------------------------------------------------------------

    IndexError                                Traceback (most recent call last)

    /tmp/ipython-input-20-3915273516.py in <cell line: 0>()
          1 mySet.remove(100)
          2 print(mySet)           # [1, 234, 1001]
    ----> 3 mySet.remove(999)      # IndexError: Giá trị không tồn tại
    

    /tmp/ipython-input-17-1989375222.py in remove(self, val)
         37       return val
         38     else:
    ---> 39       raise IndexError("Giá trị không tồn tại")
         40 
         41 
    

    IndexError: Giá trị không tồn tại


# TADA, HẾT RỒI !!! 🥳🥳🥳🥳

Cảm ơn các bạn đã theo dõi đến cuối notebook này!  
Mình biết chương này khá phức tạp vì `Set` và `Dictionary` đều dựa trên cấu trúc **bảng băm** (`hash table`) – đi kèm là nhiều khái niệm như `collision`, `hash value`, `hash code`... mà mình chưa kịp đề cập trong phần **OVERVIEW**. Cũng vì tính phức tạp nên tôi cũng không có các bài tập để bàn rèn luyện mong các bạn thông cảm nhé 😌😌😌

Vì các khái niệm này có tính kỹ thuật cao, mình khuyến khích các bạn nên xem thêm các video giải thích từ những ngôn ngữ như **C++** hoặc **Java** trước, rồi quay lại Python để hiểu rõ hơn. Python là ngôn ngữ bậc cao, nên đôi khi nó làm ẩn đi những chi tiết "thô" nhưng lại rất quan trọng nếu muốn học nghiêm túc về cấu trúc dữ liệu.

Một lần nữa, cảm ơn các bạn đã dành thời gian đọc hết notebook.  
Mong nhận được góp ý từ các bạn để mình cải thiện hơn trong những bài tiếp theo về **DSA** nhé! 😌✨

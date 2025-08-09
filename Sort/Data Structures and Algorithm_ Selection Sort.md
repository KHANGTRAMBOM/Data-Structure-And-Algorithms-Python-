# **OVERVIEW**

Chào các bạn! 👋 Đây là bài `Notebook` đầu tiên trong chương **Sắp xếp (Sort)**, một phần tiếp theo trong series **Cấu trúc dữ liệu và Thuật toán** của mình.

**Sắp xếp** là một thao tác **cơ bản** nhưng cực kỳ quan trọng, không hề thua kém các thao tác như **tìm kiếm**, **thêm**, **xóa**, hay **sửa**. Chính vì thế, việc tìm ra những thuật toán sắp xếp hiệu quả luôn là mục tiêu được nhiều người quan tâm. Trong đó, **Selection Sort** là một trong những thuật toán sắp xếp cơ bản và dễ hiểu nhất.

Hơn nữa, sắp xếp còn có thể được hiểu là quá trình **sửa các cặp nghịch thế** – tức là các cặp phần tử có thứ tự sai – để đưa mảng về trạng thái đúng thứ tự.

Sắp xếp có thể chia thành: **Sắp xếp có so sánh** và **Sắp xếp không có so sánh**

Hãy cùng đọc và khám phá nhé! Nếu bạn có ý kiến hay góp ý, đừng ngần ngại chia sẻ nha! 😊

[Video tham khảo](https://www.youtube.com/watch?v=g-PGLbMth_g)

# **Ý TƯỞNG**

`Selection Sort` đúng như tên gọi của nó – **lựa chọn** để sắp xếp! Ý tưởng rất đơn giản: ta duyệt qua mảng, tìm **phần tử nhỏ nhất** (hoặc lớn nhất, tùy mục đích) và đặt nó vào **vị trí đầu tiên**. Sau đó, tiếp tục lặp lại với các vị trí tiếp theo cho đến khi toàn bộ mảng được sắp xếp xong.

**Độ phức tạp**: $n^2$

Ví dụ: `Sắp xếp tăng dần`
```python
array = [3 ,1 ,9 , 5, 4, 2]
```
**BƯỚC 1:** Đầu tiên vị trí cần sắp xếp là vị trí đầu  →  tìm số bé nhất
```python      
            ↓ Số bé nhất là số 1
array = [3 ,1 ,9 , 5, 4, 2]
         ↑ vị trí đầu
```
**BƯỚC 2:** `Swap` giá trị

```python       
array = [1 ,3 ,9 , 5, 4, 2]
```
**BƯỚC 3:** Lặp lại **BƯỚC 1** nhưng với vị trí tiếp theo
```python
                         ↓ Số bé nhất là số 2
array = [1 ,3 ,9 , 5, 4, 2]
            ↑ vị trí tiếp theo
```




# **CODE**
Phần này tôi sẽ dùng `Python` để minh họa lại thuật toán `Selection Sort`


```python
arr = [64, 25, 12, 22, 11]
n = len(arr)

# i đại diện cho vị trí cần sắp xếp
for i in range(n - 1):
  smallest_index = i
  # j để duyệt tìm số bé nhất (với trường hợp sắp xếp tăng)
  for j in range(i + 1, n):
    # Nếu sắp xếp tăng thì xài dấu '>' còn sắp xếp giảm thì sài dấu '<'
    if arr[smallest_index] > arr[j]:
       smallest_index = j
  # Tiến hành swap giá trị
  arr[i], arr[smallest_index] = arr[smallest_index], arr[i]

print(f"**Mảng sau khi được sắp xếp tăng dần: {arr}")
```

    **Mảng sau khi được sắp xếp tăng dần: [11, 12, 22, 25, 64]
    

# **TADA HẾT RỒI !!! 🥳🥳🥳🥳**

Cảm ơn các bạn đã đọc hết bài `notebook` này, mong các bạn góp ý và ủng hộ mình trong các bài `notebook` sau nhé 😌😌😌

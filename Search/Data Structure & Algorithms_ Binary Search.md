# **OVERVIEW**
Tiếp nối `Linear Search`, tôi sẽ giới thiệu về `Binary Search`.  

`Binary Search`(`Binary Tree` + `Search`) là một thuật toán tìm kiếm hiệu quả, hoạt động trên `mảng đã sắp xếp`. Nó khắc phục nhược điểm chậm của `Linear Search` bằng cách chia đôi không gian tìm kiếm trong mỗi bước, giúp giảm đáng kể thời gian tìm kiếm.

[Video tham khảo](https://www.youtube.com/watch?v=fDKIpRe8GW4)

# **Ý TƯỞNG**
`Binary Search`, như đã đề cập trong phần `OVERVIEW`, nổi bật nhờ khả năng loại bỏ một nửa không gian tìm kiếm ở mỗi bước. Thuật toán hoạt động trên mảng đã sắp xếp, chia đôi mảng và tiếp tục tìm ở nửa phù hợp.

**Cụ thể:**
1. Tính chỉ số giữa (`mid_index`).
2. Kiểm tra `target` với phần tử tại `mid_index`:
   - Nếu `target == arr[mid_index]`, trả về chỉ số `mid_index`.
   - Nếu `target > arr[mid_index]`, tìm tiếp ở nửa bên phải (từ `mid_index + 1` đến cuối).
   - Nếu `target < arr[mid_index]`, tìm tiếp ở nửa bên trái (từ đầu đến `mid_index - 1`).
3. Lặp lại cho đến khi tìm thấy `target` hoặc không còn nửa mảng nào để tìm (trả về -1).

**Độ phức tạp:** `O(log n)`

**Ví dụ**

```python
      left=0                  right=8
        ↓                       ↓
 arr = [1, 3, 5, 7, 9, 11, 13, 15]  Mảng đã sắp xếp tăng dần

 target = 3
```

```python
 mid = (right + left) // 2
           =  (8 + 0) // 2
           =      8   // 2
           =         4

       left=0     mid=4         right=8
        ↓           ↓            ↓
 arr = [1, 3, 5, 7, 9, 11, 13, 15]  Mảng đã sắp xếp tăng dần

 arr[mid] = 9 > target = 3  
 → Đi tìm nửa trái → right = mid - 1 → right = 4 - 1
                                             = 3
```

```python
 mid = (right + left) // 2
           =  (3 + 0) // 2
           =      3  // 2
           =         1

       left=0  right=3
        ↓        ↓            
 arr = [1, 3, 5, 7, 9, 11, 13, 15]  Mảng đã sắp xếp tăng dần
           ↑
          mid=1

 arr[mid] = target → Trả về chỉ mục 1

```



# **CODE**
Trong phần này tôi sẽ tiến hành code lại thuật toán `Binary Search` bằng ngôn ngữ `Python`. 😤😤😤


```python
def binary_search(arr: list, target: int) -> int:
  n = len(arr)

  # Nếu mảng rỗng thì trả về False
  if n == 0:
    return -1

  left = 0
  right = n

  # Tiến hành tìm
  while left <= right:
     # Tính chỉ số giữa
    mid = (left + right) // 2
    # Nếu tìm thấy target, trả về chỉ số
    if target == arr[mid]:
        return mid
    # Nếu target lớn hơn, tìm nửa bên phải
    elif target > arr[mid]:
        left = mid + 1
    # Nếu target nhỏ hơn, tìm nửa bên trái
    else:
        right = mid - 1

  # Không tìm thấy thì trả về False
  return -1

arr = [1, 3, 5, 7, 9, 11, 13, 15]
target = 3
index_find = binary_search(arr, target)
print(f"Chỉ mục của {target} trong mảng là: {index_find}")
```

    Chỉ mục của 3 trong mảng là: 1
    

# **TADA HẾT RỒI !!! 🥳🥳🥳🥳**

Cảm ơn các bạn đã đọc hết bài `notebook` này, mong các bạn góp ý và ủng hộ mình trong các bài `notebook` sau nhé 😌😌😌

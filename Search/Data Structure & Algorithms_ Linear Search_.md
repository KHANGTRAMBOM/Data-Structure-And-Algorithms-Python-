# **OVERVIEW**
`Linear Search` là một thuật toán tìm kiếm đơn giản và cơ bản, dựa vào cơ chế tuần tự.

[Video tham khảo](https://www.youtube.com/watch?v=246V51AWwZM)

# **Ý TƯỞNG**
`Linear Search` duyệt tuần tự mảng, thường từ trái sang phải. Mỗi lần duyệt, nó kiểm tra xem `phần tử hiện tại` có phải là giá trị cần tìm không. Nếu tìm thấy, thuật toán `trả về chỉ số` của phần tử. Nếu không, nó tiếp tục duyệt cho đến khi hết mảng và thông báo không tìm thấy.

**Độ phức tạp:** O(n)

**Ví dụ**

```python
 arr = [1, 5, 3, 9, 4]
 target = 9
```

```python
        i=0
        ↓
 arr = [1, 5, 3, 9, 4]

 arr[i] = 1 != target → tăng i
```

```python
          i=1
           ↓
 arr = [1, 5, 3, 9, 4]

 arr[i] = 5 != target → tăng i
```

```python
             i=2
              ↓
 arr = [1, 5, 3, 9, 4]

 arr[i] = 3 != target → tăng i
```

```python
                i=3
                 ↓
 arr = [1, 5, 3, 9, 4]

 arr[i] = 9 == target → trả về i
```



# **CODE**
Trong phần này tôi sẽ tiến hành code lại thuật toán `Linear Search` bằng ngôn ngữ `Python`. 😤😤😤


```python
def linear_search(arr: list, target: int) -> int:
  n = len(arr)
  # Trả về False nếu mảng rỗng
  if n == 0:
    return -1

  for i in range(n):
    # Tìm thấy thì trả về chỉ mục của số đó
    if arr[i] == target:
      return i

  # Không tìm thấy thì trả về False
  return -1

arr = [1, 5, 3, 9, 4]
target = 9
index_find = linear_search(arr, target)
print(f"Chỉ mục của {target} trong mảng là : {index_find}")
```

    Chỉ mục của 9 trong mảng là : 3
    

# **TADA HẾT RỒI !!! 🥳🥳🥳🥳**

Cảm ơn các bạn đã đọc hết bài `notebook` này, mong các bạn góp ý và ủng hộ mình trong các bài `notebook` sau nhé 😌😌😌

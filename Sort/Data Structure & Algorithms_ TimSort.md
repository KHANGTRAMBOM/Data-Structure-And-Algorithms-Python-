# **OVERVIEW**

Trong các ngôn ngữ lập trình phổ biến như `Python`, `Java`, `C#`, các hàm `sort` tích hợp sẵn giúp chúng ta sắp xếp dữ liệu dễ dàng. Bạn có bao giờ tự hỏi thuật toán nào đứng sau những hàm này không? Câu trả lời chính là `TimSort`.

`TimSort` là một thuật toán sắp xếp lai `(hybrid)`, kết hợp giữa `Merge Sort` và `Insertion Sort`, được thiết kế để:

1. Tận dụng các đoạn con đã sắp xếp sẵn trong mảng (gọi là `"runs"`).
2. Tối ưu hóa hiệu suất trên dữ liệu thực tế, thường không hoàn toàn ngẫu nhiên.
3. Đảm bảo tính ổn định, với độ phức tạp gần giống `Merge Sort`, nhưng hiệu quả hơn khi mảng gần sắp xếp nhờ `Insertion Sort`.


[Video tham khảo](https://www.youtube.com/watch?v=0Dg41UEK3Io)

# Ý TƯỞNG VỀ `TimSort`

Như đã nêu trong phần `OVERVIEW`, `TimSort` kết hợp khả năng tối ưu hóa của `Insertion Sort` cho các đoạn nhỏ và tính ổn định cùng hiệu quả hợp nhất của `Merge Sort` để đạt hiệu suất cao.

**Cụ thể:**
1. Chia `mảng` thành các `run`, mỗi `run` có từ 32 đến 64 phần tử (gọi là `min-run`), tùy thuộc vào kích thước mảng.
2. Sử dụng `Insertion Sort` để sắp xếp từng `run`.
3. Hợp nhất các `run` đã sắp xếp bằng `Merge Sort` để tạo mảng hoàn chỉnh.

**Độ phức tạp:**
- Trường hợp trung bình và xấu nhất: `O(n log n)`
- Trường hợp tốt nhất: `O(n)` (khi mảng gần như đã sắp xếp)

**Ví dụ:**

```python
 arr = [9, 3, 7, 1, 6, 4, 8, 2, 5]
```

```python
 Do số lượng phần tử nhỏ nên ta sẽ tạm thời lấy min-run = 3

 arr = [9, 3, 7, 1, 6, 4, 8, 2, 5]

 R1 = [9, 3, 7]
 R2 = [1, 6, 4]
 R3 = [8, 2, 5]
```

```python
 Insertion Sort trên các runs

 R1 = [9, 3, 7]  →  [3, 7, 9]
 R2 = [1, 6, 4]  →  [1, 4, 6]
 R3 = [8, 2, 5]  →  [2, 5, 8]
```

```python
 Merge R1 và R2
  
 R1 = [3, 7, 9]
 R2 = [1, 4, 6]

 → R12 = [1, 3, 4, 6, 7, 9]
```

```python
 Merge R12 và R3
  
 R12 = [1, 3, 4, 6, 7, 9]
 R3 = [2, 5, 8]

 → R123 = [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

**📌Lưu ý:**

Trong phần này tôi chưa tận dụng được các `runs tự nhiên` 😓😓😓 (các runs đã được sắp xếp tăng dần, hoặc giảm dần). Chính vì thế những gì tôi nói chưa chính xác hoàn toàn nên mong các bạn thông cảm. 😣😣😣



# **CODE**
Sau đây tôi sẽ tiến hành code lại thuật toán `TimSort` bằng ngôn ngữ `Python`. 😤😤😤


```python
def insertion_sort(arr: list, start:int, end:int) -> list:
  for i in range(start + 1, end):
    key = arr[i]
    j = i
    while j > start and key < arr[j - 1]:
      arr[j] = arr[j - 1]
      j -= 1
    arr[j] = key

  return arr

def merge(arr1: list, arr2: list) -> list:
  n = len(arr1)
  m = len(arr2)
  i = 0
  j = 0
  sorted_array = []

  while i < n and j < m:
    if arr1[i] < arr2[j]:
      sorted_array.append(arr1[i])
      i += 1
    else:
      sorted_array.append(arr2[j])
      j += 1

  while i < n:
      sorted_array.append(arr1[i])
      i += 1

  while j < m:
      sorted_array.append(arr2[j])
      j += 1

  return sorted_array

def tim_sort(arr: list, min_run: int = 32) -> list:

  # Tạo một mảng để chứa các runs
  runs = []

  # Chia + sắp xếp các runs
  for i in range(0 , len(arr), min_run):
    # Đảm bảo end luôn <= len(arr)
    end = min(i + min_run, len(arr))
    insertion_sort(arr, i, end)

    # Đưa runs mới được sắp xếp vào danh sách runs
    runs.append(arr[i:end])

  # Tiến hành merge các run lại với nhau
  # Kỹ thuật merge theo cặp thay vì sử dụng một mảng trung gian
  # Để merge tuần tự

  while len(runs) > 1:
    new_runs = []
    for i in range(0 , len(runs), 2):
      if i + 1 < len(runs):
        pair_merge = merge(runs[i] , runs[i + 1])
        new_runs.append(pair_merge)
      else:
        new_runs.append(runs[i])

    runs = new_runs

  return runs[0]


arr = [9, 3, 7, 1, 6, 4, 8, 2, 5]
min_run = 3
sorted_array = tim_sort(arr , min_run)
print(f"**Mảng sau khi được sắp xếp là: {sorted_array}")
```

    **Mảng sau khi được sắp xếp là: [1, 2, 3, 4, 5, 6, 7, 8, 9]
    

## **Tích hợp tìm `run tự nhiên`**


```python
def find_runs(arr:list, start:int, min_run: int) -> list:

 n = len(arr)

 # Nếu còn 1 phần tử thì trả về luôn
 if n - start <= 1:
  return arr[start:]

 i = start

 increased = False

 if arr[i] <= arr[i+1]:
  increased = True

 while i + 1 < n:
   if increased and arr[i] <= arr[i+1]:
      i += 1
   elif not increased and arr[i] > arr[i+1]:
      i += 1
   else:
    break

 run = arr[start: i + 1]

 if len(run) < min_run:
  left = min_run - len(run)
  return insertion_sort(arr, start, i + 1 + left)

 return run

def insertion_sort(arr: list, start:int, end:int) -> list:
  for i in range(start + 1, end):
    key = arr[i]
    j = i
    while j > start and key < arr[j - 1]:
      arr[j] = arr[j - 1]
      j -= 1
    arr[j] = key

  return arr[start:end]

def merge(arr1: list, arr2: list) -> list:
  n = len(arr1)
  m = len(arr2)
  i = 0
  j = 0

  sorted_array = []

  while i < n and j < m:
    if arr1[i] < arr2[j]:
      sorted_array.append(arr1[i])
      i += 1
    else:
      sorted_array.append(arr2[j])
      j += 1

  while i < n:
      sorted_array.append(arr1[i])
      i += 1

  while j < m:
      sorted_array.append(arr2[j])
      j += 1

  return sorted_array

def tim_sort(arr: list, min_run: int = 32) -> list:

  # Tạo một mảng để chứa các runs
  runs = []

  # Chia + sắp xếp các runs
  i = 0
  while i < len(arr):
    run = find_runs(arr, i, min_run)
    runs.append(run)
    i += len(run)


  # Tiến hành merge các run lại với nhau
  # Kỹ thuật merge theo cặp thay vì sử dụng một mảng trung gian
  # Để merge tuần tự

  while len(runs) > 1:
    new_runs = []
    for i in range(0 , len(runs), 2):
      if i + 1 < len(runs):
        pair_merge = merge(runs[i] , runs[i + 1])
        new_runs.append(pair_merge)
      else:
        new_runs.append(runs[i])

    runs = new_runs

  return runs[0]


arr = [9, 3, 7, 1, 6, 4, 8, 2, 5]
min_run = 3
sorted_array = tim_sort(arr , min_run)
print(f"**Mảng sau khi được sắp xếp là: {sorted_array}")
```

    **Mảng sau khi được sắp xếp là: [1, 2, 3, 4, 5, 6, 7, 8, 9]
    

# **TADA HẾT RỒI !!! 🥳🥳🥳🥳**

Cảm ơn các bạn đã đọc hết bài `notebook` này, mong các bạn góp ý và ủng hộ mình trong các bài `notebook` sau nhé 😌😌😌

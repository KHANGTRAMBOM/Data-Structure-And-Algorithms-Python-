# **OVERVIEW**

Chào mừng bạn đến với bài `Notebook` cuối cùng trong chuỗi series về `Data Structure & Algorithms`. Trong phần này, chúng ta sẽ tìm hiểu về một thuật toán sắp xếp khá đặc biệt mang tên `Bucket Sort`.

`Bucket Sort` cũng giống như `Radix Sort` và `Counting Sort`, đều thuộc nhóm các thuật toán sắp xếp `non-comparison sort`. Khác với cách tiếp cận trực tiếp như `Counting Sort`, `Bucket Sort` hoạt động đúng như tên gọi của nó — **sắp xếp theo xô**. Ý tưởng chính là:
- Phân phối các phần tử vào nhiều `bucket` (xô) dựa trên giá trị của chúng.
- Sau đó, ta sắp xếp từng `bucket` riêng biệt.
- Cuối cùng, các `bucket` đã sắp sẽ được nối lại thành mảng kết quả đã sắp xếp hoàn chỉnh.

Nhờ cách tiếp cận phân vùng thông minh, `Bucket Sort` phát huy sức mạnh rõ rệt khi dữ liệu đầu vào `phân bố đều`. Tuy nhiên, hiệu quả của thuật toán cũng phụ thuộc khá nhiều vào `cách chia bucket` và `thuật toán sắp xếp nội bộ` được sử dụng.


[Video tham khảo](https://www.youtube.com/watch?v=DkhpHdo9h0A)

# **Ý TƯỞNG**

`Bucket Sort` là một thuật toán sắp xếp dựa trên phân phối, lấy cảm hứng từ cơ chế của `hash table`. Thuật toán chia các phần tử trong mảng thành các nhóm (gọi là `xô` hoặc `thùng`) dựa trên giá trị, sau đó sắp xếp từng xô riêng lẻ và ghép lại để tạo mảng đã sắp xếp. Thuật toán này đặc biệt hiệu quả khi dữ liệu phân bố đều.

## Các bước thực hiện
1. **Tạo các xô**: Tạo `n` xô tương ứng với số phần tử hoặc dựa trên khoảng giá trị của dữ liệu.
2. **Phân phối phần tử**: Sử dụng hàm `hash` (ví dụ: chia giá trị phần tử cho giá trị tối đa để xác định xô) để đặt mỗi phần tử vào xô phù hợp.
3. **Sắp xếp từng xô**: Dùng `Insertion Sort` để sắp xếp phần tử trong mỗi xô, vì thuật toán này hiệu quả với danh sách nhỏ.
4. **Ghép các phần tử**: Lấy các phần tử từ các xô theo thứ tự để tạo mảng đã sắp xếp.

**Độ phức tạp:** `O(n + k)`


**Ví dụ**

```python
 arr = [29, 25, 3, 49, 9, 37, 21, 43]
```

```python
 Hiện tại arr có 8 phần tử nên ta chọn số lượng xô là n=8 (có thể chọn số khác)

 bucket[0] = []
 bucket[1] = []
 bucket[2] = []
 bucket[3] = []
 bucket[4] = []
 bucket[5] = []
 bucket[6] = []
 bucket[7] = []


 Tính bucket_size: mỗi xô có bucket_size phần tử)
       bucket_size = (max_value - min_value) / n
                   = (49 - 3 )  / 8
                   =     5
```

```python
 Tiến hành đưa các phần từ vào xô tương ứng 👉 đối với phần tử min và max thì sẽ đưa nó vào xô đầu tiên và cuối cùng.

        i=0
         ↓
  arr = [29, 25, 3, 49, 9, 37, 21, 43]

  bucket_index = (arr[i] - min_value) / bucket_size
               = (29 - 3) / 5
               = 26 / 5             
               = 5

  → bucket[5] = [29]
```

```python
             i=1
              ↓
  arr = [29, 25, 3, 49, 9, 37, 21, 43]

  bucket_index = (arr[i] - min_value) / bucket_size
               = (25 - 3) / 5
               = 22 / 5   
               = 4

  → bucket[4] = [25]
```

```python
                i=2
                 ↓
  arr = [29, 25, 3, 49, 9, 37, 21, 43]
  → 3 là min_value nên đưa thẳng vào bucket[0]
  → bucket[0] = [3]
```

```python
                   i=3
                    ↓
  arr = [29, 25, 3, 49, 9, 37, 21, 43]
  → 49 là max_value nên đưa thẳng vào bucket[7]
  → bucket[7] = [49]
```

```python
                       i=4
                        ↓
  arr = [29, 25, 3, 49, 9, 37, 21, 43]

  bucket_index = (arr[i] - min_value) / bucket_size
               = (9 - 3) / 5
               = 6 / 5   
               = 1

  → bucket[1] = [9]
```

```python
                           i=5
                            ↓
  arr = [29, 25, 3, 49, 9, 37, 21, 43]

  bucket_index = (arr[i] - min_value) / bucket_size
               = (37 - 3) / 5
               = 34 / 5   
               = 6

  → bucket[6] = [37]
```

```python
                               i=5
                                ↓
  arr = [29, 25, 3, 49, 9, 37, 21, 43]

  bucket_index = (arr[i] - min_value) / bucket_size
               = (21 - 3) / 5
               = 18 / 5   
               = 3

  → bucket[3] = [21]
```



```python
                                   i=5
                                    ↓
  arr = [29, 25, 3, 49, 9, 37, 21, 43]

  bucket_index = (arr[i] - min_value) / bucket_size
               = (43 - 3) / 5
               = 40 / 5   
               = 8
  → vì bucket chỉ từ  0-7 nên 8 là không hợp lệ nên sẽ đưa 43 vào xô cuối cùng
  → bucket[7] = [49,43]
```

```python
 bucket[0] = [3]
 bucket[1] = [9]
 bucket[2] = []
 bucket[3] = [21]
 bucket[4] = [25]
 bucket[5] = [29]
 bucket[6] = [37]
 bucket[7] = [49,43]

 → Sắp xếp trên từng bucket

 bucket[0] = [3]
 bucket[1] = [9]
 bucket[2] = []
 bucket[3] = [21]
 bucket[4] = [25]
 bucket[5] = [29]
 bucket[6] = [37]
 bucket[7] = [43,49]
```

```python
 Lấy các phần tử từ các xô theo thứ tự để tạo mảng

 bucket[0] = [3]
 bucket[1] = [9]
 bucket[2] = []
 bucket[3] = [21]
 bucket[4] = [25]
 bucket[5] = [29]
 bucket[6] = [37]
 bucket[7] = [43,49]

 → sorted_array = [3, 9, 21, 25, 29, 37, 43, 49]
```


**📌 Lưu ý:**

Cơ chế `phân phối phần tử` vào các `xô` là yếu tố cốt lõi của thuật toán `Bucket Sort`, tương tự như cách chọn `pivot` trong `Quick Sort`. Có nhiều phương pháp phân phối khác nhau, tùy thuộc vào đặc điểm của bài toán. Phương pháp được trình bày trong ví dụ (dựa trên giá trị min/max và bucket_size) là một cách tiếp cận `phổ biến và dễ hiểu`.




# **CODE**
Trong phần này tôi sẽ tiến hành code lại thuật toán `Bucket Sort` bằng ngôn ngữ `Python`. 😤😤😤


```python
def insertion_sort(arr: list) -> list:
  n = len(arr)
  for i in range(1, n):
    j = i
    insert_value = arr[i]
    while j > 0 and insert_value < arr[j - 1]:
      arr[j] = arr[j - 1]
      j -= 1
    arr[j] = insert_value

  return arr

def bucket_sort(arr: list) -> list:
  # Tạo xô
  n = len(arr)

  if n == 0:
    return []

  bucket = [ [] for _ in range(n) ]

  # Tạo mảng mục tiêu
  sorted_array = []

  # Tính bucket_size
  max_value = max(arr)
  min_value = min(arr)

  # Cộng thêm 1 giả nếu tính ra bucket_size là 0
  bucket_size = ((max_value - min_value) // n) + 1

  # Tiến hành đưa các phần tử vào xô
  for num in arr:
    bucket_index = (num - min_value) // bucket_size
    if bucket_index >= n:
      bucket_index = n - 1
    if bucket_index <= 0:
      bucket_index = 0
    bucket[bucket_index].append(num)

  # Sắp xếp trên các xô
  for i in range(n):
    bucket[i] = insertion_sort(bucket[i])

  # Lấy các phần tử theo thứ tự xô để tạo mảng
  for i in range(n):
    for num in bucket[i]:
      sorted_array.append(num)

  return sorted_array


arr = [29, 25, 3, 49, 9, 37, 21, 43]
sorted_array = bucket_sort(arr)
print(f"**Mảng sau khi được sắp xếp là: {sorted_array}")
```

    **Mảng sau khi được sắp xếp là: [3, 9, 21, 25, 29, 37, 43, 49]
    

# HOÀN THÀNH SERIES RỒI!!! 🥳

Cảm ơn các bạn đã đồng hành cùng tôi trong chuỗi bài viết về `Cấu trúc Dữ liệu & Thuật toán`. Series này đã giới thiệu các kiến thức cơ bản như `mảng`, `danh sách liên kết`, `cây`, cùng với các thuật toán `sắp xếp`, `tìm kiếm`, và nhiều nội dung khác.

Để thành thạo `Cấu trúc Dữ liệu & Thuật toán`, ngoài các cấu trúc dữ liệu cơ bản và thuật toán đã đề cập, các bạn cần tìm hiểu thêm nhiều kỹ thuật và thuật toán nâng cao như: `Đệ quy`, `Backtracking`, `Sliding Window`, `Topological Sort`, `Greedy`, `BFS`, `DFS`, `Bitmask`, `Ma trận`, `Quy hoạch động`, v.v. Vì vậy, hãy chăm chỉ luyện tập để nâng cao kỹ năng của mình nhé! Một số trang web như `LeetCode`, `Codeforces`, `HackerRank` là những nền tảng tuyệt vời để các bạn rèn luyện.

Cuối cùng, vì đây là series đầu tay của tôi, nếu có bất kỳ thiếu sót hay sai sót nào, mong các bạn thông cảm và góp ý để tôi có thể cải thiện trong tương lai! 😊

Một lần nữa, xin chân thành cảm ơn và chúc các bạn thành công! 🚀

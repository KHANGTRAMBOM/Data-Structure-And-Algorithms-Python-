# **OVERVIEW**
Thuật toán **`Bubble Sort`** là một trong những thuật toán sắp xếp cơ bản, hoạt động dựa trên nguyên tắc: `"Cái gì nhẹ thì nổi lên trước"`, hay còn gọi là `"nổi bọt"`.

Trong **`Bubble Sort`**, "nhẹ" hay "nặng" phụ thuộc vào `mục đích sắp xếp`. Nếu sắp xếp theo thứ tự **`tăng dần`**, các số nhỏ hơn được xem là "nhẹ" và sẽ dần nổi lên đầu danh sách. Ngược lại, với sắp xếp **`giảm dần`**, các số lớn hơn sẽ được ưu tiên.

[Video tham khảo](https://www.youtube.com/watch?v=xli_FI7CuzA)
---

# **Ý TƯỞNG**
Như đã đề cập trong phần **OVERVIEW**, ý tưởng của `Bubble Sort` dựa trên sự "nổi bọt" của các phần tử nhẹ, tức là đưa các phần tử nhỏ hơn (hoặc lớn hơn, tùy mục đích sắp xếp) lên đầu danh sách.

Cụ thể hơn, `Bubble Sort` sắp xếp mảng bằng cách xử lý các cặp phần tử `nghịch thế` (tức là cặp phần tử không đúng thứ tự). Chúng ta duyệt qua mảng, so sánh hai phần tử liền kề, và nếu chúng tạo thành cặp `nghịch thế`, ta hoán đổi (swap) vị trí của chúng. Quá trình này lặp lại nhiều lần cho đến khi mảng được sắp xếp hoàn toàn.

**Độ phức tạp**: O($n^2$)

**Ví dụ: Sắp xếp tăng**

```python
 arr = [2, 8, 5, 3, 9, 4, 1]
```


```python
           ↓ j + 1        ↓ i đại diện cho vị trí cần nổi bọt
 arr = [2, 8, 5, 3, 9, 4, 1]
      j ↑

 j và j + 1 hiện tại đang đúng thứ tự → tăng j lên 1
```
```python
              ↓ j + 1     ↓ i đại diện cho vị trí cần nổi bọt
 arr = [2, 8, 5, 3, 9, 4, 1]
         j ↑

 j và j + 1 hiện tại sai vị trí (cặp nghịch thế) → tiến hành swap giá trị
```

```python

              ↓ j + 1     ↓ i đại diện cho vị trí cần nổi bọt
 arr = [2, 5, 8, 3, 9, 4, 1]
         j ↑
         
 sau khi swap xong thì lại cho j tăng lên 1
```

```python

                 ↓ j + 1  ↓ i đại diện cho vị trí cần nổi bọt
 arr = [2, 5, 8, 3, 9, 4, 1]
            j ↑
         
 j và j + 1 hiện tại sai vị trí (cặp nghịch thế) → tiến hành swap giá trị.
```
👉 Cứ làm như thế cho đến khi `j + 1 <= i`, là ta đã sắp xếp được `vị trí nổi bổi đầu tiên` và đến `vị trí nổi bọt tiếp theo`.





---

# **CODE**
Phần này tôi sẽ `mô phỏng` lại thuật toán `Bubble Sort` bằng `Python` 😤😤😤


```python
 # Trường hợp sắp xếp tăng dần
def bubble_sort(arr: list) ->list:
  n = len(arr)
  # i đại diện cho vị trí nổi bọt lên
  for i in range(n - 1, 0 , -1):
    # j để tìm các cặp nghịch thế
    for j in range(i):
      # Phát hiện cặp nghịch thế, liền đổi vị trí của nó
      if arr[j] > arr[j + 1]:
        arr[j], arr[j + 1] = arr[j + 1], arr[j]

  return arr

arr = [2, 8, 5, 3, 9, 4, 1]
sorted_arr = bubble_sort(arr)
print(f"Mảng sau khi được sắp xếp tăng: {sorted_arr}")
```

    Mảng sau khi được sắp xếp tăng: [1, 2, 3, 4, 5, 8, 9]
    

# **TADA HẾT RỒI !!! 🥳🥳🥳🥳**

Cảm ơn các bạn đã đọc hết bài `notebook` này, mong các bạn góp ý và ủng hộ mình trong các bài `notebook` sau nhé 😌😌😌

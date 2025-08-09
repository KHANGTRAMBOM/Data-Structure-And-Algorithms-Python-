# **TỔNG QUAN**

`Insertion Sort` là một kỹ thuật sắp xếp đơn giản, tiếp theo sau `Selection Sort` và `Bubble Sort`. Về ý tưởng, nó tương tự `Selection Sort`, nhưng thay vì tìm và đặt phần tử nhỏ nhất vào vị trí đúng, `Insertion Sort` lấy từng phần tử và `CHÈN` nó vào vị trí phù hợp trong dãy đã được sắp xếp. Cách tiếp cận này giúp dãy số dần trở nên có thứ tự một cách tự nhiên.

Trong các thuật toán sắp xếp dựa trên so sánh có độ phức tạp O($n^2$), `Insertion Sort` thường hiệu quả hơn về thời gian trong một số trường hợp, chẳng hạn khi dãy số đã gần được sắp xếp hoặc có kích thước nhỏ. Điều này là do nó thực hiện ít hoán đổi hơn so với các thuật toán như `Bubble Sort` hay `Selection Sort`.

[Video tham khảo](https://www.youtube.com/watch?v=JU767SDMDvA)


---

# **Ý TƯỞNG**
Như đã đề cập ở phần **OVERVIEW**, ý tưởng của thuật toán **Insertion Sort** là tìm vị trí phù hợp cho mỗi phần tử và `CHÈN` nó vào đó.

**Cụ thể**:
1. Duyệt mảng từ trái sang phải (từ vị trí 1 đến cuối).
2. Lấy phần tử tại vị trí `i` (phần tử cần chèn) và so sánh ngược về phía trái.
3. Nếu phần tử trước đó lớn hơn, dịch chuyển phẩn tử đó sang phải và `CHÈN` phần tử vào vị trí phù hợp.

**Độ phức tạp**: O($n^2$)

**Ví dụ**: Sắp xếp tăng dần

```python
 arr = [2, 8, 1, 3, 9, 4]
```

```python
           temp chứa giá trị của i hay temp = arr[i]
           ↓ i đại diện cho phần tử cần chèn
 arr = [2, 8, 1, 3, 9, 4]
        ↑ j so sánh với temp để có thể chèn vào được không
```

```python
           temp: 8
           ↓ i
 arr = [2, 8, 1, 3, 9, 4]
        ↑ j
 ta thấy temp không thể chèn vào vị trí j → tăng i lên 1
```

```python
              temp: 1
              ↓ i
 arr = [2, 8, 1, 3, 9, 4]
           ↑ j
 ta thấy temp có thể chèn vào vị trí j nên tiến hành: đưa giá trị tại vị trí j arr[j] sang phải, cụ thể arr[j+1] = arr[j] và giảm j lại 1

```

```python
              temp: 1
              ↓ i
 arr = [2, 8, 8, 3, 9, 4]
        ↑ j
 ta thấy temp có thể chèn vào vị trí j nên tiến hành đưa giá trị tại vị trí j arr[j] sang phải, cụ thể arr[j+1] = arr[j] và giảm j lại 1
```

```python
              temp: 1
              ↓ i
 arr = [2, 2, 8, 3, 9, 4]
     ↑ j
 Ta thấy lúc này j đã nằm ngoài mảng -> tiến hành chèn thực sự. Cụ thể arr[j+1] = tmp
```

```python
              temp: 1
              ↓ i
 arr = [1, 2, 8, 3, 9, 4]
     ↑ j
 Ta thấy lúc này j đã nằm ngoài mảng -> tiến hành chèn thực sự. Cụ thể arr[j+1] = tmp
```

**📌 Lưu Ý:**
Trong `Notebook` về `Link List` cũng có một bài tập liên quan đến `Insertion Sort` nhưng sự khác nhau là: `Link List` mới thật sự là tìm vị trí rồi chèn vào, nhưng đối với `array` thì là do cấu trúc dữ liệu mà các phần tử nằm kề nhau nên phải có sự dịch chuyển giá trị trước khi chèn.



---

# **CODE**
Trong phần này tôi sẽ tiến hành code lại thuật toán `Insertion Sort` bằng `Python` 😤😤😤


```python
# Hàm insertion sort
def insertion_sort(arr: list) -> list:
  # Lấy độ dài của mảng
  n = len(arr)
  for i in range(1, n):
    # Tạo biến tmp và j để tiến hành so sánh
    tmp = arr[i]
    j = i - 1
    # Nếu có thể CHÈN thì dịch chuyển giá trị tại j sang j + 1
    while j >= 0 and arr[j] > tmp:
      arr[j + 1] = arr[j]
      j -= 1
    # CHÈN giá trị cần chèn vào vị trí j + 1
    arr[j + 1]  = tmp
  return arr

arr = [2, 8, 1, 3, 9, 4]
sorted_arr = insertion_sort(arr)
print(f"**Mảng sau khi được sắp xếp là: {sorted_arr}")
```

    **Mảng sau khi được sắp xếp là: [1, 2, 3, 4, 8, 9]
    

# **TADA HẾT RỒI !!! 🥳🥳🥳🥳**

Cảm ơn các bạn đã đọc hết bài `notebook` này, mong các bạn góp ý và ủng hộ mình trong các bài `notebook` sau nhé 😌😌😌

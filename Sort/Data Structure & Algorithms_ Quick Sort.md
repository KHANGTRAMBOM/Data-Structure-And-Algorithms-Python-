# **OVERVIEW**
Sau `Notebook` về `Merge Sort`, chúng ta sẽ tìm hiểu `Quick Sort`, một thuật toán sắp xếp hiệu quả với độ phức tạp trung bình `O(n log n)`. `Quick Sort` hoạt động bằng cách chọn một phần tử pivot và phân hoạch mảng thành các phần nhỏ hơn. Cùng với `Merge Sort`, `Quick Sort` thường được so sánh khi nói về các thuật toán sắp xếp có hiệu suất tối ưu.


[Video tham khảo](https://www.youtube.com/watch?v=pM-6r5xsNEY)

---

# **Ý TƯỞNG**
Tương tự như `Merge Sort`, `Quick Sort` sử dụng nguyên tắc `chia để trị` để sắp xếp mảng.

**Các bước thực hiện:**

1. Chọn một phần tử làm `pivot`, có thể là phần tử đầu, cuối, giữa, hoặc ngẫu nhiên.
2. Tiến hành `Partition` (phân hoạch)
3. Tiếp tục thực hiện `Partition` cho các mảng con bên trái và bên phải của `pivot`. Ví dụ:
   ```python
   left = partition(arr[:pivot_index])
   right = partition(arr[pivot_index + 1:])
   ```
4. Thực hiện lại **BƯỚC 2** cho đến khi mảng được `sắp xếp xong`.


**Độ phức tạp:** O(nlogn), tệ nhất O($n^2$)


**Ví Dụ:** Mảng sắp xếp tăng với phần tử cuối là `pivot` `(Lamuto Partition)`

```python
 arr = [10, 3, 5, 8, 2, 7, 1, 6, 4, 9]
```

```python
         ↓ right                    ↓ pivot
 arr = [10, 3, 5, 8, 2, 7, 1, 6, 4, 9]
    ↑ left

1. right sẽ tìm các phần tử bé hơn pivot
2. Nếu gặp thì tăng left lên 1 đơn vị rồi tiến hành swap giá trị right và left 3. Đồng thời tăng right lên 1 đơn vị                     
```

```python
            ↓ right                 ↓ pivot
 arr = [10, 3, 5, 8, 2, 7, 1, 6, 4, 9]
     ↑ left                    

ta thấy arr[right] = 10 > 9 → tăng right
```

```python
            ↓ right                 ↓ pivot
 arr = [10, 3, 5, 8, 2, 7, 1, 6, 4, 9]
     ↑ left                   

arr[right] = 3 < 9, tiến hành tăng left và swap

             ↓ right                 ↓ pivot
→ arr = [3, 10, 5, 8, 2, 7, 1, 6, 4, 9]
         ↑ left    

tăng right
                ↓ right              ↓ pivot
→ arr = [3, 10, 5, 8, 2, 7, 1, 6, 4, 9]
         ↑ left    
```

```python
                ↓ right              ↓ pivot
→ arr = [3, 10, 5, 8, 2, 7, 1, 6, 4, 9]
         ↑ left   

arr[right] = 5 < 9, tiến hành tăng left và swap

                ↓ right              ↓ pivot
→ arr = [3, 5, 10, 8, 2, 7, 1, 6, 4, 9]
            ↑ left    

tăng right
                   ↓ right           ↓ pivot
→ arr = [3, 5, 10, 8, 2, 7, 1, 6, 4, 9]
            ↑ left    
```

```python

                  ↓ right           ↓ pivot
 arr = [3, 5, 10, 8, 2, 7, 1, 6, 4, 9]
           ↑ left                        

arr[right] = 8 < 9, tiến hành tăng left và swap

                   ↓ right          ↓ pivot
→ arr = [3, 5, 8, 10, 2, 7, 1, 6, 4, 9]
               ↑ left    

tăng right
                      ↓ right        ↓ pivot
→ arr = [3, 5, 8, 10, 2, 7, 1, 6, 4, 9]
               ↑ left     
```

```python

                     ↓ right        ↓ pivot
 arr = [3, 5, 8, 10, 2, 7, 1, 6, 4, 9]
              ↑ left                        

arr[right] = 2 < 9, tiến hành tăng left và swap

                      ↓ right        ↓ pivot
→ arr = [3, 5, 8, 2, 10, 7, 1, 6, 4, 9]
                  ↑ left     

tăng right
                         ↓ right     ↓ pivot
→ arr = [3, 5, 8, 2, 10, 7, 1, 6, 4, 9]
                  ↑ left       
```

```python
                        ↓ right     ↓ pivot
 arr = [3, 5, 8, 2, 10, 7, 1, 6, 4, 9]
                 ↑ left                         

arr[right] = 7 < 9, tiến hành tăng left và swap

                         ↓ right     ↓ pivot
→ arr = [3, 5, 8, 2, 7, 10, 1, 6, 4, 9]
                     ↑ left     

tăng right
                            ↓ right  ↓ pivot
→ arr = [3, 5, 8, 2, 7, 10, 1, 6, 4, 9]
                     ↑ left        
```


```python
                           ↓ right  ↓ pivot
 arr = [3, 5, 8, 2, 7, 10, 1, 6, 4, 9]
                    ↑ left                           

arr[right] = 1 < 9, tiến hành tăng left và swap

                            ↓ right  ↓ pivot
→ arr = [3, 5, 8, 2, 7, 1, 10, 6, 4, 9]
                        ↑ left    

tăng right
                               ↓ right  
→ arr = [3, 5, 8, 2, 7, 1, 10, 6, 4, 9]
                        ↑ left       ↑ pivot
```


```python
                              ↓ right  
 arr = [3, 5, 8, 2, 7, 1, 10, 6, 4, 9]
                       ↑ left       ↑ pivot                        

arr[right] = 6 < 9, tiến hành tăng left và swap

                               ↓ right  
→ arr = [3, 5, 8, 2, 7, 1, 6, 10, 4, 9]
                           ↑ left    ↑ pivot        

tăng right
                                  ↓ right  
→ arr = [3, 5, 8, 2, 7, 1, 6, 10, 4, 9]
                           ↑ left    ↑ pivot     
```


```python
                                 ↓ right  
 arr = [3, 5, 8, 2, 7, 1, 6, 10, 4, 9]
                           ↑ left    ↑ pivot                         

arr[right] = 4 < 9, tiến hành tăng left và swap

                                  ↓ right  
→ arr = [3, 5, 8, 2, 7, 1, 6, 4, 10, 9]
                         left ↑     ↑ pivot       

tăng right
                                     ↓ right  
→ arr = [3, 5, 8, 2, 7, 1, 6, 4, 10, 9]
                         left ↑      ↑ pivot      
```


```python
                                    ↓ right  
 arr = [3, 5, 8, 2, 7, 1, 6, 4, 10, 9]
                        left ↑      ↑ pivot                       

Lúc này right = pivot, dừng lặp và cho left tăng lên 1 đơn vị và swap giữa left và pivot   

                                    ↓ right  
 arr = [3, 5, 8, 2, 7, 1, 6, 4, 9, 10]
                            left ↑  ↑ pivot
```

**📌 Lưu ý**
Trong `Quick Sort`, `Partition` là linh hồn của thuật toán. Ngoài `Lomuto Partition` (thường chọn `pivot` là phần tử cuối), còn có `Hoare Partition` (do Tony Hoare, cha đẻ `Quick Sort`, đề xuất, thường chọn `pivot` ở đầu hoặc giữa) và `Randomized Partition` (chọn `pivot` ngẫu nhiên để tránh trường hợp xấu). Nên chọn phương pháp phân hoạch phù hợp.





---

# **CODE**
Trong phần này tôi sẽ viết lại `Quick Sort` bằng ngôn ngữ `Python` 😤😤😤


## **1. Lomuto Partition**


```python
# Lamuto Partition
def partition(arr: list, start: int , end: int) -> int:
  # i xuất phát sau j
  i = start - 1
  j = start
  # Chọn phần tử cuối làm pivot
  pivot = end

  # Tiến hành duyệt
  while j < end:
    # Nếu j là phần tử bé hơn pivot
    if arr[j] < arr[pivot]:
      # Tăng i lên và swap giá trị giữa i và j
      i += 1
      arr[j] , arr[i]  = arr[i], arr[j]
    # Tăng j sau mỗi lần duyệt
    j += 1

  # Kết thúc vòng lập, tăng i lên và swap với pivot,
  # vị trí i cũng sẽ là vị trí đúng của phần tử đó
  i += 1
  arr[i] , arr[pivot]  = arr[pivot], arr[i]

  return i

def quick_sort(arr: list, start: int, end: int) -> None:
  # Nếu số lượng phần tử là 1 thì trả về luôn
  if end - start <= 0:
    return

  # Tiến hành phân hoạch
  pivot = partition(arr, start, end)
  # phân hoạch nữa trái pivot
  quick_sort(arr, start, pivot - 1)
  # phân hoạc nữa phải pivot
  quick_sort(arr, pivot + 1, end)
  return

arr = [10, 3, 5, 8, 2, 7, 1, 6, 4, 9]
quick_sort(arr, 0, len(arr)-1)
print(f"**Mảng sau khi được sắp xếp: {arr}")
```

## **2. Hoare Partition**

1. **Chọn một phần tử làm `pivot`** (thường là phần tử `đầu` hoặc `giữa`)
2. Tạo **2 con trỏ**:
   - `i` đi từ **trái sang phải**
   - `j` đi từ **phải sang trái**
3. Con trỏ `i` sẽ tìm các **phần tử lớn hơn** `pivot`  
   Con trỏ `j` sẽ tìm các **phần tử nhỏ hơn** `pivot`
4. Nếu `arr[i] > arr[j]`, thực hiện `swap` 2 phần tử đó
5. Lặp lại **Bước 3** cho đến khi `i >= j`
6. Trả về chỉ số `j` làm vị trí phân chia
7. *(Tùy chọn)* Nếu `pivot` là phần tử đầu mảng, **có thể `swap` pivot với phần tử ở vị trí `j`** để đưa nó về đúng vùng


### **2.1 Pivot là phần tử đầu**


```python
# Hoare Partition
def partition(arr: list, start: int , end: int) -> int:
  # Chọn phần từ đầu làm pivot
  pivot = arr[start]

  # 2 con trỏ: môt đầu, một cuối
  i = start + 1
  j = end

  while True:
    # con trỏ i tìm phần tử lớn hơn pivot
    while i <= end and arr[i] < pivot:
      i += 1
    # con trỏ j tìm phần tử bé hơn pivot
    while j >= start and arr[j] > pivot:
      j -= 1

    # nếu j < i thì dừng, không thì tiến hành swap
    if j > i:
      arr[i], arr[j] = arr[j], arr[i]
    else:
      break
  # Đưa giá trị của pivot vào vị trí phân chia đúng
  arr[j] , arr[start] = arr[start], arr[j]
  return j

def quick_sort(arr: list, start: int, end: int) -> None:
  # Nếu số lượng phần tử là 1 thì trả về luôn
  if end - start <= 0:
    return

  # Tiến hành phân hoạch
  pivot = partition(arr, start, end)
  # phân hoạch nữa trái pivot
  quick_sort(arr, start, pivot)
  # phân hoạc nữa phải pivot
  quick_sort(arr, pivot + 1, end)
  return

arr = [6, 3, 5, 8, 2, 7, 1, 10, 4, 9]
quick_sort(arr, 0, len(arr)-1)
print(f"**Mảng sau khi được sắp xếp: {arr}")
```

    **Mảng sau khi được sắp xếp: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    

### **2.2 Pivot là phần tử giữa**


```python
# Hoare Partition
def partition(arr: list, start: int , end: int) -> int:
  # Chọn phần từ giữa làm pivot
  mid = (start + end) // 2
  pivot = arr[mid]

  # 2 con trỏ: môt đầu, một cuối
  i = start
  j = end

  while True:
    # con trỏ i tìm phần tử lớn hơn pivot
    while i <= end and arr[i] < pivot:
      i += 1
    # con trỏ j tìm phần tử bé hơn pivot
    while j >= start and arr[j] > pivot:
      j -= 1

    # nếu j < i thì dừng, không thì tiến hành swap
    if j > i:
      arr[i], arr[j] = arr[j], arr[i]
      i += 1
      j -= 1
    else:
      return j
def quick_sort(arr: list, start: int, end: int) -> None:
  # Nếu số lượng phần tử là 1 thì trả về luôn
  if end - start <= 0:
    return

  # Tiến hành phân hoạch
  pivot = partition(arr, start, end)
  # phân hoạch nữa trái pivot
  quick_sort(arr, start, pivot)
  # phân hoạc nữa phải pivot
  quick_sort(arr, pivot + 1, end)
  return

arr = [6, 3, 5, 8, 2, 7, 1, 10, 4, 9]
quick_sort(arr, 0, len(arr)-1)
print(f"**Mảng sau khi được sắp xếp: {arr}")
```

    **Mảng sau khi được sắp xếp: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]
    

## **3. Tổng kết**
1. `Partition` là linh hồn của `Quick Sort`
2. Các thuật toán `Partition` khác nhau sẽ cho ra phiên bản `Quick Sort` khác nhau
3. `Partition` là quá trình đưa các `phần từ lớn hơn` `arr[pivot_index]` `sang phải` và `bé hơn` `sang trái`, hay nói cách khác làm tìm `vị trí phân chia` (pivot) hợp lý, ngoài ra **KHÔNG NHẤT THIẾT** là `pivot_index` phải nằm `đúng vị trí`.

## **So sánh Lomuto vs Hoare Partition**

| Tiêu chí                    | Lomuto                              | Hoare                               |
|-----------------------------|--------------------------------------|-------------------------------------|
| **Pivot chọn**              | `arr[end]` (cuối mảng)               | `arr[start]` (đầu mảng), hoặc `arr[mid]` (giữa mảng)             |
| **Kỹ thuật con trỏ**        |  con trỏ `i` và `j` (→)     | 2 con trỏ `i` (→) và `j` (←)        |
| **Phân chia mảng**          | Trái `< pivot`, phải `≥ pivot`       | Trái `≤ pivot`, phải `≥ pivot`      |
| **kết quả phân hoạch** | `Pivot` là vị trí đúng                  | Các `phần tử lớn` nằm `bên phải` pivot, `phần tử nhỏ` nằm `bên trái` pivot                |

---


# **TADA HẾT RỒI !!! 🥳🥳🥳🥳**

Cảm ơn các bạn đã đọc hết bài `notebook` này, mong các bạn góp ý và ủng hộ mình trong các bài `notebook` sau nhé 😌😌😌

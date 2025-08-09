# **OVERVIEW**

Các thuật toán sắp xếp như `Selection Sort`, `Bubble Sort` và `Insertion Sort` đều có độ phức tạp thời gian là O(n²). Trong một số trường hợp, `Insertion Sort` có thể tối ưu hơn về tốc độ, nhưng nhìn chung, cả ba thuật toán này chỉ hiệu quả khi xử lý danh sách có số lượng phần tử nhỏ.

Khi số lượng phần tử tăng lên rất lớn, các thao tác cơ bản của những thuật toán này sẽ trở thành gánh nặng về `thời gian`, `tốc độ xử lý` và thậm chí là `phần cứng` của máy tính. Vì vậy, cần có những thuật toán sắp xếp hiệu quả hơn, với độ phức tạp thấp hơn, chẳng hạn như `Merge Sort`, Chính nhu cầu này đã thúc đẩy sự ra đời của các thuật toán sắp xếp tiên tiến, giúp xử lý dữ liệu lớn một cách nhanh chóng và hiệu quả.

[Video tham khảo](https://www.youtube.com/watch?v=4VqmGXwpLqc)

---


# Ý TƯỞNG

Thuật toán `Merge Sort` dựa trên nguyên tắc `Chia để trị`. Thay vì sắp xếp toàn bộ mảng cùng lúc, ta chia mảng thành các phần nhỏ hơn, sắp xếp từng phần, rồi kết hợp chúng lại để tạo thành mảng đã được sắp xếp hoàn chỉnh.

**Cụ thể:**
1. Chia mảng thành hai nửa, tiếp tục chia mỗi nửa thành hai phần nhỏ hơn, cho đến khi mỗi phần chỉ còn 1 phần tử.
2. Sắp xếp và kết hợp hai phần đã chia bằng cách so sánh và ghép các phần tử theo thứ tự.
3. Lặp lại bước kết hợp cho đến khi được một mảng hoàn chỉnh, đã sắp xếp.

**Độ phức tạp:** O(nlogn)

**Ví dụ: Sắp xếp tăng**

```python
  arr = [3, 8, 5, 2, 9, 4, 1, 7]
```

```python
  arr1 = [3, 8, 5, 2]
  arr2 = [9, 4, 1, 7]
```

```python
  arr1_1 = [3, 8]
  arr1_2 = [5 ,2]

  arr2_1 = [9, 4]
  arr2_2 = [1, 7]
```

```python
  arr1_1_1 = [3]
  arr1_1_2 = [8]

  arr1_2_1 = [5]
  arr1_2_1 = [2]

  arr2_1_1 = [9]
  arr2_1_2 = [4]

  arr2_2_1 = [1]
  arr2_2_2 = [7]

  khi không thể chia hết ta tiến hành sắp xếp và kết hợp 2 phần đã chia
```


```python
  arr1_1_1 = [3]
                   → arr_1_1 = [3,8]
  arr1_1_2 = [8]
                                      → arr_1 = [2, 3, 5, 8]
  arr1_2_1 = [5]
                   → arr_1_2 = [2,5]
  arr1_2_1 = [2]
                                                                 → arr = [1, 2, 3, 4, 5, 7, 8, 9]
  arr2_1_1 = [9]
                   → arr_2_1 1= [4,9]
  arr2_1_2 = [4]
                                      → arr_2 = [1, 4, 7, 9]
  arr2_2_1 = [1]
                   → arr_2_2 = [1,7]
  arr2_2_2 = [7]

```



# 📌 CHÚ Ý: CÁCH THỰC HIỆN BƯỚC `MERGE`

Trong phần này, ta sẽ tìm hiểu cách `Merge` hai mảng đã được sắp xếp tăng dần để tạo ra một mảng mới, vẫn giữ thứ tự tăng dần. Quy trình này là bước quan trọng trong thuật toán `Merge Sort`. Cách thực hiện như sau:

**BƯỚC 1**: Khởi tạo hai chỉ số (`i` cho mảng thứ nhất, `j` cho mảng thứ hai) bắt đầu từ `0`, và một mảng tạm thời `merge_array` để lưu kết quả.

**BƯỚC 2**: So sánh phần tử tại vị trí `i` của mảng thứ nhất với phần tử tại vị trí `j` của mảng thứ hai.

**BƯỚC 3**: Chọn phần tử nhỏ hơn, thêm vào `merge_array`, và tăng chỉ số của mảng chứa phần tử vừa chọn lên `1`.

**BƯỚC 4**: Lặp lại BƯỚC 2 và BƯỚC 3 cho đến khi một trong hai mảng không còn phần tử nào để so sánh.

**BƯỚC 5**: Nếu một mảng còn phần tử, thêm tất cả phần tử còn lại của mảng đó vào `merge_array`.

**Ví dụ minh họa**:
Giả sử ta có hai mảng đã sắp xếp tăng dần:

```python
 arr_1 = [2, 5]
 arr_2 = [1, 4]
```

```python
         ↓ i
 arr_1 = [2, 5]

         ↓ j
 arr_2 = [1, 4]


 ta thấy arr_1[i] = 2, arr_2[j] = 1 và 1 < 2 nên ta sẽ lấy 1 đưa vào merge_arr và tăng j lên 1
```

```python
 merge_arr = [1]

          ↓ i
 arr_1 = [2, 5]

             ↓ j
 arr_2 = [1, 4]

 ta thấy arr_1[i] = 2, arr_2[j] = 4 và 2 < 4 nên ta sẽ lấy 2 đưa vào merge_arr và tăng i lên 1

```

```python
 merge_arr = [1, 2]

             ↓ i
 arr_1 = [2, 5]

             ↓ j
 arr_2 = [1, 4]

 ta thấy arr_1[i] = 5, arr_2[j] = 4 và 4 < 5 nên ta sẽ lấy 4 đưa vào merge_arr và tăng j lên 1

```


```python

 merge_arr = [1, 2, 4]

 vì j đã duyệt hết mảng arr_2 cho nên, ta chỉ cần đưa những phần tử còn lại vào merge_arr

 → merge_arr = [1, 2, 4, 5 ]
```




---

# **CODE**
Phần này tôi sẽ tiến hành code thuật toán `Merge Sort` bằng ngôn ngữ `Python` 😤😤😤


```python
def merge(arr_1: list,arr_2: list) -> list:

  # Trường hợp đầu vào là rỗng
  if arr_1 is None or arr_2 is None:
    return []

  # Tiến hành tạo merge_arr
  merge_arr = []
  i = 0
  j = 0

  # Duyệt 2 mảng cùng lúc
  while i < len(arr_1) and j < len(arr_2):
    # Lấy phần từ bé hơn gán vào merge_arr, đồng thời tăng chỉ mục của mảng đó lên 1
    if arr_1[i] <= arr_2[j]:
      merge_arr.append(arr_1[i])
      i += 1
    else:
      merge_arr.append(arr_2[j])
      j += 1

  # Nếu còn phần tử nào chưa đưa vào merge_arr thì thêm vào
  while i < len(arr_1):
    merge_arr.append(arr_1[i])
    i += 1

  while j < len(arr_2):
    merge_arr.append(arr_2[j])
    j += 1

  return merge_arr

def merge_sort(arr: list) -> list:
  # Nếu chỉ còn 1 phần tử thì trả về luôn
  if len(arr) == 1:
    return arr

  # Tìm điểm chính giữa để chia thành 2 nữa
  middle = len(arr) // 2

  # Nếu nhiều hơn 1 phần từ thì tiến hành chia tiếp
  left = merge_sort(arr[:middle])
  right = merge_sort(arr[middle:])

  # Gọi hàm merge để tiến hành merge 2 phần lại
  return merge(left, right)


arr = [3, 8, 5, 2, 9, 4, 1, 7]
sorted_arr = merge_sort(arr)
print(f"**Mảng sau khi được sắp xếp là: {sorted_arr}")
```

    **Mảng sau khi được sắp xếp là: [1, 2, 3, 4, 5, 7, 8, 9]
    

# **TADA HẾT RỒI !!! 🥳🥳🥳🥳**

Cảm ơn các bạn đã đọc hết bài `notebook` này, mong các bạn góp ý và ủng hộ mình trong các bài `notebook` sau nhé 😌😌😌

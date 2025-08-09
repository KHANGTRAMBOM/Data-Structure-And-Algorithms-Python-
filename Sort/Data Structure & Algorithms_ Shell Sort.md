# **OVERVIEW**
`Shell Sort` là một thuật toán cải tiến từ `Insertion Sort`.

Vấn đề của `Insertion Sort` là: Khi một phần tử cần chèn ở vị trí cách xa vị trí hiện tại, việc duyệt ngược (sang trái) để tìm vị trí chèn đúng sẽ mất nhiều thời gian.

`Shell Sort` ra đời để khắc phục nhược điểm này. Thuật toán chia mảng thành các nhóm nhỏ cách nhau một khoảng (gọi là `gap`), sau đó sắp xếp các nhóm này bằng cách "chèn" giống `Insertion Sort`. Khoảng cách `gap` ban đầu lớn, rồi dần thu hẹp cho đến khi bằng 1, lúc này thuật toán hoạt động như `Insertion Sort` để hoàn thiện sắp xếp.

[![image.png](images/image_1.png)](https://www.youtube.com/watch?v=qzXAVXddcPU)

# **Ý TƯỞNG**
Trong `Shell Sort` các `cặp` phần tử có khoảng cách (`gap`) sẽ được ưu tiên sắp xếp, hay nói rõ hơn là `sắp xếp chèn` lại các `cặp nghịch thế` với khoảng cách `gap`

**Cụ thể:**
1. Tính `gap` ban đầu `gap = len(arr) // 2` (sau đó giảm dần theo công thức `gap = gap // 2` cho đến khi `gap = 0`).
2. So sánh giữa các cặp nghịch thế.
3. `Swap` giá trị (nếu sai vị trí) đồng thời giảm `i = i - gap` rồi thực hiện **Bước 2**.
4. So sánh cặp nghịch thế tiếp theo.
5. Thực hiện lại **Bước 1**, cho đến khi `gap = 0` thì dừng.

```python
 arr = [19, 22, 63, 105, 2, 46]
```

```python
 arr = [19, 22, 63, 105, 2, 46]

 tính gap (ban đầu) = len(arr) // 2
                gap = 6 // 2
                gap = 3
```

```python
 với gap = 3
 ta có các cặp nghịch thế:
 1.(19, 105) → đúng vị trí
 2.(22, 2) → sai vị trí  → swap (22,2)
                                  
                i=1       j=4               i=1       j=4
                ↓           ↓              ↓           ↓  
    2.1 arr = [19, 22, 63, 105, 2, 46] →  [19, 2, 63, 105, 22, 46]
    
        i=-2      j=1              
        ↓          ↓       
    2.2 arr = [19, 22, 63, 105, 2, 46] → i < 0 nên sẽ xét cặp nghịch thế tiếp theo

 3.(63, 46) → sai vị trí → swap → (46,63)

                       i=2       j=5              i=2        j=5
                       ↓           ↓              ↓            ↓  
    3.1 arr = [19, 22, 46, 105, 2, 63] →  [19, 2, 46, 105, 22, 63]

          i=-1       j=2             
          ↓           ↓     
    3.2 arr = [19, 22, 46, 105, 2, 63] →  i < 0
```

```python
 arr = [19, 22, 46, 105, 2, 63]

 tính gap = gap // 2
      gap = 3 // 2
      gap = 1
```

```python
 với gap = 1
 ta có các cặp nghịch thế:
 1. (19,2) → sai vị trí → swap (2,19)
               i=0  j=1                     i=0  j=1
                ↓   ↓                       ↓   ↓
    1.1. arr = [19, 2, 46, 105, 22, 63]  → [2, 19, 46, 105, 22, 63]

           i=-1  j=0                     
             ↓   ↓                     
    1.2. arr = [2, 19, 46, 105, 22, 63]  → i < 0 xét cặp nghịch thế tiếp theo

 2. (19,46) → đúng vị trí
 3. (46,105) → đúng vị trí
 4. (105,22) → sai vị trí → swap (22,105)
                           i=3  j=4                    i=3  j=4
                            ↓   ↓                       ↓   ↓
    4.1. arr = [2, 19, 46, 105, 22, 63]  → [2, 19, 46, 22, 105, 63]

                       i=2  j=3                    i=2  j=3
                        ↓   ↓                       ↓   ↓
    4.2. arr = [2, 19, 46, 22, 105, 63]  → [2, 19, 22, 46, 105, 63]

                   i=2  j=3                 
                    ↓   ↓                  
    4.3. arr = [2, 19, 22, 46, 105, 63] → arr[j] = 22 > arr[i] = 19, không thể chèn nữa dừng → lại xét cặp nghịch thế tiếp theo

 5. (105,63) → sai vị trí → swap (63,105)
                              i=4  j=5                    i=4  j=5
                               ↓   ↓                       ↓   ↓
    5.1 arr = [2, 19, 22, 46, 105, 63]  → [2, 19, 22, 46, 63, 105]

                          i=4  j=5                  
                           ↓   ↓                     
    5.2 arr = [2, 19, 22, 46, 63, 105]  → arr[j] = 63 > arr[i] = 22, không thể chèn nữa dừng → lại xét cặp nghịch thế tiếp theo
```

```python
 arr = [2, 19, 46, 22, 64, 105]

 tính gap = gap // 2
      gap = 1 // 2
      gap = 0

Khi `gap = 0`, thuật toán kết thúc và mảng đã được sắp xếp theo thứ tự tăng dần.
```


# **CODE**
Trong phần này tôi sẽ tiến hành code lại thuật toán `Shell Sort` với ngôn ngữ `Python` 😤😤😤


```python
def shell_sort(arr: list) -> list:
    # Kiểm tra mảng rỗng hoặc chỉ có 1 phần tử
    if len(arr) <= 1:
        return arr

    n = len(arr)
    # Tính gap ban đầu
    gap = n // 2

    # Lặp cho đến khi gap = 0
    while gap > 0:
        # Áp dụng Insertion Sort cho các nhóm phần tử cách nhau gap
        for i in range(gap, n):
            j = i
            # So sánh và hoán đổi nếu phần tử tại j nhỏ hơn phần tử trước nó cách gap
            while j >= gap and arr[j - gap] > arr[j]:
                arr[j - gap], arr[j] = arr[j], arr[j - gap]
                j -= gap
        # Giảm gap
        gap //= 2

    return arr

# Ví dụ sử dụng
arr = [19, 22, 63, 105, 2, 46]
sorted_array = shell_sort(arr)
print(f"Mảng sau khi được sắp xếp: {sorted_array}")
```

# **TADA HẾT RỒI !!! 🥳🥳🥳🥳**

Cảm ơn các bạn đã đọc hết bài `notebook` này, mong các bạn góp ý và ủng hộ mình trong các bài `notebook` sau nhé 😌😌😌

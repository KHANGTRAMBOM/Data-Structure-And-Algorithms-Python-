# **OVERVIEW**
Mảng (`Array`) là một cấu trúc dữ liệu `đơn giản và cơ bản`.

Nó được thiết kế để `lưu trữ nhiều giá trị cùng lúc` mà không cần khai báo từng biến riêng lẻ. Với mảng, bạn có thể `truy cập nhanh` các phần tử thông qua `chỉ số`, giúp tiết kiệm thời gian và tối ưu hóa việc quản lý dữ liệu.

[Video tham khảo](https://www.youtube.com/watch?v=9dr2mHYYoug)

# **Ý TƯỞNG**
Mảng (`Array`) được xây dựng dựa trên cách `phân bố bộ nhớ` trên `RAM`. Mỗi biến được `RAM` cấp` một địa chỉ bộ nhớ` cụ thể. Khi tạo mảng (ví dụ: mảng 10 phần tử), `RAM` cấp phát `10 vùng nhớ liền kề nhau`. Nhờ đó, ta có thể `truy cập` nhanh các phần tử qua `chỉ mục`, cả tuần tự lẫn trực tiếp.

## **PHÂN LOẠI**
Có 2 loại mảng chính:
1. `Mảng tĩnh`: Phải khai báo số lượng phần tử khi khởi tạo, ví dụ `int arr[10]` trong C.
2. `Mảng động`: Có thể tự mở rộng khi đầy, không cần khai báo số lượng phần tử ban đầu, ví dụ danh sách trong Python (`list = [1, 2, 3]`) hay `calloc`, `malloc` trong C

## **THAO TÁC**

### **THÊM**
Để thêm một phần tử vào mảng, ta gán giá trị mới vào vị trí chỉ mục `n` (`arr[n] = new_value`) và tăng số lượng phần tử lên 1. Với mảng tĩnh, cần kiểm tra xem mảng đã đầy chưa trước khi thêm. Với mảng động (như danh sách trong Python), ta dùng phương thức như `append()`.

### **XÓA**
Trong `Array`, thao tác **xóa** là quá trình dịch chuyển các phần tử từ vị trí `i + 1` sang vị trí `i`, bắt đầu từ phần tử cần xóa cho đến phần tử cuối cùng. Sau đó, **giảm số lượng phần tử đi 1**.  
Thực chất, việc **xóa** chỉ là `dịch chuyển các phần tử`, số lượng `vùng nhớ được cấp phát` vẫn còn `tồn tại`, nhưng `bị ẩn` do `số lượng phần tử` được quản lý đã giảm.  
Nói cách khác, tầm nhìn của chúng ta đối với mảng `bị giới hạn` bởi giá trị `"số lượng phần tử"`.

**Ví dụ:**  Mảng có 5 phần tử, xóa 1 phần tử thì bộ nhớ vẫn còn đủ 5 phần tử (vùng nhớ), nhưng ta chỉ thấy 4 phần tử vì giới hạn này.

```python
 arr: int = [0] * 5 # Khởi tạo mảng số nguyên có 5 phần tử
```

```python
 # Gán giá trị cho mảng bằng vị trí chỉ mục (index) của nó
 # arr[0] = 0, arr[1] = 1 ,..., arr[4] = 4
 for i in range(5):
   arr[i] = i

   số lượng phần tử = 5
 → arr = [0, 1, 2, 3, 4]
```
```python
 # Ta tiên hành xóa vị trí 2
             vị trí cần xóa
                ↓
 → arr = [0, 1, 2, 3, 4]

 # Dời giá trị i+1 sang i

                i  i+1
                ↓  ↓
 → arr = [0, 1, 2, 3, 4]

                i  i+1
                ↓  ↓
 → arr = [0, 1, 3, 3, 4] →  đồng thời tăng i lên 1

                   i
                   ↓
 → arr = [0, 1, 2, 3, 4]
```

```python
                   i  i+1
                   ↓  ↓
 → arr = [0, 1, 3, 3, 4]


 → arr = [0, 1, 3, 4, 4] → Đồng thời tăng i lên 1

                      i
                      ↓
 → arr = [0, 1, 3, 4, 4] → Do i đã bằng số lượng phần tử - 1 nên ta dừng. Đồng thời giảm số lượng phần tử đi 1

 số lượng phần tử = số lượng phần tử - 1
                  = 5 - 1    
                  = 4                         
```

```python
       <có thể thấy>
 arr = [0, 1, 3, 4,  4]
 Khi xóa xong mảng vẫn còn 5 phần tử, bộ nhớ trong RAM vẫn còn 5 vùng nhớ nằm cạnh nhau nhưng bị số lượng phần tử "che" lại rồi
```

# **CODE**
Phần này, tôi sẽ viết lại cấu trúc `Array` bằng ngôn ngữ `Python`, mô phỏng cách hoạt động của `mảng tĩnh` trong các` ngôn ngữ như `C. Mặc dù Python đã có kiểu danh sách `List` (mảng động) với đầy đủ chức năng, tôi sẽ quản lý thủ công số lượng phần tử và thực hiện các thao tác thêm/xóa để minh họa rõ cách hoạt động của mảng. 😤😤😤


```python
class my_array:
  def __init__(self, n: int):
    self.max_size = n
    self.n = 0
    # Tạo một List có n phần tử
    self.mylist = [0] * n

  def isFull(self) -> bool:
    return self.n == self.max_size

  # Hàm thêm phần tử
  def append(self, value: int):
    if self.isFull():
      return False

    self.mylist[self.n] = value
    self.n += 1

    return True

  def delete(self, position: int) -> bool:
    # Vị trí xóa không hợp lệ
    if position >= self.n or position < 0:
       raise IndexError("Index is out of range")
       return False

    for i in range(position, self.n - 1):
      self.mylist[i] = self.mylist[i + 1]

    self.n -= 1
    return True

  def __getitem__(self, index: int) -> int:
    # Kiểm tra chỉ mục hợp lệ
    if index >= self.n or index < 0:
        raise IndexError("Index is out of range")

    return self.mylist[index]

  def __str__(self):
    result = "["
    for i in range(self.n):
      result += f"{self.mylist[i]}"
      if i != self.n - 1:
        result += ", "
    return result + "]"


# Tạo mảng có 10 phần tử
mylist = my_array(5)

# Gán giá trị cho các phần tử trong mảng
for i in range(5):
  mylist.append(i)

# In mảng ra
print("="*30)
print(f"mylist: {mylist}")

print("="*30)
print(f"Phần tử thứ {2} là: {mylist[2]}")

print("="*30)
# Vì mỗi phần tử int trong python là 32 byte (0x20 trong hệ 16 hexa)
# Do đó mỗi phần tử sẽ cách nhau 0x20
for i in range(5):
  print(f"Địa chỉ của phần tử mylist[{i}]: {hex(id(mylist[i]))}")
print("="*30)
print("Tiến hành xóa phần tử ở vị trí 2 ....")
mylist.delete(2);
print("Xóa phần tử ở vị trí 2 thành công !!!")
print(f"mylist sau khi xóa phần tử thứ {2} là: {mylist}")
```

    ==============================
    mylist: [0, 1, 2, 3, 4]
    ==============================
    Phần tử thứ 2 là: 2
    ==============================
    Địa chỉ của phần tử mylist[0]: 0xa42648
    Địa chỉ của phần tử mylist[1]: 0xa42668
    Địa chỉ của phần tử mylist[2]: 0xa42688
    Địa chỉ của phần tử mylist[3]: 0xa426a8
    Địa chỉ của phần tử mylist[4]: 0xa426c8
    ==============================
    Tiến hành xóa phần tử ở vị trí 2 ....
    Xóa phần tử ở vị trí 2 thành công !!!
    mylist sau khi xóa phần tử thứ 2 là: [0, 1, 3, 4]
    

# **MỘT SỐ BÀI TẬP RÈN LUYỀN**
Do bài `Notebook` này là bài đầu tiên trong chuỗi series cũng như la bài cơ bản nhất, với lại các bài tập về `array` nhiều vô số kể. Do đó tôi sẽ giới thiệu một số bài tập cơ bản để bạn có thể rèn luyện thuật toán nhé. 😜😜😜

## **CHUYỂN CƠ SỐ 2 SANG CƠ SỐ 10**
**Ý TƯỞNG:**
Để chuyển một số nhị phân (cơ số 2) như `101` sang số thập phân (cơ số 10), ta thực hiện các bước sau:
1. Duyệt từng chữ số của số nhị phân từ phải sang trái, bắt đầu từ vị trí 0.
2. Nhân chữ số tại vị trí `i` với lũy thừa $2^i$.
3. Cộng dồn kết quả vào biến thập phân `dec` (dec = dec + (chữ số * $2^i$)).
4. Lặp lại bước 1 cho đến khi duyệt hết các chữ số.


```python
def convert2to10(binary: int) -> int:
  # Kết quả thập phân
  dec = 0
  # Hàng số bắt đầu
  pos = 0

  # Thực hiện lập
  while binary > 0:
    # Lấy số ở hàng pos
    digit = binary % 10
    # Tính toán cộng vào kết quả
    dec = dec + digit * (2**pos)
    # Tăng hàng số lên
    pos += 1
    binary = binary // 10

  return dec

# Số nhị phân cần chuyển
binary = 1010100
decimal = convert2to10(binary)
print(f"**Số nhị phân {binary} sao khi chuyển sang thập phân là: {decimal}")
```

    **Số nhị phân 1010100 sao khi chuyển sang thập phân là: 84
    

## **CHUYỂN CƠ SỐ 10 SANG CƠ SỐ 16**
**Ý TƯỞNG**: Để chuyển một số thập phân (cơ số 10) sang số thập lục phân (cơ số 16), ta thực hiện các bước sau:
1. Chia số `n` (cơ số 10) cho 16, lấy phần dư và phần nguyên.
2. Thêm phần dư vào chuỗi kết quả. Nếu phần dư từ 10 đến 15, thay bằng ký tự `A` đến `F`. Phần nguyên được dùng để tiếp tục chia.
3. Lặp lại bước 1 cho đến khi phần nguyên bằng 0.
4. Đảo ngược chuỗi kết quả để được số thập lục phân đúng.


```python
def dec2hex(dec: int) -> str:
  # Kết quả thập lục phân
  hex = ""
  # Hàng số bắt đầu
  pos = 0
  hex_mapping = ['0', '1', '2', '3', '4', '5', '6', '7', '8', '9','A', 'B', 'C', 'D', 'E', 'F']
  # Thực hiện lập
  while dec > 0:
    # Lấy phần dư
    index = dec % 16
    # Ánh xạ lấy số phù hợp rồi gán vào hex
    hex = hex_mapping[index] + hex
    # Phần nguyên làm nguyên liệu cho lần chia tiếp
    dec = dec // 16
  return hex

# Số nhị phân cần chuyển
dec = 1715004
hex_number = dec2hex(dec)
print(f"**Số thập phân {dec} chuyển sang heximal là: {hex_number}")
```

    **Số thập phân 1715004 chuyển sang heximal là: 1A2B3C
    

## **CHUYỂN CƠ SỐ N SANG CƠ SỐ M**
**Ý TƯỞNG**: Để chuyển một số từ cơ số `N` sang cơ số `M` (với `N`, `M` từ 2 đến 36), ta thực hiện các bước sau:
1. Chuyển số từ cơ số `N` sang `cơ số 10` (Bài 1).
2. Chuyển số từ `cơ số 10` sang cơ số `M` (Bài 2).


```python
# Hàm chuyển từ cơ số N sang cơ số 10
def convertNto10(n: str , base: int) -> int:

  # Nếu cơ số không hợp lệ
  if base < 2 or base > 36:
    raise IndexError("Base is out of range")

  if n == "0":
    return "0"

  # Đảm bảo các kí tự luôn in HOA
  n = n.upper()

  mapping = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ"
  length = len(n)
  pos = 0
  dec = 0

  # Tiến hành chuyển N sang 10
  while length > 0:
    # Lấy phần tử cuối
    digit = n[length - 1]

    # Ánh xạ xem nó là số bao nhiêu
    for i in range(len(mapping)):
      if digit == mapping[i]:
        digit = i
        break

    # Nhân với mũ của cơ số rồi cộng thêm vào kết quả
    dec = dec + digit * (base ** pos)
    pos += 1
    length -= 1

  return dec

# Hàm chuyển từ cơ số 10 sang cơ số M
def convert10toM(n: int, base: int) -> str:

  # Nếu cơ số không hợp lệ
  if base < 2 or base > 36:
    raise IndexError("Base is out of range")

  if n == 0:
    return "0"

  result = ""
  mapping = "0123456789ABCDEFGHIJKLMNOPQRSTUVWXYZ"

  # Tiến hành chuyển 10 sang M
  while n > 0:
    # Láy phần dư
    digit = n % base
    # Gán vào theo kiểu ngược
    result = mapping[digit] + result
    # Phần nguyên sẽ được dùng dể tiếp tục chia
    n = n // base
  return result

# Hàm chuyển cơ số N sang cơ số M
def convertN2M(n: int, baseN: int, baseM: int) -> str:
  # Chuyển N sang 10
  dec = convertNto10(n, baseN)

  # Chuyển 10 sang M
  return convert10toM(dec, baseM)


# Giả sử chuyển tử hex sang binary
Hex="1A2B3C"
BaseN = 16
BaseM = 2

result = convertN2M(Hex, BaseN, BaseM)
print(f"**Chuyển từ cơ số {BaseN} sang cơ số {BaseM} với {Hex} là : {result}")
```

    **Chuyển từ cơ số 16 sang cơ số 2 với 1A2B3C là : 110100010101100111100
    

## **ĐẢO NGƯỢC CHUỖI**
**Ý TƯỞNG**: Để đảo ngược một chuối (hoặc mảng), ta có thể sử dụng một trong hai phương pháp sau:

1. **Sử dụng mảng phụ**: Tạo một` mảng mới`, duyệt mảng gốc `từ cuối về đầu` và gán các phần tử vào mảng mới theo thứ tự tuần tự.
2. **Sử dụng hai con trỏ**: Dùng một `con trỏ` `từ đầu mảng` và một `con trỏ` `từ cuối mảng`. `Hoán đổi giá trị` tại hai con trỏ, sau đó `tăng `con trỏ đầu và `giảm` con trỏ cuối, lặp lại đến khi hai con trỏ gặp nhau.

**Link bài:** [**ĐẢO NGƯỢC CHUỖI**](https://leetcode.com/problems/reverse-string/)


```python
class Solution:
    def reverseString(self, s: List[str]) -> None:
        n = len(s)
        # Tạo 2 con trỏ trái và phải
        left = 0
        right = n -1

        # Tiến hành duyệt
        while left < right:
            # Hoán đổi giá trị giữa 2 con trỏ
            s[left], s[right] = s[right], s[left]
            left += 1
            right -= 1

        return s
```

## **KIỄM TRA CHUỖI ĐỐI XỨNG**
**Ý TƯỞNG**: Tương tự với cách làm của [**ĐẢO NGƯỢC CHUỖI**](https://leetcode.com/problems/reverse-string/) ta chỉ cần thay vì hoán đổi giá trị ta sẽ kiễm tra giá trị nó có bằng nhau không


**Link bài:** [**KIỄM TRA CHUỖI ĐỐI XỨNG**](https://leetcode.com/problems/valid-palindrome/)


```python
class Solution:
    def isPalindrome(self, s: str) -> bool:
        n = len(s)
        # Đưa chuỗi về dạng chữ thường
        s = s.lower()
        # Tạo 2 con trỏ
        left = 0
        right = n -1

        while left < right:
            while left < n :
                c_left = s[left]
                # Kiễm tra con trỏ hiện tại có là kí tự hoặc số không, nếu không thì kiễm tra tiếp
                if ord(c_left) >= ord('a') and ord(c_left) <= ord('z') or ord(c_left) >= ord('0') and ord(c_left) <= ord('9'):
                    left += 1
                    break
                left += 1

            while right >= 0:
                c_right = s[right]
                # Kiễm tra con trỏ hiện tại có là kí tự hoặc số không, nếu không thì kiễm tra tiếp
                if ord(c_right) >= ord('a') and ord(c_right) <= ord('z') or ord(c_right) >= ord('0') and ord(c_right) <= ord('9'):
                    right -= 1
                    break
                right -= 1

            # Nếu cả 2 con trỏ đi hết mảng thì có nghĩa là không có kí tự hoặc số nào
            if right == -1 and left == n:
                return True

            # Kiễm tra 2 kí tự được phát hiện giống nhau không
            if c_right != c_left:
                return False

        return True
```

## **KIỂM TRA SỐ ĐỐI XỨNG**
**Ý TƯỞNG**: Để kiểm tra một số có `đối xứng` hay không (tức là số đọc xuôi hay ngược đều giống nhau), ta xây dựng `số đảo ngược` từ `số ban đầu` và `so sánh với số gốc`:
1. Lấy `phần dư` và `phần nguyên` của số `n` khi chia cho `10` để lấy `từng chữ số`.
2. Cập nhật `n_reverse` bằng cách nhân `n_reverse` với `10` và cộng phần dư (`n_reverse = n_reverse * 10 + remainder`).
3. Dùng `phần nguyên` để tiếp tục **bước 1**, lặp lại cho đến khi `phần nguyên` bằng 0.
4. So sánh `n_reverse` với số gốc `n`. Nếu bằng nhau, số đó đối xứng.


```python
# Hàm kiễm tra số đối xứng
def palindrome_number(num : int) -> bool:
  # Số âm không phải số đối xứng
  if num < 0:
    return False

  # Tạo số reverse
  reverse = 0
  temp = num
  # Tiến hành tạo số đối xứng
  while num > 0:
    digit = num % 10
    reverse = reverse * 10 + digit
    num = num // 10

  # Kiễm tra kết quả
  return reverse == temp

num = 123454321
print(f"**Số {num} có phải là số đối xứng không: {palindrome_number(num)}")
```

    **Số 123454321 có phải là số đối xứng không: True
    

# **TADA HẾT RỒI !!! 🥳🥳🥳🥳**

Cảm ơn các bạn đã đọc hết bài `notebook` này, mong các bạn góp ý và ủng hộ mình trong các bài `notebook` sau nhé 😌😌😌

Writeup - Challenge: forensics/deep-layers
![image](https://github.com/user-attachments/assets/89717294-091b-482f-a037-d12eaf273480)

Mô tả:
Tên: deep-layers
Thể loại: Forensics

Gợi ý: "Not everything ends where it seems to..."

---

Bước 1: Phân tích file ảnh PNG

Mở file 'chall.png' bằng trình hex hoặc đọc nhị phân. Tìm thấy sau chunk kết thúc chuẩn IEND có thêm dữ liệu lạ.
![image](https://github.com/user-attachments/assets/57c1db3d-6e62-4cb4-ad46-76d20a40ef09)

---

Bước 2: Phát hiện file ZIP

Sau IEND có chuỗi bắt đầu bằng 'PK\x03\x04' => dấu hiệu của file ZIP được nhúng.

Giải nén ra được file: secret.gz

Có thể dùng binwalk, nhưng ở đây do đang dùng win nên mình dùng python:
`with open("chall.png", "rb") as f:
    data = f.read()

start = data.find(b"PK\x03\x04")
with open("hidden.zip", "wb") as f:
    f.write(data[start:])
``
---

Bước 3: File GZ bị mã hóa

Khi giải nén file secret.gz, yêu cầu nhập mật khẩu.

---

Bước 4: Tìm mật khẩu ẩn trong metadata

Dùng Python trích xuất các chuỗi có thể đọc từ file PNG:
```
import re
import base64

# Đọc toàn bộ file PNG dạng nhị phân
with open("chall.png", "rb") as f:
    data = f.read()

# Tìm tất cả các chuỗi ASCII có độ dài >= 4 ký tự
ascii_strings = re.findall(rb"[ -~]{4,}", data)

# Giải mã chuỗi từ bytes sang utf-8
decoded_strings = []
for s in ascii_strings:
    try:
        decoded_strings.append(s.decode("utf-8"))
    except UnicodeDecodeError:
        continue

# Tìm chuỗi base64 sau metadata có tên như 'Password'
password_b64 = None
for i, line in enumerate(decoded_strings):
    if "Password" in line and i + 1 < len(decoded_strings):
        password_b64 = decoded_strings[i + 1]
        break

# Giải mã base64 nếu tìm thấy
if password_b64:
    try:
        password = base64.b64decode(password_b64).decode("utf-8")
        print("🔐 Mật khẩu tìm được là:", password)
    except Exception as e:
        print("⚠️ Lỗi khi giải mã base64:", e)
else:
    print("❌ Không tìm thấy mật khẩu trong file.")
```
Tìm thấy:

%iTXtPassword
cDBseWdsMHRwM3NzdzByZA==

Giải mã base64: p0lyg1tp3ssw0rd

---

Bước 5: Giải nén và lấy flag

Dùng mật khẩu để giải nén file secret.gz, thu được:

Flag: tjctf{p0lygl0t_r3bb1t_h0l3}

---

Kết luận:

Flag cuối cùng:
tjctf{p0lygl0t_r3bb1t_h0l3}

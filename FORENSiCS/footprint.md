# 🕵️‍♂️ TJCTF 2024 – Forensics: `footprint`
![image](https://github.com/user-attachments/assets/e8d867ea-47d7-4c99-a362-b89a5ee6619c)

**Category:** Forensics  

> The folder used to hold some important files — including one with the flag as its name.  
> Unfortunately, all the files were deleted. Can you piece together the flag from what’s left behind?

---

## 🧠 Challenge Analysis

Chúng ta được cung cấp một file `.DS_Store`. Đây là file ẩn do Finder của macOS tạo ra để lưu **metadata về thư mục** — bao gồm cả **tên file** trong thư mục, kể cả khi chúng đã bị xóa.

Flag được giấu trong **tên file**, nên `.DS_Store` là một hướng đi hợp lý để truy vết.

---

## 🛠️ Solution Steps

### 1. Unzip file

Sau khi tải file `.zip` về:

```bash
unzip files.zip
```

→ Thư mục `files/` chứa một file `.DS_Store`.

---

### 2. Phân tích sơ bằng `strings`

```bash
strings files/.DS_Store
```

Kết quả chỉ có vài chuỗi lặp đi lặp lại, nổi bật nhất là `Bud1`, nhưng thử flag `flag{Bud1}` **không đúng**.

---

### 3. Dùng tool `ds_store` để đọc đúng cấu trúc

Cài thư viện chuyên phân tích `.DS_Store`:

```bash
pip install ds_store
```

Tạo script Python để in ra các tên file đã từng tồn tại:

```python
from ds_store import DSStore

with DSStore.open("files/.DS_Store", "r") as d:
    for entry in d:
        print(entry.filename)
```

Chạy script:

```bash
python3 read_dsstore.py
```

---

### 4. Kết quả trả về

Một danh sách rất dài gồm các chuỗi kiểu base64 hoặc ngẫu nhiên. Trong số đó có 2 chuỗi đáng chú ý:

```txt
dGpjdGZ7ZHNfc3RvcmVfIA
aXNfdXNlZnVsP30gICAgIA
```

Giải mã từng phần bằng `base64`:

```bash
echo dGpjdGZ7ZHNfc3RvcmVfIA== | base64 -d
# tjctf{ds_store_

echo aXNfdXNlZnVsP30gICAgIA== | base64 -d
# is_useful?}
```

---

### ✅ Flag:

```txt
tjctf{ds_store_is_useful?}
```

---

## 🧩 Lessons Learned

- `.DS_Store` lưu **danh sách tên file**, kể cả sau khi file đã bị xóa.
- Trong forensics, phân tích metadata có thể là chìa khóa duy nhất để phục hồi thông tin.
- Dữ liệu nhị phân (như `.DS_Store`) nên được phân tích bằng **công cụ chuyên dụng**, không chỉ `strings`.

---

**Flag:** `tjctf{ds_store_is_useful?}`  
**Status:** ✅ Solved

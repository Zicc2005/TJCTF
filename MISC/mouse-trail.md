# 🕵️‍♂️ TJCTF 2024 – Forensics: `mouse-trail`

**Category:** Misc
![image](https://github.com/user-attachments/assets/445b838f-4d45-4d3b-b3e1-133f5685855e)
Ta sẽ nhận dc 1 file txt dạng tọa độ 
![image](https://github.com/user-attachments/assets/c03bd30b-fff0-4df5-842a-3ee4ea40636e)

Sau khi đọc đề ta hiểu là đây sẽ là dạng phần misc phân tích file tạo độ.Cứ cứt lên gpt gen code vẽ dữ liệu:
```
import matplotlib.pyplot as plt

def read_mouse_data(filename):
    coords = []
    with open(filename, 'r') as f:
        for line in f:
            try:
                x_str, y_str = line.strip().split(',')
                x, y = int(x_str), int(y_str)
                coords.append((x, y))
            except ValueError:
                continue  # bỏ qua dòng lỗi
    return coords

def plot_mouse_path(coords):
    if not coords:
        print("Không có dữ liệu để vẽ.")
        return

    x_vals, y_vals = zip(*coords)

    plt.figure(figsize=(10, 6))
    plt.plot(x_vals, y_vals, marker='o', linestyle='-', color='blue', alpha=0.7)
    plt.title("Đường đi của chuột")
    plt.xlabel("X")
    plt.ylabel("Y")
    plt.gca().invert_yaxis()  # Đảo trục Y nếu bạn dùng hệ toạ độ màn hình
    plt.grid(True)
    plt.show()

if __name__ == "__main__":
    filename = "mouse_movements.txt"
    coords = read_mouse_data(filename)
    plot_mouse_path(coords)
```
Sau đó chạy file và ta có ảnh:
![image](https://github.com/user-attachments/assets/e99862ad-c3e1-434a-8ac4-d9c978ffcf63)

Đọc đoạn mở trong ảnh ta có flag:
TJCTF{we__cartesian_plane}

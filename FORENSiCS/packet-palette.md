![image](https://github.com/user-attachments/assets/7141ebbc-ae11-4a02-aaba-e7156e5824d1)
Ở đây khi đưa file pcapng vào x64 để check ta để ý có 1 lớp mã hóa phai trước 1 file PNG.
![image](https://github.com/user-attachments/assets/a7b15f39-a377-485e-afce-a61de991824e)

Sau đó copy toàn bộ mã từ %PNG rồi paste sang 1 file mới save dạng .png 
Đây là file ta có:

![image](https://github.com/user-attachments/assets/8e585b86-33b9-401a-8f94-3f803fc42d7b)
Nhìn kĩ xíu có thể thấy giữa những dòng ảnh nhiễu có 1 vài kí tự.
Bật file .pcapgn băgf wireshare để check:

![image](https://github.com/user-attachments/assets/78007b30-793b-43a1-840b-2a0c52b2aaf6)

Hãy để ý 23 packet này đều có 1 đoạn mã hóa chèn trước đoạn PNG:![image](https://github.com/user-attachments/assets/9cc6eb27-e37a-48b9-9c97-92def58512a1)

Do đó ta nhận ra 23 packet này đều ẩn 1 file png, em đưa vào linux dùng tshare để xuất dc tất cả mã hóa ra 1 file txt:
tshark -r chall.pcapng -Y "tcp" -T fields -e tcp.payload > payloads.txt
![image](https://github.com/user-attachments/assets/864a090e-25a3-484d-84e7-99addbff0a02)


Sau đó ta vim vào file payloads.txt để đọc file:
![Screenshot 2025-06-10 224539](https://github.com/user-attachments/assets/967ecf5e-de1b-45ed-b5bb-75330e17829d)
 Để ý thì bất kì 1 khúc nào đều có 1 đoạn ở đầu là 555349000...f4.
 Đây là đoạn mã hóa chèn để khiến ảnh kh dc đọc hoặc xác định.H hãy xóa đoạn đó và ném vào cyberchef, from hex + raw.Sau khi ném đủ 23 packet thì ta sẽ ra dc flag:
 ![Screenshot 2025-06-10 231809](https://github.com/user-attachments/assets/e3d74e6e-5aa6-4864-a8ab-6b3e0a0db1d0)

 

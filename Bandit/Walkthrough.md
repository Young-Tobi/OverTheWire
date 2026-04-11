# Level 0 

Mục tiêu của level này là đăng nhập vào trò chơi bằng cách sử dụng SSH. Kết nối với 
- Host: `bandit.labs.overthewire.org` trên port `2220`
- Username: `bandit0`
- Password: `bandit0`

Ta có câu lệnh kết nối SSH với port riêng:
```
ssh username@hostname -p [port]
```

![alt text](images/image.png)

# Level 0 -> Level 1

Mật khẩu cho level tiếp theo được lưu trong file `readme` nằm trong thư mục chính. Sử dụng mật khảu này để đăng nhập vào bandit1 sử dụng SSH

Sử dụng `ls` để liệt kê danh sách tệp tin và thư mục
- `ls`: Liệt kê các tệp/thư mục trong thư mục hiện tại (không bao gồm tệp ẩn).
- `ls -a`: Liệt kê tất cả tệp, bao gồm cả các tệp ẩn (tên bắt đầu bằng dấu .).
- `ls -l`: Hiển thị danh sách dài (chi tiết về quyền, người sở hữu, kích thước, thời gian).

Sau đó, sử dụng `cat` để đọc, hiển thị nội dung tệp tin
![alt text](images/image-1.png)

-> Password: `ZjLjTmM6FvvyRnrb2rfNWOZOTa6ip5If`

# Level 1 -> Level 2

Mật khẩu cho level tiếp theo được lưu trong file `-` nằm trong thư mục chính

Sử dụng lệnh SSH để kết nối tới server với user: `bandit1` và password tìm được ở level trước

Sử dụng lệnh `ls`, kết quả hiển thị một file có tên `-`

Trong Linux, kí tự `-` thường được hiểu là `option (tham số)` của lệnh, không phải tên file. Do đó không thể truy cập file theo cách thông thường

Cách xử lí:
- Cách 1: Sử dụng đường dẫn tương đối

```
cat ./-
```

`./` đại diện cho thư mục hiện tại
-> Giúp hệ thống hiểu rõ `-` là tên file

- Cách 2: Sử dụng kí hiệu `--`

```
cat -- -
```
`--` báo hiệu kết thúc các option
-> Các tham số phía sau sẽ được hiểu là tên file

![alt text](images/image-2.png)

-> Password: `263JGJPfgU6LtdEvgfWU1XP5yac29mFx`

# Level 2 -> Level 3

Mật khẩu cho level tiếp theo được lưu trong file
```
-- spaces in this filename --
```
nằm ở trong thư mục chính

Tên file chứa `dấu cách (spaces)` -> Shell sẽ hiểu mỗi phần là một argument riêng biệt

Ví dụ:
```
cat --spaces in this filename--
```
Shell sẽ hiểu thành:
- `--spaces`
- `in`
- `this`
- `filename--`

Cách xử lí: Để xử lí tên file chứa dấu cách, cần gom toàn bộ tên file thành một chuỗi
-> Dùng dấu ngoặc kép `" "`

![alt text](images/image-3.png)

-> Password: `MNk8KNH3Usiio41PRUEoDFPqfxLPlSmx`

# Level 3 -> Level 4

Mật khẩu cho level tiếp theo được lưu trong 1 file ẩn trong thư mục `inhere`

Sử dụng `ls -a` để liệt kê tất cả các tệp, bao gồm cả tệp ẩn

![alt text](images/image-4.png)

-> Password: `2WmrDFRmJIq3IPxneAaMGhap0pFhF3NJ`

# Level 4 -> Level 5

Mật khẩu cho level tiếp theo được lưu trong `file duy nhất có thể đọc được (human-readable)` nằm trong thư mục `inhere`

Di chuyển vào thư mục `inhere` và liệt kê các file, kết quả sẽ thấy nhiều file dạng:

```
-file00
-file01
-file02
...
```

- Các file đều có tên bắt đầu bằng `-` -> dễ bị hiểu là option
- Không biết file nào chứa nội dung `readable`
- Một số file là `binary` -> Nếu `cat` sẽ làm terminal bị loạn

Cách xử lí:
- Xác định loại file bằng cách sử dụng lệnh `file` để kiểm tra
  
-> Kết quả sẽ có dạng: ASCII text / data / binary

- `cat` file ASCII text để tìm được mật khẩu

![alt text](images/image-5.png)

-> Password: `4oQYVPkxZOOEOO5pTW81FB8j8lxXGUQw`

# Level 5 -> Level 6

Mật khẩu cho level tiếp theo được lưu trữ trong một tệp tin nằm ở đâu đó trong thư mục `inhere` và có tất cả các thuộc tính sau:

- `human-readable`
- `1033 bytes in size`
- `not executable`

Vì thư mục `inhere` chứa nhiều thư mục con và file nên không thể tìm thủ công. Do đó cần lọc theo:
- type: file thường
- size: 1033 bytes
- permission: không `executable`

Sau đó kiểm tra human-readable

![alt text](images/image-6.png)


```
find inhere -type f -size 1033c ! -executable
```

- `inhere` : thư mục tìm kiếm
- `-type f`: chỉ lấy file
- `-size 1033c`: 1033 bytes
- `! -executable`: không có quyền thực thi

```
file inhere/maybehere07/.file2
```
-> `ASCII text`

-> Password: `HWasnPhtq9AVKe0dmk45nxy20cvUa6EG`

# Level 6 -> Level 7

Mật khẩu cho level tiếp theo được lưu trữ ở đâu đó trên server và có các thuộc tính sau:
- `Owner (user)`: bandit7
- `Group`: bandit6
- `size:` 33 bytes

Tìm file với `find`
```
find / -type f -user bandit7 -group bandit6 -size 33c 2>/dev/null
```
- `/`: Tìm toàn bộ hệ thống
- `-type f`: Chỉ file
- `-user bandit7`: owner là bandit7
- `-group bandit6`: group là bandit6
- `size 33c`: đúng 33 bytes
- `2>/dev/null`: ẩn lỗi permission denied

Sau khi tìm đường đường dẫn file, sử dụng `cat` để đọc được mật khẩu

![alt text](images/image-7.png)

-> Password: `morbNTDkSW6jIlUc0ymOdMaLnOlFVAaj`

# Level 7 -> Level 8

Mật khẩu của level tiếp theo được lưu trong file `data.txt` nằm cạnh từ `millionth`

Vì file `data.txt` chứa rất nhiều dòng dữ liệu nên không thể tìm thủ công

Cách xử lí: Tìm dòng có chứa từ `millionth` và lấy giá trị nằm cạnh từ đó (chính là password)

Sử dụng lệnh `grep` để tìm kiếm các mẫu văn bản, chuỗi kí tự hoặc biểu thức chính quy bên trong tập tin
```
grep "millionth" data.txt
```

![alt text](images/image-8.png)

-> Password: `dfwvzFQi4mU0wfNbFOe9RoWskMLg7eEc`

# Level 8 -> Level 9

Mật khẩu cho level tiếp theo được lưu trong file `data.txt` và là dòng duy nhất chỉ xuất hiện một lần

Vì file `data.txt` chứa rất nhiều dòng:
- Có dòng bị lặp lại nhiều lần
- Chỉ có 1 dòng duy nhất xuất hiện đúng 1 lần

Vì vậy, cần loại bỏ các dòng trùng và tìm dòng `unique`

*Note*: Lệnh `uniq` chỉ hoạt động đúng khi các dòng giống nhau phải đứng cạnh nhau -> Cần sort trước
```
sort data.txt | uniq -u
```
- `sort data.txt`: Sắp xếp các dòng
- `uniq -u`: Chỉ giữ lại dòng xuất hiện đúng một lần

Ngoài ra còn một số biến thể của `uniq`:
- Lọc dòng duy nhất: `uniq file.txt` (chỉ hiển thị 1 dòng đại diện cho các dòng liền kề giống nhau).
- Đếm số lần xuất hiện: `uniq -c file.txt` (đếm số lần mỗi dòng xuất hiện).
- Chỉ hiển thị dòng trùng lặp: `uniq -d file.txt`.
- Chỉ hiển thị dòng độc nhất (không trùng): `uniq -u file.txt`.
- Bỏ qua phân biệt chữ hoa/thường: `uniq -i file.txt`.

![alt text](images/image-9.png)

-> Password: `4CKMh1JI91bUIZZPXDqGanal4xvAg0JM`

# Level 9 -> Level 10

Mật khẩu cho level tiếp theo được lưu trữ trong file `data.txt` 
- Nằm trong một chuỗi có thể đọc được (human-readable string)
- Đứng sau nhiều kí tự `=`

Sau khi `cat`, chủ yếu file `data.txt` chứa:
- Các `binary data`
- Chỉ có một vài đoạn text có thể đọc được

Cách xử lí:
- Đầu tiên phải trích xuất các chuỗi readable: `strings data.txt`

-> Lệnh này sẽ kich ra các chuỗi text từ file binary

- Sau đó tìm các chuỗi có chứa dấu `=`: 
`grep "==="`

![alt text](images/image-10.png)

-> Password: `FGUW5ilLVJrxX9kMYMmlN4MgbpfMiqey`

# Level 10 -> Level 11

Mật khẩu cho level tiếp theo được lưu trong file `data.txt`, nơi chứa dữ liệu được mã hóa `base64`

Sau khi `cat` file data.txt, ta thấy nội dung có dạng: `VGhpcyBpcyBlbmNvZGVkIHN0cmluZw==`

Đây là `Base64 encoding`, vì vậy ta sử dụng lệnh sau để decode:
```
base64 -d data.txt
```
- `-d`: decode
- Chuyển từ Base64 -> Plaintext

![alt text](images/image-11.png)

-> Password: `dtR173fZKb0RRsDFSGsg2RWnpNVj3qRr`

# Level 11 -> Level 12

Mật khẩu cho level tiếp theo được lưu trong file `data.txt`, nơi tất cả chữ cái thường (a-z) và chữ hoa (A-Z) đã bị xoay 13 vị trí (rotated by 13 positions)

Sau khi `cat` file data.txt, ta thấy nội dung có dạng:
`Gur cnffjbeq vf 7k16JArUVv5LxVuJfsSVdbbtaHGlw9D4`

Đây là `ROT13 cipher`
- Là một dạng `Caesar cipher`
- Dịch mỗi chữ cái 13 vị trí

```
cat data.txt | tr 'A-Za-z' 'N-ZA-Mn-za-m'
```
- `tr`: translate (chuyển đổi kí tự)
- `A-Za-z`: toàn bộ chữ cái
- `N-ZA-Mn-za-m`: mapping ROT13

![alt text](images/image-12.png)

-> Password: `7x16WNeHIi5YkIhWsfFIqoognUTyj9Q4`

# Level 12 -> Level 13

Mật khẩu cho level tiếp theo được lưu trong file `data.txt`, là một file đã bị `hexdump` và nén nhiều lớp

Cách xử lí: Đầu tiên phải convert hexdump -> binary, sau đó unpack từng lớp một

- Tạo workspace an toàn:
```
mktemp -d
```
- Di chuyển vào thư mục vừa tạo
```
cat /tmp/tmp.yjUJUYRhpb
```
- Copy file vào workspace
```
cp ~/data.txt
```
- Convert hexdump -> binary
```
xxd -r data.txt > data.bin
```
`xxd`: hexdump tool

`-r`: reverse
- Kiểm tra `file type` của `data.bin`
```
file data.bin
```
-> data.bin: gzip compressed data
- Đổi tên tệp từ `.bin` sang `.gz` sau đó unzip
```
mv data.bin data.gz
gunzip data.gz
```
-> Thu được file mới `data`
- Kiểm tra file type của `data`
-> data: bzip2 compressed data
- Đổi tên tệp từ `.gz` sang `.bz2` sau đó unzip

-> Thu được file `data` có file type: data: gzip compressed data

- Thực hiện lại đổi tên và unzip thu được file `tar`
```
mv data data.tar
tar -xf data.tar
```
...

- Lặp lại cho tới khi tìm được file ASCII text thì `cat` và thu được password

![alt text](images/image-13.png)

-> Password: `FO5dwFsc0cbaIiH0h8J2eUks2vdTDwAn`

# Level 13 -> Level 14

Mật khẩu cho level tiếp theo được lưu trong `/etc/bandit_pass/bandit14` và chỉ user `bandit14` đọc được. 

Ở level này bạn không nhận được mật khẩu tiếp theo, nhưng nhận được 1 khóa SSH riêng tư (private SSH key) được sử dụng để đăng nhập vào cấp độ tiếp theo.

Đăng nhập vào server với user `bandit13` và tìm được file `sshkey.private` trong thư mục chính.

Đã biết vị trí của file `sshkey.private`, chuyển nó sang máy của mình.

```
1 bandit13@bandit:~$ ls
2 sshkey.private
3 bandit13@bandit:~$ exit
4 logout
5 Connection to bandit.labs.overthewire.org closed.
```
Sử dụng lệnh `scp` để copy file `sshkey.private` từ server về máy của mình
```
scp -P 2220 bandit13@bandit.labs.overthewire.org:~/sshkey.private .
```
`-P 2220`: port
`bandit13@...`: server
`:~/sshkey.private`: file trên server
`.`: thư mục local

SSH bằng private ssh key, ta nhận được cảnh báo `Permissions 0640 for 'sshkey.private' are too open.`
- File private key đang quá "open"
- SSH từ chối sử dụng key vì không an toàn

Quyền hiện tại: 0640
- Owner: read + write
- Group: read
- Others: none

SSH yêu cầu chỉ Owner được phép, vì vậy ta phải sửa lại quyền cho file `sshkey.private`
```
chmod 600 sshkey.private
```
Chạy lại và login thành công vào server
```
ssh -i sshkey.private bandit14@bandit.labs.overthewire.org -p 2220
```

Lấy mật khẩu
```
cat /etc/bandit_pass/bandit14
```

![alt text](images/image-14.png)

-> Password: `MU4VWeTyJk8ROof1qqmcBPaLh7lDCPvS`

# Level 14 -> Level 15

Mật khẩu cho level tiếp theo được lấy bằng cách submit password của level hiện tại tới `port 30000` trên `localhost`

Kết nối tới port 30000 bằng `nc`

```
nc localhost 30000
```

Dán password vào và lấy được password cho level tiếp theo

![alt text](images/image-15.png)

-> Password: `8xCjnmgoKbGLhHFAZlGE5Tmu4M2tKJQo`

# Level 15 -> Level 16

Mật khẩu cho level tiếp theo được lấy bằng cách submit password của level hiện tại tới `port 30001` trên `localhost` sử dụng mã hóa `SSL/TLS`

Sử dụng `openssl` để làm việc với mã hóa và giao thức SSL/TLS
```
openssl s_client -connect localhost:30001
```

Dán password vào và lấy được password cho level tiếp theo

![alt text](images/image-16.png)

-> Password: `kSkvUpMQ7lBYyCM4GBPvCvT1BfWRy0Dx`

# Level 16 -> Level 17

Thông tin đăng nhập cho level tiếp theo có thể lấy bằng cách gửi mật khẩu hiện tại tới 1 port trên localhost trong phạm vi từ `31000 tới 32000`.

Đầu tiên, tìm xem port nào có server đang `lắng nghe`. Sau đó tìm xem cái nào hỗ trợ `SSL/TLS`

Chỉ có 1 server sẽ cung cáp thông tin đăng nhập tiếp theo, các server khác sẽ chỉ gửi lại cho bạn bất cứ thứ gì bạn gửi đến.

- Đầu tiên sử dụng `nmap` để scan port xem port nào đang `open`
```
nmap -p 31000-32000 localhost
```

Kết quả:
```
PORT      STATE SERVICE
31046/tcp open  unknown
31518/tcp open  unknown
31691/tcp open  unknown
31790/tcp open  unknown
31960/tcp open  unknown
```

- Tiếp theo xác định port hỗ trợ SSL/TLC bằng cách test port thường (nc)

Ví dụ: `nc localhost 31046`
-> Nếu chỉ echo lại input thì bỏ

Kết quả: Lọc được còn lại 2 port `31581` và `31790`

- Test TLS với từng port
```
cat /etc/bandit.pass/bandit16 | openssl s_client -connect localhost:31790
```

Kết quả: Thu được private key
![alt text](images/image-17.png)

Lưu lại private key vào file và set quyền cho key
```
chmod 600 key17
```

SSH bằng private key
```
ssh -i key17 bandit17@bandit.labs.overthewire.org -p 2220
```

Lấy mật khẩu

![alt text](images/image-18.png)

-> Password: `EReVavePLFHtFlFsjn3hyzMlvSuSAcRD`

# Level 17 -> Level 18

Có 2 file trong thư mục chính: `passwords.old` và `passwords.new`

Mật khẩu cho level tiếp theo nằm trong `passwords.new` và có 1 dòng đã bị thay đổi giữa `passwords.old` và `passwords.new`

Sử dụng lệnh `diff` để so sánh 2 tệp văn bản theo từng dòng, hiển thị các điểm khác biệt giữa chúng.

```
diff passwords.old passwords.new
```

Thu được kết quả:
```
bandit17@bandit:~$ diff passwords.old passwords.new
42c42
< 390zFj2NETFVZkqYw8UEFdN6h40oGVtT
---
> x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO
```

`<`: dòng trong file `old`
`>`: dòng trong file `new`

-> Password: `x2gLTTjFwMOhQ8oWNbMN362QKxfRqGlO`

Sử dụng mật khẩu này để SSH vào level tiếp theo, ta thấy trên màn hình hiển thị

```
Byebye !
Connection to bandit.labs.overthewire.org closed.
```

Đây không phải là sai password mà là cơ chế `auto logout` của level tiếp theo

# Level 18 -> Level 19
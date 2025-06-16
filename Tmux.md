# Tmux

## Tạo một session mới:
```
tmux
```
## Attach nó:
```
tmux a
```

## Tạo một session mới kèm theo tên gọi:
```
tmux new -s s_name
```

## Attach session đã được đặt tên:
```
tmux a -t s_name
```

## Hiện thị danh sách các sessions:
```
tmux ls
```

## Xoá một session:
```
tmux kill-session -t s_name
```

# Khi đã ở trong tmux

Mặc định của tmux, prefix của các lệnh sẽ là tổ hợp Ctrl+b, cũng giống như gõ Esc để chuyển về chế độ dòng lệnh như Vim vậy.

## Các lệnh làm việc với cửa sổ
```bash
Ctrl+b c  # Tạo một cửa sổ mới
Ctrl+b w  # Xem danh sách cửa sổ hiện tại
Ctrl+b n/p  #　Chuyển đến cửa sổ tiếp theo hoặc trước đó
Ctrl+b f  #　Tìm kiếm cửa sổ
Ctrl+b ,  #　Đặt/Đổi tên cho cửa sổ
Ctrl+b &  #　Đóng cửa sổ
Ctrl+b l  #　Chuyển đến cửa sổ cuối cùng
Ctrl+b 0-9  # Chuyển đến cửa sổ theo số thứ tự
Ctrl+b d  # Tách session hiện tại
Ctrl+b t  # Hiện thị thời gian
Ctrl+b ?  # Hiện thị danh sách các lệnh
Ctrl+b :  # Vào chế độ dòng lệnh
Ctrl+b [  # Vào chế độ xem lại lịch sử
Ctrl+b ]  # Dán nội dung đã sao chép
Ctrl+b r  # Làm mới cửa sổ
Ctrl+b z  # Phóng to/thu nhỏ cửa sổ
Ctrl+b .  # Di chuyển cửa sổ
Ctrl+b x  # Đóng cửa sổ
Ctrl+b |  # Chia đôi cửa sổ theo chiều dọc
Ctrl+b -  # Chia đôi cửa sổ theo chiều ngang
Ctrl+b q  # Hiện số thứ tự cửa sổ
```

## Cách lệnh làm việc với các panel trong 1 cửa sổ

```bash

Ctrl+b %  # chia đôi màn hình theo chiều dọc
Ctrl+b "  # chia đôi màn hình theo chiều ngang
Ctrl+b o  # Di chuyển đến panel tiếp theo
Ctrl+b ;  # Di chuyển đến panel trước đó
Ctrl+b h/j/k/l  # Di chuyển đến panel bên trái/dưới/trên/phải
Ctrl+b {  # Di chuyển panel sang bên trái
Ctrl+b }  # Di chuyển panel sang bên phải
Ctrl+b x  # Đóng panel
Ctrl+b z  # Phóng to/thu nhỏ panel
Ctrl+b o/<phím mũi tên>  # Di chuyển giữa các panel
Ctrl+b q  # Hiện số thứ tự trên
Ctrl+b x  # Xoá panel
Ctrl+b !  # Tách panel thành cửa sổ mới
Ctrl+b d  # Tách session hiện tại
```
## Các lệnh làm việc với thanh trạng thái

```bash
Ctrl+b t  # Hiện thị thời gian
Ctrl+b :  # Vào chế độ dòng lệnh
Ctrl+b ?  # Hiện thị danh sách các lệnh
Ctrl+b r  # Làm mới thanh trạng thái
Ctrl+b a  # Hiện thị tên session
Ctrl+b s  # Hiện thị danh sách các session
Ctrl+b l  # Hiện thị danh sách các cửa sổ
Ctrl+b f  # Tìm kiếm cửa sổ
Ctrl+b w  # Hiện thị danh sách các cửa sổ
Ctrl+b x  # Đóng cửa sổ
Ctrl+b ,  # Đặt/Đổi tên cho cửa sổ
Ctrl+b d  # Tách session hiện tại
```

# Tuỳ biến Tmux

Tạo file .tmux.conf tại thư mục home của user của bạn và bạn có thể config tmux theo ý mình.

```bash
tmux source-file .tmux.conf
```

## Ví dụ bạn thích Ctrl+c thay vì Ctrl+b? Chỉ cần viết vào file .tmux.conf mới tạo
```
unbind C-b
set -g prefix C-a
```
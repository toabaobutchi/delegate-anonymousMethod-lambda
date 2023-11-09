# Phương thức ẩn danh - Anonymous Method

Phương thức ẩn danh (Anonymous Method) được giới thiệu ở **C# 2.0**, đúng theo tên gọi, chúng là các phương thức không có tên để gọi đến và sử dụng, mà chỉ có phần định nghĩa.

Phương thức ẩn danh là phương thức chỉ được khai báo và định nghĩa bên trong 1 phương thức nào đó trong chương trình.

Cú pháp:

```cs
    delegate (<danh sách tham số>)
    {
        // Phần định nghĩa phương thức – có thể có hoặc không trả về giá trị
    }
```


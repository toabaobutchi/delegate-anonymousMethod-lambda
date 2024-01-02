# Hàm cục bộ trong C# - Local Function

**Hàm cục bộ** (Local Function) là các phương thức được khai báo và định nghĩa trong 1 thành viên của `class`, `struct`, ...

Khai báo và định nghĩa phương thức (chính xác hơn thì gọi là hàm) bên trong 1 thành viên là không hợp lệ cho đến khi Hàm cục bộ được giới thiệu ở **C# 7.0** (phiên bản .NET Core 2.0 trong Visual Studio 2017).

Các thành viên có thể khai báo và gọi hàm cục bộ bao gồm:

- Phương thức (**Method**).

- Phương thức khởi tạo (**Constructor**).

- Phương thức huỷ (**Finalizer**).

- Thuộc tính (**Property**).

- Sự kiện (**Event**).

- Phương thức ẩn danh (**Anonymous Method**).

- Biểu thức Lambda (**Lambda Expression**).

- Trong 1 hàm cục bộ khác.

Hàm cục bộ khác với [**Phương thức ẩn danh**](/2_anonymous_method.md) và [**Biểu thức Lambda**](/3_lambda_expression.md), hàm cục bộ có tên cụ thể để gọi đến mà không cần dùng `delegate`.

Cú pháp khai báo hàm cục bộ tương tự phương thức thông thường:

```cs
    <từ khoá bổ sung> <kiểu trả về> <tên hàm> (<danh sách tham số>)
    {
        // phần định nghĩa hàm cục bộ
    }
```

Tuy nhiên, `<từ khoá bổ sung>` cho hàm cục bộ chỉ bao gồm: `async`, `extern`, `unsafe`, `static`.

Hiện tại ta chỉ cần quan tâm đến từ khoá `static`. Một hàm cục bộ tĩnh gần như tương tự với phương thức ẩn danh tĩnh (Xem lại phần [**Phương thức ẩn danh – Anonymous Method**](/2_anonymous_method.md)). Tức là không thể gọi các biến / đối tượng cục bộ cũng như tham số của thành viên chứa nó (thông thường là 1 phương thức).

**Ví dụ:**

```cs
    int CountPrimeNumber(params int[] arr)
    {
        // khai báo hàm cục bộ kiểm tra số nguyên tố
        bool IsPrime(int num) 
        {
            if (num < 2) return false;
            for(int i = 2; i * i < num; ++i)
                if (num % i == 0)
                    return false;
            return true;
        }

        int count = 0;
        foreach(var item in arr)
        {
            if (isPrime(item)) count++; // gọi sử dụng trong phương thức chứa
        }
        return count;
    }
```

Hàm cục bộ không thể được gọi từ bên ngoài tầm vực khai báo và định nghĩa.

Các hàm cục bộ non-static sẽ có thể dùng các biến / đối tượng nào mà thành viên chứa nó có thể dùng.

> [!Warning]
> Hàm cục bộ là hàm riêng tư của thành viên chứa nó và không chỉ định thêm từ khoá chỉ định truy cập như `public`, `private`, ...

> Xem thêm: [**So sánh Hàm cục bộ và Phương thức ẩn danh**](https://learn.microsoft.com/en-us/dotnet/csharp/programming-guide/classes-and-structs/local-functions#local-functions-vs-lambda-expressions)

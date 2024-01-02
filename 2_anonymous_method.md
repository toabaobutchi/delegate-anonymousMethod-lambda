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

Nhưng phương thức ẩn danh không có tên, nên ta phải tạo 1 đối tượng `delegate` và tham chiếu đến phương thức ẩn danh vừa tạo (vì thực tế nó cũng được xem là phương thức nên `delegate` có thể tham chiếu đến được) - Xem lại nội dung về [**Delegate**](https://github.com/toabaobutchi/delegate-lambda-expression/blob/main/1_delegate.md).

Tuỳ vào Delegate mà phương thức ẩn danh sẽ quyết định có phải trả về giá trị hay không. Ở đây ta sẽ sử dụng `Func<>` và `Action<>` để đưa ra các ví dụ.

**Ví dụ:**

```cs
    // khai báo phương thức ẩn danh trong phương thức
    static void Main()
    {
        Func<int, int, int> func = delegate (int a, int b)
        {
            return a + b;
        };

        Action<string> action = delegate (string str)
        {
            Console.Write(str.ToUpper()); ]
        };

        Console.WriteLine(func(5, 6));
        action("Paragrapth");
    }
```

Khác với phương thức thông thường (khai báo thuộc `class`, `struct`, ...), phương thức ẩn danh chỉ có tầm vực bên trong một phương thức, là một cách triển khai nhanh, nhất thời cho một delegate mà không có tính tái sử dụng cho phương thức khác.

> [!Note]
> Phương thức ẩn danh không chỉ có thể sử dụng tham số mà còn có thể gọi bên trong chúng các thành phần mà phương thức chính (phương thức chứa phương thức ẩn danh) có thể gọi đến như biến cục bộ, trường (Field), thuộc tính (Property), ...

**Ví dụ:**

```cs
    static void Main()
    {
        int num = 10;

        // sử dụng biến của phương thức chính
        Func<int, int> func = delegate (int a)
        {
            return a + num;
        };

        int result = func(5);
        Console.WriteLine(result); // output: 15
    }
```

Bên cạnh đó các biến, trường hay thuộc tính cũng sẽ **bị ảnh hưởng trực tiếp** nếu trong phương thức ẩn danh có các câu lệnh **mang tính cập nhật giá trị**. Tức là chúng có thể bị thay đổi giá trị từ bên trong phương thức ẩn danh.

Khi khai báo phương thức ẩn danh với từ khoá `delegate`, ta **có thể bỏ qua phần tham số cho phương thức**. Điều này khiến cho mọi đối tượng Delegate đều có thể tham chiếu đến với bất kỳ loại kiểu tham số, kiểu trả về nào.

**Ví dụ:**

```cs
    // phương thức ẩn danh không có tham số
    Action<int, string> action = delegate
    {
        Console.WriteLine("Hello World");
    };

    // chẳng có ý nghĩa gì khi truyển đối số
    action(5, "String"); // output: Hello World
```

Từ **C# 9.0** (phiên bản .NET 5.0 trong Visual Studio 2019), ta có thể chỉ định từ khoá `static` trước phương thức ẩn danh.

Phương thức ẩn danh `static` **không thể sử dụng**:

- Biến cục bộ của phương thức chính.

- Các trường, thuộc tính, phương thức, ... không tĩnh (non-static) mà phương thức chính có thể gọi.

**Ví dụ:**

```cs
    class Program
    {
        static int a = 10; // static field

        int b = 10; // non-static field

        void Method() {} // non-static method

        void Test() {
            int c = 10; // local variable

            Func<int> func = static delegate
            {
                Method(); // không phải thành viên static, OK
                return a + b + c; // Lỗi: `c` là biến cục bộ, `b` không phải thành viên static
            };

        }
    }
```

Tuy nhiên trong quá trình phát triển của C#, phương thức ẩn danh không còn được sử dụng phổ biến với sự thay thế của [**Biểu thức Lambda**](/3_lambda_expression.md) (C# 3.0) và **Hàm cục bộ** (C# 7.0).

Các hạn chế của Phương thức ẩn danh:

- Không chứa các câu lệnh như `goto`, `break` hay `continue` (vẫn có thể dùng trong vòng lặp nếu có, `goto` vẫn có thể dùng nếu như di chuyển trong phạm vi phương thức ẩn danh).

- Không thể truy cập đến các tham số `ref`, `out` và `in` của phương thức chứa nó (phương thức chính).

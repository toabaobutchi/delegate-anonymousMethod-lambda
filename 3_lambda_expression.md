# Biểu thức Lambda - Lambda Expression

**Biểu thức Lambda** (Lambda Expresion) được C# giới thiệu trong **C# 3.0** (_phiên bản .NET Framework 3.0/3.5 trong Visual Studio 2008_) và được xem là phương án để thay thế Phương thức ẩn danh (Anonymous Method).

Về cơ bản, Lambda cũng có thể được xem là phương thức ẩn danh, nhưng nó có cú pháp đơn giản và hiệu quả. Bên cạnh đó về cách dùng cũng tương tự như phương thức ẩn danh.

Lambda có 2 dạng biểu diễn bao gồm: 

* Lambda chỉ chứa 1 biểu thức và trả về giá trị của biểu thức đó: `(params) => expression`.

* Lambda chứa một khối lệnh:

```
    (params) => {
        // code ...
        [return ...]
    }
```

Trong đó:

* `params` là danh sách tham số của biểu thức Lambda, phân cách nhau bằng dấu phẩy như phương thức thông thường. Với:

    * Nếu **chỉ chứa một tham số**, có thể bỏ qua dấu ngoặc `()` như `x => ...` hoặc `(x) => ...`.
 
    * Nếu không, các tham số phải đặt trong dấu ngoặc, thậm chí là không có tham số nào.

Muốn dùng được biểu thức Lambda, ta cũng sẽ sử dụng đối tượng Delegate để tham chiếu đến.

**Ví dụ:**

```js
    // trường hợp không có tham số
    Action action = () => Console.WriteLine("Lambda Expression");

    // trường hợp có 1 tham số - có thể bỏ ngoặc hoặc không
    Func<string, int> func = str => {
        int num = int.Parse(str);
        return num;
    };

    // trường hợp có nhiều tham số - không lược bỏ ngoặc
    Action<int[], int> action2 = (arr, size) => {
        if (size > arr.Length)
            size = arr.Length;
        for(int i = 0, i < size; ++i)
        {
            Console.Write(arr[i] + "\t");
        }
    };
```

Kiểu dữ liệu của các tham số trong biểu thức Lambda có thể ngầm định được suy ra dựa vào Delegate tham chiếu đến nó. 

Trong một số trường hợp trình biên dịch không thể suy ra kiểu cụ thể (tiêu biểu là khi ta sử dụng từ khoá `var`), ta có thể chỉ định kiểu cho tham số như với tham số của phương thức thông thường.

**Ví dụ:**

```js
    // hoặc là chỉ định tường minh cho toàn bộ tham số
    Action<string, string> action = (string s1, string s2) => { /* ... */ };

    // hoặc là tất cả đều là ngầm định
    Action<string, string> action = (s1, s2) => { /* ... */ };

    // không có trường hợp chỉ một số tham số có kiểu ngầm định, một số khác lại tường minh
    Action<string, string> action = (s1, string s2) => { /* ... */ }; // lỗi biên dịch
```

Biểu thức Lambda cũng có các hạn chế giống như Phương thức ẩn danh:

* Không sử dụng được `goto`, `break` hay `continue` với mục đích ra khỏi biểu thức Lambda.

```js
    outOfLambda: // đặt nhãn cho goto
    int i = 0;
    Action action = () => {
        goto outOfLambda; // lỗi biên dịch, di chuyển ra ngoài phạm vi lambda
        insideLambda:
        i++;
        if (i < 0)
            goto insideLambda; // hợp lệ - sử dụng trong phạm vi lambda
    };
```

* Không sử dụng được các tham số `ref`, `out` và `in` của phương thức chứa biểu thức Lambda.

```js
    delegate void Method(ref int num);

    void Add(ref int x, int y)
    {
        Method method = (ref int a) => {
            a += y;
            x++; // lỗi biên dịch, không thể sử dụng x vì x là tham số ref của phương thức chính
        };
        method(ref x);
    }
```


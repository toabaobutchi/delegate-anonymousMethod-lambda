# Delegate trong C#

## Giới thiệu về Delegate

**Delegate** là một kiểu dữ liệu tham chiếu trong C# kế thừa từ lớp `System.Delegate`. Delegate là kiểu để đóng gói 
phương thức, tương tự như **Con trỏ hàm** (Function Pointer) trong C và C++, **Functional Interface** trong Java, ...

Ta có thể hiểu một cách đơn giản, Delegate là một kiểu tham chiếu dùng để tạo các biến tham chiếu đến 1 hoặc một vài phương thức nào đó đã có (thay vì tham chiếu đến giá trị, dữ liệu, ...).

## Khai báo Delegate

Cú pháp khai báo Delegate:

```js
    [access-modifier] delegate <data-type> <delegate-name>(<params>);
```
Trong đó:

* `<data-type>` (kiểu dữ liệu) và `<params>` (danh sách tham số) là 2 thành phần quyết định xem Delegate sẽ có thể tham chiếu đến dạng phương thức như thế nào. Chỉ những phương thức có cùng `<data-type>` và cùng `<params>` thì Delegate mới có thể tham chiếu được đến (Lưu ý rằng chỉ cần giống về kiểu tham số trong `<params>`).

* `delegate` là từ khoá dành để khai báo kiểu Delegate.

> [!Note]
>  Delegate có thể khai báo không thuộc lớp nào. Đó là tuỳ vào mục đích của người lập trình.

**Ví dụ:**

```js
    class Program
    {
        delegate string FormatString(string str); // khai báo delegate

        // khai báo phương thức cùng kiểu trả về và kiểu tham số với Delegate
        static string Upper(string str)
        {
            return str.ToUpper();
        }

        static void Main()
        {
            FormatString format = new FormatString(Upper);
            Console.WriteLine(format("Hello World")); // sử dụng delegate thay vì gọi phương thức
            // output: HELLO WORLD
        }
    }
```

Ở ví dụ trên, Delegate `FormatString` được khai báo sẽ tham chiếu đến hàm/phương thức nào nhận vào 1 tham số kiểu `string` và trả về kiểu `string`.

Ngoài việc khởi tạo từ khoá `new`, Delegate có thể tham chiếu trực tiếp đến hàm/phương thức.

**Ví dụ:**

```js
    FormatString format = Upper; // thay cho "FormatString format = new FormatString(Upper);"
```

Giống như biến thông thường, Delegate có thể thay đổi phương thức mà nó tham chiếu như thay đổi giá trị.

**Ví dụ:**

```js
    delegate void DoSomething(string str); // delegate nhận tham số kiểu string, trả về void

    static void PrintUpper(string str)
    {
        Console.WriteLine(str.ToUpper());
    }

    static void PrintLower(string str)
    {
        Console.WriteLine(str.ToLower());
    }

    static void Main()
    {
        DoSomething func = PrintUpper; // tham chiếu đến PrintUpper()
        func("Dong chu nay duoc in hoa");
        func = PrintLower; // tham chiếu đến PrintLower()
        func("Dong chu nay duoc viet thuong");
    }
```

Một đối tượng Delegate có thể tham chiếu nhiều phương thức (**Multicast Delegate**). Khi gọi đến đối tượng Delegate, các phương thức tham chiếu sẽ được gọi 1 cách tuần tự. Để tham chiếu đến nhiều phương thức, ta có thể sử dụng toán tử `+` hoặc `+=` để thêm phương thức vào đối tượng Delegate.

**Ví dụ:**

```js
    string str = "String";

    DoSomething do = PrintUpper;
    do += PrintLower; // hoặc 'do = do + PrintLower'

    do(str); // gọi delegate 1 lần, 2 phương thức sẽ được gọi lần lượt

    // output:
    // STRING
    // string
```

Bên cạnh đó, toán tử `-` hoặc `-=` cũng có thể dùng cho đối tượng Delegate để loại bỏ 1 phương thức đang tham chiếu đến.

> [!Note]
> Nếu tham số là kiểu giá trị (và `string`) thì tham số giữa các phương thức là độc lập và không ảnh hưởng với các phương thức sau. Nếu tham số là kiểu tham chiếu thì tham số sẽ bị ảnh hưởng đến phương thức tiếp theo. Hãy lưu ý điều này.

Delegate cũng giúp ích rất nhiều trong trường hợp ta cần truyền một phương thức vào 1 phương thức khác thông qua tham số. Vì đối tượng Delegate cũng được xem là biến, nên cũng có thể trở thành tham số cho phương thức.

**Ví dụ:**

```js
    delegate void Output(object obj);

    static void Print(object obj, Output How_To_Print)
    {
        How_To_Print(obj);
    }

    static void Main()
    {
        float score = 10;
        Print(score, Console.WriteLine); // Console.WriteLine là phương thức nhận object trả về void
    }
```

## Giới thiệu các Delegate dựng sẵn

Trong C#, có một số Delegate được định nghĩa sẵn, tiêu biểu là `Func<>` và `Action<>`.

Cú pháp:

* **`Func<T1, T2, T3, ... Tn, TResult>`**: tham chiếu đến hàm/phương thức trả về kiểu `TResult` và nhận các tham số `T1`, `T2`, `T3`, ... `Tn`.

* **`Action<T1, T2, T3, ... Tn>`**: tham chiếu đến hàm/phương thức trả về kiểu `void` (hay không trả về giá trị) và nhận các tham số `T1`, `T2`, `T3`, ... `Tn`.

> [!Note]
> Số tham số mà `Func<>` và `Action<>` nhận được tối đa là 16 tham số, không tính đến `TResult`.

Tuy nhiên, điểm chung là cả 2 Delegate dựng sẵn này đều không nhận tham số có từ khoá `ref`, `out` hay `in`.

**Ví dụ:**

```js
    static int Method_return(int a, int b)
    {
        return a + b;
    }

    static void Method(string str) {
        Console.WriteLine(str);
    }

    static void Main()
    {
        // 'int' ở cuối Func<> biểu thị rằng phương thức sẽ trả về số nguyên
        // các thành phần phía trước là danh sách kiểu tham số, ở đây là int và int
        Func<int, int, int> func = Method_return; // phương thức có trả về

        Action<string> action = Method; // phương thức void, string là kiểu tham số

        Console.WriteLine(func(5, 2)); // sử dụng như các delegate bình thường

        action("Hello World");
    }
```

Cả 2 Delegate này đều thuộc namespace `System`.

Ngoài ra, nếu phương thức được tham chiếu không có tham số nào, ta sẽ sử dụng `Action` thay cho `Action<>`.

**Ví dụ:**

```js
    static void Method() { }

    static void Main()
    {
        Action action = Method; // trả về void và không có tham số
    }
```

> [!Important]
> Một điều quan trọng cuối cùng là các từ khoá bổ sung, đặc biệt là `static` không ảnh hưởng đến Delegate. Delegate chỉ quan tâm đến kiểu trả về và tham số. Nhưng phương thức mà nó tham chiếu tới phải gọi được trong phương thức chứa Delegate. Các ví dụ trong phần này khai báo Delegate bên trong `Main()` nên đã sử dụng các phương thức `static`. Nếu gọi trong phương thức thường, sử dụng `static` hay không đều không quan trọng.

**Gợi ý:** Có thể tìm hiểu thêm về `Predicate<T>`, một Delegate nhận _**một tham số**_ và trả về kiểu `bool`. Nhưng vì chỉ nhận một tham số, do đó `Predicate<T>` thường bị thay thế bởi `Func<..., bool>`.


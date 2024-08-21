# کد های شاید جالب

### تبدیل یک کنترل به عکس

``` csharp
Bitmap btm = new Bitmap(this.Width, this.Height);
this.DrawToBitmap(btm, new Rectangle(0, 0, this.Width, this.Height));
```

### تبدیل PDF به JPG

پکیج مورد نیاز:

``` bash
Insatll-Package PDFtoImage
```

کد:

``` csharp
var inputStream = new FileStream("input.pdf", FileMode.Open, FileAccess.Read);
Conversion.SavePng("output.png", inputStream, page: 0);
```

### گزارش خطا (Crash Report)

روش دوم:
``` csharp
Application.ThreadException += Application_ThreadException;
private static void Application_ThreadException(object sender, System.Threading.ThreadExceptionEventArgs e)
{
    StreamWriter sw = new StreamWriter("crashreporter.log");
    sw.Flush();
    sw.WriteLine("--------------------" + DateTime.Now + "------------------");
    sw.WriteLine(e.Exception);
    sw.Close();
    MessageBox.Show("Application closed wiht one error", "Crash reporter", MessageBoxButtons.OK, MessageBoxIcon.Warning);
    Application.Exit();
}
```

روش اول:
``` csharp
AppDomain.CurrentDomain.UnhandledException += CurrentDomain_UnhandledException;
private static void CurrentDomain_UnhandledException(object sender,UnhandledExceptionEventArgs e)
{
    StreamWriter sw = new StreamWriter("crashreporter.log");
    sw.WriteLine("--------------------" + DateTime.Now +"------------------");
    sw.WriteLine(e.ExceptionObject);
    sw.Close();
    MessageBox.Show("Application closed wiht one error", "Crashreporter", MessageBoxButtons.OK, MessageBoxIcon.Warning);
    Application.Exit();
}
```

### Base64 تبدیل رشته به آرایه و برعکس

``` csharp
Convert.ToBase64String(bytes); // return string
Convert.FromBase64String(content); // return byte[]
```

### عدد باینری به عدد صحیح

``` csharp
Console.WriteLine(Convert.ToInt16("1100100", 2)); // output: 100
Console.WriteLine(Convert.ToString(100, 2)); // output: 1100100
```

### Callback

``` csharp
static void Main()
{
    Console.WriteLine("Start");
    ExecuteAsyncCallback(SomeMethod, CallbackMethod);
    Thread.Sleep(10000);
}
static void CallbackMethod()
{
    Console.WriteLine("FINISH");
}
static void SomeMethod()
{
    Console.WriteLine("Job started");
    Thread.Sleep(5000);
    Console.WriteLine("Job done");
}
static void ExecuteAsyncCallback(Action method, Action callback)
{
    Action action = () =>
    {
        method();
        callback();
    };
    action.BeginInvoke(null, null);
}
```

### Delegate

``` csharp
static void Main(string[] args)
{
    Console.WriteLine("Start");
    DoSome(CallbackMethod);
    Console.ReadLine();
}
delegate void MyCallbackPointer(int random);
static void CallbackMethod(int random)
{
    Console.WriteLine(random);
}
static void DoSome(MyCallbackPointer cp)
{
    Console.WriteLine("Doing");
    Thread.Sleep(2000);
    Console.WriteLine("Done!");
    cp(new Random().Next(0, 10000));
}
```

### زمان سنج

``` csharp
Stopwatch sw = new Stopwatch();
sw.Start(); // sw.Restart(); // restart
// do some ...
sw.Stop();
sw.ElapsedMiliseconds; // return elapsed miliseconds
```

### تبدیل تاریخ میلادی به شمسی

``` csharp
public static string ToShamsi(this DateTime value)
{
    PersianCalendar pc = new PersianCalendar(); // or GregorianCalendar
    return $"{pc.GetYear(value)}/{pc.GetMonth(value)}/{pc.GetDayOfMonth(value)}";
}
```

### SHA-256 / MDF Hash مخلوط کردن

``` csharp
var crypt = new SHA256Managed();
string hash = String.Empty;
byte[] crypto = crypt.ComputeHash(Encoding.ASCII.GetByte(randomString));
foreach (byte theByte in crypto)
    hash += theByte.ToString("x2");

using (MD5 md5 = MD5.Create())
    return md5.ComputeHash(data); // output byte array
```

### نمایش یک تاریخ به تقویم دیگر

``` csharp
DateTime.Now.ToString("yyyy/MM/dd hh:mm:ss tt", new CultureInfo("fa-IR"));
DateTime.Now.ToString("yyyy/MM/dd hh:mm:ss tt", new CultureInfo("en-US"));
```

میتوانید تاریخ یک نخ تغییر دهید

``` csharp
Thread.CurrentThread.CurrentCulture = new CultureInfo("fa-IR");
```

### گذر از هشدار SSL در درخواست های HTTp

``` csharp
ServicePointManager.Expect100Continue = true;
ServicePointManager.SecurityProtocol = SecurityProtocolType.Tls12;
```

### دانلود محتویات یک آدرس

``` csharp
HttpClient client = new HttpClient();
var response = client.GetAsync("url").Result;
string content = result.Content.ReadAsStringAsync().Result;
// or
WebRequest request = WebRequest.Create("https://divar.ir/s/mashhad");
WebResponse response = request.GetResponse();
Stream dataStream = response.GetResponseStream();
StreamReader reader = new StreamReader(dataStream);
string responseFromServer = reader.ReadToEnd();
```

### بارگیری فایل از آدرس(دانلود از URL)

``` csharp
var webClient = new WebClient();
byte[] imageBytes = webClient.DownloadData(textBox1.Text);
MemoryStream ms = new MemoryStream(imageBytes);
```

### اندازه فایل قبل از دانلود

``` csharp
System.Net.WebClient wc = new System.Net.WebClient();
Console.Write("Enter File Url: ");
wc.OpenRead(Console.ReadLine());
Int64 bytes_total = Convert.ToInt64(wc.ResponseHeader["Content-Length"]);
Console.WriteLine(bytes_total.ToString() + " Bytes");
```

### ارسال پیام الکترونیک (email)

``` csharp
public static void Send(string from, string password, string displayname, string to, string subject, string body)
{
    MailMessage mail = new MailMessage();
    mail.From = new MailAddress(from, displayname);
    mail.To.Add(to);
    mail.Subject = subject;
    mail.Body = body;
    mail.IsBodyHtml = true;
    SmtpClient client = new SmtpClient("smtp.elasticemail.com");
    client.Port = 2525;
    client.Credentials = new NetworkCredential(from, password);
    client.EnableSsl = true;
    client.Send(mail);
}
```

### استفاده از متغیر ضروری سراسری برنامه

``` chsarp
Environment.SetEnvironmentVariable("name", "value"); // ساخت
Environment.GetEnvironmentVariable("name"); // خواندن
```

### استفاده از حافظه سراسری سیستم عامل

``` chsarp
var scope = EnvironmentVariableTarget.Machine; // or User
Environment.SetEnvironmentVariable("name", "value", scope); // ساخت
Environment.GetEnvironmentVariable("PATH", scope); // خواندن
```

### مشخصات یک کلاس

``` csharp
var properties = typeof(Person).GetProperties().ToDictionary(x => x, x => x.GetType().GetProperties());

// گرفتن مقدار یک شی ساخته شده
foreach (var item in properties)
    typeof(Person).GetProperty(item.Key.Name).GetValue(p).ToString();
```

### مشخصات Region از تنظیمات تاریخ کامپیوتر

``` csharp
Console.WriteLine(CultureInfo.CurrentUICulture.NumberFormat.NumberDecimalSeparator);
Console.WriteLine(CultureInfo.CurrentUICulture.NumberFormat.CurrencyGroupSeparator);
Console.WriteLine(CultureInfo.CurrentUICulture.NumberFormat.NegativeSign);
```

### گرفتن شماره خط خطا

``` csharp
try { throw new Exception(""); }
catch (Exception ex) {
    StackTrace st = new StackTrace(ex, true);
    foreach (StackFrame frame in st.GetFrames())
    {
        string fileName = frame.GetFileName();
        string methodName = frame.GetMethod().Name;
        int line = frame.GetFileLineNumber();
        int col = frame.GetFileColumnNumber();
        Console.WriteLine("File Name: {0} | Method Name: {1} | Line: {2}-{3}", fileName, methodName, line, col);
    }
}
```

### گرفتن سریال کامپیوتر

``` csharp
var managementObjectSearcher = new ManagementObjectSearcher("Select * From Win32_BaseBoard");
string serialboard = "";
foreach (var managementObject in managementObjectSearcher.Get())
    try
    {
        serialboard = managementObject["SerialNumber"].ToString();
    }
    catch { Console.Write("Fiald to get cpu serial"); }
ManagementObjectSearcher secpu = new ManagementObjectSearcher("select* from win32_processor");
string serialcpu = "";
foreach (ManagementObject obj in secpu.Get())
    try
    {
        serialcpu = obj.Properties["processorID"].Value.ToString();
    }
    catch { Console.Write("Fiald to get cpu serial"); }
ManagementObjectSearcher sehard = new ManagementObjectSearcher("select* from Win32_DiskDrive");
string serialhard = "";
foreach (ManagementObject obj in sehard.Get())
    try
    {
        serialhard = obj.Properties["serialnumber"].Value.ToString();
    }
    catch { Console.Write("Fiald to get hard serial"); }
```

### ساخت کلید Guid

``` csharp
Guid.NewGuid().ToString("N"); // 00000000000000000000000000000000
Guid.NewGuid().ToString("D"); // 00000000-0000-0000-0000-000000000000
Guid.NewGuid().ToString("B"); // {00000000-0000-0000-0000-000000000000}
Guid.NewGuid().ToString("X"); // {0x00000000,0x0000,0x0000,{0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00}}
```

### روند کردن اعداد

``` csharp
double value = 1534567890;
Console.WriteLine(value.ToString("#,#", CultureInfo.InvariantCulture));// Displays 1,234,567,890   
Console.WriteLine(value.ToString("#,##0,K", CultureInfoInvariantCulture));// Displays 1,234,568K
Console.WriteLine(value.ToString("#,##0,,M", CultureInfoInvariantCulture));// Displays 1,235M
Console.WriteLine(value.ToString("#,##0,,,B", CultureInfoInvariantCulture));// Displays 1B
```

### تبدیل حروف انگلیسی به فارسی و برعکس

``` csharp
private static readonly CultureInfo arabic = new CultureInfo("fa-IR");
private static readonly CultureInfo latin = new CultureInfo("en-US");
public static string ToArabic(this string input)
{
    var arabicDigits = arabic.NumberFormat.NativeDigits;
    for (int i = 0; i < arabicDigits.Length; i++)
        input = input.Replace(i.ToString(), arabicDigits[i]);
    return input;
}
public static string ToLatin(this string input)
{
    var latinDigits = latin.NumberFormat.NativeDigits;
    var arabicDigits = arabic.NumberFormat.NativeDigits;
    for (int i = 0; i < latinDigits.Length; i++)
        input = input.Replace(arabicDigits[i], latinDigits[i]);
    return input;
}
```

### باز نویسی عمگلگر های کلاس

عملیات ریاضی و مقایسه
``` csharp
public static Person operator +(Person person, Product product)
{
    return new Person { Name = person.Name, Money = person.Money - product.Price };
}
public static bool operator ==(HHeader person, HHeader product)
{
    return person.Money > product.Price;
}
```

تبدیل شدن به متغیر دیگر
``` csharp
public static implicit operator byte(Bytecode bytecode)
{
    return bytecode.Code;
}
```

گرفتن مشخصه
``` csharp
public string this[string propertyName]
{
    get
    {
        if (propertyName == "Name")
        {
            return this.Name;
        }
        else
        {
            throw new ArgumentException($"Property '{propertyName}' is not supported");
        }
    }
}
```

### پارالر (انجام عملیات بر روی آرایه در یک خط)

``` csharp
String[] files = Directory.GetFiles(args[0]);
Parallel.For(0, files.Length,
index = { FileInfo fi = new FileInfo(files[index]);
    long size = fi.Length;
    Interlocked.Add(ref totalSize, size);
} );
```

### XOR رمزنگاری

``` csharp
static string EncryptOrDecrypt(string text, string key)
{
    var result = new StringBuilder();
    for (int c = 0; c < text.Length; c++)
        result.Append((char)((uint)text[c] ^ (uint)key[c % key.Length]));
    return result.ToString();
}
```

### فرمت کردن متن پیشرفته

``` csharp
double num = 10.555;
Console.WriteLine(string.Format("Name = {0:n10}, hours = {1:hh/mm}",num, DateTime.Now)); // Name = 10.5550000000, hours = 07/11
Console.WriteLine("{0,10}", "name"); //       name
Console.WriteLine("{0,-10}Hello", "name"); // name      Hello
Console.WriteLine("{0,8:C1}", num); //    $10.6
```

### Await ادامه فرآیند یا گرفتن پاسخ

``` csharp
client2.SendAsync(request).ContinueWith(responseTask =>
{
    Console.WriteLine("Response: {0}", responseTask.Result);
});
```

### فرمت تاریخ

``` bash
yyyy 	Four digits year 	سال چهار رقمی
yy 	Two digits year 	سال دو رقمی
MMMM 	Month Name 	نام ماه
MM 	Two digits month number 	شماره دو رقمی ماه
M 	One digit year 	شماره تک رقمی ماه
dddd 	Week day name 	نام روز هفته
dd 	Two digits month day number 	شماره دو رقمی روز ماه
d 	One digit month day number 	عدد یک رقمی روز ماه
HH 	Two digits hour number 	شماره ساعت دو رقمی با فرمت 00 تا 24
H 	One digit hour number 	شماره ساعت تک رقمی با فرمت 00 تا 24
hh 	Two digits hour number 	شماره ساعت دو رقمی با فرمت 0 تا 12
h 	One digit hour number 	شماره ساعت تک رقمی با فرمت 0 تا 12
mm 	Two digits minute number 	عدد دو رقمی دقیقه
m 	One digit minute number 	عدد تک رقمی دقیقه
ss 	Two digits second number 	عدد دو رقمی ثانیه
s 	One digit second number 	عدد تک رقمی ثانیه
fff 	Three digits milliseconds number 	عدد سه رقمی میلی ثانیه
ff 	Two digits milliseconds number 	عدد دو رقمی میلی ثانیه
f 	One digit milliseconds number 	عدد تک رقمی میلی ثانیه
tt 	AM/PM 	ب.ظ یا ق.ظ
t 	First letter of AM/PM 	اولین حرف از ب.ظ یا ق.ظ 
```

### صدا زدن توابع با استفاده از رشته نام

حالت اول: در یک کلاس:

``` csharp

public class MyClass
{
    public void SayHello()
    {
        Console.WriteLine("Hello!");
    }

    public void Greet(string name)
    {
        Console.WriteLine($"Hello, {name}!");
    }
}

public class Program
{
    public static void Main(string[] args)
    {
        MyClass myClassInstance = new MyClass();

        string methodName = "SayHello";
        CallMethodByName(myClassInstance, methodName);

        string methodNameWithParams = "Greet";
        object[] parameters = new object[] { "Alice" };
        CallMethodByName(myClassInstance, methodNameWithParams, parameters);
    }

    public static void CallMethodByName(object obj, string methodName, params object[] parameters)
    {
        Type type = obj.GetType();
        MethodInfo methodInfo = type.GetMethod(methodName);

        if (methodInfo != null)
            methodInfo.Invoke(obj, parameters);
        else
        {
            Console.WriteLine($"Method '{methodName}' not found in class '{type.Name}'.");
        }
    }
}

```

حال دوم: در کلاس های ثابت و توابع ثابت:

``` csharp
public static class MyStaticClass
{
    public static void PrintMessage()
    {
        Console.WriteLine("Hello from a static method!");
    }

    public static int Add(int a, int b)
    {
        return a + b;
    }
}

public class Program
{
    public static void Main(string[] args)
    {
        CallStaticMethodByName("MyStaticClass", "PrintMessage");

        object[] parameters = new object[] { 3, 5 };
        object result = CallStaticMethodByName("MyStaticClass", "Add", parameters);
        Console.WriteLine($"The result of addition is: {result}");
    }

    public static object CallStaticMethodByName(string className, string methodName, params object[] parameters)
    {
        Type type = Type.GetType(className);
        
        if (type == null)
        {
            Console.WriteLine($"Class '{className}' not found.");
            return null;
        }

        MethodInfo methodInfo = type.GetMethod(methodName, BindingFlags.Static | BindingFlags.Public);

        if (methodInfo == null)
        {
            Console.WriteLine($"Method '{methodName}' not found in class '{className}'.");
            return null;
        }

        return methodInfo.Invoke(null, parameters);
    }
}
```

### Cantor - ساخت عدد منحصر به فرد از دو عدد متفاوت

``` csharp
public BigInteger CantorPairingFunction(BigInteger k1, BigInteger k2)
{
    return (k1 + k2) * (k1 + k2 + 1) / 2 + k2;
}
public long CantorPairingFunction(long k1, long k2)
{
    return (long)(0.5 * (k1 + k2) * (k1 + k2 + 1) + k2);
}
public void InverseCantorPairingFunction(long input, out long k1, out longk2)
{
    long w = (long)Math.Floor((Math.Sqrt(8 * input + 1) - 1) / 2);
    long t = w * (w + 1) / 2;
    k2 = input - t;
    k1 = w - k2;
}
```

# نکته ها

اپراتور شرطی ?:

``` csharp
var result = someCondition ? "value if true" : "value if false";
```

متدهای پسوند نال-امن (.?):

``` csharp
var length = someString?.Length;
```

اپراتور تخصیص شرطی ??=:

``` csharp
someVariable ??= ComputeValue();
```

پترن مچینگ:

``` csharp
if (obj is int i) Console.WriteLine($"Integer: {i}");
```

Expression-bodied members:

``` csharp
public string FullName => $"{FirstName} {LastName}";
```

توابع محلی (Local Functions):

``` csharp
int Add(int x, int y)
{
    return x + y;
    int LocalFunction(int z) => z * z;
}
```

Indexers:

``` csharp
public string this[int index]
{
    get { return internalArray[index]; }
    set { internalArray[index] = value; }
}
```

Null-coalescing operator ?? and ??=:

``` csharp
var name = possibleNullValue ?? "default name";
```

Using declarations:

``` csharp
using var file = new StreamWriter(filePath, append: true);
```

Switch expressions:

``` csharp
var state = status switch
{
    State.Active => "Active",
    State.Inactive => "Inactive",
    _ => "Unknown"
};
```

اینها فقط نمونه‌هایی از تکنیک‌ها و ویژگی‌هایی هستند که ممکن است در کد نویسی روزمره با C# کمتر استفاده شوند اما می‌توانند به بهبود خوانایی و کارایی کد کمک کنند.

Range operator:

``` csharp
var numbers = new[] {1, 2, 3, 4, 5};
var slice = numbers[1..4]; // برمی‌گرداند [2, 3, 4]
``

Deconstructing tuples:

``` csharp
var (name, age) = ("Alice", 30);
Console.WriteLine(name); // Alice
Console.WriteLine(age); // 30
```

Asynchronous streams (async/await with IAsyncEnumerable):

``` csharp
async IAsyncEnumerable<int> GetNumbersAsync()
{
    for (int i = 0; i < 5; i++)
    {
        await Task.Delay(100); // تصور کنید این عملیات زمان‌بر است.
        yield return i;
    }
}
```

Pattern matching enhancements in switch expressions:

``` csharp
var output = obj switch
{
    string s => $"String of length {s.Length}",
    int i when i > 0 => "Positive integer",
    null => "It's a null",
    _ => "Unknown"
};
```

Records:

``` csharp
public record Person(string FirstName, string LastName);
var person1 = new Person("John", "Doe");
```

Init-only properties:

``` csharp
public class Person
{
    public string Name { get; init; }
}
var person = new Person { Name = "Alice" };
```

Top-level statements:

برای کدنویسی سریع و آزمایشی، امکان نوشتن دستورات در بالاترین سطح فایل بدون نیاز به تعریف کلاس یا متد Main.
Target-typed new expressions:

``` csharp
Person person = new("John", "Doe");
Global using directives:
```

در C# 10 به بالا، امکان اعلام using هایی که در سراسر پروژه قابل استفاده هستند:
csharp
Copy code
global using System.Linq;
Lambda improvements, static anonymous functions:

``` csharp
Func<int, int> square = static x => x * x;
```

این فهرست نشان‌دهنده تنوع و پیچیدگی‌های زبان C# است که به برنامه‌نویسان امکان می‌دهد با استفاده از ویژگی‌های مختلف، کد خود را بهینه و خواناتر نویسند.

بله، البته که ویژگی‌ها و تکنیک‌های بیشتری در C# وجود دارند که می‌توانند به بهبود خوانایی و کارایی کد کمک کنند. در اینجا تعدادی دیگر از این ویژگی‌ها آورده شده‌اند:

Ref returns and locals:

``` csharp
public ref int Find(int number, int[] numbers)
{
    for (int i = 0; i < numbers.Length; i++)
    {
        if (numbers[i] == number)
        {
            return ref numbers[i]; // بازگشت یک رفرنس به محل داده
        }
    }
    throw new IndexOutOfRangeException($"{nameof(number)} not found");
}
```

Nullable reference types:

``` csharp
#nullable enable
string? mightBeNull = null;
if (mightBeNull != null)
{
    Console.WriteLine(mightBeNull.Length); // بدون خطا چون بررسی null انجام شده
}
#nullable restore
```

Pattern matching on tuples:

``` csharp
var numbers = (5, 10);
var range = numbers switch
{
    (0, 0) => "Origin",
    (var x, var y) when x == y => "Along y = x",
    _ => "Somewhere else"
};
```

Interpolated verbatim strings:

``` csharp
string name = "World";
string greeting = $@"Hello, {name}!";
Console.WriteLine(greeting); // نمایش: Hello, World!
```
Indices and ranges:

``` csharp
int[] array = { 1, 2, 3, 4, 5 };
int lastElement = array[^1]; // دسترسی به آخرین عنصر
int[] middle = array[1..^1]; // برش از دومین تا قبل از آخرین عنصر
```

Native-sized integers (nint and nuint):

``` csharp
nint nativeInteger = sizeof(nint);
Console.WriteLine(nativeInteger); // اندازه بومی اینتجر بسته به پلتفرم 32 یا 64 بیتی
```

Function pointers:

``` csharp
using System;
unsafe
{
    delegate*<int, int, int> addPtr = &Add;
    int result = addPtr(2, 3);
    Console.WriteLine(result); // نمایش: 5
}
static int Add(int x, int y) => x + y;
```

Static local functions:

``` csharp
int Add(int x, int y)
{
    return AddLocal(x, y);

    static int AddLocal(int x, int y) => x + y;
}
```

Using static members:

``` csharp
using static System.Math;
double radius = 5.0;
double area = PI * Pow(radius, 2);
```

Attributes on local functions:

``` csharp
using System.Diagnostics.CodeAnalysis;

void MyMethod()
{
    [DoesNotReturn] void Fail(string message) => throw new InvalidOperationException(message);
    Fail("Something went wrong.");
}
```
این ویژگی‌ها و تکنیک‌ها نشان می‌دهند که چگونه می‌توان با استفاده از زبان C# کدنویسی را تا حد زیادی سفارشی و بهینه کرد.

البته، اینجا برخی دیگر از تکنیک‌ها و ویژگی‌های کمتر شناخته‌شده در C# را می‌توانید ببینید:

Pattern matching with tuples:

``` csharp
var point = (x: 3, y: 4);
var quadrant = point switch
{
    (0, 0) => "Origin",
    (var x, var y) when x > 0 && y > 0 => "Quadrant 1",
    (var x, var y) when x < 0 && y > 0 => "Quadrant 2",
    (var x, var y) when x < 0 && y < 0 => "Quadrant 3",
    (var x, var y) when x > 0 && y < 0 => "Quadrant 4",
    _ => "On a line"
};
```

Ref returns and locals:

``` csharp
public ref int Find(int[] numbers, int target)
{
    for (int i = 0; i < numbers.Length; i++)
    {
        if (numbers[i] == target)
            return ref numbers[i]; // بازگرداندن یک ارجاع به عنصر آرایه
    }
    throw new IndexOutOfRangeException($"{target} not found");
}
```

Pattern matching on properties:

``` csharp
public class Person
{
    public string Name { get; set; }
    public int Age { get; set; }
}

public bool IsMinor(Person person) =>
    person switch
    {
        { Age: < 18 } => true,
        _ => false
    };
```

Using static directives:

این ویژگی اجازه می‌دهد تا متدها، خاصیت‌ها و اعضای ثابت را بدون مشخص کردن نام کلاس به کار ببرید:
``` csharp
using static System.Console;
using static System.Math;

WriteLine(Sqrt(144)); // 12
```

Value tuples for multiple return values:

``` csharp
public (int Max, int Min) FindMinMax(int[] numbers)
{
    return (numbers.Max(), numbers.Min());
}
```

Local functions with iterators:

``` csharp
IEnumerable<int> Fibonacci(int n)
{
    return Iterator();

    IEnumerable<int> Iterator()
    {
        var (a, b) = (0, 1);
        for (int i = 0; i < n; i++)
        {
            yield return a;
            (a, b) = (b, a + b);
        }
    }
}
```

Nullable reference types:
به شما اجازه می‌دهد برای جلوگیری از NullReferenceException، صراحتاً نوع‌های قابل ارجاع نال را مشخص کنید:

``` csharp
#nullable enable
string? maybeNull = null;
Attributes on local functions:

csharp
Copy code
public void Method()
{
    [Obsolete("Use NewMethod instead")]
    void OldMethod()
    {
        // ...
    }
}
Covariant return types:
```

این امکان را می‌دهد تا در کلاس‌های مشتق‌شده، نوع بازگشتی متدها را به نوعی مشتق از نوع بازگشتی در کلاس پایه تغییر دهید:
``` csharp
public class BaseClass
{
    public virtual BaseClass Clone() => new BaseClass();
}

public class DerivedClass : BaseClass
{
    public override DerivedClass Clone() => new DerivedClass();
}
Static anonymous functions:
```

``` csharp
Func<int, int, int> add = static (a, b) => a + b;

public (int Max, int Min) FindMinMax(int[] numbers)
{
    int max = numbers.Max();
    int min = numbers.Min();
    return (max, min); // Returning a tuple
}

var (max, min) = FindMinMax(new[] {1, 2, 3, 4, 5});
Console.WriteLine($"Max: {max}, Min: {min}");

public void ComplexCalculation()
{
    int Multiply(int x, int y) => x * y;
    Console.WriteLine(Multiply(5, 2));
}

public class Person
{
    public string FirstName { get; set; }
    public string LastName { get; set; }
    public string FullName => $"{FirstName} {LastName}";
}
```

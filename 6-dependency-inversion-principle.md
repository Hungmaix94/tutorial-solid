<h1 align="center">Dependency Inversion Principle (DIP)</h1>

### 1. Định nghĩa

> A. High-level modules should not depend on low-level modules. Both should depend on abstractions.
>
> B. Abstractions should not depend upon details. Details should depend upon abstractions.

A. Class cấp cao sẽ không phụ thuộc trực tiếp vào Class cấp thấp mà sẽ phụ thuộc vào abstractions (interface) và abstractions (interface) đó sẽ được implement bởi Class cấp thấp.

B. Class mang tính trừu tượng sẽ không phụ thuộc và class cụ thể. Class cụ thể sẽ phụ thuộc vào abstractions (interface)

### 2. Ví dụ

Chúng ta sẽ xem xét ví dụ sau để hiểu rõ về định nghĩa A:

```php
class MySqlDB
{
    public function insert($data)
    {
        // MySql insert data
    }
}

class User
{
    protected $db;

    public function __construct(MySqlDB $db)
    {
        $this->db = $db;
    }

    public function add($user)
    {
        $this->db->insert($user);
    }
}
```

Ở đây User là class cấp cao, MySqlDB là class cấp thấp, và class User đang phụ thuộc trực tiếp vào class MySqlDB.
Điều này đã vị phạm định nghĩa A: Class cấp cao sẽ không phụ thuộc trực tiếp vào Class cấp thấp.

Vậy tại sao lại như vậy?

Giả sử chúng ta có yêu cầu mới là sẽ không dùng MySqlDB nữa mà sẽ chuyển sang dùng MongoDB. Lúc này thì sẽ không có cách nào giải quyết vì class User phụ thuộc trực tiếp vào MySqlDB.

Để giải quyết vấn đề này thì chúng ta sẽ định nghĩa 1 interface DB. Class User sẽ phụ thuộc vào interface DB đó và class MySqlDB, MongoDB sẽ implement theo interface DB.

```php
interface DB
{
    public function insert($data);
}

class MySqlDB implements DB
{
    public function insert($data)
    {
        // MySql insert data
    }
}

class MongoDB implements DB
{
    public function insert($data)
    {
        // MongoDB insert data
    }
}

class User
{
    protected $db;

    public function __construct(DB $db)
    {
        $this->db = $db;
    }

    public function add($user)
    {
        $this->db->insert($user);
    }
}
```

Như vậy class User đơn giản chỉ cần object $db là thể hiện của interface DB là được chứ không quan tầm nó là MySql hay Mongo... Và đó cũng là ý nghĩa của vế còn lại trong định nghĩa A:

> Class cấp cao sẽ phụ thuộc vào abstractions (interface) và abstractions (interface) đó sẽ được implement bởi Class cấp thấp.


Tiếp theo chúng ta sẽ xem xét 1 ví dụ khác để hiểu định nghĩa B rõ hơn.

Giả sử chúng ta có 1 app để đọc PDF Book như sau:

```php
class PDFBookReader
{
    protected $pdfBook;

    function __construct(PDFBook $pdfBook)
    {
        $this->pdfBook = $pdfBook;
    }

    function read()
    {
        return $this->pdfBook->getContent();
    }
}

class PDFBook
{
    function getContent()
    {
        return 'PDFBook content';
    }
}

$pdfBook = new PDFBook();
$bookReader = new BookReader($pdfBook);

echo $bookReader->read();
```

Nếu yêu cầu đơn giản chỉ là đọc book PDF thì ok. Nhưng sau này chúng ta nhận được yêu cầu mới là app phải hỗ trợ thêm nhiều định dạng khác nữa chứ không chỉ riêng PDF. Sau khi update lại PDFBookReader để có thể hỗ trợ được các định dạng khác nhau thì chúng ta có 1 class BookReader mới như sau:

```php
class BookReader
{
    protected $book;

    function __construct(PDFBook $book)
    {
        $this->book = $book;
    }

    function read()
    {
        return $this->book->getContent();
    }
}
```

Lúc này thì class BookReader có thể đọc được các book nói chung nhưng nó lại bị phụ thuộc vào 1 loại book cụ thể là PDFBook. Như vậy cuối cùng thì nó cũng chỉ để sử dụng đọc PDFBook chứ không thể đọc các loại book khác được.
Điều này đã vi phạm vế đầu của định nghĩa B:

> Class mang tính trừu tượng sẽ không phụ thuộc và class cụ thể...

Để giải quyết thì chúng ta sẽ tạo 1 interface Book, BookReader sẽ phụ thuộc vào interface Book và các class book sẽ implement interface Book đó.

```php
interface Book
{
    public function getContent();
}

class BookReader
{
    protected $book;

    function __construct(Book $book)
    {
        $this->book = $book;
    }

    function read()
    {
        return $this->book->getContent();
    }
}

class DOCBook implements Book
{
    function getContent()
    {
        return 'DOCBook content';
    }
}

$docBook = new DOCBook();
$bookReader = new BookReader($docBook);

echo $bookReader->read();
```

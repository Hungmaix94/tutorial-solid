<h1 align="center">Single Responsiblity Principle (SRP)</h1>

### 1. Định nghĩa

> A class should have only one reason to change.

Một class chỉ nên có duy nhất 1 lý do để thay đổi, hay nói cách khác là 1 class chỉ nên có 1 và chỉ 1 trách nhiệm.

### 2. Ví dụ

Giả sử chúng ta đang xây dựng 1 app để tính tổng diện tích của các hình và in ra kết quả như sau:

```php
class Circle
{
    public $radius;

    public function __construct($radius)
    {
        $this->radius = $radius;
    }
}

class Square
{
    public $length;

    public function __construct($length)
    {
        $this->length = $length;
    }
}

class AreaCalculator
{
    public $shapes;

    public function __construct(array $shapes)
    {
        $this->shapes = $shapes;
    }

    public function sum()
    {
        // Thực hiện logic tính tổng diện tích các hình
    }

    public function output()
    {
        echo 'Sum: <b>' . $this->sum() . '</b>';
    }
}

$shapes = [
    new Circle(2),
    new Square(5)
];
$areaCalculator = new AreaCalculator($shapes);

$areaCalculator->output();
```

Sau đó thì có thêm yêu cầu là ngoài html thì app phải output được các định dạng khác như json, xml ...
Chúng ta sẽ làm gì? Update thêm logic vào AreaCalculator?

Theo như design hiện tại thì ta thấy class AreaCalculator đang phải xử lý 2 nhiệm vụ:

- Tính toán tổng diện tích của các hình
- Xử lý logic render kết quả tính toán

Vậy để giải quyết vấn đề này thì chúng ra sẽ tạo thêm 1 class mới là AreaOutputter

```php
class AreaCalculator
{
    protected $shapes;

    public function __construct(array $shapes)
    {
        $this->shapes = $shapes;
    }

    public function sum()
    {
        // Thực hiện logic tính tổng diện tích các hình
    }
}

class AreaOutputter
{
    public $areaCalculator;

    public function __construct(AreaCalculator $areaCalculator)
    {
        $this->areaCalculator = $areaCalculator;
    }

    public function html()
    {
        echo 'Sum: <b>' . $this->areaCalculator->sum() . '</b>';
    }

    public function json()
    {
        echo json_encode(['sum' => $this->areaCalculator->sum()]);
    }
}

$shapes = [
    new Circle(2),
    new Square(5)
];
$areaCalculator = new AreaCalculator($shapes);
$areaOutputter = new AreaOutputter($areaCalculator);

$areaCalculator->html();
$areaCalculator->json();
```

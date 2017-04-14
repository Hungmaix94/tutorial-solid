<h1 align="center">Open Closed Principle (OCP)</h1>

### 1. Định nghĩa

> Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification.

Các modules, classes và functions phải được thiết làm sao để đảm bảo khi cần triển khai tính năng mới thì không cần phải chỉnh sửa code cũ mà sẽ viết code mới và code mơi đó sẽ được sử dụng bởi code cũ.

### 2. Ví dụ

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
        $sum = 0;

        foreach ($this->shapes as $shape) {
            if ($shape instanceof Circle) {
                $sum += pi() * pow($shape->radius, 2);
            } elseif ($shape instanceof Square) {
                $sum += pow($shape->length, 2);
            }
        }

        return $sum;
    }
}
```

Với logic hiện tại thì sẽ có vấn đề là mỗi khi thêm 1 class Shape mới thì chúng ta sẽ phải update tiếp logic của method sum. Càng nhiều Shape cần thêm thì logic của method sum sẽ càng nhiều.

Để giải quyết vấn đề này thì chúng ta sẽ xử lý như sau:

- Mỗi Shape sẽ tự tính toán diện tích của nó tùy thuộc vào logic riêng.
- Class AreaCalculator chỉ đơn giản là tính tổng diện tính của các Shape mà không cần quan tâm logic tính của từng Shape là như nào.

```php
class Circle
{
    public $radius;

    public function __construct($radius)
    {
        $this->radius = $radius;
    }

    public function getArea()
    {
        return pi() * pow($this->radius, 2);
    }
}

class Square
{
    public $length;

    public function __construct($length)
    {
        $this->length = $length;
    }

    public function getArea()
    {
        return pow($this->length, 2);
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
        $sum = 0;

        foreach ($this->shapes as $shape) {
            $sum += $shape->getArea();
        }

        return $sum;
    }
}
```

Đến đây thì class AreaCalculator đã khá hoàn thiện, tuy nhiên vẫn còn 1 vấn đề.

Giả sử trong danh sách shapes được truyền vào AreaCalculator có một object không khai báo method getArea thì khi chạy method sum chúng ta sẽ nhận được 1 error nghiêm trọng.

Chúng ta sẽ giải quyết vấn đề này bằng cách khai báo 1 interface Shape và các Shape sẽ implements interface Shape đó, và AreaCalculator sẽ nhận diện các object $shape được truyền vào thông qua interface Shape.

```php
interface Shape
{
    public function getArea();
}

class Circle implements Shape
{
    public $radius;

    public function __construct($radius)
    {
        $this->radius = $radius;
    }

    public function getArea()
    {
        return pi() * pow($this->radius, 2);
    }
}

class Square implements Shape
{
    public $length;

    public function __construct($length)
    {
        $this->length = $length;
    }

    public function getArea()
    {
        return pow($this->length, 2);
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
        $sum = 0;

        foreach ($this->shapes as $shape) {
            if ($shape instanceof Shape) {
                $sum += $shape->getArea();
                continue;
            }

            throw new InvalidShapeException;
        }

        return $sum;
    }
}
```

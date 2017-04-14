<h1 align="center">Liskov Substitution Principle (LSP)</h1>

### 1. Định nghĩa

> Let q(x) be a property provable about objects x of type T. Then q(y) should be provable for objects y of type S where S is a subtype of T.

Mọi class con sẽ không bao giờ được phép phá vỡ chức năng của class cha. Hay nói cách khác thì mọi class con có thể thay thế class cha mà không là thay đổi hành vi của hệ thống.

### 2. Ví dụ

Giả sử chúng ta có 1 class Rectangle và có 1 function test chức năng tính diện tích của nó.

```php
class Rectangle
{
    protected $width;
    protected $height;

    public function setWidth($width)
    {
        $this->width = $width;
    }

    public function setHeight($height)
    {
        $this->height = $height;
    }

    public function area()
    {
        return $this->width * $this->height;
    }
}

function testRectangleArea(Rectangle $rectangle)
{
    $rectangle->setWidth(20);
    $rectangle->setHeight(10);

    return $rectangle->area() === 200;
}

$test = RectangleArea(new Rectangle);
var_dump($test);
```

Mọi thứ đang đúng như chúng ta mong muốn, tuy nhiên như chúng ta biết thì hình vuông là 1 trường hợp đặc biệt của hình chữ nhật khi có 2 cạnh bằng nhau. Vì vậy chúng ta sẽ tạo class Square và thực hiện test logic của nó.

```php
class Square extends Rectangle
{
    public function setWidth($value)
    {
        $this->width = $value;
        $this->height = $value;
    }

    public function setHeight($value)
    {
        $this->width = $value;
        $this->height = $value;
    }
}

$test = testRectangleArea(new Square);
var_dump($test);
```
Kết quả là false.

Nguyên nhân là class Square đã thay đổi hành vi của class Rectangle, cụ thể ở đây là 2 method setWidth và setHeight. Ở class cha là Rectangle thì 2 method đó thực hiện đúng nhiệm vụ của nó là gán giá trị cho từng cạnh. Nhưng ở class con là Square thì khi thực hiện gán width nó đồng thời cũng gán luôn giá trị cho height để đảm bảo logic của nó là width luôn bằng height.

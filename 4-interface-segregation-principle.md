<h1 align="center">Interface Segregation Principle (ISP)</h1>

### 1. Định nghĩa

> A client should never be forced to implement an interface that it doesn’t use or clients shouldn’t be forced to depend on methods they do not use.

Một class không nên buộc phải implement một interface mà nó không sử dụng hoặc là 1 class không nên buộc phải phụ thuộc vào những method mà nó không dùng.

### 2. Ví dụ

Quay trở lại với interface Shape ở ví dụ trước, chúng ta đã biết là ngoài hình dạng phẳng thì chúng ta cũng có hình dạng khối. Vậy chúng ta cũng muốn viết 1 class để tính tổng thể tích của các hình. Chúng ta có thể thêm method getVolume vào interface Shape

```php
interface Shape
{
    public function getArea();

    public function getVolume();
}
```

Với interface Shape như này thì mọi hình khi implement Shape thì đều phải khai báo 2 method là getArea và getVolume. Nhưng với những hình dạng phẳng thì chỉ có diện tích chứ không có thể tích. Vì vậy interface Shape đã buộc các class hình dạng phẳng phải khai báo method mà nó không sử dụng.

Để giải quyết vấn đề này thì chúng ta sẽ tạo 2 interface là Shape và SolidShape như sau

```php
interface Shape
{
    public function getArea();
}

interface SolidShape
{
    public function getVolume();
}
```

Với 2 interface này thì chúng ta có thể định nghĩa 1 class hình lập phương như sau:

```php
class Cuboid implements ShapeInterface, SolidShapeInterface
{
    public function area()
    {
        // calculate the surface area of the cuboid
    }

    public function volume()
    {
        // calculate the volume of the cuboid
    }
}
```

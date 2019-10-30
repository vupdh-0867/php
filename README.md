# 1. Các đặt điểm cơ bản của lập trình hướng đối tượng trong PHP

## 1.1 Tính đóng gói

- Đóng gói là giới hạn quyền truy cập (hoặc thay đổi) giá trị của thuộc tính hoặc quyền gọi phương thức nhằm mục đích kiểm soát quyền truy cập vào các thuộc tính, hàm của một class

- Trong PHP, tính đóng gói được thực hiện bằng cách sử dụng các từ khóa
  - `public`: Cho phép truy cập(và thay đổi giá trị) của thuộc tính cũng như gọi các phương thức của class đó ở mọi nơi.

  - `private`: Chỉ cho phép truy cập(và thay đổi giá trị) của thuộc tính cũng như gọi các phương thức của một trong phạm vi class đó.

  - `protected`: Cho phép truy cập(và thay đổi giá trị) của thuộc tính cũng như gọi các phương thức của một trong phạm vi class đó và các lớp con của class đó.

```php
<?php
  class MyClass
  {
    public function myPublicFunction()
    {
      echo "I'm visible!";
    }

    private function myPrivateFunction()
    {
      echo "I'm not visible outside!";
    }

    protected function myProtectedFunction()
    {
      echo "I'm visible in child class!";
    }
  }

  class MyChildClass extends MyClass
  {
    public function myParentProtectedFunction()
    {
      $this->myProtectedFunction();
    }
  }

  $myClass = new MyClass();
  $myChildClass = new MyChildClass();

  $myClass->myPublicFunction(); //I'm visible!

  $myClass->myPrivateFunction(); //Fatal error

  $myClass->myProtectedFunction(); //Fatal error
  $myChildClass->myParentProtectedFunction(); //I'm visible in child class!
?>
```

## 1.2 Tính đa hình

Tính đa hình: cho phép các lớp con có thể viết lại (override) các thuộc tính hoặc phương thức từ lớp cha.Trong PHP:

- Các lớp con có thể viết lại hoặc mở rộng phương thức của lớp cha mà nó kế thừa.

- Các class cùng implement một interface nhưng chúng có cách thức thực hiện khác nhau cho các method của interface đó.

- Qua đó cùng 1 phương thức sẽ cho kết quả khác nhau khi được gọi bởi các đối tượng thuộc lớp khác nhau

```php
<?php
  class MyClass
  {
    public function myPublicFunction()
    {
      echo "I'm visible!";
    }

    protected function myProtectedFunction()
    {
      echo "I'm visible in child class!";
    }
  }

  class FirstChildClass extends MyClass
  {
    public function myParentProtectedFunction()
    {
      $this->myProtectedFunction();
    }
  }

  $firstChildClass = new FirstChildClass();

  $myClass->getClassName(); //I'm my class!
  $firstChildClass->getClassName(); //I'm first child class!
  $secondChildClass->getClassName(); //I'm second child class!
?>
```

## 1.3 Tính kế thừa

- Tính kế thừa: cho phép một lớp có thể kế thừa các thuộc tính và phương thức từ các lớp khác đã được định nghĩa. Lớp được kế thừa còn được gọi là lớp cha và lớp kế thừa được gọi là lớp con.

- Điều này cho phép các đối tượng có thể tái sử dụng hay mở rộng các đặc tính sẵn có mà không phải tiến hành định nghĩa lại.

- Trong PHP, việc kế thừa được thực hiện thông qua sử dụng từ khóa `extends`, các lớp kế thừa có thể sử dụng các thuộc tính và phương thức thuộc phạm vi `public` và `protected` của lớp cha

```php
<?php
  class MyClass
  {
    public function myPublicClass()
    {
      echo "I'm my class!";
    }

    public function myProtectedClass()
    {
      echo "I'm my class!";
    }
  }

  class ChildClass extends MyClass
  {
    public function getClassName()
    {
      echo "I'm first child class!";
    }
  }

  $firstChildClass = new FirstChildClass();

  $myClass->getClassName(); //I'm my class!
  $firstChildClass->getClassName(); //I'm first child class!
  $secondChildClass->getClassName(); //I'm second child class!
?>
```

## 1.4 Tính trừu tượng

Tính trừu tượng: giúp giảm sự phức tạp thông qua việc tập trung vào các đặc điểm trọng yếu hơn là đi sâu vào chi tiết.

Như vậy khi tương tác với đối tượng chỉ cần quan tâm đến các thuộc tính, phương thức cần thiết. Chi tiết về nội dung không cần chú ý đến.

Trong PHP có `abstract` class và `interface` để trừu tượng hóa các đối tượng.

### 1.4.1 Abstract class

Là một class cha cho tất cả các class có cùng bản chất. Bản chất ở đây được hiểu là kiểu, loại, nhiệm vụ của class. Quan hệ giữa một class khi thừa kế một abstract class được gọi là `is-a`

```php
<?php
  abstract class Vehicle
  {
    abstract protected function engine() : string;
  }

  class Car extends Vehicle
  {
    public function engine()
    {
      echo "I'm using gas engine";
    }
  }

  class Plane extends Vehicle
  {
    public function engine()
    {
      echo "I'm using jet engine";
    }
  }
?>
```

### 1.4.2 interface

Là tập hợp các chức năng mà bạn có thể thêm và bất kì class nào. Interface có thể bao gồm nhiều hàm/phương thức và tất cả chúng cùng phục vụ cho một chức năng. Một class khi hiện thực một interface được gọi là `can-do`.

Ví dụ dưới đây cho thấy tuy có cùng bản chất là `Vehicle` nhưng xe hơi (Car) không thể bay (Fly) vì vậy phương thức fly không nên đưa vào abstract class mà nên đưa vào một interface `Flyable`

```php
<?php
  interface Runable {
    function run();
  }

  interface Flyable {
    function fly();
  }

  class Car extends Vehicle implements Runable
  {
    public function run()
    {
      echo "I run by rolling my wheels";
    }
  }

  class Plane extends Vehicle implements Flyable
  {
    public function fly()
    {
      echo "I fly by jet power";
    }
  }
?>
```

Đặc điểm |  Abstract Class | Interface
-------- | ---- | -------
Đa thừa kế | Không, mỗi class chỉ có thể kế thừa (extends) một class | Có, một class có thể thực hiện (implement) nhiều Interface
Định nghĩa hàm | Có thể định nghĩa code xử lý cho phương thức | Chỉ có thể khai báo phương thức, Không thể định nghĩa code xử lý cho nó
Access modifier | Có thể sử dụng các modifier public, private, protected | Mọi phương thức đều mặc định là public.
Thuộc tính và constant | Có | Không

# 2. Phân biệt self và static trong PHP

Từ khóa static

- Một hàm static là hàm có thể gọi trực tiếp thông qua tên class mà không cần tạo đối tượng.

vd:

```php
class ConNguoi
{
  public static function getName()
  {
    echo "con nguoi";
  }
}

ConNguoi::getName();
```

Kết quả:

```bash
con nguoi
```

- Một thuộc tính static là một thuộc tính tĩnh, nó hoạt động nhu một biến toàn cục và luôn lưu giá trị cuối cùng nó nhận được

Có 2 cách để  truy cập biến static trong một class đó là dùng từ khóa self và static
Xét ví dụ sau đây:

```php
<?php
class ConNguoi
{
  static $name = "ConNguoi";

  public function getName()
  {
    echo self::$name;
    echo "<br>";
    echo static::$name;
  }
}

$cn = new ConNguoi();
$cn->getName();
```

Kết quả:

```bash
ConNguoi
ConNguoi
```

Như ta thấy thì cả self và static hoạt động tốt khi sử dụng trong cùng một class. Tuy nhiên khi kế thừa thì sẽ có sự khác biệt:

```php
<?php
class ConNguoi
{
  private static $name = "ConNguoi";

  public function getName()
  {
    echo self::$name;
    echo "<br>";
    echo static::$name;
  }
}

class NguoiLon extends ConNguoi
{
  protected static $name = "NguoiLon";
}

$a = new NguoiLon();
$a->getName();
```

Kết quả:

```bash
ConNguoi
NguoiLon
```

Như vậy khi gọi hàm getName, static đại diện cho class gọi hàm chứ nó, self đại diện cho class khai báo hàm chứa nó

# 3. Trait

Được hỗ trợ từ PHP 5.4 trở đi, Trait là một class có chức năng nhóm các function có thể được sử dụng ở nhiều class lại

```php
<?php
trait Database
{
  public function listUsers()
  {
    echo "List users!";
  }
}

class User
{
  use Database;

  public function reportUsers()
  {
    $this->listUsers();
  }
}

class Report
{
  use Database;

  public function index()
  {
    $this->listUsers();
  }
}

$report = new Report();
$user = new User();

$report->index();
$user->reportUsers();
?>
```

## Trường hợp dùng nhiều trait và các trait có các hàm trùng tên nhau

```php
trait Admin
{
  public function checkLogin()
  {
    echo "Check login role";
  }

  public function isAdmin()
  {
    echo "Ok";
  }
}

trait Authenticate
{
  public function checkLogin()
  {
    echo "Check login";
  }

  public function isAdmin()
  {
    echo "Ok";
  }
}

class User
{
  use Authenticate, Admin
  {
    //Sử dụng method checkLogin ở trait Authenticate thay cho  method checkLogin ở trait Admin
    Admin::checkLogin insteadof Admin;
    //Sử dụng method isAdmin ở trait Admin thay cho method isAdmin ở trait Authenticate
    Admin::isAdmin insteadof Authenticate;
  }

  public function login()
  {
    $this->checkLogin();
    $this->isAdmin();
  }
}

$user = new User();
$user->login();
```

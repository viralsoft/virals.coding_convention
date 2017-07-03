# PHP Coding Standard


### Table of contents
***

#### [PHP] (#php)
1. [Naming convention](#naming-convention)
2. [File format](#file-format)
3. [Indentation](#indentation)
3. [Open & close tag](#open-and-close-tag)
4. [Class, interface] (#class-interface)
5. [Method, function] (#method-function)
6. [Variable, property] (#variable-property)
7. [Constant] (#constant)
8. [Conditional and loop] (#conditional-and-loop)
9. [Boolean and NULL] (#boolean-and-NULL)
10. [Spaces and brackets] (#spaces-and-brackets)
11. [Comment] (#comment)
12. [Overlength code] (#overlength-code)

#### [Laravel] (#laravel)
1. [MVC] (#MVC-Architecture)
2. [Security] (#security)
3. [Composer] (#composer)

***


### PHP
***

#### Naming convention

- Sử dụng [Camel Case](http://en.wikipedia.org/wiki/CamelCase)
- Đặt tên file phải theo camelCase như trên
	* Nếu file đấy chứa 1 class hoặc interface thì viết hoa ký tự đầu. ví dụ: UserValidator.php
	* Còn lại các trường hợp khác thì không viết hoa ký tự đầu. ví dụ: userProfile.blade.php, configDatabase.php

**[⬆ back to top](#table-of-contents)**
#### File Format 

Luôn save file với encoding utf-8

**[⬆ back to top](#table-of-contents)**	
#### Indentation
Phải sử dụng space (thay cho tab) và mặc định là 4 spaces (có thể setup trong settings của text editor)

**[⬆ back to top](#table-of-contents)**
#### Open and close tag
Với bất kì một file php nào mà chỉ chứa code php, thì ko cần close the php tag '?>' và không được có space trước open tag

```php
	// INCORRECT
	// test.php
	<?php
		php_info();
	?>
				
	// INCORRECT
	// test.php	
	<?php
		php_info();
	?>
				 
	//CORRECT
	// test.php
	<?php
		php_info();
	// End of file, don't close the php tag
```

**[⬆ back to top](#table-of-contents)**				
#### Class, Interface
* Chỉ một class cho 1 file php
* Sử dụng Camel Case, các từ viết liền, viết hoa kí tự đầu của các từ trong tên của class ví dụ:  **AdminController**
* Tên phải ngắn gọn dễ hiểu, nêu rõ mục đích của class
* Dấu { ở cùng dòng với khai báo class

	```php
	// CORRECT
	class AdminHomeController extends BaseController {

	…….

	}
		
	// INCORRECT
	class Admin_homeController 
	{
	…
	}
		
	class Adminhomecontroller 
	s{
	..
	}
	```

**[⬆ back to top](#table-of-contents)**			
#### Method, function

* Camel Case, viết liền, ko viết hoa kí tự đầu của từ đầu tiền, các từ sau thì kí tự đầu viết hoa ví dụ: **getUserProfile**
* Tên ngắn gọn dễ hiểu, nói rõ mục đích của method
* Dấu { không cùng dòng với khai báo class

	```php
	// CORRECT
	function getAdminUsers($number = NULL) 
	{
	   …
	}
		
	// INCORRECT
	function get_Amin_users() {
	}
	```

**[⬆ back to top](#table-of-contents)**
#### Variable, property

* Tương tự như đặt tên method và function
* Các tên biến gồm 1 kí tự như $i, $j chỉ dùng trong vòng lặp for. Còn lại tên biến cần rõ ràng, thể hiện rõ chức năng

	```php
	// CORRECT
	$password
	$accessTtoken
		
	// INCORRECT
	$Password
	$access_Token
	$access_token
	```	

**[⬆ back to top](#table-of-contents)**				
#### Constant

Viết hoa tất cả các ký tự, nếu gồm nhiều từ thì cách các từ bằng kí tự "_"

```php
// CORRECT
define('MY_CONSTANT', 'MY_CONSTANT');
echo MY_CONSTANT;
```

**[⬆ back to top](#table-of-contents)**
#### Conditional and loop

Với tất cả các vòng lặp và điều kiện thì dấu { phải ở 1 dòng riêng

```php
// CORRECT
if ($test > 0)
{
  ...
}
else 
{
  ...
}
	
// CORRECT
for ($i = 0; $i < rows.length; i++)
{
 …
}
	
// CORRECT
foreach ($arr as $key => $val)
{
…
}			

```

**[⬆ back to top](#table-of-contents)**	
#### Boolean and NULL
Luôn viết ở dạng thường các kí tự này:

```php
true
false
null
```

**[⬆ back to top](#table-of-contents)**
#### Space and brackets
Không có dấu cách giữa nội dung bên trong của dấu và các dấu này

```php
// CORRECT
$arr['abc']
if ($isValid)
	
// INCORRECT
$arr[ 'abc' ]
if ( $isValid )
```

**[⬆ back to top](#table-of-contents)**
#### Comment
Comment theo [DocBlock](http://phpdoc.org/docs/latest/index.html) style. 1 số ví dụ:

```php
/**
* UserController Class
* 
* @package	Package Name
* @subpackage	Subpackage
* @category	Category
* @author	Linh Tran
* @link	
*/
class User_Controller extends Base_Controller 
{
...
}

	
/**
* Decode json string to array
*
* @access	public
* @param	string
* @return	Array
*/
function jsonDecode($str)
{
…
}
	
/**
* @var property to define user data
*/
protected $userData;
	
/**
* Constant to define timezone
*/
define('DEFAULT_TIMEZONE', 'Asia/Bangkok')

```				
		
				
* Với các trường hợp khác thì sử dụng inline comment. **Trước 1 block of code để thực hiện 1 chức năng nào đấy, cần phải sử dụng comment**.

```php
			// Create the hashed string for user password
			$password = Input::get('password')
			$hash = Hash::make($password)
```

**[⬆ back to top](#table-of-contents)**	
#### Overlength code
* Số lượng character trong 1 dòng nên <= 80
* Khi vượt quá thì nên xuống dòng và xử lý như trong các trường hợp sau

**Class**

```php
class SampleClass
    extends FooAbstract
    implements BarInterface {
}
```

**Function**

```php
public function bar($arg1, $arg2, $arg3,
   $arg4, $arg5, $arg6)
{
   // all contents of function
   // must be indented four spaces
}
```
    
**String**

```php		
$sql = "SELECT `id`, `name` FROM `people` "
	. "WHERE `name` = 'Susan' "
	. "ORDER BY `name` ASC ";
```
	     
**Array**

```php
// Normal Array
$sampleArray = array(1, 2, 3, 'Zend', 'Studio',
                	$a, $b, $c,
                	56.44, $d, 500);
// Associative array                 
$sampleArray = array(
'first_key'  => 'firstValue',
'second_key' => 'secondValue',
);
```
		
**Chained function call**

```php
$users = Users::where('id', 10)->whereIn('status', array(1,3)
	->select('users.*)->orderBy('created_at');
```
	
### Laravel
***
#### MVC Architecture
Trong 1 application cần phân biệt rõ giữa Model, View, Controller

* Model: Bao gồm các business logic ví dụ như:

  * Database Interactions
  * File I/O
  * Interactions with Web Services
  
* Controller: Chức năng kết nối giữa Model và View, xử lý Input, lấy dữ liệu từ Model cho View
* View: **View ko được chứa code logic**. View chỉ dùng đễ thể hiện các dữ liệu mà controller đã trả về cho View. 
  * Nên sử dụng Blade template của Laravel để có thể có được sự phân tách rõ ràng hơn giữa View và Controller <http://laravel.com/docs/views/templating> 
 
**[⬆ back to top](#table-of-contents)** 
  
#### Security
* Luôn lấy input thông qua hàm của Laravel: Input::get('…') tránh lấy trực tiếp từ $_GET, $_POST vì Laravel sẽ xử lý input tránh XSS, SQL injection
* Trừ những trường hợp mã hoá không quan trọng, thì nên sử dụng các hàm mã hoá của Laravel
  * Ví dụ như lấy hash password (không cần giải mã)thì dùng Hash::make('…')
  * Các trường hợp cần giải mã thì dùng Crypter::encrypt('..')
  * Tránh dùng md5 vì nó không bảo mật, dễ bị phá
 
**[⬆ back to top](#table-of-contents)**  
#### Composer
* Laravel > 4 sử dụng [Composer](http://getcomposer.org/) để quản lý các 3rd packages sử dụng cho application của mình. Có thể tìm các packages này ở trang [https://packagist.org/](https://packagist.org/)
* Có thể chỉnh sửa các thiết lập composer cũng như add thêm các thư mục để application autoload ở trong file ```composer.json``` trong thư mục gốc 

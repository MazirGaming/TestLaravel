**Câu 1**. Cho 3 URI tương ứng với 3 controller và method sau
    - `/admin/product` - ProductController@index
    - `/admin/user` - UserController@index
    - `/admin/category` - CategoryController@index
​
Hãy định nghĩa các route cho các URI trên với các yêu cầu sau
- Sử dụng prefix chung là `admin`
- Cả 3 route đều là `GET`
- User phải login mới đc access vào 3 route trên
- Sau khi tạo xong, chỉ duy nhất 3 route đc tạo ra, ko đc phép hơn

**Trả Lời**

    Route::prefix('admin')->middleware('auth')->group(function () {
        Route::get('/product', [App\Http\Controllers\Admin\ProductController::class, 'index']);
        Route::get('/user', [App\Http\Controllers\Admin\UserController::class, 'index']);
        Route::get('/category', [App\Http\Controllers\Admin\CategoryController::class, 'index']);
    });

**Câu 2**. Hãy cho biết sự khác nhau giữa post và get (nêu rõ sự giống nhau và khác nhau). 
Trong thực tế POST hay get được dùng nhiều hơn.

**Trả Lời**

Trong lập trình web. Để xử lý việc nhận gửi thông tin từ 1 form của người dùng nhập vào là rất thường xuyên.
Chúng ta thường sử dụng 2 phương thức POST và GET.
- Giống nhau: Đều gửi dữ liệu tới server để xử lý, sau khi người dùng nhập thông tin vào Form và submit.
- Khác nhau:

POST: Bảo mật hơn GET vì dữ liệu được gửi ngầm, không xuất hiện trên URL
GET: Dữ liệu được gửi tường minh, chúng ta có thể nhìn thấy trên URL, đây là lý do khiến nó không bảo mật so với POST. 
Nó còn bị giới hạn số ký tự bởi URL của web browsers.

GET thực thi nhanh hơn POST vì nhứng dữ liệu gủi đi luôn được Webbrowser cached lại

Khi dùng phương thức POST thì server luôn thực thi và trả về kết quả cho client, còn phương thức GET ứng với cùng 1 yêu cầu đó webbrowser sẽ xem trong cached có kết quả tương ứng với yêu cầu đó ko và trả về ngay không cần phải thực thi các yêu cầu đó ở phía server

Đối với những dữ liệu luôn được thay đổi thì chúng ta nên sử dụng phương thức POST, còn dữ liệu ít thay đổi chúng ta dùng phương thức GET để truy xuất và xử lý nhanh hơn.

Tùy vào mục đích, tính năng của từng trang web: Nếu web tĩnh, không cần xử lý dữ liệu nhiều bên phía server và không yêu cầu bảo mật khi
gửi dữ liệu thì sẽ dùng GET nhiều hơn POST và ngược lại.


**Câu 3** Kể tên các loại Route bạn thường sử dụng trong Laravel

**Trả Lời**
- Route::get($uri, $callback);
- Route::post($uri, $callback);
- Route::put($uri, $callback);
- Route::patch($uri, $callback);
- Route::delete($uri, $callback);
- Route::resource;

**Câu 4**. Nếu trong thư mục `database/migrations` của bạn đang có 3 file migration lần lượt là 1,2,3. Bạn đã chạy `php artisan migrate` cho 3 file đó rồi.
​
Sau đó bạn tạo thêm 2 file migration lần lượt là 4,5. Lúc này bạn tiến hành chạy lệnh `php artisan migrate`. Những file nào sẽ được thực thi, vì sao lại là những file đó?

**Trả Lời**

Sau khi tiến hành chạy lệnh `php artisan migrate`. File 4,5 sẽ được thực thi. Lệnh trên sẽ chạy những file migration chưa thực thi và bỏ qua file migrations đã hoàn thành.

**Câu 5**. Nếu trong thư mục `database/migrations` của bạn đang có 3 file migration lần lượt là 1,2,3. Bạn đã chạy `php artisan migrate` cho 3 file đó rồi.
Sau đó bạn tạo thêm 2 file migration lần lượt là 4,5. Lúc này bạn tiến hành chạy lệnh `php artisan migrate:rollback`. Những file nào sẽ được thực thi, vì sao lại là những file đó?

**Trả Lời**

Rollback sẽ thực hiện lại hoạt động di chuyển mới nhất. Vì trước đó đã chạy xong 3 file migration rồi, nên 3 file 1,2,3 sẽ được thực thi.
Còn file 4,5 chưa chạy `php artisan migrate`, nên sẽ không được thực thi.

**Câu 6**. Hãy cho biết sự khác nhau giữa migrate:refresh và migrate:reset trong Laravel

**Trả Lời**

Lệnh migrate:reset sẽ khôi phục tất cả các lần di chuyển của project.
Lệnh migrate:refresh sẽ khôi phục tất cả các lần di chuyển và sau đó thực hiện lệnh `php artisan migrate`

**Câu 7**. Hãy lấy một ví dụ về 1 trường hợp Query N+1 và giải thích nó bằng code demo

**Trả Lời**

Ví dụ bảng Courses và Categories liên kết với nhau. Để lấy ra những Course thuộc Category nào thì chúng ta sẽ phải truy vấn. Trong quá trình truy vấn, sẽ truy xuất tất cả Course từ CSDL, rồi sau đó thực hiện một truy vấn khác cho từng Category để truy xuất Category tương ứng với Course.

Code Demo Query N+1. Giả sử bảng Courses và Categories đã liên kết với nhau.

    use App\Models\Course;
    
    $courses = Course::all();
    
    foreach ($courses as $course) {
        echo $course->category->name;
    }

- Để khắc phục lỗi này. Sẽ dùng phương thức with()

**Câu 8**. Hãy kể tên một số ORM Relationship trong Laravel mà bạn đã sử dụng (tối thiểu 5) kèm giải thích phía sau.

**Trả Lời**

Một số ORM Relationship trong Laravel: 
- One To One: Liên kết các bảng mối quan hệ 1 - 1.
- One To Many: Mối quan hệ một nhiều được sử dụng trong trường hợp một model (A) sẽ được liên kết đến một hoặc nhiều model khác (B).
- Has One Through: Mối quan hệ 1-1 tuy nhiên chúng phải liên kết với nhau thông qua một model khác.
- Many To Many: Cho phép ứng dụng của bạn có một bảng có thể được liên kết với các bảng khác.

**Câu 9**. Hãy cho biết sự khác nhau giữa Session và Cookie trong PHP
**Trả Lời**
- Session dùng để lưu trữ dữ liệu trên Server và đồng thời nó sẽ có một đoạn code dữ liệu được lưu trữ ở client (cookie), session sẽ bị mất khi tắt trình duyệt hoặc hủy bằng lệnh.
- Cookie được lưu trữ trên máy tính của Client và PHP có thể truy xuất tới được. Và để sử dụng được Cookie thì trình duyệt phải hỗ trợ chức năng này, nếu không thì Cookie trở nên vô dụng. Cookie sẽ không bị mất khi bạn đóng ứng dụng, nó phụ thuộc vào thời gian sống mà bạn thiết lập cho nó.

**Câu 10**. Để kiểm tra một `column name` là email có phải là column của table `users` hay không, bạn kiểm tra như thế nào trong Laravel. Cho ví dụ
**Trả Lời**
Sử dụng Schema Builder
use Illuminate\Support\Facades\Schema;
Schema::hasColumn('users', 'email')

**Câu 11**. Để tạo một Rule trong Laravel bạn sử dụng câu lệnh nào?
    Để truyền một giá trị từ Request vào trong Rule bạn truyền thông qua method nào
    Cho ví dụ về cách truyền
**Trả Lời**
Để tạo một Rule trong Laravel:
    Cách 1: Chạy lệnh: php artisan make:request FormRequestName
    Cách 2: Chạy lệnh: php artisan make:rule Uppercase
Để truyền một giá trị từ Request vào trong Rule sẽ truyền thông qua method rule(), validate()
VD: 

    public function rules()
        {
            return [
                'id' => ['required', 'integer']
            ];
        }

**Câu 12**. Hãy cho biết cách tạo một đối tượng (object) từ một class trong PHP. cho ví dụ 

**Trả Lời**

Sử dụng từ khóa new để tạo Object từ một class trong PHP.
VD: 

    $name = new Human();

**Câu 13**. Hãy cho biết ý nghĩa của abstract class, khi nào nên sử dụng abstract class
**Trả Lời**
- Lớp Abstract sẽ định nghĩa các hàm (phương thức) mà từ đó các lớp con sẽ kế thừa nó và Overwrite lại (tính đa hình). Tất cả các phương thức của lớp abstract đều phải được khai báo là abstract và phải ở mức protected và public, không được ở mức private. Lớp Abstract có thể có thuộc tính nhưng thuộc tính không được khai báo là abstract, và bạn không thể khởi tạo một biến của lớp Abstract được.
- Sử dụng abstract class khi muốn một class cha cho tất cả các class có cùng bản chất. Bản chất ở đây được hiểu là kiểu, loại, nhiệm vụ của class

**Câu 14**. Hãy cho biết ý nghĩa của interface, khi nào nên sử dụng interface

**Trả Lời**

- Interface Trong hướng đối tượng là một khuôn mẫu, giúp cho chúng ta tạo ra bộ khung cho một hoặc nhiều đối tượng và nhìn vào interface thì chúng ta hoàn toàn có thể xác định được các phương thức và các thuộc tính cố định (hay còn gọi là hằng) sẽ có trong đối tượng implement nó.
- Interface thường được sử dụng trong trường hợp các class kế thừa không có cùng bản chất (nhóm đối tượng) nhưng chúng có thể thực hiện các hành động giống nhau.

**Câu 15**. Hãy cho biết sự giống và khác nhau giữa where và whereIn trong Laravel
**Trả Lời**
- Giống nhau: Đều tìm theo điều kiện.
- Khác nhau: 
    + where sẽ nhận 3 đối số truyền vào. Sẽ tìm ra tất cả dữ liệu đúng với điều kiện truyền vào
    + whereIn sẽ nhận 2 đói số truyền vào. Sẽ kiểm tra giá trị của cột nằm trong một mảng.

**Câu 16**. Hãy cho biết ý nghĩa của việc đặt tên route trong laravel. Cho ví dụ về cách đặt tên route trong Laravel

**Trả Lời**

Việc đặt tên route trong laravel giúp tường minh hơn về nghĩa, giúp lập trình viên dễ nhớ, dễ hiểu và dễ dàng sử dụng. 
Sẽ tiện lợi trong quá trình code, bảo trì, sửa chữa code. Nếu như phải can thiệp, chỉnh sửa đường dẫn thì ở phía view cũng sẽ không phải chỉnh sửa nhiều.

**Câu 17**. Một object user có attribute là first_name và last_name. Giá trị của first_name là `Mr.` giá trị của `last_name` là Laravel. Định nghĩa trong model để có thể tạo ra một attribute mới `fullname`. Fullname là giá trị của first_name + last_name.
**Trả Lời**
Định nghĩa trong Model User.

    use Illuminate\Database\Eloquent\Casts\Attribute;

    public function getFullNameAttribute()
    {
        return "{$this->first_name} {$this->last_name}";
    }

**Câu 18**. Một product có thể thuộc nhiều category và một category có thể có nhiều product.
Hãy viết Relationship thể hiện điều này (viết nó trong model Product, Category)

**Trả Lời**

Bảng Product

    <?php
    
    namespace App\Models;
    
    use Illuminate\Database\Eloquent\Model;
    
    class Product extends Model
    {
        public function categories()
        {
            return $this->hasMany(Category::class);
        }
    }

Bảng Category

    <?php
 
    namespace App\Models;
    
    use Illuminate\Database\Eloquent\Model;
    
    class Category extends Model
    {
        public function product()
        {
            return $this->belongsTo Product::class);
        }
    }



**Câu 19**. Điều gì sảy ra khi bạn submit một form không có input name='_token'

**Trả Lời**

Khi submit một form không có input name='_token' sẽ gặp lỗi page expired.

**Câu 20**. Làm thế nào để kiểm tra một table đã tồn tại trong database (trong Laravel)

**Trả Lời**

Sử dụng phương thức hasTable

    Schema::hasTable('Tên Bảng')

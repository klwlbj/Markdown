### 请求周期

- 进入入口文件`index.php`
- 加载`composer`的自动加载文件及`boostrap/app.php`中检索应用实例，<mark>创建容器</mark>

- 请求发送至HTTP内核`Kernel.php`(检测应用环境，异常处理、定义中间件等)
- <mark>注册服务提供者，实例化每个服务（如数据库、队列、验证器等），绑定到容器上</mark>
- 进入路由，服务提供者`App\Providers\RouteServiceProvider`（包含<mark>中间件</mark>）
- 实例化控制器
- 控制器响应内容
- 中间件返回
- 内核返回响应对象
- `index.php`调用send方法响应内容

### 底层核心

#### 服务容器

在构建 Laravel 应用程序时，您将要编写的许多类都可以通过容器自动接收它们的依赖关系，包括 控制器、事件监听器、 中间件 等等

在路由中注入Request类

```php
use Illuminate\Http\Request;

Route::get('/', function (Request $request) {
    // ...
});
```

##### 绑定

简单绑定

```php
use App\Services\Transistor;
use App\Services\PodcastParser;

$this->app->bind(Transistor::class, function ($app) {
    return new Transistor($app->make(PodcastParser::class));
});
```

单例绑定

只实例化一次

#### 服务提供者

所有 Laravel 应用程序的引导中心，保存在`config/app.php`

#### 契约Contracts

显示地定义依赖；构造函数中引入

#### 门面Facades

在`Illuminate\Support\Facades`中定义

使用门面时，对应的类不能太大

辅助函数也是用了门面

```php
class Cache extends Facade
{// 实际调用类
protected static function getFacadeAccessor()
    {
        return Cache::class;// 或者返回名称，但需要额外在alias 中定义
    }
}
```

##### 实时门面

要生成实时 Facade，请在导入类的名称空间中加上Facades

##### 优点

简化类名；

##### 缺点

易范围溢出，易使用，无需注入，但可能使类越来越大

#### 中间件

过滤http请求

在响应前后都能执行；

需要在Kernel中注册，区分全局中间键和路由中间键(仅针对单个路由)

handle方法示例

```php
namespace App\Http\Middleware;

use Closure;

class ExampleMiddleware
{
    public function handle($request, Closure $next)
    {
        // 处理请求
        $request->headers->set('Authorization', 'Bearer my-token');

        // 调用下一个中间件或者控制器方法
        $response = $next($request);

        // 处理响应
        $content = $response->getContent();
        $newContent = str_replace('Hello', 'Hi', $content);
        $response->setContent($newContent);

        return $response;
    }
}
```

##### 使用场景

-  认证授权

- 数据验证

-  日志

-  缓存

-  异常处理

- 跨域访问

### 数据库

#### 事务

自动事务

```php
DB::transaction(function () {
    DB::table('users')->update(['votes' => 1]);
    DB::table('posts')->delete();
}, 5);//5表示死锁重试次数
```

#### 查询

whereKey

whereKeyNot

firstWhere  where后再first

$flight->fill(['name' => 'Amsterdam to Frankfurt']);填充数组至对象值

chunk 分块

```php
每次只从数据库查询100个，处理完再查下100个
DB::table('users')->orderBy('id')->chunk(100, function ($users) {
    foreach ($users as $user) {
        //
    }
});
```

pluck

```php
User::pluck('id'); // 相当于select(['id'])->get()->pluck()

$users = User::selectRaw('id, CONCAT(first_name," ",last_name) as full_name')->pluck('full_name', 'id'); 
// 尽量减少mysql函数使用，查询多字段用，再用集合分组会更好；
```

exists() 是否存在

or

使用闭包

```php
$users = DB::table('users')
            ->where('votes', '>', 100)
            ->orWhere(function($query) {
                $query->where('name', 'Abigail')
                      ->where('votes', '>', 50);
            })
            ->get();	
```

##### 批量更新

```php
YunUserBorrow::whereIn('id', $record_id_arr)
            ->where('status', 0)
            ->update(
                [
                    'status' => 1,
                    'number' => DB::raw('borrow_no'),
                    'name' => 'asd',
                ]);
```

#### 关联

##### 一对一

```php
return $this->hasOne(Phone::class,'自定义键名’);
```

反向关联

```php
return $this->belongsTo(User::class);
```

##### 一对多

```php
return $this->hasMany(Comment::class);
```

反向关联

```php
return $this->belongsTo(Post::class);
```

##### 多对多

```php
return $this->belongsToMany(Role::class);
```

##### **多态关联**



#### 游标查询

```php
foreach (Flight::where('destination', 'Zurich')->cursor() as $flight) {
    
}
```

#### with与load

with

一次性查询多个模型

```php
User::with('podcasts')->get();
$books = Book::with('author.contacts')->get();//嵌套预加载

$books = Book::with('author:id,name,book_id')->get();// 加载指定字段
Student::with(['parents:id', 'user:id,district_id'])->get();// 可加载多个模型的不同字段

$users = User::with(['posts' => function ($query) {
    $query->where('title', 'like', '%code%');
}])->get(); // 闭包内添加约束
```

load

惰性加载，需要时才加载，例如满足某些条件的情况

两次查询

针对model实例有效，不可用于collection实例

```php
$model->load(['user:id,name', 'book:id,name', 'department:id,name'])
```

##### N+1问题

详见：[链接](https://zhuanlan.zhihu.com/p/91559533)

```php
// 查询n+1次
$posts = App\Post::all();
$posts->map(function ($post) {
     return $post->author;
});
// 查询两次
$posts = App\Post::with('author')->get();
$posts->map(function ($post) {
    return $post->author;
});
```

#### 模型事件

saved updated等

### 集合

```php
// 创建集合
$collection = collect([1, 2, 3, 4, 5, 6, 7]);
// 集合分块
$chunks = $collection->chunk(4);
// [[1, 2, 3, 4], [5, 6, 7]]

$collection = collect(['name', 'age']);
$collectionB = collect(['George', 29]);
$combined = $collection->combine($collectionB);
// ['name' => 'George', 'age' => 29]

$collection = collect(['John Doe']);
$concatenated = $collection->concat(['Jane Doe1'])
// ['John Doe', 'Jane Doe', 'Johnny Doe1']

$collection = collect(['name' => 'Desk', 'price' => 100]);
$collection->contains('Desk');//松散比较
// true

$collection = collect([1, 2, 3, 4]);
$collection->count();

$collection = collect([1, 2, 2, 2, 3]);// 各值出现的次数
$counted = $collection->countBy();
$counted->all();
// [1 => 1, 2 => 3, 3 => 1]

$collection->dd();// 打印集合并exit

$collection = collect([1, 2, 3, 4, 5]);
$diff = $collection->diff([2, 4, 6, 8]);
$diff->all();
// [1, 3, 5]

$collection = collect(['a', 'b', 'a', 'c', 'b']);
$collection->duplicates();
// [2 => 'a', 4 => 'b']

$collection->each(function ($item, $key) {
    // 遍历每个元素，可替代foreach，模型集合和普通集合的方法有区别
});

// 过滤集合
$collection = collect([1, 2, 3, 4]);
$filtered = $collection->filter(function ($value, $key) {
    return $value > 2;
});
$filtered->all();
// [3, 4]

// 返回第一个元素
collect([1, 2, 3, 4])->first();
// 1

// 交换键和值
$collection = collect(['name' => 'taylor', 'framework' => 'laravel']);
$flipped = $collection->flip();
$flipped->all();
// ['taylor' => 'name', 'laravel' => 'framework']

// 分页
$collection = collect([1, 2, 3, 4, 5, 6, 7, 8, 9]);
$chunk = $collection->forPage(2, 3);
$chunk->all();
// [4, 5, 6]

$collection = collect(['name' => 'taylor', 'framework' => 'laravel']);
$value = $collection->get('name');
// taylor


$collection->isEmpty()

$collection->union() //合并集合，相当于，两个集合相加

pluck
pop
prepend
keyBy
map
```

#### 数组合并

```php
// merge 底层相当于array_merge
$collection1 = collect(['a' => 1, 'b' => 2]);
$collection2 = collect(['b' => 3, 'c' => 4]);
$merged = $collection1->merge($collection2);
// 结果为 ['a' => 1, 'b' => 3, 'c' => 4]

// union 底层相当于数组相加
$collection1 = collect(['a' => 1, 'b' => 2]);
$collection2 = collect(['b' => 3, 'c' => 4]);
$collection3 = collect(['d' => 5]);
$unioned = $collection1->union($collection2, $collection3);
// 结果为 ['a' => 1, 'b' => 2, 'c' => 4, 'd' => 5]


$collection1 = collect(['a', 'b', 'c']);
$collection2 = collect([1, 2, 3]);
$combined = $collection1->combine($collection2);
// 结果为 ['a' => 1, 'b' => 2, 'c' => 3]
```

### 验证器

常用验证规则

```text
required
between:min,max
```

#### Tinker

```
php artisan tinker 命令行模式
换行[linux shift+enter][windows \+enter];
退出[linux ctrl + d] [windows ctrl + d] 
```

### 辅助函数

『自定义辅助函数』存放于` bootstrap/helpers.php`（`composer.json` 文件autoload 引入）

```PHP
Arr::first($arr) //获取第一个元素
Arr::wrap 函数可以将给定值转换为一个数组，数组=>数组，空值=>空数组，集合=>数组
Arr::collapse 函数将多个数组合并为一个数组
Arr::pluck
Arr::get 函数使用「.」符号从深度嵌套的数组根据指定键检索值
$discount = Arr::get($array, 'products.desk.discount', $defaultValue);

Arr::only() 指定key的键值对
$array = ['name' => 'Desk', 'price' => 100, 'orders' => 10];

$slice = Arr::only($array, ['name', 'price']);

// ['name' => 'Desk', 'price' => 100]
Arr::set() 设定指定键的值
Arr::set($array, 'products.desk.price', 200);
Arr::forget($array,'key') 删除指定键值对
Arr::random($array); 随机返回一个值

Str::camel('foo_bar');驼峰大小写
Str::random(40) 生成随机字符串
```

### 迁移

为某个表创建迁移文件

`php artisan make:migration users_add_email_verified --table=users`

执行迁移文件

`php artisan migrate`

回滚

`php artisan migrate:rollback`

回滚所有迁移

`php artisan migrate:reset`

查看当前迁移状态

`php artisan migrate:status`

回滚所有迁移，并执行所有迁移

`php artisan migrate:refresh`

建表及更新表

```php
Schema::create('users', function (Blueprint $table) {
    $table->id();
    $table->string('name');
    $table->timestamps();
    $table->engine = 'InnoDB';
    $table->charset = 'utf8mb4';
    $table->collation = 'utf8mb4_unicode_ci';
    $table->comment('Business calculations'); //添加表注释
});

Schema::table('users', function (Blueprint $table) {
    $table->integer('votes')->after('id')->default(0)->comment('votes')；
    $table->string('visitor',100);//相当于varchar
    $table->char('name',100);
    $table->dropColumn(['votes','visitor']); // 删除字段
    
    $table->unique('email');//唯一索引
    $table->index(['account_id', 'created_at']);//联合索引
});
Schema::dropIfExists('users');//删除表
```

### 数据填充

生成工厂类

`php artisan make:factory AddressFactory`

生成填充文件

`php artisan make:seeder UserSeeder`

填充文件调用工厂类，生成测试数据

### 任务

#### 创建

`php artisan make:job CloseOrder`

### 队列

默认状态下，一个任务在执行，其它任务只能排队；可配置重试次数，失败的任务保存在failed_jobs表中；

继承自`ShouldQueue`接口

队列中调起任务

`dispatch`

`dispatchSync` 同步调度，立即执行

#### 命令

```
php artisan queue:work //部署时，要重新启动队列进程。

php artisan queue:listen
```

### 缓存

#### 驱动

Redis

File

保存至`storage/framework/cache`

```php
Cache::get('key', 'default value');
Cache::has('key');
Cache::increment('key', $amount);

$value = Cache::remember('users', $seconds, function () {
    return DB::table('users')->get();
});// 从缓存中读取，读不到就从数据库中读取，并写入缓存

Cache::pull('key'); // 查找后删除
Cache::put('key', 'value', $seconds = 10); // 写入缓存，缓存10s，时间为小于1，则是删除缓存
Cache::add('key', 'value', $seconds); // 不存在则写入
Cache::forever('key', 'value');// 永久存储
Cache::forget('key'); // 永久删除
Cache::flush(); // 删除所有缓存
```

### 事件

<mark>观察者模式</mark>

默认在服务提供者的`EventServiceProvider`中手动注册，<mark>高度解耦</mark>。

事件Events对应监听器Listeners

```php
use App\Events\OrderShipped;
use App\Listeners\SendShipmentNotification;

/**
 * 系统中的事件和监听器的对应关系。
 *
 * @var array
 */
protected $listen = [
    OrderShipped::class => [
        SendShipmentNotification::class,
    ],
];

public function boot()
    {
    // 也可在此注册
    }
```

#### 调用

```php
// 定义事件
__construct(Topic $topics)
// 定义监听器
handle()
调用事件
Event(new TopicAchievementCollect($topics));
```

### 日志

基于通道，使用Monolog库

配置在`config/logging.php`

可配置不同日志级别

有Handler和Formatter

#### 常用通道驱动

 ● slack

 ● single

 ● stack

● daily（每天）

#### 存储或发送方式

本地文件

警报邮件

第三方平台

前端浏览器

数据库

#### 默认格式

`[2023-05-08T15:44:36.377106+08:00] my_logger.WARNING: My logger is now ready [] {"dummy":"Hello world!"}`

示例：配置本地保留14天内日志

```php
[
    'daily' => [
            'driver'     => 'daily',
            'path'       => storage_path('logs/laravel.log'),
            'level'      => env('LOG_LEVEL', 'debug'),
            'days'       => 14,
            'permission' => 0777,
        ],
 ]
```

#### 日志级别

```php
Log::emergency($message);
Log::alert($message);
Log::critical($message);
Log::error($message);
Log::warning($message);
Log::notice($message);
Log::info($message);
Log::debug($message);
```

### 错误处理

写在`App\Exceptions\Handler`类中

```php
namespace App\Exceptions;

use Exception;
use Illuminate\Foundation\Exceptions\Handler as ExceptionHandler;
use Illuminate\Http\Request;

class Handler extends ExceptionHandler
{
    public function report(Exception $exception)
    {
        // 记录异常日志
        \Log::error($exception->getMessage());

        parent::report($exception);
    }

    public function render(Request $request, Exception $exception)
    {
        if ($exception instanceof \Illuminate\Database\Eloquent\ModelNotFoundException) {
            return response()->view('errors.404', [], 404);
        }
        
        // 返回通用的错误页面
        return response()->view('errors.500', [], 500);
    }
}
```

### 框架缓存

保存在`bootstrap/cache`目录，可删除

```shell
php artisan cache:clear # 清除缓存 
```

### 测试

概念

单元测试：针对单个方法

功能测试：针对功能

执行测试`php artisan test`

定义环境变量`phpunit.xml`

创建测试

```shell
php artisan make:test UserTest  # 创建功能测试
php artisan make:test UserTest --unit # 创建单元测试
```

测试覆盖率

```shell
php artisan test --coverage
```














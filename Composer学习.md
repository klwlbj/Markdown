#### 常用命令

```shell
composer init 初始化生成composer.json文件
composer install 如有 composer.lock 文件，直接安装，
若没有，则从 composer.json 安装最新扩展包和依赖;初次安装会生成composer.lock文件

composer update 按照 composer.json 指定的扩展包版本规则，把所有扩展包更新到最新版本，谨慎使用
composer show 展示composer安装过的所有组件
composer require new/package - 添加安装 new/package, 可以指定版本，如： composer require new/package ~2.5.
    --with-all-dependencies   一并更新新装包的依赖，防止不同组件版本不匹配
    --ignore-platform-reqs   忽略版本匹配
    --dev 仅开发环境安装
    -vvv 打印debug信息

composer remove new/package 移除包

composer global require new/package 全局安装

composer.json 只有几个核心包，虽然这些包会依赖其它包
composer.lock 支持版本控制

vim ~/.composer/composer.json 修改全局配置，不一定存在
composer config -l -g  查看全局配置

```

#### 额外命令

```shell
composer selfupdate 更新composer版本
composer diagnose 诊断
composer clear 清除缓存
```

详见：[Composer 命令行](https://docs.phpcomposer.com/03-cli.html)

#### 常用扩展包

##### brick/math

代替PHP原生bcmath

```php
整数，实例化用int是安全的，只要不超过最大值
BigInteger::of(123546);
BigInteger::of('9999999999999999999999999999999999999999999');
小数，浮点数实例化，用string类型更好
BigDecimal::of(1.99999999999999999999);打印出2
BigDecimal::of('1.99999999999999999999');打印出本来的小数
分数
BigRational::of('2/3');
BigRational::of('1.1'); // 11/10

$ten = BigInteger::of(10);

echo $ten->plus(5); // 15 加
echo $ten->multipliedBy(3); // 30 乘

BigInteger::of(1000)->dividedBy(3， RoundingMode::DOWN); // 333 除，四舍
BigInteger::of(1000)->dividedBy(3， RoundingMode::UP); // 334 除，五入
BigInteger::of(1000)->quotient(3); // 333，除，取整
BigInteger::of(1000)->remainder(3); // 1 求余
[$quotient, $remainder] = BigInteger::of(1000)->quotientAndRemainder(3);// 取整数加小数

BigDecimal::of(1)->dividedBy('8', 3); // 0.125，1/8保留三位小数
BigDecimal::of(1)->dividedBy('8', 2); // RoundingNecessaryException
BigDecimal::of(1)->dividedBy('8', 2, RoundingMode::HALF_DOWN); // 0.12
BigDecimal::of(1)->dividedBy('8', 2, RoundingMode::HALF_UP); // 0.13

有限小数
echo BigDecimal::of(1)->exactlyDividedBy(256); // 0.00390625
echo BigDecimal::of(1)->exactlyDividedBy(11); // RoundingNecessaryException
```

##### **elasticsearch/elasticsearch**

ES官方客户端

替代旧版<mark>已废弃</mark>的es客户端， tamayo/laravel-scout-elastic

##### **laravel/horizon**

laravel队列和定时任务管理工具

##### **vinkla/hashids**

url中的id转成uuid，可设置长度和盐值

##### **mikehaertl/phpwkhtmltopdf**

html转pdf

##### **monolog/monolog** 

打印日志

##### **yansongda/pay**

支付宝微信支付统一封装

##### **wnx/laravel-stats**

代码统计

##### **jenssegers/mongodb**

mongodb

##### **sentry/sentry-laravel**

异常监控

##### **mews/captcha**

验证码插件

##### **overtrue/laravel-wechat**

微信SDK

##### **endroid/qr-code**

链式生成二维码

##### **ipip/datx**

查询IP归属地

##### ~~**pda/pheanstalk**~~

beanstalkd队列客户端

##### **phpoffice/phpspreadsheet**

读写Excel表单

#### 本地Composer包开发

```shell
composer init
composer config repositories.icbc path ../composer/icbc # 链接
composer require klwlbj/icbc:dev-master # 引入

```



#### **引入**

引入类库时，一般做法：

1.composer require XXX/XXX

2.引入类库时，还要在服务提供者config/app.php中注册

3.复制配置文件到config目录，发布配置

```shell
php artisan vendor:publish --provider="XXXProvider"
```

#### 拓展包结构

```
weather/
├── .editorconfig # 编辑器配置文件，比如缩进大小、换行模式等
├── .gitattributes # git 配置文件，可以设计导出时忽略文件等
├── .gitignore # git 忽略文件配置列表
├── .php_cs # PHP-CS-Fixer 配置文件
├── README.md  # 说明
├── composer.json #该拓展包的依赖关系
├── phpunit.xml.dist
├── src  #源代码
│   └── .gitkeep
└── tests    #测试用例
    └── .gitkeep

```


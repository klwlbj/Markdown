### **Composer安装**

```shell
composer require --dev phpunit/phpunit ^6.2
```

#### 概念

区分功能测试和单元测试

#### 惯例

测试类继承自`PHPUnit\Framework\TestCase  `

以test开头的方法为测试方法

常用断言方法`assertEquals`

测试间的依赖

使用注释`@depends  `

#### 数据供给器

填充数据以测试

### 配置基境

`setUP`创建测试对象

`tearDown`清理测试对象

测试类的每个测试方法都会运行一次 setUp() 和 tearDown() 模板方法（同时，每个测
试方法都是在一个全新的测试类实例上运行的）。
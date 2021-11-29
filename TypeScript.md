# TypeScript
## 一、TS介绍
+ 以JavaScript为基础构建的语言
+ 一个JavaScript的超集
+ 可以在任何支持JavaScript的平台中执行
+ TypeScript扩展了JavaScript，并添加了类型

*TS不能被JS解析器直接执行 => 编译

## 二、TS开发环境搭建
1. 安装Node.js
2. 使用npm全局安装typescript：`npm i -g typescript`
3. 创建一个ts文件
4. 使用`tsc xxx.ts`对ts文件进行编译

## 三、TS的类型声明
### 1. 基本类型
  + 类型声明
    - 通过类型声明可以指定TS中变量（参数、形参）的类型
    - 指定类型后，当为变量赋值时，TS编译器会自动检查值是否符合类型声明，符合则赋值，否则报错
    - 语法：
      ```
      let a: number;

      let b: boolean = true;

      function sum(a: number, b: number): number{
        return a + b;
      }
      ```
  + 自动类型判断
    - 当对变量的声明和赋值是同时进行的，TS编译器会自动判断变量的类型       
  + 类型
    - 字面量
      ```
      let a: 10;
      let b: 'male' | 'female';
      let c: boolean | string;  // 联合类型
    - any
      * any表示的是任意类型，一个变量设置类型为any后相当于对该变量关闭了TS的类型检测（不建议使用）
      * 声明变量如果不指定类型，TS解析器会自动判断变量的类型为any（隐式any）
      ```
      let d: any;
      let d;
      d = 10;
      d = 'hello';
      d = true;
      ```
    - unknown（类型安全的any）
      * unknown表示未知类型的值
      ```
      let e: unknown;
      e = 10;
      e = 'hello';
      e = true;
      ```
      **any类型可以赋值给任意变量，unknown类型不能直接赋值给其他变量**
      ```
      let e: unknown;
      let s: sring;
      e = 'hello';
      if(typeof e === 'string'){
        s = e;
      }
      ```
      ```
      // 类型断言,可以用来告诉解析器变量的实际类型
      s = e as sting; // 变量 as 类型
      s = <sring>e; // <类型>变量
      ```
    - void
      * 用来表示空，以函数为例，表示没有返回值的函数
      ```
      function fn(): void{
        return undefined/null;
      }
    - never
      * 表示永远不会返回结果
      ```
      function fn(): never{
        throw new Error('报错了！');
      }
      ```
    - object
      ```
      // {} 用来指定对象中可以包含哪些属性
      let b: {
        name: string,
        age?: number  // ?表示属性可选
      };
      let c: { 
        name: string, 
        [propName: string]: any // 表示任意类型的属性
      };
      
      // 设置函数结构的类型声明
      let d: (a: number, b: number) => number;
      ```
    - array
      * 类型[]
      * Array<类型>
      ```
      let e: string[];  // 表示字符串数组
      let g: Array<number>;
      ```
    - turple
      * 元祖，固定长度的数组
      * [类型1, 类型2, 类型3]
      ```
      let h: [string, string];
      ```
    - enum
      * 枚举
      * enum 枚举名{ 枚举项1 = 枚举值1, 枚举项2 = 枚举值2 }
      * enum 枚举名{ 枚举项1, 枚举项2} => enum 枚举名{ 枚举项1 = 0, 枚举项2 = 1 }
      ```
      enum Gender{
        Male = 1,
        Female = 0
      }
      let i: {name: string, gender: Gender};
      i = {
        name: '',
        gender: Gender.Male
      }
      ```
      ```
      // &表示同时
      let j: { name: string } & { age: number };
      ```
      ```
      // 类型的别名
      type myType = 1|2|3|4|5;
      let m: myType;
      ```
## 四、TS编译选项
  + 自动编译
    - 使用-w指令后，TS编译器会自动监视文件的变化，并在文件发生变化时对文件进行重新编译
    - ```tsc xxx.ts -w```
  + 自动编译整个项目
    - `tsc`：将当前项目下的所有ts文件编译为js文件
      * 需要在根目录下创建ts的配置文件tsconfig.json
      * `tsc --init`
    - `tsc -w`：监视所有ts文件
  + tsconfig.json配置选项
    - include
      * 用来指定哪些ts文件目录需要被编译
      * 路径：\**表示任意目录，\*表示任意文件
      ```
      {
        "include": [
          "./src/**/*"
        ]
      }
      ```
    - exclude
      * 不需要被编译的文件目录
      * 默认值： `["node_modules","bower_components","jspm_packages"]
      ```
      {
        "exclude": [
          "./src/hello/**/*"
        ]
      }
      ```
    - extends
      * 继承配置文件
    - files
      * 指定被编译的文件的列表
      ```
      {
        "files": [
          "core.ts",
          "sys.ts"
        ]
      }
      ```
    - compilerOptions
      * 编译器选项
        * target：指定ts被编译为的es版本
          * `"target": "ES3"`
        * module：指定要使用的模块化的规范
          * `"module": "es2015"`
        * lib：指定项目中要使用的库
        * outDir：指定编译后文件所在的目录
          * `outDir: "./dist"`
        * outFile：所有的全局作用域中的代码会合并到同一个文件
          * `outFile: "./dist/app.js"`  
        * allowJs：是否对js文件进行编译，默认是false
          * `"allowJs: false"`  
        * checkJs：是否检查js文件是否符合语法规范，默认是false
          * `"checkJs": false`
        * removeComments：是否移除注释
        * noEmit：不生成编译后的文件
        * noEmitOnError：当有错误时不生成编译后的文件
        * alwaysStrict：用来设置编译后的文件是否使用严格模式
        * noImplicitAny：不允许隐式的any类型
        * noImplicitThis：不允许不明确类型的this
        * strictNullChecks：严格检查空值
          ```box1?.addEventListener('click', function(){});```
        * strict：所有严格检查的总开关
## 五、使用webpack打包ts代码
### 1. 
  + `npm init -y`：初始化项目，生成package.json
  + `npm i -D webpack webpack-cli typescript ts-loader`
  + 创建 webpack.config.json
    ```
    // 引入一个包
    const path = require('path');

    // webpack中所有的配置信息都应该写在module.exports中
    module.exports = {
        // 指定入口文件
        entry: "./src/index.ts",
        // 指定打包文件所在目录
        output : {
            // 指定打包文件的目录
            path: path.resolve(__dirname, 'dist'),
            // 打包后文件的名字
            filename: "bundle.js"
        },
        // 指定webpack打包时要使用的模块
        module: {
            // 指定要加载的规则
            rules: [
                {
                    // test指定的是规则失效的文件
                    test: /\.ts$/,
                    // 要使用的loader
                    use: 'ts-loader',
                    // 要排除的文件
                    exclude: /node_modules/
                }
            ]
        }
    }
    ```
  + 创建tsconfig.json
    ```
    {
      "compilerOptions": {
        "module": "es2015",
        "target": "es2015",
        "strict": true
      }
    }  
    ```
  + package.json中，scripts下加`"build": "webpack"`
### 2.
  + `npm i -D html-webpack-plugin`：自动生成html文件（dist目录下）
  + 在webpack.config.js中
    - 引入html插件：`const HTMLWebpackPlugin = require('html-webpack-plugin');`
    - 配置webpack插件：
      ```
      plugins: [
        new HTMLWebpackPlugin(),
      ]
      ```
  + 修改网页模板
    - 在webpack.config.js中
      ```
      plugins: [
        new HTMLWebpackPlugin({
          title: "自定义title"
        }),
      ]
      ```
      
    - 设置网页模板
      ```
      plugins: [
        new HTMLWebpackPlugin({
          template: "./src/index.html"
        })
      ]
      ```  
### 3. 
  + `npm i -D webpack-dev-server`：内置服务器
  + package.json中，scripts下加`"start": "webpack serve --open chrome.exe"`
### 4.
  + `npm i -D clean-webpack-plugin`：清除dist目录
  + 在webpack.config.js中
    - 引入clean插件：`const { CleanWebpackPlugin } = require('clean-webpack-plugin');`
    - 配置webpack插件
      ```
      plugins: [
        new CleanWebpackPlugin()
      ]
      ```
### 5.
  + 在webpack.config.js中设置引用模块
    ```
    resolve: {
     extensions: ['.ts', '.js']
    }
    ```
### 6. 
  + `npm i -D @babel/core @babel/preset-env babel-loader core-js`：兼容性
  + 配置webpack.config.js
    ```
    module: {
        // 指定要加载的规则
        rules: [
            {
                // 要使用的loader
                use: [
                    // 配置babel
                    {
                        // 指定加载器
                        loader: "babel-loader",
                        // 设置babel
                        options: {
                            // 设置预定义的环境
                            presets: [
                                [
                                    // 指定环境的插件
                                    "@babel/preset-env",
                                    // 配置信息
                                    {   
                                        // 要兼容的目标浏览器
                                        targets: {
                                            "chrome": "58",
                                            "ie": "11"
                                        },
                                        // 指定corejs的版本
                                        "corejs": "3",
                                        // 使用corejs的方式
                                        "useBuiltIns": "usage"  // "usage"表示按需加载
                                    }
                                ]
                            ]
                        }
                    },
                    'ts-loader'
                ]
            }
        ]
    }
    ```
    + 生成的bundle.js前面含箭头函数
      - webpack.config.js
        ```
        output : {
          // 告诉webpack不使用箭头函数
          environment: {
            arrowFunction: false
          }
        }
        ```
## 六、面向对象
### 1.类
  + 使用class关键字来定义类
    - 属性
      * 直接定义的属性是**实例属性**，需要通过对象的实例（new）去访问
      * 使用static开头的属性是**静态属性（类属性）**，可以直接通过类去访问
      * readonly开头的属性表示一个只读的属性，无法修改
      * 顺序：static readonly
    - 方法
      * 方法名() {}
      * 实例方法
      * 类方法 
### 2.构造函数和this
  ```
  class Dog {
    name: string;
    age: number;
    // constructor 被称为构造函数
    // 构造函数会在对象创建时调用
    constructor(name: string, age: number) {
      // 在实例方法中，this就表示当前的实例
      // 在构造函数中当前对象就是当前新建的那个对象
      // 可以通过this向新建的对象中添加属性
      this.name = name;
      this.age = age;
    }
    bark(){
      // 在方法中可以通过this来表示当前调用方法的对象
      console.log(this.name);
    }
  }
  ```
### 3.继承
  + 子类覆盖掉父类方法的形式，称为方法重写  
### 4.super关键字
  + 如果在子类中写了构造函数，在子类构造函数中必须对父类的构造函数进行调用
### 5.抽象类
  + 以abstract开头的类是抽象类，不能用来创建对象
  + 抽象类就是专门用来被继承的类
  + 抽象方法使用abstract开头，没有方法体
  + 抽象方法只能定义在抽象类中，子类必须对抽象方法进行重写
  + ```abstract sayHello():void;```
### 6.接口
  + 接口用来定义一个类结构，用来定义一个类中应该包含哪些属性和方法
  + 接口也可以当成类型声明去使用
  + 接口可以在定义类的时候去限制类的结构
    - 接口中的所有属性都不能有实际值
    - 在接口中所有的方法都是抽象方法
    - 接口只定义对象的结构，而不考虑实际值
  ```
  (function(){
    interface myInterface{
        name: string;
        age: number;
    }
    interface myInterface{
        gender: string;
        sayHello():void;
    }
    const obj: myInterface = {
        name: 'sss',
        age: 11,
        gender: '男',
        sayHello(){}
    }
  })();
  ```
  + 定义类时，可以使类去实现一个接口，即：使类满足接口的要求
  + ```class MyClass implements myInterface{}```
### 7.属性的封装
  + ts可以在属性前添加属性的修饰符
  + public 修饰的属性可以在任意位置访问修改（默认值）
  + private 私有属性，只能在类内部进行访问修改
    - 通过在类中添加方法使得私有属性可以被外部访问
    - 属性的存取器
      - getter方法用来读取属性
      - setter方法用来设置属性
    ```
    (function(){
      class Person{
        private name: string;
        private age: number;
        constructor(name: string, age: number){
          this.name = name;
          this.age = age
        }
        // 定义方法，用来获取name属性
        getName(){
            return this.name;
        }
        // 定义方法，用来设置name属性
        setName(value: string){
            this.name = value;
        }
      }
      const per = new Person('孙悟空', 18);
      per.setName('猪八戒');
      console.log(per.getName());
    })();
    ```
    ```
    (function(){
      class Person{
        private name: string;
        private age: number;
        constructor(name: string, age: number){
          this.name = name;
          this.age = age
        }
        // ts中设置getter方法的方式
        get name(){
            return this.name;
        }
        // ts中设置setter方法的方式
        set name(value: string){
            this.name = value;
        }
      }
      const per = new Person('孙悟空', 18);
      per.name = '猪八戒';
      console.log(per.name);
    })();
    ```
  + protected 受保护的属性，只能在当前类和当前类的子类中访问修改
  + 语法糖
    ```
    class Person{
      constructor(public name: string, public age: number){
      }
    }
    ```
### 8.泛型
  + 在定义函数或是类时，如果遇到类型不明确就可以使用泛型
  + 定义
    ```
    function fn<T>(a: T): T{
      return a;
    }
    ```
    ```
    // 泛型可以同时指定多个
    function fn2<T, K>(a: T, b: K): T{
      return a;
    }
    ``` 
    ```
    interface Inter{
      length: number;
    }
    // 泛型T是Inter实现类（子类）
    function fn3<T extends Inter>(a: T): number{
      return a.length;
    }
    ``` 
    ```
    class Myclass<T>{
      name: T;
      constructor(name: T) {
        this.name = name;
      }
    }
    ```
  + 使用
    ```
    // 可以直接调用
    fn(10); // 不指定泛型，ts可以自动对类型进行推断
    
    fn<string>('hello'); // 指定泛型
    ```
    ```
    const mc1 = new MyClass('孙悟空');
    const mc2 = new MyClass<string>('猪八戒');
    ```

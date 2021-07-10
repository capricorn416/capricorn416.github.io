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
      ```
      let h: [string, string];
      ```
    - enum
      * 枚举
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

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
      let c: boolean | string;
    - any
    
      any表示的是任意类型，一个变量设置类型为any后相当于对该变量关闭了TS的类型检测（不建议使用）
      
      声明变量如果不指定类型，TS解析器会自动判断变量的类型为any（隐式any）
      ```
      let d: any;
      let d;
      d = 10;
      d = 'hello';
      d = true;
      ```
    - unknown
      
      unknown表示未知类型的值
      ```
      let e: unknown;
      e = 10;
      e = 'hello';
      e = true;
      ```
      any类型可以赋值给任意变量，unknown类型不能直接赋值给其他变量
      ```
      let s: sring;
      e = 'hello';
      if(typeof e === 'string'){
        s = e;
      }
      ```
      ```
      // 类型断言
      s = e as sting;
      ```
      

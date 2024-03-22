---
title: TypeScript开发规范
date: 2019-11-11 18:36:50
categories: 规范
tags: [规范, JavaScript, TypeScript]
---

## 前言

随着TypeScript 的不断发展，越来越多的开发者认可并使用TypeScript开发应用。
<br>
本文的目标是使TypeScript新特性的代码风格保持一致。
<br>
由于TypeScript依然在快速发展，也随时保持更新。
<br>
最新更新时间2019-6-11

## 项目规范
- 项目必须有 README.md 描述文件
    - 项目描述
    - 引用技术描述
    - 代码风格描述
    - 目录结构
    - 运行环境
    - 开发命令，运行命令，以及其他命令
    - 复杂流程介绍等

## 环境

- TypeScript 文件使用.ts拓展名。含JSX语法的TypeScript 文件使用 .tsx扩展名。
- tsconfig.json 配置文件应开启 script, noImplicitReturns, noUnusedLocals 选项（建议）
- tsconfig.json 配置文件应开启 allowSyntheticDefaultImports 选项（建议）

```js
// good
import React, {PureComponent} from 'react';
// bad
import * as React from 'react';
```

## 文件

- 在文件结尾处保留一个空行。

## 命名

- 接口 使用 Pascal 命名法
- 接口名 不使用 I 作为前缀。

```js
// good
interface ButtonProps {}
// bad
interface IButtonProps {}
```

## 特性

- 使用const 声明 枚举

```typescript
// good
const enum Directions {
    UP,
    DOWN
}
// bad
enum Directions {
    UP,
    DOWN
}
```

## 类型

- 使用string / number / boolean 声明基本类型，不使用 String / Number / Boolean。

```typescript
// good
let str: string;
// bad
let str: String;
```

- 不使用 Object / Function 声明类型。
- 数组元素为简单类型（非匿名且不含泛型）时，使用T[]声明类型，否则应使用Array<T>。
- 数组元素为不可变数据时，使用 ReadonlyArray<T> 声明类型

```typescript
// good
let files: string[];
let array: Array<string | number>;
let buffer: Buffer[];
let responses: Array<Promise<number>>;

// bad
let files: Array<string>;
let array: (string | number)[];
let buffer: Array<Buffer>
let responses: Promise<number>[];
```

- 不使用 ！声明对象属性非空

```js
// good
if (foo.bar && foo.bar.baz) {}
// bad
if (foo!.bar!.baz) {}
```

- 使用 as 进行类型声明和转换，不适用<>

```ts
// good
const root = document.getElementById('root') as HTMLDivElement;
// bad
const root = <HTMLDivElement>document.getElementById('root');
```

- 接口不应为空
- 接口中同一函数重载的类型声明需相邻

```ts
// good
interface AnyInterface {
    foo();
    foo(x: string);
    bar();
    bar(x: string);
}

// bad
interface AnyInterface {
    foo();
    bar();
    foo(x: string);
    bar(x: string);
}
```

- 使用 === !=判断，不使用 == !=

```ts
// good
if (foo !== null && foo !== undefined) {}

// bad
if (foo != null) {}
```

- 使用 ... 将类数组对象转换为数组，不使用Array.form / Array.prototype.slice

```ts
// good
const element = [...document.querySelectorAll('.foo')];
// bad
const element = Array.from(document.querySelectorAll('.foo'));
// worst
const element = Array.prototype.slice.call(document.querySelectorAll('.foo'));
```
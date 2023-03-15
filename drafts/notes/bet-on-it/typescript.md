# TypeScript

## 1 TS 与 JS 的区别是什么？有什么优势？

**对比题** **记忆题**

1. 语法层面：TS 是 JS 的超集。(JS 有的 TS 都有)
2. 执行环境层面：浏览器、Node.js 可以直接执行 JS，但不能执行 TS（Deno 可以执行 TS）
3. 编译层面：TS 有编译阶段；JS 没有编译阶段（ 只有转移阶段和 lint 阶段）
4. 编写层面：TS 更难写一些，但类型更安全。
5. 文档层面：TS 的代码写出来就是文档，IDE 可以完美智能提示；JS 主要由 TS 贡献。

## 2 any、unknown、never 的区别是什么？

**对比题** **记忆题**

### 2.1 any 与 unknown

- 两者都是顶层类型，任何类型的值都可以赋值给顶级类型变量。
- 但是 unknown 比 any 的类型检查更严格，any 什么检查都不做，unknown 要求先收窄类型。
- 如果使用 any，基本不会报错，所以优先使用 unknown 类型会更安全些。

```typescript
// TypeScript
// 都不会报错
let foo: any = 123;
let bar: unknown = 123;

// 报错：Type "unknown" is not assignable to type "string".
const value: unknown = 123;
const count: number = value;

// 不报错
const value: unknown = 123;
const count: number = value as number;
```

### 2.2 never

- never 是底层类型，表示不应该出现的类型。

## 3 type 和 interface 的区别是什么？

**对比题** **记忆题**

- 官方文档给出的说明：

### 3.1 组合方式

- `interface` 使用 `extends` 来实现继承
- `type` 使用 `&` 来实现交叉类型

```typescript
// TypeScript
interface A {
  a: string;
}
interface B extends A {
  b: string;
}

type AA = {
  aa: string;
};
type BB = {
  bb: string;
} & AA;

const obj1: B = { a: "", b: "" };
const obj2: BB = { aa: "", bb: "" };
```

### 3.2 扩展方式

- `interface` 可以重复声明来扩展
- `type` 一个类型只能声明一次

```typescript
// TypeScript
// 1.js
interface A {
  a: string;
}

// 2.js
interface A {
  b: string;
}

const obj: A = { a: "", b: "" };

type B = {
  a: string;
};

// 报错 Duplicate identifier 'B'.
type B = {
  b: string;
};
```

### 3.3 范围不同

- `type` 适用于基本类型
- `interface` 一般不行

### 3.4 命名方式

- `interface` 会创建新的类型名
- `type` 只是创建类型别名，并没有创建新的类型

```typescript
// TypeScript
type X = { a: string };

const x: X = { a: "" };

// Y 的类型是 { a: string }，而不是 X
type Y = typeof x;
```

## 4 TS 工具类型 Partial、Required、Readonly、Exclude、Extract、Omit、ReturnType 的作用和实现？

**记忆题**

1. 将英文翻译为中文。
2. 举例说明每个工具类型的用法。

### 4.1 Partial 部分类型

```typescript
// TypeScript
interface User {
  id: string;
  name: string;
}

// Partial<User> 是 User 类型的一部分
const user: Partial<User> = {
  name: "Woozyzzz",
};
```

### 4.2 Required 必填类型

```typescript
// TypeScript
interface User {
  id?: string;
  name: string;
}

// Required<User> 的所有属性都必须存在
const user: Required<User> = {
  id: "111",
  name: "Woozyzzz",
};
```

### 4.3 Readonly 只读类型

```typescript
// TypeScript
interface User {
  id?: string;
  name: string;
}

// Readonly<User> 是不能修改的
// 报错 Cannot assign to 'id' because it is a read-only property.
const user: Readonly<User> = {
  id: "111",
  name: "Woozyzzz",
};

user.id = "222";
```

### 4.4 Pick/Omit 挑出/排除 key 类型

```typescript
// TypeScript
interface User {
  id?: string;
  name: string;
  age: number;
}

interface God {
  id?: string;
  name: string;
}

// Pick<User, "id" | "name"> 挑出对象中某些属性
type Goddess = Pick<User, "id" | "name">;

// Omit<User, "age"> 省略对象中某些属性
type Lilith = Omit<User, "age">;

// Readonly<User> 是不能修改的
// 报错 Cannot assign to 'id' because it is a read-only property.
const user: Readonly<User> = {
  id: "111",
  name: "Woozyzzz",
};

user.id = "222";
```

### 4.5 Extract 提取类型 / Exclude 排除类型

```typescript
// TypeScript
type Dir = "东" | "南" | "西" | "北";

// Extract<Dir, "北"> 提取基本类型操作
type Dir2 = Extract<Dir, "北">;
// Exclude<Dir, "北"> 排除基本类型操作
type Dir3 = Exclude<Dir, "北">;
```

### 4.6 ReturnType 返回值类型

```typescript
// TypeScript
function fn(a: number, b: number) {
  return a + b;
}

// ReturnType<typeof fn> 获取函数返回值类型
type A = ReturnType<typeof fn>;
```

### 4.7 Record 记录类型

```typescript
// TypeScript
type A = Record<string, number>;
// 等价于
type B = {
  [x: string]: number;
};
// 设置对象的键与值的类型，其中键只能为 number|string|symbol
type C = Record<"name" | "age", number>;
```

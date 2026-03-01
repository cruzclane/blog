---
title: React+TypeScript 组件类型注解的两种核心写法
aliases:
  - React+TypeScript 组件类型注解的两种核心写法
date: 2025-11-13
type:
tags:
  - React
  - TypeScript
updated: 2025-11-13
---
对于 React 初学者来说，TypeScript 类型注解常常是入门难点——尤其是组件 props 的类型定义，既想让 IDE 给出清晰提示（必选/可选、类型、参数名），又不想被复杂的类型语法绕晕。

本文将总结两种最实用的 React 组件类型注解写法，帮你快速上手并做出合适选择。


## 一、核心前提：为什么需要组件类型注解？
在写 React 组件时，类型注解的核心价值的是：
1. **IDE 智能提示**：父组件使用时，自动显示 props 名称、必选/可选标记、类型说明；
2. **提前报错**：漏传必选 props、传错类型时，TypeScript 直接标红，避免运行时bug；
3. **代码可读性**：他人（或未来的你）看代码时，能快速知道组件需要什么参数。

而我们要讲的两种写法，都能实现这些价值——只是风格和适用场景不同。


## 二、写法一：Function 关键词 + 单独 Props 接口
这是最贴合直觉的写法，尤其适合刚接触 React+TS 的同学，核心是“先定义 props 类型，再给组件函数标注类型”。

### 1. 基础写法（完整示例）
```tsx
// 1. 导入 React（可选，若用 JSX.Element 需导入）
import React from 'react';

// 2. 定义语义化 Props 接口（组件名 + Props 命名规范）
interface GreetingCardProps {
  // 必选属性：无 ? 符号，类型明确
  username: string;
  // 可选属性：带 ? 符号，可配合默认值
  message?: string;
  // 复杂类型：回调函数（参数类型+返回值类型）
  onSend?: (content: string) => void;
}

// 3. 组件定义：function 关键词 + 参数解构 + 类型标注
function GreetingCard({
  username,
  message = '欢迎使用 React+TS！', // 可选属性设置默认值
  onSend
}: GreetingCardProps): JSX.Element { // 显式标注返回值类型
  const handleClick = () => {
    // 可选链语法：避免 onSend 为 undefined 时报错
    onSend?.(`${username}说：${message}`);
  };

  return (
    <div className="greeting-card">
      <h3>你好，{username}！</h3>
      <p>{message}</p>
      <button onClick={handleClick}>发送消息</button>
    </div>
  );
}

// 4. 导出组件（分开导出，提升可读性和扩展性）
export default GreetingCard;
```

### 2. 核心要点（初学者必看）
- **Props 接口命名**：必须遵循 `[组件名]Props`（如 `GreetingCardProps`），语义化且易关联；
- **解构位置**：在函数参数中直接解构 props，父组件使用时能看到具体参数提示（而非模糊的 `props` 变量）；
- **返回值类型**：用 `JSX.Element` 更直观（与 JSX 语法对应），和 `React.ReactElement` 本质等价；
- **export 写法**：分开导出（先定义后导出），避免一行代码拥挤，后续添加静态属性/扩展更方便。

### 3. 父组件使用效果
```tsx
import GreetingCard from './GreetingCard';

function Parent(): JSX.Element {
  return (
    <div>
      {/* 输入 <GreetingCard 后，IDE 自动提示：
        - username（必选）: string
        - message（可选）: string | undefined
        - onSend（可选）: (content: string) => void
      */}
      <GreetingCard
        username="小明"
        message="一起学习 TypeScript 吧！"
        onSend={(content) => console.log(content)}
      />
    </div>
  );
}
```


## 三、写法二：Const 箭头函数 + React.FC（函数式风格首选）
这种写法是“const 变量 + 箭头函数 + React.FC 泛型”，核心是用 React 内置的 `React.FC`（FunctionComponent）类型模板，简化类型标注。

### 1. 基础写法（完整示例）
```tsx
import React from 'react';

// 1. 定义 Props 接口（无需手动加 children！）
interface GreetingCardProps {
  username: string;
  message?: string;
  onSend?: (content: string) => void;
}

// 2. 组件定义：React.FC<Props> 标注变量类型
const GreetingCard: React.FC<GreetingCardProps> = ({
  username,
  message = '欢迎使用 React+TS！',
  onSend,
  children // 自动获得 children 属性，无需在 Props 接口定义
}) => {
  const handleClick = () => {
    onSend?.(`${username}说：${message}`);
  };

  return (
    <div className="greeting-card">
      <h3>你好，{username}！</h3>
      <p>{message}</p>
      {children} {/* 直接使用 children，无需额外配置 */}
      <button onClick={handleClick}>发送消息</button>
    </div>
  );
};

export default GreetingCard;
```

### 2. 核心要点（搞懂 React.FC 的本质）
- **React.FC 是什么？**：它是 React 内置的**泛型类型模板**，本质是给变量标注“这是一个 React 函数组件”，语法简化为 `React.FC<Props接口>`；
- **自动包含的特性**：
  1. 内置 `children?: React.ReactNode` 属性，无需在 Props 接口手动定义；
  2. 自动约束返回值类型为 `JSX.Element`，不用额外写返回值标注；
- **变量标注的意义**：`const GreetingCard: React.FC<GreetingCardProps>` 是给变量 `GreetingCard` 贴“组件标签”，告诉 TypeScript 这个变量对应的箭头函数是 React 组件，需遵循 Props 接口规范。

### 3. 父组件使用效果
和写法一完全一致——父组件使用时，IDE 同样会显示完整的 props 提示（必选/可选、类型、名称），漏传必选 props 会标红报错。


## 四、两种写法核心对比（初学者怎么选？）
| 对比维度                | 写法一（Function + 单独 Props）       | 写法二（Const 箭头 + React.FC）        |
| :---------------------- | :------------------------------------ | :------------------------------------- |
| **可读性**              | 高，符合传统函数认知，新手易理解      | 简洁，但需理解 React.FC 泛型，略抽象    |
| **children 支持**       | 需手动在 Props 接口添加 `children?: React.ReactNode` | 自动内置，无需手动定义                  |
| **返回值类型**          | 需显式标注 `: JSX.Element`            | 自动推导，无需额外标注                  |
| **静态属性扩展**        | 直接挂载（`GreetingCard.static = ...`） | 需先赋值给变量，再挂载（多一行代码）    |
| **useCallback 兼容**    | 完全兼容，直接包裹 function 函数      | 兼容，包裹箭头函数（写法更统一）        |
| **学习成本**            | 低，只需掌握接口和函数类型标注        | 中，需额外理解 React.FC 泛型概念        |

### 选择建议：
1. **优先选写法一**：如果你是初学者，觉得 `function` 更易理解，或组件很少用 `children`，选这种——直观、少踩坑，核心功能完全满足；
2. **选写法二的场景**：如果团队用函数式编程风格（箭头函数为主），或组件经常需要 `children` 属性，选这种——能省掉 `children` 定义，写法更简洁；
3. **核心原则**：两种写法在功能上无优劣，**优先选择你能看懂、写起来顺手的**，项目内保持风格一致即可。


## 五、初学者避坑指南
1. **不要混用两种写法**：比如 `function GreetingCard: React.FC<Props> () {}` 是错误的，React.FC 只能标注变量（箭头函数/函数表达式），不能标注 function 声明；
2. **Props 接口不要省略**：即使组件只有一个 props，也建议定义单独接口（如 `interface ButtonProps { text: string }`），比直接写 `(props: { text: string })` 更易维护；
3. **可选属性必加默认值**：可选属性（带 `?`）建议设置默认值（如 `message = '默认文案'`），避免组件内频繁判断 `undefined`；
4. **export 写法推荐**：无论哪种写法，都建议“先定义组件，后导出”（分开写），避免一行代码过长，后续扩展更方便。


## 总结
React+TypeScript 组件类型注解的核心是“明确 props 类型”，两种写法都能实现 IDE 提示、类型校验和可读性提升：
- 初学者优先选 **Function 关键词 + 单独 Props 接口**，直观易理解，学习成本低；
- 熟悉泛型后可尝试 **Const 箭头函数 + React.FC**，适合函数式风格和频繁使用 `children` 的场景。

不用纠结“哪种更好”，掌握一种写法并熟练运用，再根据项目需求或团队规范切换即可——核心是让 TypeScript 帮你减少 bug，让代码更清晰。

如果需要进一步学习，建议先熟练掌握接口定义和函数类型标注，再深入理解 React.FC 等 React 内置类型工具，逐步提升 TypeScript 运用能力～


**解构赋值已经在当前作用域 “声明” 了变量（比如`bookmarks`），后续再用`const/let/var`声明同名变量，就违反了 “同一作用域不能重复声明” 的规则**

### 解决办法
#### 1. 解构时直接 “重命名”（推荐）
直接在解构时给 props 里的变量起个 “别名”，避免冲突 —— 这是 JS 解构语法支持的原生功能
```javascript
function BookmarkList({ bookmarks: propsBookmarks }: Props) 
```
#### 2. 不重复命名
换个语义化的变量名，props 里的`bookmarks`是 “原始数据”，后续处理后的可以叫`filteredBookmarks`（过滤后）、`sortedBookmarks`（排序后），既避免冲突，代码可读性也更高。
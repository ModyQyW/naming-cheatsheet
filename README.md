<p align="center">
  <a href="https://github.com/kettanaito/naming-cheatsheet">
    <img src="./naming-cheatsheet.png" alt="Naming cheatsheet" />
  </a>
</p>

# 命名指南

- [命名指南](#命名指南)
  - [使用英语](#使用英语)
  - [命名约定](#命名约定)
  - [S-I-D](#s-i-d)
  - [避免缩写](#避免缩写)
  - [避免重复上下文](#避免重复上下文)
  - [反映预期结果](#反映预期结果)
  - [命名函数](#命名函数)
    - [A/HC/LC 模式](#ahclc-模式)
      - [动作 Actions](#动作-actions)
        - [`get`](#get)
        - [`set`](#set)
        - [`reset`](#reset)
        - [`remove`](#remove)
        - [`delete`](#delete)
        - [`compose`](#compose)
        - [`handle`](#handle)
      - [上下文 Context](#上下文-context)
      - [前缀 Prefixes](#前缀-prefixes)
        - [`is`](#is)
        - [`has`](#has)
        - [`should`](#should)
        - [`min`/`max`](#minmax)
        - [`prev`/`next`](#prevnext)
  - [单数和复数](#单数和复数)

---

> [!NOTE]
>
> 译者注：
>
> 👉 [点击查看英文原文](https://github.com/kettanaito/naming-cheatsheet)
>
> 2024-12-29 调整部分翻译；2023-08-24 第一次翻译。
>
> 推荐使用 AI 工具和 [codelf](https://unbug.github.io/codelf/) 辅助命名。
>
> 以下为翻译正文。

命名是一件难事。这个指南试图让它变得更容易。

这些建议适用于任何编程语言，我将使用 JavaScript 来实际演示它们。

## 使用英语

在为变量和函数命名时，请使用英语。

```js
/* 不太好 */
const primerNombre = "Gustavo";
const amigos = ["Kate", "John"];

/* 不错 */
const firstName = "Gustavo";
const friends = ["Kate", "John"];
```

> 不管你喜不喜欢，英语都是编程中的主导语言：所有编程语言的语法都使用英语编写，还有无数的文档和教材也是这样。用英语编写代码可以大大提高代码一致性。

## 命名约定

选择 **一个** 命名约定并遵循它。可以是 `camelCase`、`PascalCase`、`snake_case` 或其它命名约定，只要保持一致就好。很多编程语言都有自己的命名约定传统，你可以查阅编程语言的文档或 GitHub 上受欢迎的代码库！

```js
/* 不太好 */
const page_count = 5;
const shouldUpdate = true;

/* 不错 */
const pageCount = 5;
const shouldUpdate = true;

/* 也不错 */
const page_count = 5;
const should_update = true;
```

## S-I-D

名称必须 _简短 short_、_直观 intuitive_ 并 _带有描述性 descriptive_：

- **简短 Short**。名称不应该过长，以便于输入和记忆；
- **直观 Intuitive**。名称应该读起来自然，尽可能接近常用语言；
- **带有描述性 Descriptive**。名称必须以最有效的方式反映其功能和属性。

```js
/* 不太好 */
const a = 5; // “a” 可以表示任何东西
const isPaginatable = a > 10; // "Paginatable" 听起来非常不自然
const shouldPaginatize = a > 10; // 自造动词，有点意思！

/* 不错 */
const postCount = 5;
const hasPagination = postCount > 10;
const shouldPaginate = postCount > 10; // 另一种做法
```

## 避免缩写

**不要** 使用缩写。它们只会降低代码的可读性。寻找一个简短并带有描述性的名称可能很困难，但缩写并不是不这样做的借口。

```js
/* 不太好 */
const onItmClk = () => {};

/* 不错 */
const onItemClick = () => {};
```

## 避免重复上下文

名称不应该重复其定义上下文。如果去除上下文不会降低名称可读性，那就应该从名称中去除上下文。

```js
class MenuItem {
  /* 方法名重复了上下文 "MenuItem" */
  handleMenuItemClick = (event) => { ... }

  /* `MenuItem.handleClick()` 不错 */
  handleClick = (event) => { ... }
}
```

## 反映预期结果

名称应该反映预期结果。

```jsx
/* 不太好 */
const isEnabled = itemCount > 3;
return <Button disabled={!isEnabled} />;

/* 不错 */
const isDisabled = itemCount <= 3;
return <Button disabled={isDisabled} />;
```

---

## 命名函数

### A/HC/LC 模式

命名函数时，有一个有用的模式可供遵循：

```text
prefix? + action (A) + high context (HC) + low context? (LC)

前缀? + 动作 (A) + 高层级上下文 (HC) + 低层级上下文? (LC)
```

请看下面的表格，了解如何应用这个模式。

| Name 名称              | Prefix 前缀 | Action 动作 (A) | High context 高层级上下文 (HC) | Low context 低层级上下文 (LC) |
| ---------------------- | ----------- | --------------- | ------------------------------ | ----------------------------- |
| `getUser`              |             | `get`           | `User`                         |                               |
| `getUserMessages`      |             | `get`           | `User`                         | `Messages`                    |
| `handleClickOutside`   |             | `handle`        | `Click`                        | `Outside`                     |
| `shouldDisplayMessage` | `should`    | `Display`       | `Message`                      |                               |

> **注意**：上下文的顺序会影响变量的含义。例如，`shouldUpdateComponent` 表示 _你_ 将要更新一个组件，而 `shouldComponentUpdate` 告诉你 _组件_ 将会自行更新，而你只是控制它 _何时_ 更新。换句话说，**高上下文强调了变量的含义**。

---

#### 动作 Actions

函数名中的动词部分就是动作。这是最重要的部分，它负责描述函数的 _功能_。

##### `get`

立即访问数据（即内部数据的简写 getter）。

```js
function getFruitCount() {
  return this.fruits.length;
}
```

> 另请参阅 [compose](#compose)。

在执行异步操作时，也可以使用 `get`。

```js
async function getUser(id) {
  const user = await fetch(`/api/user/${id}`);
  return user;
}
```

##### `set`

以声明性的方式将变量从值 A 设置为值 B。

```js
let fruits = 0;

function setFruits(nextFruits) {
  fruits = nextFruits;
}

setFruits(5);
console.log(fruits); // 5
```

##### `reset`

将变量恢复到其初始值或状态。

```js
const initialFruits = 5;
let fruits = initialFruits;
setFruits(10);
console.log(fruits); // 10

function resetFruits() {
  fruits = initialFruits;
}

resetFruits();
console.log(fruits); // 5
```

##### `remove`

_从某个地方中_ 移除某个东西。

例如，在搜索页面上有一个已选择的过滤器集合，从集合中移除其中一个过滤器应该使用 `removeFilter`，而 **不是** `deleteFilter`（这也是在英语中表达的方式）。

```js
function removeFilter(filterName, filters) {
  return filters.filter((name) => name !== filterName);
}

const selectedFilters = ["price", "availability", "size"];
removeFilter("price", selectedFilters);
```

> 另请参阅 [delete](#delete)。

##### `delete`

完全从存在的领域中抹去某个东西。

想象一下，你是一位内容编辑者，你想要彻底删掉一个帖子。一旦你点击了“删除帖子”按钮，内容管理系统会执行 `deletePost` 操作，而 **不是** `removePost`。

```js
function deletePost(id) {
  return database.find({ id }).delete();
}
```

> 另请参阅 [remove](#remove)。

> **使用 `remove` 还是 `delete`？**
>
> 如果你很难区分 `remove` 和 `delete`，我建议你看一下它们的相反动作 —— `add` 和 `create`。`add` 和 `create` 之间的关键区别在于 `add` 需要一个目的地，而 `create` **不需要目的地**。你可以 `add` 某项 _到某个地方_，但你不会将 `create` 某项 _到某个地方_。类比理解 `remove` 和 `add`、`delete` 和 `create`。
>
> 更详细的解释请参阅 [这里](https://github.com/kettanaito/naming-cheatsheet/issues/74#issue-1174942962)。

##### `compose`

基于已有数据创建新的数据。主要适用于字符串、对象或函数。

```js
function composePageUrl(pageName, pageId) {
  return pageName.toLowerCase() + "-" + pageId;
}
```

> 另请参阅 [get](#get)。

##### `handle`

处理一个动作。通常用于回调方法命名。

```js
function handleLinkClick() {
  console.log("Clicked a link!");
}

link.addEventListener("click", handleLinkClick);
```

---

#### 上下文 Context

函数操作的域。

函数通常是对 _某些东西_ 的操作。重要的是要说明它的可操作域是什么，或者至少要说明预期的数据类型。

```js
/* 操作 primitives 的纯函数 */
function filter(list, predicate) {
  return list.filter(predicate);
}

/* 操作 posts 的函数 */
function getRecentPosts(posts) {
  return filter(posts, (post) => post.date === Date.now());
}
```

> 某些特定语言的假设可能允许省略上下文。例如，在 JavaScript 中，`filter` 通常对 Array 执行操作，因此没有必要添加明确的 `filterArray`。

---

#### 前缀 Prefixes

前缀增强了变量的含义。在函数名中很少使用。

##### `is`

描述当前上下文的特征或状态（通常为 `boolean`）。

```js
const color = "blue";
const isBlue = color === "blue"; // characteristic
const isPresent = true; // state

if (isBlue && isPresent) {
  console.log("Blue is present!");
}
```

> [!NOTE]
>
> 译者注：
>
> 不使用 `is` 前缀，而直接使用形容词来表示特征或状态，也很常见，如 `disabled`、`visible` 等。请注意统一。

##### `has`

描述当前上下文是否具有某个值或状态（通常为 `boolean`）。

```js
/* 不太好 */
const isProductsExist = productsCount > 0;
const areProductsPresent = productsCount > 0;

/* 不错 */
const hasProducts = productsCount > 0;
```

##### `should`

反映与某个动作相关的肯定条件语句（通常为 `boolean`）。

```js
function shouldUpdateUrl(url, expectedUrl) {
  return url !== expectedUrl;
}
```

##### `min`/`max`

代表最小值或最大值。用于描述范围或限制。

```js
/**
 * 在给定的最小最大范围内随机渲染一定数量的帖子
 */
function renderPosts(posts, minPosts, maxPosts) {
  return posts.slice(0, randomBetween(minPosts, maxPosts));
}
```

##### `prev`/`next`

表示变量在当前上下文中的上一个或下一个状态。用于描述状态转换。

```jsx
async function getPosts() {
  const prevPosts = this.state.posts;

  const latestPosts = await fetch("...");
  const nextPosts = concat(prevPosts, latestPosts);

  this.setState({ posts: nextPosts });
}
```

## 单数和复数

和前缀一样，变量名也可以以单数或复数形式出现，这取决于它们包含一个值还是多个值。

```js
/* 不太好 */
const friends = "Bob";
const friend = ["Bob", "Tony", "Tanya"];

/* 不错 */
const friend = "Bob";
const friends = ["Bob", "Tony", "Tanya"];
```

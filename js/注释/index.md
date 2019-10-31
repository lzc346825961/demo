[![notes]](../index.md)

[notes]:https://img.shields.io/badge/注释-notes-4FC08D?style=social&labelColor=339966&logo=Vue.js&logoColor=339966

好的注释可以提高代码的可读性和可维护性，从而提高代码质量。

## 注释的目的和原则

注释的目的：

* **提高代码的可读性，从而提高代码的可维护性**

注释的原则：

* **如无必要，勿增注释 ( As short as possible )**

* **如有必要，尽量详尽 ( As long as necessary )**

***
我们写注释，是为了给代码的读者（包括我们自己，也可能包括机器，如 jsdoc）看，帮助读者阅读理解代码并进行维护。

**「如无必要，勿增注释」** 是指注释要避免过多过滥，不要为了注释而注释。多余的注释等价于冗余的代码，除了对增加可读性无益，一旦代码需要修改，修改注释也会是一大负担。

我们应当追求「代码自注释」，即代码本身就拥有较高的可读性（通过清晰的命名、合理的结构等）。举个例子：

```javascript
// bad
// 如果已经准备好数据，就渲染表格
if (data.success && data.result.length > 0) {
  renderTable(data);  
}

// good
const isTableDataReady = data.success && data.result.length > 0;
if (isTableDataReady) {
  renderTable(data);
}
```

**「如有必要，尽量详尽」** 是指需要注释的地方应该尽量详尽地去写，以让阅读者可以充分了解代码的逻辑和意图为标准。

## 什么是好注释，什么是坏注释

根据注释的原则，我们应该以「能否帮助阅读者更好地阅读理解代码」为标准，判断一个注释「是否有必要」。

***
好的注释包括：

* 帮助读者更好地了解代码逻辑和结构，例如：

```javascript
init: function() {
  // 获取配置信息
  const config = getConfig();
  
  // 获取用户信息
  const userInfo = getUserInfo();
  
  // 根据配置和用户信息，进行初始化
  doInit(config, userInfo);
  
  // 如果存在自定义配置时的特殊逻辑
  if (config.custom) {
    ...
  }
}
```

* 特殊或不易理解写法的解释说明，例如：

```javascript
/** 
 * parseInt was the reason my code was slow. 
 * Bitshifting the String to coerce it to a 
 * Number made it a lot faster. 
 */
const val = inputValue >> 0;
```

* 特殊标记注释：如 TODO、FIXME 等有特殊含义的标记
* 文件注释：部分规约会约定在文件头部书写固定格式的注释，如注明作者、协议等信息
* 文档类注释：部分规约会约定 API、类、函数等使用文档类注释（如 jsdoc 风格）
* 遵循统一的风格规范，如一定的空格、空行，以保证注释自身的可读性
* 
***
坏的注释包括：

* 注释对阅读代码无益：如本文第一个示例，本可以不用注释，用清晰的命名、逻辑进行代码自注释

* 未遵循统一的风格规范：如单行注释长度、空格、空行，例如：

```javascirpt
// bad 单行注释过长，不易阅读，应写成多行
// parseInt was the reason my code was slow.Bitshifting the String to coerce it to Number made it a lot faster.
const val = inputValue >> 0;

// good
/** 
 * parseInt was the reason my code was slow. 
 * Bitshifting the String to coerce it to a 
 * Number made it a lot faster. 
 */
const val = inputValue >> 0;
```

## 如何写好注释

### 注释规约
理解注释的目的和原则，制定并遵循一份注释规约，以保证注释的可读性和一致性

### 工具保障
比如使用 ESLint 保证注释风格的一致，使用 Sonar 检查项目注释率

## 注释规约
***
这里给出一份可供参考的注释规约（参考自airbnb规约）：

* 【推荐】单行注释使用 //

注释应单独一行写在被注释对象的上方，不要追加在某条语句的后面：

```javascript
// bad
const active = true; // is current tab

// good
// is current tab
const active = true;
```

注释行的上方需要有一个空行（除非注释行上方是一个块的顶部），以增加可读性：
```javascript
// bad
function getType() {  
  console.log('fetching type...');  
  // set the default type to 'no type'
  const type = this.type || 'no type';  
  return type;
}
  
// good
function getType() {  
  console.log('fetching type...');  
  
  // set the default type to 'no type'
  const type = this.type || 'no type';  
  return type;
}

// good
// 注释行上面是一个块的顶部时不需要空行
function getType() {  
  // set the default type to 'no type'
  const type = this.type || 'no type';					
  return type;
}
```

* 【推荐】多行注释使用 /** ... */，而不是多行的 //

```javascript
// bad
// make() returns a new element
// based on the passed in tag name
function make(tag) {
  // ...

  return element;
}

// good
/**
 * make() returns a new element
 * based on the passed-in tag name
 */
function make(tag) {
  // ...

  return element;
}
```

* 【强制】注释内容和注释符之间需要有一个空格，以增加可读性。eslint: spaced-comment

```javascript
// bad
//is current tab
const active = true;

// good
// is current tab
const active = true;

// bad
/**
 *make() returns a new element
 *based on the passed-in tag name
 */
function make(tag) {  
  // ...

  return element;
}

// good
/**
 * make() returns a new element
 * based on the passed-in tag name
 */
function make(tag) {  
  // ...

  return element;
}
```

* 【推荐】使用特殊注释标记

有时我们发现某个可能的 bug，但因为一些原因还没法修复；或者某个地方还有一些待完成的功能，这时我们需要使用相应的特殊标记注释来告知未来的自己或合作者。常用的特殊标记有两种：
1. // FIXME: 说明问题是什么
2. // TODO: 说明还要做什么或者问题的解决方案

```javascript
class Calculator extends Abacus {
  constructor() {
	super();

	// FIXME: shouldn’t use a global here
	total = 0;

	// TODO: total should be configurable by an options param
	this.total = 0;
  }
}
```

* 【推荐】文档类注释，如函数、类、文件、事件等，使用 jsdoc 规范

例如：

```javascript
/**
 * Book类，代表一个书本.
 * @constructor
 * @param {string} title - 书本的标题.
 * @param {string} author - 书本的作者.
 */
function Book(title, author) {
  this.title=title;
  this.author=author;
}

Book.prototype={
  /**
   * 获取书本的标题
   * @returns {string|*}
   */
  getTitle:function(){
	return this.title;
  },

  /**
   * 设置书本的页数
   * @param pageNum {number} 页数
   */
  setPageNum:function(pageNum){
	this.pageNum=pageNum;
  }
};
```

## 工具

我们可以使用一些工具来保证注释质量，例如：

### Eslint：保证一致的注释风格

ESLint 是当下最流行的 JS 代码检查工具，ESLint 中有一些注释相关的规则，用户可选择开启：

* valid-jsdoc
* require-jsdoc
* no-warning-comments
* capitalized-comments
* line-comment-position
* lines-around-comment
* multiline-comment-style
* no-inline-comments
* spaced-comment

### Sonar：检查项目的注释率

Sonar 是一个代码持续集成平台，它可以对代码进行静态扫描，得到项目的注释率数据。

注释率反应了注释行占总代码行的比例，注释率太低不好，但也不能盲目追求高注释率。

另外，同 Eslint 类似，Sonar 也有一些针对注释风格规则可以配置。

## 后记

理解注释的目的和原则，遵循一份注释规约并结合工具保证落地，可以使注释成为代码良好的辅助，增强可读性和可维护性，从而提高代码质量。

本篇是《前端代码质量系列文章》的第一篇，后续系列文章还将讨论编码规约、复杂度、重复率等主题，敬请期待：）

本文部分内容和代码示例参考：
* aralejs注释规范 https://github.com/aralejs/aralejs.github.io/wiki/JavaScript-注释规范
* airbnb编码规约 https://github.com/airbnb/javascript#comments
* http://zh-google-styleguide.readthedocs.io/en/latest/google-cpp-styleguide/comments/

部分配图来自网络，如有侵删，请作者联系
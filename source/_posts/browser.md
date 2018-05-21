---
title: 浏览器内核
date: 2018-05-21 17:15:26
tags:
---

# 浏览器内核
> 浏览器内核由渲染引擎和JS引擎组成，不同的浏览器、即使同一浏览器不同型号可能渲染引擎和JS引擎都不一样。

## 渲染引擎
1. Trident渲染引擎 –> 老版本IE系列浏览器
2. Edge渲染引擎 -> Win10中IE浏览器
3. Gecko渲染引擎 –> Mozilla Firefox
4. Presto渲染引擎 –> Opera
5. KHTML渲染引擎 –> 早期的Safafi和Google Chrome
6. Webkit渲染引擎 -> 2001年后的Safari和Chrome以及国内的一些浏览器
7. Blink渲染引擎 -> 新版本的Chromium浏览器Google项目

## JavaScript引擎
1. JScript引擎 –> IE系列浏览器
2. spiderMonkey引擎 –> Mozilla Firefox
3. V8引擎 –> Google Chrome
4. linear b/futhark引擎 –> Opera


## 浏览器渲染引擎发展历史
![浏览器渲染引擎发展历史](/public/images/browserHistory.png)


## 浏览器的渲染引擎及依赖模块分析
![浏览器的渲染引擎及依赖模块分析](/public/images/renderFunction.png)


## 输入URL到页面展示简图
![输入URL到页面展示简图](/public/images/urlToDom.png)

### 浏览器内核各部分解释
> HTML解释器：解释HTML文本的解释器，主要作用是将HTML文本解释成DOM树，DOM是一种文档表示方法。
> 
> CSS解释器：级联样式表的解释器，它的作用是为DOM中的各个元素对象计算出样式信息，从而为计算最后网页的布局提供基础设施。
> 
> 布局：在DOM创建之后，webkit需要将其中的元素对象同样式信息结合起来，计算它们的大小> 位置等布局信息，形成一个能够表示这所有信息的内部比偶表示模型。
> 
> JavaScript引擎：使用JavaScript代码可以修改网页的内容，也能修改CSS的信息，JavaScript引擎能过解释JavaScript代码并通过DOM接口和CSSOM接口来修改网页内容和样式信息，从而改变渲染结果。
> 
> 绘图：使用图形库将布局计算后的各个网页的节点绘制成图像结果。

### HTML解释器

![HTML解释器](/public/images/htmlExplain.png)

> 首先是字节流，经过解码之后是字符流，然后通过词法分析器会被解释成词语（Tokens），时候经过分析器构建成节点，最后这些节点被组建成一棵DOM树。
> 
> 词法分析：HTMLTokenizer 类（作用是词法分析，类似于状态机），输入的是字符串，输出的是一个个的词语。
> 
> XSSAuditor验证词语：XSSAuditor （验证词语流Token Stream）XSS指的是Cross Site Security，主要是针对安全方面的考虑。
> 
> 词语到节点：webkit用来构建DOM节点，这一步骤由HTMLDocumentParser 类调用 HTMLTreeBuilder 类的 constructTree的函数来实现。
> 
> 节点到DOM树：树中的元素节点创建属性节点等工作由HTMLConstructionSite类来完成，该类中包含一个 HTMLElementStack 作为保存元素节点的栈。
> 
> JavaScript的执行：webkit将DOM树创建过程中需要执行得我Javascript代码交由HTMLScriptRunner类来负责。
> 
> DOM的事件机制：webkit中用EventTarget类来表示DOM规范中Events部分定义的事件目标，Node 节点继承自 EventTarget类，所以Node拥有EventTarget类的相关的方法。

### CSS解释器和样式布局

> css解释器和规则匹配处于DOM树建立之后，RenderObject树建立之前，css解释器解释后的结果会保存起来，然后RenderObject树基于该结果来进行规范匹配和布局计算。

![CSS解释器和样式布局](/public/images/cssToDom.png)


### CSSOM（CSS Object Model）

> CSSOM视图模块(CSSOM View Module)定义了一些 API，Web 开发人员使用这些 API 可以进行检查，也可以以编程方式更改文档及其内容的视觉属性，包括布局框定位、视区宽度和元素滚动。
> 
> document.styleSheets 可以查看当前页面的StyleSheetList对象，每个link、style都会产生 CSSStyleSheet 作为 StyleSheetList对象的value。

### CSS解释器和规则匹配（本区块内容会使我们对css选择器权重有更好的理解）

> DocumentStyleSheetCollection类（属于Document类），该类包含了所有CSS样式表，还包括了webkit的内部表示类CSSStyleSheet，它包含了CSS的href、类型、内容等信息。
> 
> CSS解释过程：css字符串经过css解释器处理后变成渲染引擎的内部规则的过程，使用CSSParser类来负责该过程。
> 
> 在解释网页中自定义的CSS样式之前，实际上webkit渲染引擎会为每个网页设置一个默认样式，这也是我们为什么要重置浏览器样式的根本原因。
> 
> 规则匹配：StyleResolver类为DOM的元素节点匹配样式，StyleResolver类根据元素的信息，例如标签名、类别等，从样式规则中查找最匹配的规则，然后将样式信息保存到新建的RenderStyle对象中。最后，这些RenderStyle对象被RenderObject类所管理和使用。
> 
> 其中，规则的匹配则是由ElementRuleCollector类来计算并获得，它根据元素的属性等信息，并从DocumentRuleSets类中获取规则集合，依次按照ID、CLASS、标签等选择器信息逐次匹配获得元素的样式。
> 
> 然后webkit对这些规则进行排序，对于该元素需要的样式属性，webkit选择从高优先级规则中选取，并将样式属性值返回。
> 
> 这里，我引入一个不太相关的知识点，块格式化上下文（Block Formatting Context，BFC） 是Web页面的可视化CSS渲染的部分，是块级盒布局发生的区域，也是浮动元素与其他元素交互的区域。

### webkit布局
> 当webkit创建RenderObject对象之后，每个对象是不知道自己的位置、大小等信息的，webkit根据盒模型来计算他们的位置、大小等信息的过程称为布局计算/排版。
>
> 布局计算分类：第一类是对整个RenderObject树进行的计算；第二类是对RenderObject树中某个子树的计算，常见于文本元素或者overflow：auto块的计算。
> 布局计算：布局计算是一个递归的过程，这是因为一个节点的大小通常需要先计算它的子节点的位置、大小等信息。
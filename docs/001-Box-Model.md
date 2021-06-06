# 盒子模型
>原文地址：https://web.dev/learn/css/box-model/
>
>CSS 显示的每一个内容都是一个盒子。理解盒子模型是如何工作，是 CSS 的核心基础。

假设你有这么一段 HTML

```html
<p>I am a paragraph of text that has a few words in it.</p>
```

然后为它写个CSS

```css
p {
  width: 100px;
  height: 50px;
  padding: 20px;
  border: 1px solid;
}
```

内容会超出元素边界。而且它宽度其实是140px，而不是100px。为什么会这样？盒子模型是CSS核心基础，理解它是如何工作的，它是如何受到CSS其他方面影响的。重要的是，如何掌握它才能我们写出更可预测的CSS代码。

<my-iframe src="https://codepen.io/web-dot-dev/embed/WNRemxN"/>

在写CSS或在写整个web时，有一个非常重要的事情，那就是CSS显示的所有内容都是一个个盒子。即使盒子使用了`border-radius`圆角属性让它看起来像个圆，或者放一些文本。唯一要记住就是：“它们都是盒子”。

## 内容和大小

::: tip 译者注
- **extrinsic sizing** 外部大小：即根据元素的上下文确定大小，而不考虑其内容。(比如直接给它设置一个`width`)
- **intrinsic sizing** 内在大小：即根据元素内容来确定大小，而不考虑其上下文。(比如 `min-content` 和 `max-content`)

参考：
- [https://drafts.csswg.org/css-sizing-3/#extrinsic](https://drafts.csswg.org/css-sizing-3/#extrinsic)
- **Extrinsic sizing** determines sizes based on the context of an element, without regard for its contents.
- [https://drafts.csswg.org/css-sizing-3/#intrinsic](https://drafts.csswg.org/css-sizing-3/#intrinsic)
- **Intrinsic sizing** determines sizes based on the contents of an element, without regard for its context.
::: 

盒子会根据不同的`display` 值会有不同的特性，设置尺寸和内容都会影响到它们。内容可以是更多的盒子（子元素）或纯文本内容。无论哪种方式，默认情况下，内容都会影响盒子的尺寸。

你可以使用**外部大小**来控制盒子大小，或者也可以让浏览器使用**内在大小**来决定。

为了快速区别它们的不同，让我们来看个例子：

<my-iframe src="https://codepen.io/web-dot-dev/embed/abpoMBL"/>

该 Demo 里面有一个固定尺寸和粗描边的盒子，盒子里面写着 "CSS is awesome"。盒子设置了宽度，所以是它是**外部大小**，限制其内容的大小。但问题是这个 "awesome" 对这个盒子来说太大了，所以它溢出了父亲的边框（稍后会详细介绍）。为了防止这种溢出的一种方式就是不要给盒子设置宽度，或在这个例子中，设置`width`的值为`min-content`。

::: tip 译者注 
原英文的解释不好理解，所以使用了zhangxinxu的原句:  
这个`min-content` 宽度表示的并不是内部哪个宽度小就是哪个宽度，而是，采用内部元素最小宽度值最大的那个元素的宽度作为最终容器的宽度。
::: 

这就让盒子恰当的环绕 "CSS is awesome"，完美！

让我们看一些更复杂的例子，看看不同大小对实际内容的影响：

<my-iframe src="https://codepen.io/web-dot-dev/embed/wvgwOJV"/>

[这段太难翻译：备注下，后面再回过头看看]
通过切换内部大小开关，来看看使用外部大小你可以控制哪些，或由内容使用内部大小来控制尺寸。为了查看这两个不同尺寸类型的效果，我们在卡片下面添加了一些文字内容。当元素使用外部大小时，那么对于添加更多的元素内容，其实是有个约束让它不会溢出元素的边界。但当使用内部大小时，情况就不是这样的了。

默认情况下，元素设置`width`和`height`都为`400px`。这两尺寸为元素内的所有内容提供了严格的界限，除非内容对于盒子来说太大，以至于导致溢出。你可以在那有朵花的图片下面添加更多的文字，来看看溢出效果。

>关键术语：当内容对盒子来说太大了，我们称之为`overflow`（即溢出）。你可以使用`overflow`属性，来管理元素如何处理溢出内容。

当你切换到内部大小时，相当于你让浏览器依据盒子内容大小为你做出决定。这时内容想要溢出是非常困难的，因为我们的盒子会随着内容动态调整自己尺寸，而不是去调整内容尺寸。最重要的一点就是这是浏览器默认的，弹性的。尽管外部大小可以更好的控制外表，但在大多数情况下，内部大小提供了更大的灵活性。

## 盒子模型的区域

盒子是由多个不同的盒子模型区域组成，它们都有特定的作用。

![The areas of the box model](https://web-dev.imgix.net/image/VbAJIREinuYvovrBzzvEyZOpw5w1/ECuEOJEGnudhXW5JEFih.svg)

盒子模型4个主要区域：内容盒子、内边距盒子、边框盒子 和 外边距盒子。

我们从**内容盒子**开始，它表示的是内容区域。正如你之前了解的：内容可以控制其父级大小，因此它通常是经常变化的。

**内边距盒子**（也可认为填充盒子）环绕内容盒子，它的空间由`padding`属性值决定。因为是填充在盒子里面，盒子的背景图是能在这个区域看到的。如果我们的盒子设置了溢出规则，例如`overflow: auto`或`overflow: scroll`，那么滚动条也是会占用这个空间的。

<my-iframe src="https://codepen.io/web-dot-dev/embed/bGNmgGW"/>

**边框盒子**环绕着内边距盒子，它的空间由`border`属性值决定。边框盒子是你能直观看到盒子范围的最大边缘。`border`属性通被用来给元素画个边框。

最后一个**外边距盒子**，是环绕着盒子的外空间，由`margin`属性值决定。像`outline`和`box-shadow`属性也占据这个空间，因为它们是画在上层的，所以它们不会影响我们盒子的尺寸。你可以在盒子上每边设置`outline-width`为`200px`，但盒子内的所有内容尺寸和边框尺寸等都不会受到影响。

<my-iframe src="https://codepen.io/web-dot-dev/embed/XWprGea"/>

## 一个实用的例子

盒子模型可能比较难理解，所以让我们在这里使用一个例子来回顾一下我们学到的知识。

![画廊](https://web-dev.imgix.net/image/VbAJIREinuYvovrBzzvEyZOpw5w1/FBaaJXdnuSkvOx1nB0CB.jpg?auto=format&w=800)

在这幅图中，你有三个相框并排安在墙上，并在图上写了一些和盒子模型相关的标签。

让我们详细分析一下这个例子：

- 艺术品本身可以看成内容盒子
- 位于边框和和艺术品之间的白色磨砂区域可以看成内边距盒子
- 这件艺术品边框就是边框盒子了
- 一件艺术品边框与另外一件艺术品边框之间的间距，可以认为是外边距盒子
- 艺术品框架的光照投影（阴影），也属于外边距空间

## 盒子模型调试

浏览器开发者工具（DevTools）提供了所选盒子模型的可视化，这可以帮助你理解盒子模型的工作原理，更重要的是，它是如何影响你正在写的网页。

自己在浏览器试一试吧。

1. [打开开发者工具](https://developers.google.com/web/tools/chrome-devtools/open)
2. [选择一个元素](https://developer.chrome.com/docs/devtools/css/reference/#select)
3. 显示盒子模型调速器（在`Styles` tab 底部，或在 `Computed` tab 下面）

## 掌控盒子模型

要想了解如何掌控盒子模型，那你得先知道浏览器做了什么。

::: tip 译者注 
`user agent stylesheet` 是指浏览器默认样式表，当用户没有写CSS时，浏览器会按照内置的样式表来渲染。但各种浏览器差异很大，所以为什么需要类似`Normalize.css`的项目
:::

每个浏览器都给HTML文档提供了默认样式表，虽然每个浏览器默认样式差异很大，但至少它们提供了合理的默认值以使得内容更易阅读。它们定义了元素的外观和行为，当页面没有使用CSS时。在默认样式中，每个格子的`display`也都设置了默认值。例如，如果我们在正常文档流，`div`元素`display`默认值是`block`，`li`元素`display`默认值是`list-item`，而`span`元素`display`默认值是`inline`。

给一个行内元素（`inline`）设置外边距，但其他元素是不会受它影响的。如果把它`display`设置为`line-block`就可以让其他元素受到它外边距的影响，而且这个元素还可继续保持内联元素的一些行为。一个块级元素（`block`）默认情况是填充整个它所在的横向内在空间。而`inline`和`inline-block`元素最大也只会和它内容一样大。

::: tip 译者注 
上面一段的翻译可能有问题，也可能是原文有问题。行内元素比如`span`（不可替换元素）的`margin-top`，`margin-bottom` 即使设置了，也不会对周围的元素产生影响。但 `margin-left` 和 `margin-right` 是会对周围元素产生影响的。如果行内元素是`img`（可替换元素），那么 `margin` 4个值都会对周围元素有影响。`padding`同理。
:::

除了理解浏览器默认样式是如何影响每一个盒子，还需要了解 `box-sizing` 属性。它告诉浏览器怎么计算盒子模型尺寸。默认情况，所有元素的默认样式是：`box-sizing:content-box;`

当元素设置了`box-sizing:content-box`，意味着当你给它设置尺寸时，比如`width`或`height`，它们就是内容盒子的尺寸。当你再设置 `padding` 和 `border`，它们将会作为额外的值添加到内容盒子的尺寸上。

::: tip 译者注
这里我举两个例子：
- `divA` 设置 `box-sizing: content-box`，然后 `width` 设置 `100px`，那么它的内容宽度就是 `100px`，然后你再设置 `padding: 20px; border: 10px solid`，那么整个 `divA` 的占据的宽度空间将是`100+20+20+10+10 = 160px`。
- `divB` 设置 `box-sizing: border-box`，然后 `width` 设置 `100px`，然后你再设置 `padding: 20px; border: 10px solid`，那么整个 `divB` 的占据的宽度空间还是 `100px`，但其内容盒子宽度是：`100-20-20-10-10 =4 0px`。
所以目前主流的设置盒子模型 `box-sizing` 为 `border-box`。

这个好处是，在写页面时心智压力上会小一些，也更容易布局。文章下面也有讲。
:::

```css
/*小测试*/
/*你认为 .my-box 的实际宽度应该是 200px 还是 260px */
.my-box {
  width: 200px;
  border: 10px solid;
  padding: 20px;
}
```

这个盒子的实际宽度将是260px。由于CSS默认设置`box-sizing: content-box`，所以它的实际宽度是由内容宽度、`padding`两边尺寸和`border`两边尺寸之和算出来的。所以就是200px的内容宽度 + 40px的`padding`值 + 20px的`border`值，等于260px。 

不过，你可以用 `border-box` 来修改盒子模型：

```css
.my-box {
  box-sizing: border-box;
  width: 200px;
  border: 10px solid;
  padding: 20px;
}
```

这 `border-box` 盒子模型，就是告诉 CSS 将实际的宽度应用到边框盒子上，而不是内容盒子。这意味着我们的`border`和`padding`将会被`推入`。当设置 `.my-box`为`200px`宽度时，那它实际上就是`200px`宽度。

我们通过下面的 demo 来看看其工作原理。请注意，当你切换`box-sizing`值时，它将会通过蓝色背景来显示哪些CSS被应用在盒子内。

<my-iframe src="https://codepen.io/web-dot-dev/embed/oNBvVpM"/>

```css
*,
*::before,
*::after {
  box-sizing: border-box;
}
```

上面一小段 CSS 代码，是让文档内的所有的元素，包括`::before` 和 `::after` 等伪类元素都应用 `box-sizing: border-box`。这意味着每个元素都将使用这个新的盒子模型。

因为 `border-box` 盒子模型更可预测，开发人员经常会添加此规则到重置样式表中，[比如这个](https://piccalil.li/blog/a-modern-css-reset)。

## 相关资源

- [Introduction to the box model](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Box_Model/Introduction_to_the_CSS_box_model)
- [What are browser developer tools?](https://developer.mozilla.org/en-US/docs/Learn/Common_questions/What_are_browser_developer_tools)

## 各浏览器默认样式

- [Chromium](https://chromium.googlesource.com/chromium/blink/+/master/Source/core/css/html.css)
- [Firefox](https://searchfox.org/mozilla-central/source/layout/style/res/html.css)
- [Webkit](https://trac.webkit.org/browser/trunk/Source/WebCore/css/html.css)
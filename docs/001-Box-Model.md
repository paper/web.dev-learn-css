# 盒子模型
CSS 显示的每一个内容都是一个盒子。理解盒子模型是如何工作，是 CSS 的核心基础。

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

[https://codepen.io/web-dot-dev/pen/WNRemxN](https://codepen.io/web-dot-dev/pen/WNRemxN)

在写CSS或在写整个web时，有一个非常重要的事情，那就是CSS显示的所有内容都是一个个盒子。即使盒子使用了`border-radius`圆角属性让它看起来像个圆，或者放一些文本。唯一要记住就是：“它们都是盒子”。

>[译者注]  
>大家可以使用 Chrome 调试工具，在`Elements` tab 下面，鼠标移到任何元素上去看看，是不是都有个透明的矩形的高亮阴影。

## 内容和大小

>[译者注]  
>- **extrinsic sizing** 外部大小：即根据元素的上下文确定大小，而不考虑其内容。(比如直接给它设置一个`width`)
>- **intrinsic sizing** 内在大小：即根据元素内容来确定大小，而不考虑其上下文。(即`min-content`和`max-content`)
>
>参考：
>- https://drafts.csswg.org/css-sizing-3/#extrinsic
>- **Extrinsic sizing** determines sizes based on the context of an element, without regard for its contents.
>- https://drafts.csswg.org/css-sizing-3/#intrinsic
>- **Intrinsic sizing** determines sizes based on the contents of an element, without regard for its context.

盒子会根据不同的`display` 值会有不同的特性，设置尺寸和内容都会影响到它们。内容可以是更多的盒子（子元素）或纯文本内容。无论哪种方式，默认情况下，内容都会影响盒子的尺寸。

你可以使用**外部大小**来控制盒子大小，或者也可以让浏览器使用**内在大小**来决定。

为了快速区别它们的不同，让我们来看个例子：

[https://codepen.io/web-dot-dev/pen/abpoMBL](https://codepen.io/web-dot-dev/pen/abpoMBL)

该 Demo 里面有一个固定尺寸和粗描边的盒子，盒子里面写着 "CSS is awesome"。盒子设置了宽度，所以是它是**外部大小**，限制其内容的大小。但问题是这个 "awesome" 对这个盒子来说太大了，所以它溢出了父亲的边框（稍后会详细介绍）。为了防止这种溢出的一种方式就是不要给盒子设置宽度，或在这个例子中，设置`width`的值为`min-content`。

>[译者注]  
>原英文的解释不好理解，所以使用了zhangxinxu的原句:  
>这个`min-content` 宽度表示的并不是内部哪个宽度小就是哪个宽度，而是，采用内部元素最小宽度值最大的那个元素的宽度作为最终容器的宽度。

这就让盒子恰当的环绕 "CSS is awesome"，完美！

让我们看一些更复杂的例子，看看不同大小对实际内容的影响：

[https://codepen.io/web-dot-dev/pen/wvgwOJV](https://codepen.io/web-dot-dev/pen/wvgwOJV)

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

[https://codepen.io/argyleink/pen/bGNmgGW](https://codepen.io/argyleink/pen/bGNmgGW)

**边框盒子**环绕着内边距盒子，它的空间由`border`属性值决定。边框盒子是你能直观看到盒子范围的最大边缘。`border`属性通被用来给元素画个边框。

最后一个**外边距盒子**，是环绕着盒子的外空间，由`margin`属性值决定。像`outline`和`box-shadow`属性也占据这个空间，因为它们是画在上层的，所以它们不会影响我们盒子的尺寸。






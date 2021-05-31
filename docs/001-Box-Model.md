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

> [译者注] 大家可以使用 Chrome 调试工具，在`Elements` tab 下面，鼠标移到任何元素上去看看，是不是都有个透明的矩形的高亮阴影。

## 内容和大小

盒子会根据不同的`display` 值会有不同的特性，设置尺寸和内容都会影响到它们。内容可以是更多的盒子（子元素）或纯文本内容。无论哪种方式，默认情况下，内容都会影响盒子的尺寸。

你可以**改变外部**来控制盒子大小，或者也可以让浏览器使用**内部大小**（依据内容本身大小）来决定。

为了快速区别它们的不同，让我们来看个例子：

[https://codepen.io/web-dot-dev/pen/abpoMBL](https://codepen.io/web-dot-dev/pen/abpoMBL)

该 Demo 里面有一个固定尺寸和粗描边的盒子，盒子里面写着 "CSS is awesome"。盒子设置了宽度，所以是它是**外部尺寸**，它控制其子内容的大小。但问题是这个 "awesome" 对这个盒子来说太大了，所以它溢出了父亲的边框（稍后会详细介绍）。为了防止这种溢出的一种方式就是不要给盒子设置宽度，或在这个例子中，设置`width`的值为`min-content`。

这个`min-content` 宽度表示的并不是内部哪个宽度小就是哪个宽度，而是，采用内部元素最小宽度值最大的那个元素的宽度作为最终容器的宽度。[译者注：原英文的解释不好理解，所以使用了zhangxinxu的原句]

这就让盒子恰当的环绕 "CSS is awesome"，完美！

让我们看一些更复杂的例子，看看不同大小对实际内容的影响：

[https://codepen.io/web-dot-dev/pen/wvgwOJV](https://codepen.io/web-dot-dev/pen/wvgwOJV)

Toggle intrinsic sizing on and off to see how you can gain more control with extrinsic sizing and let the content have more control with intrinsic sizing. To see the effect that intrinsic and extrinsic sizing has, add a few sentences of content to the card. When this element is using extrinsic sizing, there's a limit of how much content you can add before it overflows out of the element's bounds, but this isn't the case when intrinsic sizing is toggled on.

通过切换不同尺寸开关，来看看使用外部尺寸你可以获取哪些信息，和使用内部尺寸获取哪些的信息。为了查看这两块不同尺寸类型的影响，





























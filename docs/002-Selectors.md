# 选择器
>原文地址：https://web.dev/learn/css/selectors/
>
> 想要把 CSS 应用在某个元素上，那首选你得选中这个元素。CSS 为你提供了很多种方法，你可以在这个模块中去探索。

这里有一段文本，如果想让文章的第一段变大变红，应该怎么做？

```html
<article>
  <p>I want to be red and larger than the other text.</p>
  <p>I want to be normal sized and the default color.</p>
</article>
```

你可以使用CSS选择器找到这一段，然后应用CSS规则，像这样：

```css
article p:first-of-type {
  color: red;
  font-size: 1.5em;
}
```

CSS提供了非常多的这一类选项，有非常简单的，也有非常复杂的，用来解决这种情况。

## CSS规则的组成部分

要想了解选择器的工作方式和它们在CSS中的作用，那么理解CSS规则原理很重要。

![An image of CSS rule with the selector .my-css-rule](./images/002-1.svg)
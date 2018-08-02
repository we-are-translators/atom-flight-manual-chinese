# 字体图标

Atom 使用 [Octicons 4.4.0](https://github.com/github/octicons/tree/v4.4.0) 来捆绑图标集。使用它们来添加给你的包添加图标。

> 注意：一些从版本 `2.1.2` 遗留下来的老的图标依旧保持着向后兼容性，可以继续使用。

## 概览

在 [样式指南](https://flight-manual.atom.io/hacking-atom/sections/creating-a-theme/#atom-styleguide) 一节中的 "Icons" 部分，你将找到所有可用的 Octicons 图标。

![Octicons in the Styleguide](https://flight-manual.atom.io/hacking-atom/images/iconography.png)

## 用法

Octicons 可以在你的 HTML 中用简单地用 CSS 类来添加。用 `icon icon-` 来给图标名字添加前缀。

作为一个示例，要添加一个显示器图标（`device-desktop`），使用 `icon icon-device-desktop` 类：

```html
<span class="icon icon-device-desktop"></span>
```

## 大小

Octicons 图标在 `font-size` 为 `16px` 时看起来最佳。它是默认使用的大小，所以无需担心这个问题。如果你喜欢不同的图标大小，可以尝试使用 16 的倍数（`32px`，`48px` 等）来获得最显著的效果。在它们中间的大小也是 ok 的，但是对于带有直线的图标可能看起来会有点模糊。

## 可用性

尽管图标能让你的 UI 在视觉上富有吸引力，但是当没有文本标签的时候，很难猜测出它所要表达的意思。如果没有给文本标签放置的位置，可以考虑在鼠标悬停的时候添加一个 [tooltip](https://atom.io/docs/api/v1.29.0/TooltipManager)。或者添加一个更巧妙的 `title="label"` 属性也会有所帮助。

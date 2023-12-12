# 精确的进度保存

## 主要原理

不仅仅是保存用户读到了哪一章，还保存用户离开时屏幕上的那几段。

KMP算法可以很快找到对应的位置。

## GPT 怎么说

为了实现阅读进度保存功能，我们可以通过JavaScript (或者在您的情况下是TypeScript) 来检测屏幕上可见的最上面的 `<p>` 标签。这个功能涉及到浏览器的视窗（viewport）内元素的可见性检测。

下面是一个TypeScript函数的示例，它可以找到当前视窗中最上面的 `<p>` 元素。这个函数将遍历所有的 `<p>` 元素，检查它们是否在视窗内，并记录最上面的一个。

```typescript
const getTopVisibleParagraph = (): HTMLElement | null => {
  const paragraphs = document.querySelectorAll('p');
  let topParagraph: HTMLElement | null = null;
  let topParagraphTop = Infinity;

  paragraphs.forEach(paragraph => {
    const rect = paragraph.getBoundingClientRect();
    const isVisible = rect.top >= 0 && rect.bottom <= window.innerHeight;

    if (isVisible && rect.top < topParagraphTop) {
      topParagraph = paragraph as HTMLElement;
      topParagraphTop = rect.top;
    }
  });

  return topParagraph;
};

// 使用例子
window.addEventListener('scroll', () => {
  const topParagraph = getTopVisibleParagraph();
  if (topParagraph) {
    console.log('Top visible paragraph:', topParagraph.innerText);
    // 这里可以进一步实现保存进度的逻辑，比如使用localStorage
  }
});
```

这个函数首先获取页面上所有的 `<p>` 元素，然后遍历它们来确定它们是否在当前视窗内。通过比较各个 `<p>` 元素的顶部位置，我们可以找到最上面的一个。

请注意，这个函数假设页面中的 `<p>` 元素不会重叠，并且它们是按照在文档中出现的顺序排列的。

要实现保存和读取进度的功能，您可能还需要将找到的 `<p>` 元素的某种标识（比如它的ID或者是在文档中的位置）保存到浏览器的 `localStorage` 或者是服务器上。然后，在用户下次访问时，您可以使用这个标识来找到之前保存的位置，并将视窗滚动到那个位置。

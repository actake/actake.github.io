---
title: '记一次fixed布局的坑'
date: 2021-02-27 14:08:35
tags: css
---

挺久没有更新了，这周刚好碰到一个坑，顺手记录下来，方便以后 review 吧。
## 背景
- 最近在做一个移动端项目，功能就先不谈了，反正呢，我这边需要接入一个其他人开发的组件
- 这个组件的主要内容的布局是 fixed 布局

## 问题
- 在学习模式一切正常
- 但是在解析模式下，看不到这个组件。打开控制台查看，发现 fixed 布局的 dom 结构计算出来的 left 值为三百多，超出了屏幕

经过了一番漫长的排查，怀疑是 fixed 布局的基准变化了，所以到网上查了下资料，确实有这种情况。
对于这个问题，MDN 其实已经有概述了：
> [当元素祖先的 transform, perspective 或 filter 属性非 none 时，容器由视口改为该祖先。](https://developer.mozilla.org/zh-CN/docs/Web/CSS/position)

## 写个 demo
<p class="codepen" data-height="265" data-theme-id="dark" data-default-tab="html,result" data-user="actake" data-slug-hash="MWbQRwK" style="height: 265px; box-sizing: border-box; display: flex; align-items: center; justify-content: center; border: 2px solid; margin: 1em 0; padding: 1em;" data-pen-title="MWbQRwK">
  <span>See the Pen <a href="https://codepen.io/actake/pen/MWbQRwK">
  MWbQRwK</a> by actake (<a href="https://codepen.io/actake">@actake</a>)
  on <a href="https://codepen.io">CodePen</a>.</span>
</p>
<script async src="https://cpwebassets.codepen.io/assets/embed/ei.js"></script>

## 最后
所以确实是 fixed 的基准错了导致 bug，再排查下，发现在解析模式下，上层使用 Carousel 组件，该组件容器带有不为 none 的 transform 样式。
排查到原因后，修改就很简单了，设置一个正确的基准就 ok 了
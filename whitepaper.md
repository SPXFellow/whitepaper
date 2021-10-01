# SPXDesign：一种将 Minecraft.net 博文转换为 BBCode 的排版方案

*最后修改日期：2021-10-02*



## I. 前言

自从 2016 年 Minecraft.net 更换了全新的设计之后，官网也同步地引入了博文系统。此后，有关 Minecraft 的各种消息将通过博文系统进行发布。博文的排版经过精心设计，在保证美观的同时，也提供了如博文分类、创作者形象等新颖而有用的元素。

然而，在将官网博文翻译并发布到使用了 BBCode 系统的论坛时，排版问题也随之而来。一般的 BBCode 论坛并不支持将源网站复制粘贴后就进行原模原样的排版，而如果直接将翻译文字不加排版地发布出来，又会降低文字的可阅读性。

在我的世界中文论坛，早期很少有用户尝试排版。即使是美化样式，也通常只是调整字号大小或者修改字体颜色。而在其中，部分用户突破了这种朴素的风格，改进了排版的视觉效果：

* [@Smokey_Days](https://www.mcbbs.net/home.php?mod=space&uid=2065001)（UID：2065001）首先尝试了翻译官网博文，并进行了相应的排版，其中帖子主体以黄色为底，字体为绿色，而原文以引用文字的方法呈现。相比于当时直接发布文字的其它译者，该排版已经非常美观了。
* [@像素君](https://www.mcbbs.net/home.php?mod=space&uid=479737)（UID：479737）首先使用了双重表格的排版方案，呈现出描边文本框的效果。随后也有其它一些用户使用了类似的方案。
* [@qsefthuopq](https://www.mcbbs.net/home.php?mod=space&uid=579717)（UID：579717）使用了美化过的信息栏，使得博文的发布日期、原文地址等不再是简单地附在原文之后。

SPXDesign 的原型来自于[@kakagou12](https://www.mcbbs.net/home.php?mod=space&uid=10240)（UID：10240）。最早的时候，仅仅包含了文章末尾标注和译者信息文字。而在一段时间的迭代后，该排版方案被整理出来，并且借助脚本实现了自动化的转换。随后，[@SPGoding](https://www.mcbbs.net/home.php?mod=space&uid=2444378)（UID：2444378）接管了排版自动化工具的开发，同时采纳了由 kakagou12 提出的设计方案。这一设计方案在后来继续不断地迭代，并成为了现今的 SPXDesign。

SPXDesign 背后的设计旨在遵循以下原则：

1. **还原官网设计**：排版没有高低之分，在保证美观和高可阅读性下，每一种排版方案都是适宜的。SPXDesign 选择尽可能地还原官网中的设计元素，让读者获得和在官网下几乎一致的阅读体验。
2. **低调的原文**：有些排版方案中可能会抛弃原文，只呈现最终的译文。但我们相信，不是所有人都能完全、准确地表达原文的意思，错译、笔误在所难免。因此，保留原文可以使读者在发觉译文不通顺时迅速获得参考。然而，若原文的样式与译文一致，那么也会出现原文喧宾夺主，阻碍译文阅读的可能性。因此，SPXDesign 选择在保留原文的同时，尽可能地让读者忽略原文的存在，只在需要的时候才阅览文本。
3. **自动化友好**：SPXDesign 最早就是伴随着自动化工具而生的，很多设计权衡了自动化工具的实现难度。如果某种方案可能使得译者在翻译工作之外还要进行其它繁琐的工作，那么就尽可能地不选择这种实现方法。此外，通过良好的定义，每个开发者都可以独立地实现一个将官网博文转换为使用 SPXDesign 方案进行 BBCode 排版的自动化工具。
4. **面向桌面版视图**：某些设计在手机端视图可能会有不同的效果，甚至破坏可阅读性。然而，SPXDesign 是专为桌面版视图设计的，不会考虑所使用的设计元素在手机端视图效果不同的问题。



## II. 官网博文的构成要素

一篇 Minecraft.net 官网博文中，通常包含以下要素：

* 题图
* 文章的分类
* 大标题，以及紧随其后的副标题
* 正文段落，其中有基础的链接文本、粗体/斜体格式等，代码文本还有独特的涂色
* 有序或无序列表
* 图片，且多张图片并列时，有幻灯片滑动效果
* 引用文字，且通常会有被引用者的形象和名称
* 作者信息，包括作者的形象、名字以及发布时间

在某些时候，也会出现一些不常用的元素，包括但不限于：

* 按钮
* 表格

接下来，本文就将阐述如何将这些构成要素翻译为 BBCode。请注意，官网的前端样式是在不停地更新的，因此本文只介绍转换方法，而不会介绍 html 标签到 BBCode 的转换方案。

## III. 文章的起始部分

文章的起始部分包含了：题图、文章分类、大标题、副标题。在 SPXDesign 中，除了复现这些内，还应当注意论坛背景图片的使用（若论坛不支持则可忽略）。

* 在 BBCode 中，首先使用 `postbg` 标签，添加对 `bg3.png` 的使用。

* 使用 `align` 标签创建居中的域，并依次添加以下内容：
  * 题图，并限定尺寸为 `965 x 412`。
  * 文章分类，背景色为 `Black`，字体颜色为 `White`，加粗，文本全大写。题图和文章分类之间间隔一行。
  * 大标题原文，字号为 `6`，字体颜色为 `Silver`，加粗，文本全大写。大标题原文和文章分类间隔一行。
  * 大标题译文，字号为 `6`，加粗。
  * 副标题原文，字号为 `2`，字体颜色为 `Silver`，加粗。副标题原文和大标题译文间隔一行。
  * 副标题译文，字号为 `4`，加粗。



以 [Taking Inventory: Candle](https://www.minecraft.net/en-us/article/taking-inventory--candle) 为例，转换的 BBCode 方案应该是：

```
[postbg]bg3.png[/postbg][align=center][img=965,412]https://www.minecraft.net/content/dam/games/minecraft/screenshots/candle-header.jpg[/img]

[backcolor=Black][color=White][b]DEEP DIVES[/b][/color][/backcolor]

[size=6][b][color=Silver]TAKING INVENTORY: CANDLE[/color][/b][/size]
[size=6][b]背包盘点：蜡烛[/b][/size]

[size=2][b][color=Silver]A wick-edly good source of light[/color][/b][/size]
[size=4][b]副标题译文[/b][/size][/align]
```



## IV. 段落正文

段落正文是最普通的文本。给定一段文本，按以下方式进行转换：

* 使用两个 `indent` 便签创建左右缩进，在这个缩进域中：
  * 写出原文，字体颜色为 `Silver`，字号为 `2`。
  * 写出译文，不作任何特殊处理。

原文和译文之间不留间隔行，两个段落间间隔一行。



以 [Taking Inventory: Candle](https://www.minecraft.net/en-us/article/taking-inventory--candle) 的两段为例，转换的 BBCode 方案应该是：

```
[indent][indent][size=2][color=Silver]Candles are easy to make. Combine some string (for the wick) and some honeycomb (for the wax) in a crafting grid, and voila – one candle is yours. Plop it down and er... it’ll just sit there unlit. You’ll need to light it with a flint and steel, fire charge, or persuade a friendly ghast to shoot a fireball at it (not recommended). [/color][/size]
元工来非选省再南传用组儿界三设花，三业存动最原更上建式旱建说。也置示土不等五技量，从快算存素方型织，外建刷形广隶除。实生结半快小分听证展你量，没活界东世响向如结西，位满除4金村律证农磨。构观务始级特族们按并局叫，这共第与过利代做四王。[/indent][/indent]

[indent][indent][size=2][color=Silver]Once lit, the candle will light up your life. But not much. A single candle only emits a light level of 3 – far from enough to deter hostile monsters from spawning. But here’s something cool. Place another candle on the same spot and it’ll add another three to the light level. You can add up to four candles in the same block, for a maximum light level of 12.[/color][/size]
边空角百业土较样直与，习积命个边级上清七，开支Z线法歼价九习。记要活题技矿料全位县调至，这并话你叫科强去六但。件节务安适布技该可指常，转着业系周则个因种下四，料第1可三多按老身。[/indent][/indent]
```



## V. 文本的特殊格式

文本中通常会存在一些特殊格式，如加粗等。对于这些特殊格式，按以下方式进行处理：

* 加粗：在原文和译文中使用 `b` 标签包围加粗文本。
* 斜体：在原文和译文中使用 `i` 标签包围斜体文本。
* 链接：原文中直接用 `url` 标签（文本颜色应该是默认的 `Silver`），译文中先使用 `url` 标签，然后使用 `color` 标签将链接文本改为颜色`#388d40`。

这些特殊格式会在各种地方出现，如果没有特别声明，均按照本方法处理。



以 [Taking Inventory: Candle](https://www.minecraft.net/en-us/article/taking-inventory--candle) 的两段为例，关于链接的转换的 BBCode 方案应该是：

```
[size=2][color=Silver]For a very long time in Minecraft, there were only a handful of sources of light. There were the Sun and Moon (obviously), lava, fire, the humble [url=https://www.minecraft.net/article/taking-inventory-torch.html][color=Silver]torch[/color][/url], the Nether’s radiant [url=https://www.minecraft.net/article/block-week-glowstone.html][color=Silver]glowstone[/color][/url], and the creepy [url=https://www.minecraft.net/article/block-week--jack-o-lantern.html][color=Silver]jack o’lantern[/color][/url] .[/color][/size]
素车每亲果海院联以，已者分我里去史才选，认示先也设型都屈，达员标队志茄火[url=https://www.minecraft.net/article/taking-inventory-torch.html][color=#388d40]火把[/color][/url]，生平究展百马量法[url=https://www.minecraft.net/article/block-week-glowstone.html][color=#388d40]荧石[/color][/url]，元难按一见起前无果长今达[url=https://www.minecraft.net/article/block-week--jack-o-lantern.html][color=#388d40]南瓜灯[/color][/url]。
```

注意，为了简洁，这里没有写段落文本的 `indent` 标签。



## ？. 针对新闻类帖子的特殊处理



## ？. 关于自动化工具的建议



## 




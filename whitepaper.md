# SPXDesign：一种将 Minecraft.net 博文转换为 BBCode 的排版方案

*最后修改日期：2021-10-03*



## I. 前言

自从 2016 年 Minecraft.net 更换了全新的设计之后，官网也同步地引入了博文系统。此后，有关 Minecraft 的各种消息将通过博文系统进行发布。博文的排版经过精心设计，在保证美观的同时，也提供了如博文分类、创作者形象等新颖而有用的元素。

然而，在将官网博文翻译并发布到使用了 BBCode 系统的论坛时，排版问题也随之而来。一般的 BBCode 论坛并不支持将源网站复制粘贴后就进行原模原样的排版，而如果直接将翻译文字不加排版地发布出来，又会降低文字的可阅读性。

在我的世界中文论坛，早期很少有用户尝试排版。即使是美化样式，也通常只是调整字号大小或者修改字体颜色。而在其中，部分用户突破了这种朴素的风格，改进了排版的视觉效果：

* [@Smokey_Days](https://www.mcbbs.net/home.php?mod=space&uid=2065001)（UID：2065001）首先尝试了翻译官网博文，并进行了相应的排版，其中帖子主体以黄色为底，字体为绿色，而原文以引用文字的方法呈现。相比于当时直接发布文字的其它译者，该排版已经非常美观了。
* [@像素君](https://www.mcbbs.net/home.php?mod=space&uid=479737)（UID：479737）首先使用了双重表格的排版方案，呈现出描边文本框的效果。随后也有其它一些用户使用了类似的方案。
* [@qsefthuopq](https://www.mcbbs.net/home.php?mod=space&uid=579717)（UID：579717）使用了美化过的信息栏，使得博文的发布日期、原文地址等不再是简单地附在原文之后。

SPXDesign 的原型来自于[@kakagou12](https://www.mcbbs.net/home.php?mod=space&uid=10240)（UID：10240）。最早的时候，仅仅包含了文章末尾标注和译者信息文字。而在一段时间的迭代后，该排版方案被整理出来，并且借助脚本实现了自动化的转换。随后，[@SPGoding](https://www.mcbbs.net/home.php?mod=space&uid=2444378)（UID：2444378）接管了排版自动化工具的开发，同时采纳了由 kakagou12 提出的设计方案。这一设计方案在后来继续不断地迭代，并成为了现今的 SPXDesign。

SPXDesign 背后的设计旨在遵循以下理念：

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

文章的起始部分包含了：题图、文章分类、大标题、副标题。在 SPXDesign 中，除了复现这些内，还应当注意论坛背景图片的使用，使其尽可能契合官网的背景底色 `#F8F5F4`（若论坛不支持则可忽略）。

* 在 BBCode 中，首先使用 `postbg` 标签，添加对 `bg3.png` 的使用（仅限我的世界中文论坛）。

* 使用 `align` 标签创建居中的域，并依次添加以下内容：
  * 题图，使用 `img` 标签引用原图链接，并限定尺寸为 `965 x 412`。
  * 文章分类，背景颜色为 `Black`，字体颜色为 `White`，加粗，文本全大写。题图和文章分类之间间隔一行。
  * 大标题原文，字号为 `6`，字体颜色为 `Silver`，加粗，文本全大写。大标题原文和文章分类间隔一行。
  * 大标题译文，字号为 `6`，加粗。
  * 副标题原文，字号为 `4`，字体颜色为 `Silver`，加粗。副标题原文和大标题译文间隔一行。
  * 副标题译文，字号为 `4`，加粗。



以 [Taking Inventory: Candle](https://www.minecraft.net/en-us/article/taking-inventory--candle) 为例，BBCode 转换方案应该是：

```
[postbg]bg3.png[/postbg][align=center][img=965,412]https://www.minecraft.net/content/dam/games/minecraft/screenshots/candle-header.jpg[/img]

[backcolor=Black][color=White][b]DEEP DIVES[/b][/color][/backcolor]

[size=6][b][color=Silver]TAKING INVENTORY: CANDLE[/color][/b][/size]
[size=6][b]背包盘点：蜡烛[/b][/size]

[size=4][b][color=Silver]A wick-edly good source of light[/color][/b][/size]
[size=4][b]副标题译文[/b][/size][/align]
```



## IV. 常规文本

常规文本即一般性的文字，例如段落正文都是常规文本。给定一段常规文本，按以下方式进行转换：

* 使用两个 `indent` 便签创建左右缩进，在这个缩进域中：
  * 写出原文，字体颜色为 `Silver`，字号为 `2`。
  * 写出译文，不作任何特殊处理。

对于段落文本，原文和译文之间不留间隔行，两个段落间间隔一行。

如果没有特别声明，常规文本默认按本方法处理。除了文本外，文章的各部件若没有特殊声明，也放入缩进域中。



以 [Taking Inventory: Candle](https://www.minecraft.net/en-us/article/taking-inventory--candle) 为例，BBCode 转换方案应该是：

```
[indent][indent][size=2][color=Silver]Candles are easy to make. Combine some string (for the wick) and some honeycomb (for the wax) in a crafting grid, and voila – one candle is yours. Plop it down and er... it’ll just sit there unlit. You’ll need to light it with a flint and steel, fire charge, or persuade a friendly ghast to shoot a fireball at it (not recommended). [/color][/size]
元工来非选省再南传用组儿界三设花，三业存动最原更上建式旱建说。也置示土不等五技量，从快算存素方型织，外建刷形广隶除。实生结半快小分听证展你量，没活界东世响向如结西，位满除4金村律证农磨。构观务始级特族们按并局叫，这共第与过利代做四王。

[size=2][color=Silver]Once lit, the candle will light up your life. But not much. A single candle only emits a light level of 3 – far from enough to deter hostile monsters from spawning. But here’s something cool. Place another candle on the same spot and it’ll add another three to the light level. You can add up to four candles in the same block, for a maximum light level of 12.[/color][/size]
边空角百业土较样直与，习积命个边级上清七，开支Z线法歼价九习。记要活题技矿料全位县调至，这并话你叫科强去六但。件节务安适布技该可指常，转着业系周则个因种下四，料第1可三多按老身。[/indent][/indent]
```

注意，不必每一个段落都要重新加入 `indent` 标签，没有特殊要求的常规文本可以合并到一个缩进域内。



## V. 文本的特殊格式

文本中通常会存在一些特殊格式，如加粗等。对于这些特殊格式，按以下方式进行处理：

* 加粗：在原文和译文中使用 `b` 标签包围加粗文本。
* 斜体：在原文和译文中使用 `i` 标签包围斜体文本。
* 链接：原文中直接用 `url` 标签（文本颜色应该是默认的 `Silver`），译文中先使用 `url` 标签，然后使用 `color` 标签将链接文本改为颜色`#388d40`。
* 代码：在原文和译文中均使用以下样式：背景颜色为 `#f1edec`，字体为 `SFMono-Regular,Menlo,Monaco,Consolas,"Liberation Mono","Courier New",monospace`。译文额外添加字体颜色为 `#7824c5`。

这些特殊格式会在各种地方出现，如果没有特别声明，均按照本方法处理。



以 [Taking Inventory: Candle](https://www.minecraft.net/en-us/article/taking-inventory--candle) 为例，关于链接的 BBCode 转换方案应该是：

```
[size=2][color=Silver]For a very long time in Minecraft, there were only a handful of sources of light. There were the Sun and Moon (obviously), lava, fire, the humble [url=https://www.minecraft.net/article/taking-inventory-torch.html][color=Silver]torch[/color][/url], the Nether’s radiant [url=https://www.minecraft.net/article/block-week-glowstone.html][color=Silver]glowstone[/color][/url], and the creepy [url=https://www.minecraft.net/article/block-week--jack-o-lantern.html][color=Silver]jack o’lantern[/color][/url] .[/color][/size]
素车每亲果海院联以，已者分我里去史才选，认示先也设型都屈，达员标队志茄火[url=https://www.minecraft.net/article/taking-inventory-torch.html][color=#388d40]火把[/color][/url]，生平究展百马量法[url=https://www.minecraft.net/article/block-week-glowstone.html][color=#388d40]荧石[/color][/url]，元难按一见起前无果长今达[url=https://www.minecraft.net/article/block-week--jack-o-lantern.html][color=#388d40]南瓜灯[/color][/url]。
```



以 [Minecraft Snapshot 21w39a](https://www.minecraft.net/en-us/article/minecraft-snapshot-21w39a) 为例，关于代码的 BBCode 转换方案应该是：

```
[backcolor=#f1edec][color=Silver][font=SFMono-Regular,Menlo,Monaco,Consolas,"Liberation Mono","Courier New",monospace]minecraft/textures/gui/container/inventory.png[/font][/color][/backcolor] now contains an extra sprite for a thin-layout version of the effect list in the inventory
[backcolor=#f1edec][color=#7824c5][font=SFMono-Regular,Menlo,Monaco,Consolas,"Liberation Mono","Courier New",monospace]minecraft/textures/gui/container/inventory.png[/font][/color][/backcolor]真必或率数每上直儿定般他非决道气识，作然满这约斗感维询求权C民芦茎。
```



## VI. 文章内部的标题

文章内部有时会使用标题，这些标题在 html 代码中以 `h1`, `h2`, `h3` 等呈现，对应一级标题、二级标题、三级标题等。

对于标题，假设当前的标题级数为 `x`，则需要将其字号修改为 `7 - x`，并加粗。其余样式均按段落文本的方式实现。

标题应该与上方和下方的内容间隔一行。然而，如果标题紧跟着另一个标题，那么它们直接只留一个空行。



以 [Minecraft Snapshot 21w39a](https://www.minecraft.net/en-us/article/minecraft-snapshot-21w39a) 为例，BBCode 转换方案应该是：

```
[size=5][b][color=Silver]Loot Tables[/color][/b][/size]
[size=5][b]Loot Tables[/b][/size]

[size=4][b][color=Silver]New functions[/color][/b][/size]
[size=4][b]New functions[/b][/size]

[size=3][b][color=Silver]set_potion[/color][/b][/size]
[size=3][b]set_potion[/b][/size]

[size=2][color=Silver]Sets Potion tag on any item[/color][/size]
Sets Potion tag on any item

[size=2][b][color=Silver]Parameters:[/color][/b][/size]
[size=2][b]Parameters:[/b][/size]

[list]
[*][color=Silver]id - potion id[/color]
[*]id - potion id
[/list]
```

注意，在原文中这里有代码样式，这里为了简洁没有添加进来。



## VII. 有序列表和无序列表

对于无序列表，使用 `list` 标签来创建原生的 BBCode 无序列表。对于每一个列表项，都需要分出原文和译文。原文和译文各占一个列表项，其余样式采用默认方法。

对于有序列表，由于保留原文会影响到有序列表的计数，因此采用无序列表+添加计数文本的方式来实现有序列表。其余格式与无序列表相同。



以 [Minecraft Snapshot 21w39a](https://www.minecraft.net/en-us/article/minecraft-snapshot-21w39a) 为例，关于无序列表的 BBCode 转换方案应该是：

```
[size=3][b][color=Silver]ride_entity_in_lava[/color][/b][/size]
[size=3][b]ride_entity_in_lava[/b][/size]

[list]
[*][color=Silver]Triggered for every tick when player rides in lava[/color]
[*]当玩家在岩浆中骑乘时，每刻都会触发
[*][color=Silver]Conditions[/color]
[*]条件
[list]
[*][color=Silver]player - a player for which this trigger runs[/color]
[*]player - 触发此触发器的玩家
[*][color=Silver]start_position - position where riding started (first tick on lava)[/color]
[*]start_position - 骑乘开始时的位置（处于岩浆中的第一刻）
[*][color=Silver]distance - predicate for distance between start_position and player[/color]
[*]distance - 记录start_position和玩家之间距离的谓词
[/list]
[/list]
```

假设该列表是有序列表，则对应的转换方案应该是：

```
[size=3][b][color=Silver]ride_entity_in_lava[/color][/b][/size]
[size=3][b]ride_entity_in_lava[/b][/size]

[list]
[*][color=Silver]1. Triggered for every tick when player rides in lava[/color]
[*]1. 当玩家在岩浆中骑乘时，每刻都会触发
[*][color=Silver]2. Conditions[/color]
[*]2. 条件
[list]
[*][color=Silver]1. player - a player for which this trigger runs[/color]
[*]1. player - 触发此触发器的玩家
[*][color=Silver]2. start_position - position where riding started (first tick on lava)[/color]
[*]2. start_position - 骑乘开始时的位置（处于岩浆中的第一刻）
[*][color=Silver]3. distance - predicate for distance between start_position and player[/color]
[*]3. distance - 记录start_position和玩家之间距离的谓词
[/list]
[/list]
```



## VIII. 图片

图片分为单图和多图。注意，无论单图还是多图，均不使用 `indent` 缩进。

对于单图，按以下方式进行处理：

* 使用 `align` 标签创建居中的域，并依次添加以下内容：
  * 图片，使用 `img` 标签引用原图链接，不限定尺寸。
  * 图片描述原文和译文，加粗，其余采用默认样式。图片描述与图片之间不加间隔行。

对于多图，按以下方式处理：

* 若论坛支持 `album` 标签，则：
  * 使用 `album` 标签创建幻灯片域，并使用 `[aimg=<图片链接>]<描述文本>[/aimg]` 添加一张图片。
  * 描述文本按单图方式排版。*注意，我的世界中文论坛在幻灯片实现上有问题，建议描述文本中只添加不带样式的译文，且没有图片描述时添加一个空格进行占位。*
* 若论坛不支持任何多图机制，则以单图方式将多图依次排列下来。

如果选择更换图片（如汉化了图片中的文本），直接修改 `img` 指向新图片链接，或更换为论坛图片附件格式。

图片和上下文之间应该保留一个间隔行。



以 [Taking Inventory: Flower Pot](https://www.minecraft.net/en-us/article/taking-inventory-flower-pot) 为例，关于有描述多图的 BBCode 转换方案应该是：

```
[align=center][img]https://www.minecraft.net/content/dam/games/minecraft/screenshots/flower-pot-realworld.jpg[/img]
[b][size=2][color=Silver][url=https://www.geograph.org.uk/profile/18934][color=Silver]Pam Fray[/color][/url] // CC BY-SA 2.0[/color][/size]
[url=https://www.geograph.org.uk/profile/18934][color=#388d40]Pam Fray[/color][/url] // CC BY-SA 2.0[/b][/align]
```

以 [Meet the Enderlings](https://www.minecraft.net/en-us/article/meet-enderlings) 为例，关于无描述多图的 BBCode 转换方案应该是：

```
[album]
[aimg=https://www.minecraft.net/content/dam/games/dungeons/game-characters/enderling-blastling.png] [/aimg]
[aimg=https://www.minecraft.net/content/dam/games/dungeons/game-characters/enderling-snareling.png] [/aimg]
[aimg=https://www.minecraft.net/content/dam/games/dungeons/game-characters/enderling-watchling.png] [/aimg]
[/album]
```



## IX. 普通引用和绿色引用

普通引用是指对某人的话进行引用的文章元素，在普通引用块中，会包含此人的形象、此人的发言，在最后还会附带此人的名字。而绿色引用是一类特殊的引用，是对文章内某段文字进行引用，起强调的作用。

对于普通引用，按以下方法处理：

* 使用 `quote` 标签创建原生的引用域，并依次添加以下内容：
  * 被引用者形象，最外层使用 `float=left` 标签，内部使用 `img` 标签引用形象的图片，尺寸为 `53 x 92`
  * 引用的文字，按常规文本处理，但因为缩进域已在 `quote` 外添加，这里不再需要加入更多的 `indent`。
  * 被引用者的名字，直接呈现原文，不需要处理为原文-译文。如果除了名字还包含职称等信息，也直接翻译，不添加灰色原文。

对于绿色引用，按以下方法处理：

* 在已有的两层缩进的基础上，再加入一层 `indent` 缩进。同时原文与译文的字号均设为 `4`。
* 原文和译文的开头加入一个竖线符，字体颜色为 `SeaGreen`，加粗。



以 [Meet the Enderlings](https://www.minecraft.net/en-us/article/meet-enderlings) 为例，关于普通引用的 BBCode 转换方案应该是：

```
[quote][float=left][img=53,92]https://www.minecraft.net/content/dam/games/minecraft/game-characters/Enderman%20avatar.png[/img][/float][size=2][color=Silver]The most important thing for our artists to nail was giving each of the enderlings a distinct look, emphasising their core gameplay abilities and ensuring they maintained the otherworldly feel the enderman is known for. The watchling is most like the enderman of the three. With its many eyes, it’s always on the lookout for intruders. When it spots a player, it instantly teleports right beside them and lash out with a vicious melee attack. [/color][/size]
度把着支头声照界长价克置情青多红构，片斗快段次较走医变于才五霸于。候常治长但线信起都，说断量型给细低指会，车K决被8杆民。看时今领增局行于今实，京育除经外业识部，设个2术在枪高研。部两解任价小积百，半置连声明当具，由区霸华际县自。效率除受期育，现入金后入政，可L事弦。张有严根日己决半亲角，结需料众议消学得清当，起究2霸报说育V。

 —— MATT DUNTHORNE - LEAD GAME DESIGNER
[/quote]
```



以 [Introducing: Always Building](https://www.minecraft.net/en-us/article/introducing-always-building) 为例，关于绿色引用的 BBCode 转换方案应该是：

```
[indent][size=4][color=SeaGreen][b]|[/b][/color][color=Silver]"One of the main challenges was trying to avoid making the island look like a giant ice cream cone."[/color][/size][/indent]
[indent][size=4][color=SeaGreen][b]|[/b][/color]米间多矿法素儿红干方定，青名九人如入一它没，质和束华董定下半取。[/size][/indent]
```



## X. 视频

相比于图片可以直接访问，文章当中的视频往往引用自 Youtube，这给视频搬运带来了困难。因此，本文提供两种方案：

* 如果能搬运视频，则使用 `media` 标签直接引用视频，并用 `align` 标签使视频居中。 可以自行选择是否引用视频提供者信息。
* 如果不能搬运视频，则引用文章中的视频封面的链接，然后按“可点击的多图”来处理。同时加入信息提醒读者该图片可点击。



以 [Dungeons Diaries: Sound](https://www.minecraft.net/en-us/article/dungeons-diaries--sounds) 为例，在能搬运视频时的 BBCode 转换方案应该是：

```
[align=center][media=x,500,375]https://www.bilibili.com/video/av85125168[/media]
视频提供：@广药 [/align]
```

以 [Making a Minecraft Board Game](https://www.minecraft.net/en-us/article/making-minecraft-board-game) 为例，在不能搬运视频时的 BBCode 转换方案可以是：

```
[align=center][url=https://www.youtube.com/watch?v=OnblaPBorjY][img]https://www.minecraft.net/content/dam/minecraft/insider-features/making-a-board-game/NewVidCover.JPG.transform/minecraft-video-original/image.JPG[/img][/url]
(点击图片跳转到 Youtube 查看视频)[/align]
```



## XI. 按钮与表格

按钮与表格曾在过往博文中出现过，但是次数极为稀少。因此，本文不对这些元素进行专门的设计要求。

如果文章中出现了表格，本文建议使用 `table` 标签进行排版。如果文章中出现了按钮，本文建议对原网页审查元素，修改按钮文本后进行截图，并结合 `url` 标签实现跳转。



## XII. 文章的结束部分

文章的结束部分应该包含文章作者、原文标题与链接，同时附带译者信息。具体来说，应该依次添加以下内容：

* 作者形象，最外层是 `float=left` 标签，内层使用 `img` 标签引用作者形象图片，尺寸为 `82 x 121`
* 作者与译者信息，格式为 `【<译者> 译自[url=<原文网址>][color=#388d40][u]官网 <yyyy> 年 <mm> 月 <dd> 日发布的 <原文标题>[/u][/color][/url]；原作者 <作者名称>】`



以 [Meet the Enderlings](https://www.minecraft.net/en-us/article/meet-enderlings) 为例，文章的结束部分应该是：

```
[float=left][img=82,121]https://www.minecraft.net/content/dam/archive/673fbce045729846ea80943491f2a4ab-PerAvatar.png[/img][/float]

[b]【Ricolove 译自[url=https://www.minecraft.net/en-us/article/meet-enderlings][color=#388d40][u]官网 2021 年 07 月 25 日发布的 Meet the Enderlings[/u][/color][/url]；原作者 Per Landin】[/b]
```



## XIII. 一些建议

1. 请注意，官网的前端在时刻进行改动，可能造成工具失效。
2. 有时候文章会缺少一些本来应当是必填项的东西，例如文章分类。请注意对每个“必填项”的检查。
3. 可以考虑在文章的结束部分添加对你的工具的引用。例如：【本文排版借助了 XXX】




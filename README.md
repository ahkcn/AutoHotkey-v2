关于AHK V2的信息
=============

##文件索引
* [**AutoHotkey v2 alpha.md**](/AutoHotkey v2 alpha.md)为[AHKScript论坛](http://ahkscript.org/boards)帖子[View topic - AutoHotkey v2 alpha ? AHKScript](http://ahkscript.org/boards/viewtopic.php?f=24&t=2120)的**汉化版**。
* 文件夹[**AutoHotkey-v2**](/AutoHotkey-v2)为关于以上AutoHotkey v2 alpha的计算机辅助翻译工具[OmegaT](www.omegat.org)的**项目文档**。
* [**中英对照 中间文件**](/%E4%B8%AD%E8%8B%B1%E5%AF%B9%E7%85%A7%20%E4%B8%AD%E9%97%B4%E6%96%87%E4%BB%B6)便于游览者脱离以上OmegaT工具**提交更正**。

##翻译流程

1. 将[AHKScript论坛](http://ahkscript.org/boards)中主题[View topic - AutoHotkey v2 alpha ? AHKScript](http://ahkscript.org/boards/viewtopic.php?f=24&t=2120)中的帖子，以帖子为单位复制-粘贴-保存为纯文本形式的**源文档**。
2. 将*源文档* **放置**至[OmegaT](www.omegat.org)文档中文件夹[AutoHotkey-v2\ **source**](AutoHotkey-v2/source)下。
3. 在OmegaT中**翻译**。
4. 翻译完毕，自动**输出**至OmegaT文档中文件夹[AutoHotkey-v2\ **target**](AutoHotkey-v2/target)，*源文档* 对应**目标文档**。
5. 合并/追加 *目标文档* 至[AutoHotkey v2 alpha](/AutoHotkey v2 alpha.md)，增加MarkDown **格式**。
6. 额外/可选，从OmegaT项目中复制-粘贴-保存中英对照版本至文件夹[中英对照 中间文件](/%E4%B8%AD%E8%8B%B1%E5%AF%B9%E7%85%A7%20%E4%B8%AD%E9%97%B4%E6%96%87%E4%BB%B6)，手动保存每份对照纯文本/无格式文档。  

###更正流程

提交..


##OmegaT配置
####设置编码：

1. 选择菜单栏`Options`\菜单项`File Filters...`，将弹出窗口`File Filters`；  
2. 选中项`Text Files`，点击按钮`Edit`，将弹出窗口`Edit Filter`；  
3. 选中项`*.txt`，设置（同行）列`Source File Encoding`、`Target File Encoding`为`UTF-8`。

####关于OmegaT的协同模式

>	貌似OmegaT的bug：
>	>它会检查*OmegaT项目文件夹* 是否在*GitHub项目文件夹* 下，若在，则认为GitHub项目是自己的**协同项目**；  
>	>当前把*OmegaT项目文件夹* 放在*GitHub项目文件夹* 下，于是，OmegaT会误判为处于**协同模式**，并提示连接/登陆失败。

解决方法×2：

* 在OmegaT的快捷方式中添加参数`--no-team`，以禁用协同模式。  
形如，`"D:\Program Files\OmegaT\OmegaT.exe" --no-team`。
* 使用软链接，在（*GitHub项目文件夹*）外引用/链接OmegaT项目，通过此外部入口访问OmegaT项目，将不会被识别成在*GitHub项目文件夹*下，于是不会被识别为协同模式。  
**软链接**：参考[Link Shell Extension](http://schinagl.priv.at/nt/hardlinkshellext/hardlinkshellext.html)。
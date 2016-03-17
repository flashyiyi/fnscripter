其实非要我说这个东西有什么用……我也觉得没什么用。不过反正也就用了一周感觉性价比还是很高的，可以把GALGAME搬到浏览器上多多少少也是个选择。

我以前也没做过游戏移植，指令列表都是翻网上的资料查的，然后试验了几个游戏，一些细节还是不怎么清楚。所以希望哪个做移植的能跟我联系把这个做成真正的等价引擎。

QQ:45324714，注明理由即可。

http://code.google.com/p/fnscripter/ 项目地址


下面是用googlecode的空间暂时弄的一个示例，因为服务器限制音乐播放不了，当然为了避免被封已经和谐过了- -
http://fnscripter.googlecode.com/svn/trunk/src/FNScripter.html


请使用IE浏览


---


简介

FNScripter是一个基于FLASH播放器的NScripter脚本解释器，可以在浏览器中即时下载资源直接运行游戏。游戏会自动预先下载一部分资源，如果当前资源尚未下载，依然可以继续阅读之后的文本。

授权

FNScripter基于BSD协议发布，可以自由地应用于商业及非商业应用中。

使用

具体可参考ONScripter的使用方法，这里仅指出不同之处：

1.文件放置
FNScripter不支持文件打包，素材必须以解包形式存在。（可以使用nsout.exe来进行解包）
FNScripter不支持nscript.dat的加密形式，必须以0.txt的文本形式存在。（可以使用nsdec.exe解密）。文本编码为ANSI。此外，还可以支持用G-ZIP方式压缩文本以减少体积，可以使用GhostCatTools来进行文本压缩与解压。
FNScripter不支持ttf等形式的字体文件，需要将字体转换成FLASH支持的SWF格式。同样可以用GhostCatTools进行这个操作。

2.分辨率
游戏的分辨率为800x600，图片应该遵循此规则。如果使用的是已经缩放后的素材，可以在定义区间加上相应指令，诸如PSP使用的360x270可以用mode360指令来进行兼容。要缩放屏幕大小则可以直接控制SWF播放器的宽高。

3.素材转换
FNScripter支持JPG,PNG,GIF图片格式，但不支持BMP。此外，为了减少体积，应该尽量将图片转换成JPG格式。
FNScripter只支持MP3格式的音频，必须将其他格式的音频转换成MP3。

4.修正脚本
如果在之前改变了素材的文件名，就需要在角本里改过来。此外，由于实现方式不同，部分指令会受到影响。
getspsize:由于FNScripter被设计成用于网络环境，所有的资源加载都是异步操作，所以在使用lsp等指令加载图片后，因为图片实际上并没有被加载，立即执行getspsize会无法获得正确的图片大小。你可以在定义区间执行getspsizewait使这个操作成为同步操作，但这会在图片加载完成前锁定游戏。因此，应该尽量避免getspsize指令。可以使用固定大小的素材，或者用ld指令来显示人物立绘。
for:由于FLASH是单线程程序，所以在使用for制作动画时，其中的wait指令不能省略，否则无法显示出中间步骤。

5.游戏进度保存
FNScripter使用保存成一个本地sav文件的方式来保存游戏进度，这个进度也包括了系统存档，因此在读取时将会覆盖系统存档。

6.参数设置
FNScripter没有配置文件，可以在运行时通过FLASHVARS传入配置，目前仅支持下面的参数
charset：指定文本编码（utf-8,gb2312）
gameurl：指定素材文件根目录

以上提到的工具打包下载：
http://fnscripter.googlecode.com/svn/trunk/tool.rar


---


字体文件制作

由于此部分相对比较复杂，就放在最后来讲。为了在游戏中使用自定义字体，必须在游戏目录下放置一个default.swf的字体文件，其中包含字体名为default的嵌入字体资源。制作这样的文件有多种方法，这里说的是用GhostCatTools的做法。

首先，你需要备齐GhostCatTools的运行环境。首先，它需要Flex SDK，可以从官方地址下载：
http://opensource.adobe.com/wiki/display/flexsdk/Flex+SDK

运行Flex SDK需要JRE运行环境，以及相应的配置，具体可请教Google

安装运行GhostCatTools后先在配置界面设置Flex SDK路径，然后点击左下角的“字体生成”。在新的界面中选择一个系统字体，或者选择外部字体并设置路径，并填写字体名称为default，然后点击生成SWF按钮。如果之前配置正常，就可以在目标位置生成我们需要的swf文件。

你可以在外部文本里填上0.txt的文件路径（注意0.txt必须是uft-8的编码格式才能被工具正确读取），或者在下面的文本框复制上0.txt的内容，这时候再生成SWF就会根据文本内容筛选需要的文字，可以减少字体文件大小。
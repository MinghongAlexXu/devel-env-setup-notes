# [Fontconfig](https://gitlab.freedesktop.org/fontconfig/fontconfig)

## Problem

sway/alacritty 启动 qutebrowser 浏览时会发现汉字、日文字等字符无法显示。

> https://codeberg.org/dnkl/foot#fonts
> 
> foot supports all fonts that can be loaded by freetype, including bitmap fonts and color emoji fonts.
>
> Foot uses fontconfig to locate and configure the font(s) to use. Since fontconfig's fallback mechanism is imperfect, especially for monospace fonts (it doesn't prefer monospace fonts even though the requested font is one), foot allows you, the user, to configure the fallback fonts to use.
>
> This also means you can configure each fallback font individually; you want that fallback font to use this size, and you want that other fallback font to be italic? No problem!
>
> If a glyph cannot be found in any of the user configured fallback fonts, then fontconfig's list is used.

## References
- [Choosing font type](https://fonts.google.com/knowledge/choosing_type)
- [Linux fontconfig 的字体匹配机制](https://catcat.cc/post/2020-10-31/)
- [用 fontconfig 治理 Linux 中的字体](https://catcat.cc/post/2021-03-07/)
- https://wiki.archlinux.org/title/font_configuration
- https://wiki.gentoo.org/wiki/Fontconfig
- https://wiki.archlinux.org/title/Fonts#Families
- [Linux 下的字体调校指南](https://szclsya.me/zh-cn/posts/fonts/linux-config-guide/)
- [How To Set Default Fonts and Font Aliases on Linux](https://jichu4n.com/posts/how-to-set-default-fonts-and-font-aliases-on-linux/)
- ext: OTC TTC OTF TTF
  - https://en.wikipedia.org/wiki/OpenType
  - https://docs.microsoft.com/en-us/typography/opentype/spec/recom#filenames
- https://qr.ae/pGjOt0

## 配置的读取流程

首先读取 `/etc/fonts/fonts.conf`，该文件中有句：
```xml
<include ignore_missing="yes">conf.d</include>
```
将 `/etc/fonts/conf.d/` 目录中的文件纳入读取中。在这个目录中的配置文件，按照文件名前的数字的顺序进行读取。而当读取到 `50-user.conf` 的时候，其中的语句：
```xml
<include ignore_missing="yes" prefix="xdg">fontconfig/conf.d</include>
<include ignore_missing="yes" prefix="xdg">fontconfig/fonts.conf</include>
<include ignore_missing="yes" deprecated="yes">~/.fonts.conf.d</include>
<include ignore_missing="yes" deprecated="yes">~/.fonts.conf</include>
```
指示 fontconfig 开始读取用户家目录下的配置文件。语句中的属性值 `prefix="xdg"`，代表 `XDG_CONFIG_HOME` 目录，默认是 `~/.config/`。

fontconfig 在读取家目录的配置文件后，再接着读取完 `/etc/fonts/conf.d/` 中剩余的配置文件。

以上流程，可以指定环境变量 `FC_DEBUG=1024` 看到 fontconfig 读取了哪些配置文件。运行：
```
$ FC_DEBUG=1024 fc-match
```

总结。fontconfig 主要读取
- `/etc/fonts/fonts.conf`
- `/etc/fonts/conf.d/*.conf`
- `~/.config/fontconfig/fonts.conf`
- `~/.config/fontconfig/conf.d/*.conf`

历史遗留的目录位置 `~/.fonts.conf.d/*.conf` 和 `~/.fonts.conf`，由于不遵守 XDG 规范不再使用。

## Enable/Disable [Presets](https://wiki.archlinux.org/title/font_configuration#Presets)

There are presets installed in the directory `/usr/share/fontconfig/conf.avail`. They can be enabled by creating symbolic links to them, both per-user and globally, as described in `/etc/fonts/conf.d/README`. These presets will override matching settings in their respective configuration files.

 For example, to enable sub-pixel RGB rendering globally:

```
$ cd /etc/fonts/conf.d
$ ln -s /usr/share/fontconfig/conf.avail/10-sub-pixel-rgb.conf
```
To do the same but instead for a per-user configuration:
```
$ mkdir $XDG_CONFIG_HOME/fontconfig/conf.d
$ ln -s /usr/share/fontconfig/conf.avail/10-sub-pixel-rgb.conf $XDG_CONFIG_HOME/fontconfig/conf.d
```

## 前缀数字的含义

```
/etc/fonts/conf.d/README
------------------------
conf.d/README

Each file in this directory is a fontconfig configuration file. Fontconfig scans this directory, loading all files of the form [0-9][0-9]*.conf. These files are normally installed in @TEMPLATEDIR@ and then symlinked here, allowing them to be easily installed and then enabled/disabled by adjusting the symlinks.

The files are loaded in numeric order, the structure of the configuration has led to the following conventions in usage:

Files beginning with:	Contain:

00 through 09		Font directories
10 through 19		system rendering defaults (AA, etc)
20 through 29		font rendering options
30 through 39		family substitution
40 through 49		generic identification, map family->generic
50 through 59		alternate config file loading
60 through 69		generic aliases, map generic->family
70 through 79		select font (adjust which fonts are available)
80 through 89		match target="scan" (modify scanned patterns)
90 through 99		font synthesis
```

## Noto Fonts

Google Noto fonts 字库大。
```
$ paru -S noto-fonts noto-fonts-cjk noto-fonts-emoji
```

### Noto CJK

> Noto CJK fonts are rebranded versions of Adobe Source Han fonts, developed by Adobe and Google which contains **Chinese characters**, **Kana**, and **Hangul**; Latin-script letters and numerals are taken from the Source Pro fonts.

语言分为
- Simplified Chinese
- Traditional Chinese
- Hong Kong
- Japanese
- Korean

提供 serif、sans-serif、和 monospace。Hong Kong 没有 serif。

## 三种修改 font pattern 的方式

font pattern 代表 fallback 的顺序。

以传入 fontpattern "Noto Serif" 为例
```
Debug fc-match 观察效果
$ FC_DEBUG=4 fc-match "Noto Serif" | bat
```

### #1. `<alias>`...`<family>`...`<default>`...

```xml
/etc/fonts/conf.d/46-noto-serif.conf
------------------------------------
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
  <alias>
    <family>Noto Serif</family>
    <default>
      <family>serif</family>
    </default>
  </alias>
</fontconfig>
```
"Noto Serif" 经过这个文件后
```
[...]
Rule Set: /etc/fonts/conf.d/46-noto-serif.conf
FcConfigSubstitute test pattern any family Equal(ignore blanks) "Noto Serif"
Substitute Edit family AppendLast "serif"

Append List before "Noto Serif"(s) [marker]
Append List after "Noto Serif"(s) "serif"(w)
FcConfigSubstitute editPattern has 6 elts (size 16)
    family: "Noto Serif"(s) "serif"(w)
    hintstyle: 2(i)(w)
    rgba: 1(i)(w)
    lang: "en"(w)
    lcdfilter: 1(i)(w)
    prgname: "fc-match"(s)
[...]
```
(不是 "Noto Serif", 是 List 的)尾部增加了 "serif"。

FIXME: 这个例子传入的 List 只包含一个 `<family>` 元素，"Noto Serif"，不能直观说明 `<default>` 的作用是在整个 List 最尾部添加元素 (Substitute Edit family AppendLast)；而 `<accept>` 的作用才是紧跟在匹配的 `<family>` 之后添加元素。

### #2. `<alias>`...`<family>`...`<prefer>`...

紧接上一个例子中 font pattern 的结果，"Noto Serif"(s) "serif"(w)

```xml
/etc/fonts/conf.d/60-latin.conf
-------------------------------
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
  <description>Ser preferable fonts for Latin</description>
 <alias>
     <family>serif</family>
     <prefer>
         <family>DejaVu Serif</family>
         <family>Times New Roman</family>
         <family>Thorndale AMT</family>
         <family>Luxi Serif</family>
         <family>Nimbus Roman No9 L</family>
         <family>Nimbus Roman</family>
         <family>Times</family>
     </prefer>
  </alias>
</fontconfig>
```
"Noto Serif"(s) "serif"(w) 经过这个文件后
```
[...]
Rule Set: /etc/fonts/conf.d/60-latin.conf
FcConfigSubstitute test pattern any family Equal(ignore blanks) "serif"
Substitute Edit family Prepend "DejaVu Serif" Comma "Times New Roman" Comma "Thorndale AMT" Comma "Luxi Serif" Comma "Nimbus Roman No9 L" Comma "Nimbus Roman" Comma "Times"

Prepend List before "Noto Serif"(s) [marker] "serif"(w)
Prepend List after "Noto Serif"(s) "DejaVu Serif"(w) "Times New Roman"(w) "Thorndale AMT"(w) "Luxi Serif"(w) "Nimbus Roman No9 L"(w) "Nimbus Roman"(w) "Times"(w) "serif"(w)
FcConfigSubstitute editPattern has 6 elts (size 16)
    family: "Noto Serif"(s) "DejaVu Serif"(w) "Times New Roman"(w) "Thorndale AMT"(w) "Luxi Serif"(w) "Nimbus Roman No9 L"(w) "Nimbus Roman"(w) "Times"(w) "serif"(w)
    hintstyle: 2(i)(w)
    rgba: 1(i)(w)
    lang: "en"(w)
    lcdfilter: 1(i)(w)
    prgname: "fc-match"(s)
[...]
```
"serif" 前加上了 "DejaVu Serif" "Times New Roman" "Thorndale AMT" "Luxi Serif" "Nimbus Roman No9 L" "Nimbus Roman" "Times"

### #3. `<match>`...`<test>`...`<edit>`...

#1 和 #2 只是 #3，`<match>`...`<test>`...`<edit>`...， 所能表达的其中两种操作的语法糖。i.e.
1. `<alias>`...`<family>`...`<default>`... **==** `<match target="pattern">`...`<test qual="any" name="family" ignore-blanks="true">`...`<edit name="family" mode="append_last">`...
2. `<alias>`...`<family>`...`<prefer>`... **==** `<match target="pattern">`...`<test qual="any" name="family" ignore-blanks="true">`...`<edit name="family" mode="prepend">`...

`<match>`...`<test>`...`<edit>`... 是 fontconfig 最通用的操作方式。

根据 [ArchWiki](https://wiki.archlinux.org/title/Font_configuration#Replace_or_set_default_fonts), #3 更可靠。

### Tags

`<alias>`<br>
Alias elements provide a shorthand notation for the set of common match operations needed to substitute one font family for another. They contain a `<family>` element followed by optional `<prefer>`, `<accept>` and `<default>` elements. Fonts matching the `<family>` element are edited to prepend the list of `<prefer>`ed families before the matching `<family>`, append the `<accept>`able families after the matching `<family>` and append the `<default>` families to the end of the family list.

`<family>`<br>
Holds a single font family name

`<prefer>`, `<accept>`, `<default>`<br>
These hold a list of `<family>` elements to be used by the `<alias>` element.

### Summary

1. Fonts matching the `<family>` element are edited to prepend the list of `<prefer>`ed families before the matching `<family>`, append the `<accept>`able families after the matching `<family>` and append the `<default>` families to the end of the family list.

2. Alias elements provide a shorthand notation for the set of common match operations needed to substitute one font family for another. e.g.
    - `<alias>`...`<family>`...`<default>`... **==** `<match target="pattern">`...`<test qual="any" name="family" ignore-blanks="true">`...`<edit name="family" mode="append_last">`...
    - `<alias>`...`<family>`...`<prefer>`... **==** `<match target="pattern">`...`<test qual="any" name="family" ignore-blanks="true">`...`<edit name="family" mode="prepend">`...

## Rendering

> It's strongly advised that lcdfilter, if available, is used with sub-pixel rendering. It comes in different varieties but the default (11-lcdfilter-default.conf) should be appropriate for all common fonts.
>
> https://wiki.gentoo.org/wiki/Fontconfig/zh-cn

删除默认配置
```
$ sudo rm /etc/fonts/conf.d/10*
$ sudo rm /etc/fonts/conf.d/11*
```
使用
```
$ sudo ln -s /usr/share/fontconfig/conf.avail/10-hinting-slight.conf /etc/fonts/conf.d/                              
$ sudo ln -s /usr/share/fontconfig/conf.avail/10-sub-pixel-rgb.conf /etc/fonts/conf.d/                      
$ sudo ln -s /usr/share/fontconfig/conf.avail/11-lcdfilter-default.conf /etc/fonts/conf.d/ 
```
重载配置
```
$ fc-cache --force --verbose (or -fv)
```

## User Config

全局配置仅专注提供泛用的最低保障，而美化、个性化等不泛用的需求在用户层面配置。

```
$ mkdir -p ~/.config/fontconfig/conf.d
$ cd ~/.config/fontconfig/conf.d
```

## Symbols Nerd Font

```
$ paru -S ttf-nerd-fonts-symbols-mono
```

## Emoji

ttf-twemoji 是 CBDT/CBLC 格式。ttf-twemoji-color 是 SVGinOT 格式。

根据 ArchWiki 的[表格](https://wiki.archlinux.org/title/Fonts#Emoji_and_symbols) (29 Jan 2022)，SVGinOT 只有 Mozilla 支持。考虑兼容性，选择 bitmap 格式的 ttf-twemoji。

```
$ paru -S ttf-twemoji
```
官方参考全局配置：
```xml
/usr/share/fontconfig/conf.avail/75-twemoji.conf
------------------------------------------------
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>

    <!--
    Treat this file as a reference and modify as necessary if you are not satisfied with the results.


    This config attempts to guarantee that colorful emojis from Twemoji will be displayed,
    no matter how badly the apps and websites are written.

    It uses a few different tricks, some of which introduce conflicts with other fonts.
    -->


    <!--
    This adds a generic family 'emoji',
    aimed for apps that don't specify specific font family for rendering emojis.
    -->
    <match target="pattern">
        <test qual="any" name="family"><string>emoji</string></test>
        <edit name="family" mode="assign" binding="same"><string>Twemoji</string></edit>
    </match>

    <!--
    This adds Twemoji as a final fallback font for the default font families.
    In this case, Twemoji will be selected if and only if no other font can provide a given symbol.

    Note that usually other fonts will have some glyphs available (e.g. Symbola or DejaVu fonts),
    causing some emojis to be rendered in black-and-white.
    -->
    <match target="pattern">
        <test name="family"><string>sans</string></test>
        <edit name="family" mode="append"><string>Twemoji</string></edit>
    </match>

    <match target="pattern">
        <test name="family"><string>serif</string></test>
        <edit name="family" mode="append"><string>Twemoji</string></edit>
    </match>

    <match target="pattern">
        <test name="family"><string>sans-serif</string></test>
        <edit name="family" mode="append"><string>Twemoji</string></edit>
    </match>

    <match target="pattern">
        <test name="family"><string>monospace</string></test>
        <edit name="family" mode="append"><string>Twemoji</string></edit>
    </match>

    <!--
    If other fonts contain emoji glyphs, they could interfere and make some emojis rendered in wrong font (often in black-and-white).
    For example, DejaVu Sans contains black-and-white emojis, which we can remove using the following trick:
    -->
    <match target="scan">
        <test name="family" compare="contains">
            <string>DejaVu</string>
        </test>
        <edit name="charset" mode="assign" binding="same">
            <minus>
                <name>charset</name>
                <charset>
                    <range>
                        <int>0x1f600</int>
                        <int>0x1f640</int>
                    </range>
                </charset>
            </minus>
        </edit>
    </match>

    <!--
    Recognize legacy ways of writing EmojiOne family name.
    -->
    <match target="pattern">
        <test qual="any" name="family"><string>EmojiOne</string></test>
        <edit name="family" mode="assign" binding="same"><string>Twemoji</string></edit>
    </match>

    <match target="pattern">
        <test qual="any" name="family"><string>Emoji One</string></test>
        <edit name="family" mode="assign" binding="same"><string>Twemoji</string></edit>
    </match>

    <match target="pattern">
        <test qual="any" name="family"><string>EmojiOne Color</string></test>
        <edit name="family" mode="assign" binding="same"><string>Twemoji</string></edit>
    </match>

    <match target="pattern">
        <test qual="any" name="family"><string>EmojiOne Mozilla</string></test>
        <edit name="family" mode="assign" binding="same"><string>Twemoji</string></edit>
    </match>

    <!--
    Use Twemoji when other popular fonts are being specifically requested.

    It is quite common that websites would only request Apple and Google emoji fonts.
    These aliases will make Twemoji be selected in such cases to provide good-looking emojis.

    This obviously conflicts with other emoji fonts if you have them installed.
    -->
    <match target="pattern">
        <test qual="any" name="family"><string>Apple Color Emoji</string></test>
        <edit name="family" mode="assign" binding="same"><string>Twemoji</string></edit>
    </match>

    <match target="pattern">
        <test qual="any" name="family"><string>Segoe UI Emoji</string></test>
        <edit name="family" mode="assign" binding="same"><string>Twemoji</string></edit>
    </match>

    <match target="pattern">
        <test qual="any" name="family"><string>Segoe UI Symbol</string></test>
        <edit name="family" mode="assign" binding="same"><string>Twemoji</string></edit>
    </match>

    <match target="pattern">
        <test qual="any" name="family"><string>Noto Color Emoji</string></test>
        <edit name="family" mode="assign" binding="same"><string>Twemoji</string></edit>
    </match>

    <match target="pattern">
        <test qual="any" name="family"><string>NotoColorEmoji</string></test>
        <edit name="family" mode="assign" binding="same"><string>Twemoji</string></edit>
    </match>

    <match target="pattern">
        <test qual="any" name="family"><string>Android Emoji</string></test>
        <edit name="family" mode="assign" binding="same"><string>Twemoji</string></edit>
    </match>

    <match target="pattern">
        <test qual="any" name="family"><string>Noto Emoji</string></test>
        <edit name="family" mode="assign" binding="same"><string>Twemoji</string></edit>
    </match>

    <match target="pattern">
        <test qual="any" name="family"><string>Twitter Color Emoji</string></test>
        <edit name="family" mode="assign" binding="same"><string>Twemoji</string></edit>
    </match>

    <match target="pattern">
        <test qual="any" name="family"><string>JoyPixels</string></test>
        <edit name="family" mode="assign" binding="same"><string>Twemoji</string></edit>
    </match>

    <match target="pattern">
        <test qual="any" name="family"><string>Twemoji Mozilla</string></test>
        <edit name="family" mode="assign" binding="same"><string>Twemoji</string></edit>
    </match>

    <match target="pattern">
        <test qual="any" name="family"><string>TwemojiMozilla</string></test>
        <edit name="family" mode="assign" binding="same"><string>Twemoji</string></edit>
    </match>

    <match target="pattern">
        <test qual="any" name="family"><string>EmojiTwo</string></test>
        <edit name="family" mode="assign" binding="same"><string>Twemoji</string></edit>
    </match>

    <match target="pattern">
        <test qual="any" name="family"><string>Emoji Two</string></test>
        <edit name="family" mode="assign" binding="same"><string>Twemoji</string></edit>
    </match>

    <match target="pattern">
        <test qual="any" name="family"><string>EmojiSymbols</string></test>
        <edit name="family" mode="assign" binding="same"><string>Twemoji</string></edit>
    </match>

    <match target="pattern">
        <test qual="any" name="family"><string>Symbola</string></test>
        <edit name="family" mode="assign" binding="same"><string>Twemoji</string></edit>
    </match>
</fontconfig>
```

## 吴语字

Problem: Noto 不包含[吴语字](https://zh.wikipedia.org/wiki/吴语字)

Test: https://wuu.wikipedia.org

## 异体字

[Solution](https://wanweibaike.net/wiki-Unicode控制字符): 
- 用 lang tag 指定字形。已过时。
- [异体字选择器](https://zh.wikipedia.org/wiki/異體字選擇器)

## lang tag

Problem: 习惯用法 zh，zh-CN 等不合规范。规范写法多数软件又不支持 https://www.zhihu.com/question/20797118

## Config
```xml
60-common-generic-font-families-inidividual.conf
------------------------------------------------
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
<!--
    Generic font families are a fallback mechanism.
-->
    <description>Set inidividual fallback mechanism of generic font families.</description>
    
    <match target="pattern">
        <test qual="any" name="family">
            <string>system-ui</string>
	    </test>
	    <edit name="family" mode="prepend" binding="strong">
            <string>sans-serif</string>
	    </edit>
    </match>
    
    <match target="pattern">
        <test qual="any" name="family">
            <string>serif</string>
	    </test>
	    <edit name="family" mode="prepend" binding="strong">
            <string>Noto Serif</string>
            <string>Noto Serif CJK SC</string>
            <string>Symbols Nerd Font</string>
            <string>Twemoji</string>
	    </edit>
    </match>
    
    <match target="pattern">
        <test qual="any" name="family">
            <string>sans-serif</string>
	    </test>
	    <edit name="family" mode="prepend" binding="strong">
            <string>Noto Sans</string>
            <string>Noto Sans CJK SC</string>
            <string>Symbols Nerd Font</string>
            <string>Twemoji</string>
	    </edit>
    </match>
    
    <match target="pattern">
        <test qual="any" name="family">
            <string>monospace</string>
	    </test>
	    <edit name="family" mode="prepend" binding="strong">
            <string>Iosevka Term</string>
            <string>Noto Sans Mono CJK SC</string>
            <string>Symbols Nerd Font</string>
            <string>Twemoji</string>
	    </edit>
    </match>

</fontconfig>

```

```xml
70-replace-noto-cjk-sc-as-per-lang.conf
---------------------------------------
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
<!--
    Noto CJK has five variations: SC, TC, HK, JP, and KR
-->
    <description>Replace default Noto CJK SC with TC/HK/JP/KR as per lang.</description> 


<!--
    compare="contains" compares strings from the beignning.
    Moreover, <match><test> does not support OR operation.
    However, as per international standardisation, Chinese has many first subtags which stand for primary language.
    Therefore, although the second subtag, script, and the third subtag, region, are of interest,
    <match><test> pattern has to be repeated for every (language)-(script) and (language)-(script)-(region) combination, which is tedious.
-->


    <!-- Noto Serif CJK does not have HK variation -->
    <match target="pattern">
        <test name="lang" compare="contains">
	    <string>-Hant-HK</string>  <!-- Sematic of this block, {lang} contains {string}, implies this should work but does not, which is strange. -->
        </test>
        <test qual="any" name="family">
            <string>Noto Sans CJK SC</string>
	    </test>
	    <edit name="family" mode="assign" binding="strong">
            <string>Noto Sans CJK HK</string>
	    </edit>
    </match>
    <match target="pattern">
        <test name="lang" compare="contains">
            <string>-Hant-HK</string>
        </test>
        <test qual="any" name="family">
            <string>Noto Sans Mono CJK SC</string>
	    </test>
	    <edit name="family" mode="assign" binding="strong">
            <string>Noto Sans Mono CJK HK</string>
	    </edit>
    </match>


    <!-- Except (language)-Hant-HK uses Noto Sans CJK HK and Noto Sans Mono CJK HK, other (language)-Hant use Noto * CJK TC -->
    <match target="pattern">
        <test name="lang" compare="contains">
            <string>-Hant</string>
        </test>
        <test qual="any" name="family">
            <string>Noto Serif CJK SC</string>
	    </test>
	    <edit name="family" mode="assign" binding="strong">
            <string>Noto Serif CJK TC</string>
	    </edit>
    </match>
    <match target="pattern">
        <test name="lang" compare="contains">
            <string>-Hant</string>
        </test>
        <test qual="any" name="family">
            <string>Noto Sans CJK SC</string>
	    </test>
	    <edit name="family" mode="assign" binding="strong">
            <string>Noto Sans CJK TC</string>
	    </edit>
    </match>
    <match target="pattern">
        <test name="lang" compare="contains">
            <string>-Hant</string>
        </test>
        <test qual="any" name="family">
            <string>Noto Sans Mono CJK SC</string>
	    </test>
	    <edit name="family" mode="assign" binding="strong">
            <string>Noto Sans Mono CJK TC</string>
	    </edit>
    </match>


    <match target="pattern">
        <test name="lang" compare="eq">
            <string>ja</string>
        </test>
        <test qual="any" name="family">
            <string>Noto Serif CJK SC</string>
	    </test>
	    <edit name="family" mode="assign" binding="strong">
            <string>Noto Serif CJK JP</string>
	    </edit>
    </match>
    <match target="pattern">
        <test name="lang" compare="eq">
            <string>ja</string>
        </test>
        <test qual="any" name="family">
            <string>Noto Sans CJK SC</string>
	    </test>
	    <edit name="family" mode="assign" binding="strong">
            <string>Noto Sans CJK JP</string>
	    </edit>
    </match>
    <match target="pattern">
        <test name="lang" compare="eq">
            <string>ja</string>
        </test>
        <test qual="any" name="family">
            <string>Noto Sans Mono CJK SC</string>
	    </test>
	    <edit name="family" mode="assign" binding="strong">
            <string>Noto Sans Mono CJK JP</string>
	    </edit>
    </match>


    <match target="pattern">
        <test name="lang" compare="eq">
            <string>ko</string>
        </test>
        <test qual="any" name="family">
            <string>Noto Serif CJK SC</string>
	    </test>
	    <edit name="family" mode="assign" binding="strong">
            <string>Noto Serif CJK KR</string>
	    </edit>
    </match>
    <match target="pattern">
        <test name="lang" compare="eq">
            <string>ko</string>
        </test>
        <test qual="any" name="family">
            <string>Noto Sans CJK SC</string>
	    </test>
	    <edit name="family" mode="assign" binding="strong">
            <string>Noto Sans CJK KR</string>
	    </edit>
    </match>
    <match target="pattern">
        <test name="lang" compare="eq">
            <string>ko</string>
        </test>
        <test qual="any" name="family">
            <string>Noto Sans Mono CJK SC</string>
	    </test>
	    <edit name="family" mode="assign" binding="strong">
            <string>Noto Sans Mono CJK KR</string>
	    </edit>
    </match>

</fontconfig>
```

```xml
71-fuck-compare-contains-weird-behaviour.conf
---------------------------------------------
<?xml version="1.0"?>
<!DOCTYPE fontconfig SYSTEM "fonts.dtd">
<fontconfig>
<!--
    compare="contains" compares two strings from the beignning.
    Hence, <match><test> pattern has to be repeated for lang tags which have different first two/three subtag combination.
-->
    <description>Replace default Noto CJK SC by enumerating lang tags which have different first two/three subtag combination</description>



    <match target="pattern">
        <test name="lang" compare="contains">
            <string>zh-Hant-HK</string>
        </test>
        <test qual="any" name="family">
            <string>Noto Sans CJK SC</string>
	    </test>
	    <edit name="family" mode="assign" binding="strong">
            <string>Noto Sans CJK HK</string>
	    </edit>
    </match>
    <match target="pattern">
        <test name="lang" compare="contains">
            <string>zh-Hant-HK</string>
        </test>
        <test qual="any" name="family">
            <string>Noto Sans Mono CJK SC</string>
	    </test>
	    <edit name="family" mode="assign" binding="strong">
            <string>Noto Sans Mono CJK HK</string>
	    </edit>
    </match>



    <match target="pattern">
        <test name="lang" compare="contains">
            <string>zh-Hant</string>
        </test>
        <test qual="any" name="family">
            <string>Noto Serif CJK SC</string>
	    </test>
	    <edit name="family" mode="assign" binding="strong">
            <string>Noto Serif CJK TC</string>
	    </edit>
    </match>
    <match target="pattern">
        <test name="lang" compare="contains">
            <string>zh-Hant</string>
        </test>
        <test qual="any" name="family">
            <string>Noto Sans CJK SC</string>
	    </test>
	    <edit name="family" mode="assign" binding="strong">
            <string>Noto Sans CJK TC</string>
	    </edit>
    </match>
    <match target="pattern">
        <test name="lang" compare="contains">
            <string>zh-Hant</string>
        </test>
        <test qual="any" name="family">
            <string>Noto Sans Mono CJK SC</string>
	    </test>
	    <edit name="family" mode="assign" binding="strong">
            <string>Noto Sans Mono CJK TC</string>
	    </edit>
    </match>
    


    <match target="pattern">
        <test name="lang" compare="contains">
            <string>zh-HK</string>
        </test>
        <test qual="any" name="family">
            <string>Noto Sans CJK SC</string>
	    </test>
	    <edit name="family" mode="assign" binding="strong">
            <string>Noto Sans CJK HK</string>
	    </edit>
    </match>
    <match target="pattern">
        <test name="lang" compare="contains">
            <string>zh-HK</string>
        </test>
        <test qual="any" name="family">
            <string>Noto Sans Mono CJK SC</string>
	    </test>
	    <edit name="family" mode="assign" binding="strong">
            <string>Noto Sans Mono CJK HK</string>
	    </edit>
    </match>

    <match target="pattern">
        <test name="lang" compare="contains">
            <string>zh-TW</string>
        </test>
        <test qual="any" name="family">
            <string>Noto Serif CJK SC</string>
	    </test>
	    <edit name="family" mode="assign" binding="strong">
            <string>Noto Serif CJK TC</string>
	    </edit>
    </match>
    <match target="pattern">
        <test name="lang" compare="contains">
            <string>zh-TW</string>
        </test>
        <test qual="any" name="family">
            <string>Noto Sans CJK SC</string>
	    </test>
	    <edit name="family" mode="assign" binding="strong">
            <string>Noto Sans CJK TC</string>
	    </edit>
    </match>
    <match target="pattern">
        <test name="lang" compare="contains">
            <string>zh-TW</string>
        </test>
        <test qual="any" name="family">
            <string>Noto Sans Mono CJK SC</string>
	    </test>
	    <edit name="family" mode="assign" binding="strong">
            <string>Noto Sans Mono CJK TC</string>
	    </edit>
    </match>

    <match target="pattern">
        <test name="lang" compare="contains">
            <string>zh-MO</string>
        </test>
        <test qual="any" name="family">
            <string>Noto Serif CJK SC</string>
	    </test>
	    <edit name="family" mode="assign" binding="strong">
            <string>Noto Serif CJK TC</string>
	    </edit>
    </match>
    <match target="pattern">
        <test name="lang" compare="contains">
            <string>zh-MO</string>
        </test>
        <test qual="any" name="family">
            <string>Noto Sans CJK SC</string>
	    </test>
	    <edit name="family" mode="assign" binding="strong">
            <string>Noto Sans CJK TC</string>
	    </edit>
    </match>
    <match target="pattern">
        <test name="lang" compare="contains">
            <string>zh-MO</string>
        </test>
        <test qual="any" name="family">
            <string>Noto Sans Mono CJK SC</string>
	    </test>
	    <edit name="family" mode="assign" binding="strong">
            <string>Noto Sans Mono CJK TC</string>
	    </edit>
    </match>

</fontconfig>
```
---
title: "在 LaTeX 建立中文文件"
date: 2023-01-22T23:26:14+08:00
draft: false
slug: "latex-tutorial-part-ctex"
show_toc: true
---

## 引言 {#introduction}

目前我在這個寒假裡，不去過年，也不去弄期限即將在即的專研跑來研究 LaTeX，在搜尋引擎搜尋了一番得到的答案是，目前在 LaTeX 文件中使用中文有兩種辦法，一種是引入 `xeCJK` 套件，另一種方法是使用 `cTeX` 套件。

### `xeCJK`
- CTAN 頁面：[CTAN: Package xecjk](https://www.ctan.org/pkg/xecjk)

`xeCJK` 雖然可以完美達成使用中文的需求，但是如果要對文件做進階的設定，例如：圖目錄、表目錄、又或是各個階層的字體樣式，需要自行設定的東西實在是太多了，而且網路上能找到的資源較少，不如直接站在開發已久的巨人的肩膀上——cTeX。

### `cTeX`

- CTAN 頁面：[CTAN: Package ctex](https://www.ctan.org/pkg/ctex)

`cTeX` 的依賴套件中就包含 `xeCJK` 而且完美的解決在 LaTeX 中使用中文的問題，我們只需要修改字型與文本內容即可。因為該套件是針對簡體中文的使用者設計，所以只需要將內定的物件名稱（例如「目录」替換成「目錄」）翻譯成繁體中文。還有將 `cTeX` 使用的字體由簡體中文改成繁體中文，因為字體的不同可能會造成缺字的問題。

## 繁體中文字體支援 {#add-traditional-chinese-support}

雖然現階段 `cTeX` 會自動根據作業系統載入對應的字體，不過預設的字體是簡體中文的，這裡我有寫了一個專沒給 XeLaTeX 編譯用的繁體中文的版本。

根據你的作業系統複製對應的程式碼，將檔案儲存成 `ctex-fontset-<name>.def` 並與要編譯的 tex 檔案放在同一個資料夾。

> \<name\> 請替換成第一行 `\ProvidesExplFile` {} 中的檔案名稱。

{{<details title = "windows 版本">}}

```latex
\ProvidesExplFile{ctex-fontset-windows-tc.def}{\ExplFileDate}{0.0.1}{\ExplFileDescription}
\ctex_fontset_case:nnn
  { }
  { }
  {
    \setCJKmainfont   { MingLiU } [ BoldFont = Microsoft~JhengHei , ItalicFont = DFKai-SB ]
    \setCJKsansfont   { Microsoft~JhengHei } [ BoldFont = *~Bold ]
    \setCJKmonofont   { MingLiU~Regular }
    \setCJKfamilyfont { ming    } { MingLiU~Regular    }
    \setCJKfamilyfont { zhhei   } { Microsoft~JhengHei }
    \setCJKfamilyfont { zhkai   } { DFKai-SB           }
  }
\NewDocumentCommand \ming     { } { \CJKfamily { ming  } }
\NewDocumentCommand \hei      { } { \CJKfamily { zhhei } }
\NewDocumentCommand \kai      { } { \CJKfamily { zhkai } }

```
{{</details>}}

{{<details title="mac 版本">}}
將程式碼儲存成 `ctex-fontset-macnew-tc.def` 並與 tex 檔案放在同一個資料夾。

```latex
\ProvidesExplFile{ctex-fontset-macnew-tc.def}{\ExplFileDate}{0.0.1}{\ExplFileDescription}
\ctex_fontset_case:nnnn
  { \ctex_fontset_error:n { mac } }
  { }
  { }
  {
    \setCJKmainfont { Songti~TC }
      [
        BoldFont       = Songti~TC~Bold,
        ItalicFont     = Kaiti~TC,
        BoldItalicFont = Kaiti~TC~Bold
      ]
    \setCJKsansfont { PingFang~TC }
    \setCJKmonofont { Lantinghei~TC }
    \setCJKfamilyfont { zhsong } { Songti~TC~Regular } [ BoldFont = Songti~TC~Bold ]
    \setCJKfamilyfont { zhhei  } { Heiti~TC~Light    } [ BoldFont = Heiti~TC~Medium ]
    \setCJKfamilyfont { zhpf   } { PingFang~TC       }
    \setCJKfamilyfont { zhkai  } { Kaiti~TC          } [ BoldFont = Kaiti~TC~Bold ]
    \setCJKfamilyfont { zhli   } { Baoli~TC          }
    \setCJKfamilyfont { zhyuan } { Yuanti~TC~Light   } [ BoldFont = Yuanti~SC~Regular ]
  }
\NewDocumentCommand \song     { } { \CJKfamily { zhsong } }
\NewDocumentCommand \hei      { } { \CJKfamily { zhhei  } }
\NewDocumentCommand \fangsong { } { \CJKfamily { zhfs   } }
\NewDocumentCommand \kai      { } { \CJKfamily { zhkai  } }
\NewDocumentCommand \lishu    { } { \CJKfamily { zhli   } }
\NewDocumentCommand \yuan     { } { \CJKfamily { zhyuan } }
\NewDocumentCommand \pingfang { } { \CJKfamily { zhpf   } }
  
```
{{</details>}}

添加完成後，因為要使用自訂的字體，所以要先將自動抓取簡體中文字體的功能關閉。我們可以添加 `fontset = none` 的選項到 `cTeX` 的 class 中避免字體自動載入。

```latex
\documentclass[
    fontset = none,
    % 其他選項...
]{ctexart}
```

接著用 `cTeX` 的指令 `\ctexset` 設定預先定義的字體，一樣是放在 tex 檔案 preamable 的位置：

```latex
\ctexset{
    % 曲檔案名稱 ctex-fontset-<name>.def 中 name 的部分為預先定義的字體名稱
    % 例如 ctex-fontset-macnew-tc.def 就取 macnew-tc 為字體名稱
    fontset = macnew-tc,
    % 其他設定...
}
```

測試用的中文文件，記得要改 `<name>` 的部分，且使用 XeLaTeX 編譯：

```latex
\documentclass[fontset = none]{ctexart}
\ctexset{fontset = <name>}
\begin{document}
繁體中文測試。
\end{document}
```

<!-- TODO: 編譯完成的結果： -->
<!-- insert image here -->



## `cTeX` 套件的文件類型 {#ctex-cls}

這裡介紹著 `cTeX` 套件內建的文檔類型，預設文件類型會根據中文重新設定標題名稱、樣式、字體大小等等，可以直接使用它。

- `ctexart`：article 的中文化版本，適合一般短篇幅文章。
- `ctexrep`：report 的中文化版本，適合一般中篇幅文章。
- `ctexbook`：book 的中文化版本，適合一般長篇幅書籍。
- `ctexbeamer`：beamer 的中文化版本，被用來做投影片。

使用時，直接在 `\documentclass` 中設置即可。

如果不使用預設文件類型又想使用 `cTeX` 套件的話，需要用 `\usepackage` 手動將套件載入，但是預設不會開啟章節標題設定的功能，需要加入選項 `heading = true` 開啟。

例如：

```latex
\usepackage[ heading = true ]{ctex}
```

## 字體大小設定 {#font-size-config}

如果在套用[文件類型](#ctex-cls)時，沒有指令字體大小的話，預設是使用中國的「五号」字。套用後，文件的基礎字體大小會使用 10.5 pt 作為基礎字體大小，也就是套用下表五号的 `\normalsize`。

| 指令            | 五号 | 小四 | 10pt | 11pt | 12pt |
| :-------------- | ---: | ---: | ---: | ---: | ---: |
| `\tiny`         |  5.5 |  6.5 |    5 |    6 |    6 |
| `\scriptsize`   |  6.5 |  7.5 |    7 |    8 |    8 |
| `\footnotesize` |  7.5 |    9 |    8 |    9 |   10 |
| `\small`        |    9 | 10.5 |    9 |   10 |   11 |
| `\normalsize`   | 10.5 |   12 |   10 |   11 |   12 |
| `\large`        |   12 |   15 |   12 |   12 |   14 |
| `\Large`        |   15 |   18 |   14 |   14 |   17 |
| `\LARGE`        |   18 |   22 |   17 |   17 |   20 |
| `\huge`         |   22 |   24 |   20 |   20 |   25 |
| `\Huge`         |   26 |   26 |   25 |   25 |   25 |

如果要將基礎字體大小改為 12pt，要在套用文件類型時加入選項 `12pt`。

```latex
\documentclass[ 12pt ]{ctexrep}
```

如果要使用基礎字體大小為上列 `\normalsize` 以外的數字，可以搭配套件 `extsizes`。

### `extsizes`

- CTAN 頁面：[CTAN: Package extsizes](https://www.ctan.org/pkg/extsizes)

套件中提供了 8pt、9pt、10pt、11pt、12pt、14pt、17pt 與 20pt 可供使用。但，如果要與 `extsizes` 一起使用，需要使用上面提到的[手動載入 cTeX 套件](#ctex-cls)的方式，將它載入。

例如我想要讓我整個文件的基礎字體大小為 14pt，我需要這麼做：

```latex
\doumentclass[ 14pt ]{extreport}
\usepackage[ heading = true ]{ctex}
```



## 文件標題預設值翻譯 {#translate-default-value-to-tradition-chinese}

這裡會介紹各個中文標題中文化的部分，下表是可以設定的選項。可以根據自己的需求在 `\ctexset` 中使用 `<key> = <value>` 的方式設定。

| 標題位置           | 選項             |   預設值 |
| :----------------- | :--------------- | -------: |
| 目錄               | `contentsname`   |     目录 |
| 圖目錄             | `listfigurename` |     插图 |
| 表目錄             | `listtablename`  |     表格 |
| 圖標號             | `figurename`     |       图 |
| 表標號             | `tablename`      |       表 |
| 摘要               | `abstractname`   |     摘要 |
| 索引               | `indexname`      |     索引 |
| 附錄               | `appendixname`   |     附录 |
| 參考文獻           | `bibname`        | 参考文献 |
| 證明               | `proofname`      |     证明 |
| 參考文獻（beamer） | `refname`        | 参考文献 |
| 方程式             | `algorithmname`  |     算法 |
| 接續字樣（beamer） | `continuation`   |   （续） |

例如我們想要將目錄與圖目錄翻譯成中文，可以這樣做：

```latex
\ctexset{
    contentsname = 目錄,
    listfigurename = 圖目錄
}
```

## 文件標題樣式設定 {#document-style}

### 編號

#### `.../name`

- `name = {<前綴詞>, <後綴詞>}`
- `name = {<前綴詞>}`

如過要對章節名稱設定，可以使用上面兩種方式設定。一種是同時設定前綴詞與後綴詞，而另外一種只設前綴詞。`name` 的父階層需指定章節標題名稱。

預設標題名稱所對應的預設值如下表所示，其中 `scheme = chinese` 的檔案選項只要套用 [cTeX 的文件類型](#ctex-cls)就會自動套用，而 `scheme = plain` 是 LaTeX 文件類型的預設值。


| 標題名稱             | scheme = chinese | scheme = plain          |
|----------------------|------------------|-------------------------|
| part                 | {第,部分}        | `{\partname\space}`     |
| chapter              | {第,章}          | {\chaptername\space}    |
| section（beamer）    | {}               | {\sectionname\space}    |
| section              | 同右             | {}                      |
| subsection（beamer） | {}               | {\subsectionname\space} |
| subsection           | 同右             | {}                      |
| subsubsection        | 同右             | {}                      |
| paragraph            | 同右             | {}                      |
| subparagraph         | 同右             | {}                      |

> 補充說明：
> `\partname` 原為 Part；
> `\chaptername` 原為 Chapter；
> `\sectionname` 原為 `\translate{Section}`；
> `\subsectionname` 原為 `\translate{Subsection}`。

例如：

```latex
\ctexset{
  section = {
    name = {第,節}
  }
}
```

會使章節階層為 `\section` 標題套用「第 n 節」的格式設定。

#### `.../number`

- `/number = {<輸出數字的指令>}`

變更輸出數字的格式。輸出數字的指令可以參考[這裡](https://www.overleaf.com/learn/latex/Counters#Accessing_and_printing_counter_values)。而載入 cTeX 時，也會自動載入 `zhnumber` 套件，此時新增了一個輸出數字指令 `\chinese`，可以幫助你輸出對應計數器中的數字用中文表示。例如：

```latex
\ctexset{
  chapter = {
    name = {第,章},
    number = \chinese{chapter}
  }
}
```

假設目前章節的計數器為 3，上面的設定將會輸出「第三章」。

### 格式

#### `.../format`
#### `.../nameformat`
#### `.../numberformat`
#### `.../titleformat`

### 間距、縮排

#### `.../beforeskip`
#### `.../aftereskip`
#### `.../fixskip`

### 目錄、附錄

#### `tocdepth`

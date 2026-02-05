---
title: LunaTranslator制anki卡组的个人配置改进
publishDate: 2026-02-05 18:11:00
description: '玩Galgame学习日语'
tags:
  - Janpanese
  - Galgame
  - Anki
heroImage: { src: './Luna.png', color: '#010409' }
language: '中文'
---

本章内容建立在成功安装[LunaTranslator](https://github.com/HIllya51/LunaTranslator)，并可以正常添加anki卡片的基础上。

## 1. 主要目标
* 卡片美化
* 例句的汉字也标上平假名（不然初学者真有点听不出来）
* 附上单词、语法的简单解释，不然语法不好的我可能无法做到可理解输入(comprehensive input)

## 2. 卡片改变
创建新字段：
1. `Sentence_With_Reading`
2. `Word_Analysis`
3. `Grammar_Note`

修改正面、背面、样式代码：


{{< tabs "anki-template" >}}

  {{< tab "正面模板" >}}
    ```html
    <div class="centerdiv">
      {{Image}}
    </div>
    <div class="mobile example-div">
      {{Sentence_With_Reading}}
    </div>
    {{Audio}}
    ```
  {{< /tab >}}

  {{< tab "背面模板" >}}
    ```html
    {{FrontSide}}
    <hr id="answer">
    <div class="ai-analysis-container">
       <div class="class-analysis-box">
          </div>
    </div>
    ```
  {{< /tab >}}

  {{< tab "样式 (CSS)" >}}
    ```css
    /* 这里放 CSS 代码 */
    .card {
       font-family: arial;
    }
    ```
  {{< /tab >}}

{{< /tabs >}}
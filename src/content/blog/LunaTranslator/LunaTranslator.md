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

{{< tabs groupId="anki-template" >}}

{{% tab name="正面模板" %}}
```html
<div class="centerdiv">
    {{Image}} 
</div>

<div class="mobile example-div">
    {{Sentence_With_Reading}}
</div>

{{Audio}}
```
{{% /tab %}}

{{% tab name="背面模板" %}}
```html
{{FrontSide}}

<hr id="answer">

<div class="ai-analysis-container">
    <div class="analysis-box">
        <div class="analysis-title">📖 单词解析</div>
        {{Word_Analysis}}
    </div>

    <div class="analysis-box">
        <div class="analysis-title">💡 语法分析</div>
        {{Grammar_Note}}
    </div>
</div>
```
{{% /tab %}}

{{% tab name="样式 (CSS)" %}}
```css
/* 卡片基础字体与背景 */
.card {
    font-family: "BIZ UDGothic", "Microsoft YaHei", sans-serif;
    background-color: #ffffff;
    color: #333;
    font-size: 20px;
    text-align: center;
}

/* 隐藏字段样式 */
.hide-style { display: none; height: 0; width: 0; }

/* 布局样式 */
.centerdiv { display: flex; justify-content: center; align-items: center; flex-direction: column; }
.centerdiv img { max-width: 100%; height: auto; border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1); }

/* 例句样式 */
.example-div {
    text-align: center;
    font-size: 24px;
    padding: 5px;
    line-height: 2.2;
}

/* 高亮与注音 */
.target-highlight { color: #e55039; font-weight: bold; border-bottom: 2px solid #e55039; }
ruby { ruby-align: center; }
rt { font-size: 0.6em; color: #555; user-select: none; }

/* --- AI 分析区域 --- */
.ai-analysis-container {
    margin-top: 20px;
    padding: 10px;
    background-color: #fafafa;
    border-radius: 10px;
    text-align: left;
}
.analysis-box {
    margin-bottom: 15px;
    background-color: #f8f9fa;
    border-radius: 8px;
    padding: 10px;
    border-left: 5px solid #4a69bd;
}
.analysis-title { font-weight: bold; color: #4a69bd; margin-bottom: 5px; font-size: 0.9em; }

/* --- Night Mode (深色模式) 适配 --- */
.night_mode .card { background-color: #2f3542; color: #e0e0e0; }
.night_mode .target-highlight { color: #ff7675; border-bottom: 2px solid #ff7675; }
.night_mode rt { color: #a4b0be; }
.night_mode .ai-analysis-container { background-color: rgba(255, 255, 255, 0.05); }
.night_mode .analysis-box { background-color: #2f3542; border-left: 5px solid #74b9ff; color: #dfe4ea; }
.night_mode .analysis-title { color: #74b9ff; }
```
{{% /tab %}}

{{< /tabs >}}
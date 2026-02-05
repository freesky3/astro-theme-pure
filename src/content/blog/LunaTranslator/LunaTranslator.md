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


<div class="anki-code-block-wrapper">
  <style>
    /* 基础容器样式 */
    .anki-code-block-wrapper {
      font-family: Consolas, "Courier New", monospace;
      background-color: #1e1e1e; /* 背景色 */
      border-radius: 6px;
      overflow: hidden;
      border: 1px solid #333;
      margin: 1em 0;
      color: #d4d4d4;
    }
    
    /* 隐藏单选框 */
    .anki-tab-input {
      display: none;
    }

    /* 标签栏样式 */
    .anki-tab-header {
      display: flex;
      background-color: #252526;
      border-bottom: 1px solid #333;
    }

    .anki-tab-label {
      padding: 8px 16px;
      cursor: pointer;
      font-size: 13px;
      color: #969696;
      border-top: 2px solid transparent;
      transition: background 0.2s;
    }

    .anki-tab-label:hover {
      background-color: #2d2d2d;
      color: #fff;
    }

    /* 内容区域样式 */
    .anki-content-box {
      display: none; /* 默认隐藏 */
      padding: 15px;
      white-space: pre-wrap; /* 保留代码格式 */
      overflow-x: auto;
      line-height: 1.5;
      font-size: 14px;
    }

    /* --- 核心逻辑：根据选中的 Radio 显示对应内容 --- */
    
    /* 选中 Tab 1 (正面) */
    #tab-front-1:checked ~ .anki-tab-header .label-front {
      background-color: #1e1e1e;
      color: #fff;
      border-top-color: #007acc; /* 高亮色 */
    }
    #tab-front-1:checked ~ .content-front { display: block; }

    /* 选中 Tab 2 (背面) */
    #tab-back-1:checked ~ .anki-tab-header .label-back {
      background-color: #1e1e1e;
      color: #fff;
      border-top-color: #007acc;
    }
    #tab-back-1:checked ~ .content-back { display: block; }

    /* 选中 Tab 3 (样式) */
    #tab-style-1:checked ~ .anki-tab-header .label-style {
      background-color: #1e1e1e;
      color: #fff;
      border-top-color: #007acc;
    }
    #tab-style-1:checked ~ .content-style { display: block; }

  </style>

  <input type="radio" name="anki-group-1" id="tab-front-1" class="anki-tab-input" checked>
  <input type="radio" name="anki-group-1" id="tab-back-1" class="anki-tab-input">
  <input type="radio" name="anki-group-1" id="tab-style-1" class="anki-tab-input">

  <div class="anki-tab-header">
    <label for="tab-front-1" class="anki-tab-label label-front">正面模板</label>
    <label for="tab-back-1" class="anki-tab-label label-back">背面模板</label>
    <label for="tab-style-1" class="anki-tab-label label-style">样式 (CSS)</label>
  </div>

  <div class="anki-content-box content-front">
<span style="color: #569cd6;">&lt;div</span> <span style="color: #9cdcfe;">class</span>=<span style="color: #ce9178;">"card"</span><span style="color: #569cd6;">&gt;</span>
  {{Front}}
<span style="color: #569cd6;">&lt;/div&gt;</span>
  </div>

  <div class="anki-content-box content-back">
{{FrontSide}}

<span style="color: #569cd6;">&lt;hr</span> <span style="color: #9cdcfe;">id</span>=<span style="color: #ce9178;">"answer"</span><span style="color: #569cd6;">&gt;</span>

<span style="color: #569cd6;">&lt;div</span> <span style="color: #9cdcfe;">class</span>=<span style="color: #ce9178;">"meaning"</span><span style="color: #569cd6;">&gt;</span>
  {{Back}}
<span style="color: #569cd6;">&lt;/div&gt;</span>
  </div>

  <div class="anki-content-box content-style">
.card {
  font-family: arial;
  font-size: 20px;
  text-align: center;
  color: black;
  background-color: white;
}

.meaning {
  color: blue;
}
  </div>
</div>


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
    /* --- 基础容器样式 (亮色版) --- */
    .anki-code-block-wrapper {
      font-family: Consolas, "Courier New", monospace;
      background-color: #ffffff; /* 改为白色 */
      border-radius: 6px;
      overflow: hidden;
      border: 1px solid #e1e4e8; /* 浅灰边框 */
      margin: 1em 0;
      color: #24292e; /* 深色字体 */
      box-shadow: 0 2px 4px rgba(0,0,0,0.05);
    }
    
    /* 隐藏单选框 */
    .anki-tab-input {
      display: none;
    }

    /* --- 标签栏样式 --- */
    .anki-tab-header {
      display: flex;
      background-color: #f6f8fa; /* 浅灰背景 */
      border-bottom: 1px solid #e1e4e8;
    }

    .anki-tab-label {
      padding: 8px 16px;
      cursor: pointer;
      font-size: 13px;
      color: #586069; /* 次级文本颜色 */
      border-top: 3px solid transparent;
      border-bottom: 1px solid transparent;
      transition: all 0.2s;
      margin-bottom: -1px; /* 让选中的标签盖住底边框 */
    }

    .anki-tab-label:hover {
      color: #24292e;
      background-color: #eaeef2;
    }

    /* --- 内容区域样式 --- */
    .anki-content-box {
      display: none; /* 默认隐藏 */
      padding: 15px;
      white-space: pre-wrap;
      overflow-x: auto;
      line-height: 1.5;
      font-size: 14px;
      background-color: #ffffff;
    }

    /* --- 核心逻辑：选中状态 --- */
    
    /* 通用选中样式：背景变白，顶部有高亮条，底部遮挡边框 */
    input:checked + .anki-tab-label, /* 这种写法需要调整HTML结构，由于是兄弟选择器，保持原有逻辑如下 */
    #tab-front-1:checked ~ .anki-tab-header .label-front,
    #tab-back-1:checked ~ .anki-tab-header .label-back,
    #tab-style-1:checked ~ .anki-tab-header .label-style {
      background-color: #ffffff;
      color: #24292e;
      border-top-color: #fd8c73; /* 顶部高亮色，可改为 #007acc 蓝色 */
      border-bottom-color: #ffffff; /* 遮住底部分割线 */
      font-weight: bold;
    }

    /* 显示对应内容 */
    #tab-front-1:checked ~ .content-front { display: block; }
    #tab-back-1:checked ~ .content-back { display: block; }
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
<span style="color: #000080;">&lt;div</span> <span style="color: #008080;">class</span>=<span style="color: #a31515;">"card"</span><span style="color: #000080;">&gt;</span>
  {{Front}}
<span style="color: #000080;">&lt;/div&gt;</span>
  </div>

  <div class="anki-content-box content-back">
{{FrontSide}}

<span style="color: #000080;">&lt;hr</span> <span style="color: #008080;">id</span>=<span style="color: #a31515;">"answer"</span><span style="color: #000080;">&gt;</span>

<span style="color: #000080;">&lt;div</span> <span style="color: #008080;">class</span>=<span style="color: #a31515;">"meaning"</span><span style="color: #000080;">&gt;</span>
  {{Back}}
<span style="color: #000080;">&lt;/div&gt;</span>
  </div>

  <div class="anki-content-box content-style">
<span style="color: #800000;">.card</span> {
  <span style="color: #e00000;">font-family</span>: arial;
  <span style="color: #e00000;">font-size</span>: 20px;
  <span style="color: #e00000;">text-align</span>: center;
  <span style="color: #e00000;">color</span>: black;
  <span style="color: #e00000;">background-color</span>: white;
}

<span style="color: #800000;">.meaning</span> {
  <span style="color: #e00000;">color</span>: blue;
}
  </div>
</div>
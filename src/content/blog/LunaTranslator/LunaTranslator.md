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


<style>
  /* 容器样式 */
  .code-tab-container {
    background-color: #1e1e1e; /* 代码块背景色 */
    border-radius: 8px;
    overflow: hidden;
    font-family: Consolas, "Courier New", monospace;
    margin: 20px 0;
    border: 1px solid #333;
    color: #d4d4d4;
  }

  /* 顶部 Tab 栏 */
  .tab-header {
    display: flex;
    background-color: #252526;
    border-bottom: 1px solid #333;
  }

  /* Tab 按钮样式 */
  .tab-btn {
    background-color: inherit;
    border: none;
    outline: none;
    cursor: pointer;
    padding: 10px 20px;
    transition: 0.3s;
    color: #969696;
    font-size: 14px;
    font-weight: bold;
  }

  /* 鼠标悬停和激活状态 */
  .tab-btn:hover {
    background-color: #2d2d2d;
    color: #fff;
  }

  .tab-btn.active {
    background-color: #1e1e1e; /* 与内容背景一致 */
    color: #fff;
    border-top: 2px solid #007acc; /* 顶部高亮条 */
  }

  /* 内容区域 */
  .tab-content {
    display: none; /* 默认隐藏 */
    padding: 20px;
    white-space: pre; /* 保留代码格式 */
    overflow-x: auto; /* 代码过长可横向滚动 */
  }

  /* 激活的内容显示 */
  .tab-content.active {
    display: block;
  }
</style>

<div class="code-tab-container" id="myCodeTabs">
  <div class="tab-header">
    <button class="tab-btn active" onclick="switchTab(event, 'java-code')">Java</button>
    <button class="tab-btn" onclick="switchTab(event, 'cpp-code')">C++</button>
    <button class="tab-btn" onclick="switchTab(event, 'python-code')">Python</button>
  </div>

  <div id="java-code" class="tab-content active">
<span style="color: #569cd6;">public class</span> HelloWorld {
    <span style="color: #569cd6;">public static void</span> main(String[] args) {
        System.out.println(<span style="color: #ce9178;">"Hello Java"</span>);
    }
}
  </div>

  <div id="cpp-code" class="tab-content">
<span style="color: #c586c0;">#include</span> &lt;iostream&gt;

<span style="color: #569cd6;">int</span> main() {
    std::cout << <span style="color: #ce9178;">"Hello C++"</span> << std::endl;
    <span style="color: #c586c0;">return</span> 0;
}
  </div>

  <div id="python-code" class="tab-content">
<span style="color: #569cd6;">def</span> <span style="color: #dcdcaa;">main</span>():
    <span style="color: #dcdcaa;">print</span>(<span style="color: #ce9178;">"Hello Python"</span>)

<span style="color: #c586c0;">if</span> __name__ == <span style="color: #ce9178;">"__main__"</span>:
    main()
  </div>
</div>

<script>
  function switchTab(evt, langId) {
    // 1. 获取所有内容块和按钮
    var container = evt.currentTarget.closest('.code-tab-container');
    var contents = container.getElementsByClassName("tab-content");
    var buttons = container.getElementsByClassName("tab-btn");

    // 2. 隐藏所有内容
    for (var i = 0; i < contents.length; i++) {
      contents[i].classList.remove("active");
    }
    
    // 3. 移除所有按钮的激活状态
    for (var i = 0; i < buttons.length; i++) {
      buttons[i].classList.remove("active");
    }
    
    // 4. 显示当前选中的内容，并激活按钮
    container.querySelector("#" + langId).classList.add("active");
    evt.currentTarget.classList.add("active");
  }
</script>


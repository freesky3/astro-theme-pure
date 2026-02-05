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

<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>代码切换示例</title>
    <style>
        /* 基础样式设置 */
        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            padding: 20px;
            background-color: #f4f4f4; /* 页面背景设为灰色，以突显代码块的白色 */
        }

        /* 代码块主容器 */
        .code-block-container {
            background-color: #ffffff; /* 白色背景 */
            border: 1px solid #e0e0e0;
            border-radius: 6px;
            box-shadow: 0 2px 5px rgba(0,0,0,0.05);
            max-width: 800px;
            margin: 0 auto;
            overflow: hidden;
        }

        /* 顶部选项卡区域 */
        .tabs {
            display: flex;
            background-color: #f9f9f9; /* 选项卡栏稍微灰一点，区分内容 */
            border-bottom: 1px solid #e0e0e0;
        }

        /* 单个选项卡按钮 */
        .tab-btn {
            padding: 10px 20px;
            cursor: pointer;
            background: none;
            border: none;
            outline: none;
            font-size: 14px;
            font-weight: 600;
            color: #666;
            transition: all 0.2s ease;
            border-right: 1px solid #eee;
        }

        .tab-btn:hover {
            background-color: #ececec;
            color: #333;
        }

        /* 激活状态的选项卡 */
        .tab-btn.active {
            background-color: #ffffff;
            color: #007bff; /* 选中时的颜色 */
            border-bottom: 2px solid #ffffff; /* 盖住底部的边框线，实现融合效果 */
            margin-bottom: -1px;
            position: relative;
        }

        /* 代码内容区域 */
        .code-content {
            display: none; /* 默认隐藏所有内容 */
            padding: 20px;
            margin: 0;
            overflow-x: auto; /* 代码过长时横向滚动 */
        }

        /* 激活状态的代码内容 */
        .code-content.active {
            display: block; /* 只有激活的才显示 */
            animation: fadeIn 0.3s;
        }

        /* 模拟代码样式 */
        pre {
            margin: 0;
            font-family: 'Consolas', 'Monaco', 'Courier New', monospace;
            font-size: 14px;
            line-height: 1.5;
            color: #333;
        }

        /* 简单的语法高亮模拟 (为了演示效果) */
        .keyword { color: #d73a49; font-weight: bold; }
        .string { color: #032f62; }
        .comment { color: #6a737d; font-style: italic; }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
    </style>
</head>
<body>

    <div class="code-block-container">
        <div class="tabs">
            <button class="tab-btn active" onclick="switchTab(event, 'java')">Java</button>
            <button class="tab-btn" onclick="switchTab(event, 'cpp')">C++</button>
            <button class="tab-btn" onclick="switchTab(event, 'python')">Python</button>
        </div>

        <div id="java" class="code-content active">
<pre>
<span class="keyword">public class</span> HelloWorld {
    <span class="keyword">public static void</span> main(String[] args) {
        <span class="comment">// 打印 Hello World</span>
        System.out.println(<span class="string">"Hello, Java!"</span>);
    }
}
</pre>
        </div>

        <div id="cpp" class="code-content">
<pre>
<span class="keyword">#include</span> &lt;iostream&gt;

<span class="keyword">int</span> main() {
    <span class="comment">// 打印 Hello World</span>
    std::cout &lt;&lt; <span class="string">"Hello, C++!"</span> &lt;&lt; std::endl;
    <span class="keyword">return</span> 0;
}
</pre>
        </div>

        <div id="python" class="code-content">
<pre>
<span class="keyword">def</span> main():
    <span class="comment"># 打印 Hello World</span>
    print(<span class="string">"Hello, Python!"</span>)

<span class="keyword">if</span> __name__ == <span class="string">"__main__"</span>:
    main()
</pre>
        </div>
    </div>
    <script>
        function switchTab(evt, langName) {
            // 1. 获取所有的代码内容块，并隐藏它们
            var contents = document.getElementsByClassName("code-content");
            for (var i = 0; i < contents.length; i++) {
                contents[i].className = contents[i].className.replace(" active", "");
            }

            // 2. 获取所有的 Tab 按钮，并移除 active 状态
            var tabLinks = document.getElementsByClassName("tab-btn");
            for (var i = 0; i < tabLinks.length; i++) {
                tabLinks[i].className = tabLinks[i].className.replace(" active", "");
            }

            // 3. 显示当前点击的语言内容，并将按钮设为 active
            document.getElementById(langName).className += " active";
            evt.currentTarget.className += " active";
        }
    </script>
</body>
</html>
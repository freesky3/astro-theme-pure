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

1. 正面：

   ```
   <div class="hide-style">
       <div id="audio">{{audio_for_word}}</div>
       <div id="audio_sentence">{{audio_for_example_sentence}}</div>
   </div>
   
   <div class="centerdiv" onclick='playAudio("audio")'>
       <div class="ruby-div" id="word">{{ word }}</div>
       <div id="rubyword" class="ruby-div">{{ rubytextHtml }}</div>
   </div>
   
   <script>
       if (document.getElementById('rubyword').innerHTML.trim().length > 0) {
           document.getElementById('word').classList.add("hide-style");
       } else {
           document.getElementById('rubyword').classList.add("hide-style");
       }
   </script>
   
   <hr style="border: 0; border-top: 1px solid #ddd; margin: 10px 0;">
   
   <div id="example_sentence_container" class="example-div" onclick='playAudio("audio_sentence")'>
       {{#Sentence_With_Reading}}
           {{Sentence_With_Reading}}
       {{/Sentence_With_Reading}}
       
       {{^Sentence_With_Reading}}
           {{example_sentence}}
       {{/Sentence_With_Reading}}
   </div>
   
   <div id="image" class="centerdiv" style="margin-top: 10px;">
       {{screenshot}}
   </div>
   
   <script>
       function playAudio(audioId) {
           var audioDiv = document.getElementById(audioId);
           var audio = audioDiv.getElementsByTagName('*');
           if (audio.length > 0) {
               audio[0].click();
           }
       }
       // 检查图片是否为空
       function checkhide2(eid) {
           var emptyDiv = document.getElementById(eid);
           if (emptyDiv && emptyDiv.children.length == 0) {
               emptyDiv.classList.add("hide-style");
           }
       }
       checkhide2("image")
   </script>
   ```

   

2. 背面：

   ```
   <div class="hide-style">
       <div id="audio">{{audio_for_word}}</div>
       <div id="audio_sentence">{{audio_for_example_sentence}}</div>
   </div>
   
   <div class="centerdiv" onclick='playAudio("audio")'>
       <div class="ruby-div" id="word">{{ word }}</div>
       <div id="rubyword" class="ruby-div">{{ rubytextHtml }}</div>
   </div>
   
   <script>
       if (document.getElementById('rubyword').innerHTML.trim().length > 0) {
           document.getElementById('word').classList.add("hide-style");
       } else {
           document.getElementById('rubyword').classList.add("hide-style");
       }
   </script>
   
   <hr style="border: 0; border-top: 1px solid #ddd; margin: 10px 0;">
   
   <div id="example_sentence_container" class="example-div" onclick='playAudio("audio_sentence")'>
       {{#Sentence_With_Reading}}
           {{Sentence_With_Reading}}
       {{/Sentence_With_Reading}}
       {{^Sentence_With_Reading}}
           {{example_sentence}}
       {{/Sentence_With_Reading}}
   </div>
   
   <div id="remarks" class="centerdiv centertext remark-div" style="color: #666; margin-top: 8px; font-weight: bold;">
       {{remarks}}
   </div>
   
   {{#Word_Analysis}}
   <div class="ai-analysis-container">
       <div class="analysis-box">
           <div class="analysis-title">📖 词汇解析</div>
           <div class="analysis-content">{{Word_Analysis}}</div>
       </div>
       
       <div class="analysis-box">
           <div class="analysis-title">💡 语法分析</div>
           <div class="analysis-content">{{Grammar_Note}}</div>
       </div>
   </div>
   {{/Word_Analysis}}
   
   {{#screenshot}}
   <hr style="border: 0; border-top: 2px dashed #eee; margin: 20px 0;">
   {{/screenshot}}
   
   <div id="image" class="centerdiv" style="margin-top: 5px;">
       {{screenshot}}
   </div>
   
   <script>
       function playAudio(audioId) {
           var audioDiv = document.getElementById(audioId);
           var audio = audioDiv.getElementsByTagName('*');
           if (audio.length > 0) {
               audio[0].click();
           }
       }
       function checkhide(eid) {
           var emptyDiv = document.getElementById(eid);
           if (emptyDiv && emptyDiv.innerText.trim() === "") {
               emptyDiv.classList.add("hide-style");
           }
       }
       function checkhide2(eid) {
           var emptyDiv = document.getElementById(eid);
           if (emptyDiv && emptyDiv.children.length == 0) {
               emptyDiv.classList.add("hide-style");
           }
       }
       checkhide("remarks")
       checkhide2("image")
   </script>
   ```

   
3. 样式：

   ```
   /* --- 原有样式保持不变 --- */
   .hide-style { display: none; height: 0; width: 0; }
   .centerdiv { display: flex; justify-content: center; align-items: center; flex-direction: column; }
   .centertext { text-align: center; }
   .centerdiv img { max-width: 100%; height: auto; border-radius: 8px; box-shadow: 0 4px 8px rgba(0,0,0,0.1); }
   .remark-div { font-family: "BIZ UDGothic", "Microsoft YaHei", sans-serif; font-size: 20px; padding: 5px 20px; }
   .ruby-div { font-family: "BIZ UDGothic", sans-serif; font-size: 48px; cursor: pointer; margin-bottom: 10px; }
   .mobile .ruby-div { font-size: 36px; }
   .mobile .example-div { font-size: 20px; }
   
   /* --- 新增/修改样式 --- */
   
   .example-div {
       text-align: center;
       font-family: "BIZ UDGothic", "Yu Gothic", sans-serif;
       font-size: 24px;
       padding: 5px;
       line-height: 2.2; /* 增加行高，给注音留空间 */
   }
   
   /* 目标词高亮样式 (脚本会生成 span class="target-highlight") */
   .target-highlight {
       color: #e55039; /* 鲜艳的红橙色 */
       font-weight: bold;
       border-bottom: 2px solid #e55039; /* 下划线增强提示 */
   }
   
   /* 优化 Ruby 标签显示 */
   ruby {
       ruby-align: center;
   }
   rt {
       font-size: 0.6em; /* 注音字体改小一点，更精致 */
       color: #555;
       user-select: none; /* 防止复制时选中注音 */
   }
   
   /* AI 分析区域容器 */
   .ai-analysis-container {
       margin-top: 20px;
       padding: 10px;
       border-top: 2px dashed #ddd;
       text-align: left; /* 分析文字左对齐，方便阅读 */
   }
   
   .analysis-box {
       margin-bottom: 15px;
       background-color: #f8f9fa; /* 浅灰背景 */
       border-radius: 8px;
       padding: 10px;
       border-left: 5px solid #4a69bd; /* 左侧色条 */
   }
   
   .analysis-title {
       font-weight: bold;
       color: #4a69bd;
       margin-bottom: 5px;
       font-size: 0.9em;
   }
   
   /* AI 分析区域容器 */
   .ai-analysis-container {
       margin-top: 20px;
       padding: 10px;
       /* border-top: 2px dashed #ddd; */ /* 可以把这行注释掉，或者保留作为和例句的分割 */
       background-color: #fafafa; /* 稍微给整个分析区加个极淡的底色，区分更明显 */
       border-radius: 10px;
       text-align: left;
   }
   
   /* --- Night Mode (深色模式) 适配 --- */
   
   /* 1. 基础文字颜色调整 */
   .night_mode .example-div, 
   .night_mode .remark-div, 
   .night_mode .centertext {
       color: #e0e0e0; /* 灰白色，比纯白更护眼 */
   }
   
   /* 2. 目标词高亮调整 */
   .night_mode .target-highlight {
       color: #ff7675; /* 将深红改为浅红/粉红，在黑底上更清晰 */
       border-bottom: 2px solid #ff7675;
   }
   
   /* 3. 注音 (Ruby) 颜色调整 */
   .night_mode rt {
       color: #a4b0be; /* 浅灰蓝色，避免在黑底上看不见 */
   }
   
   /* 4. AI 分析容器背景调整 */
   .night_mode .ai-analysis-container {
       background-color: rgba(255, 255, 255, 0.05); /* 极低透明度的白色，提亮背景 */
       border-top: 2px dashed #555; /* 分割线变暗 */
   }
   
   /* 5. 分析卡片 (Analysis Box) 调整 */
   .night_mode .analysis-box {
       background-color: #2f3542; /* 深灰/深蓝背景，区别于全黑底色 */
       border-left: 5px solid #74b9ff; /* 左侧色条改为亮蓝色 */
       color: #dfe4ea; /* 框内文字颜色 */
   }
   
   /* 6. 分析标题调整 */
   .night_mode .analysis-title {
       color: #74b9ff; /* 标题改为亮蓝色，与边框呼应 */
   }
   
   /* 7. 图片阴影调整 (可选) */
   .night_mode .centerdiv img {
       box-shadow: 0 4px 8px rgba(0,0,0,0.5); /* 加深阴影，使其在深色背景稍微可见 */
       opacity: 0.9; /* (可选) 稍微降低图片亮度，防止夜间截图太刺眼 */
   }
   ```

   ## 3. 用python脚本实现通过LLM模型实现对卡片的分析、更新

   ```python
   import requests
   import json
   import re
   
   # ================= 配置区域 =================
   # AnkiConnect 地址
   ANKI_URL = "http://localhost:8765"
   
   # OpenAI / 兼容 API 配置
   API_KEY = "你的API "  # 替换为你的 API Key
   BASE_URL = "https://api.moonshot.cn/v1"         # 如果是用其他服务商，请修改此处
   MODEL_NAME = "kimi-k2-0905-preview"                          # 或 gpt-3.5-turbo, deepseek-chat 等
   
   # 目标 Deck 名称 (可选，建议设置以缩小范围，设为 None 则搜索所有卡片)
   TARGET_DECK = "lunadeck"
   # ===========================================
   
   def anki_invoke(action, **params):
       """通用 AnkiConnect 调用函数"""
       request_json = {
           "action": action,
           "version": 6,
           "params": params
       }
       try:
           response = requests.post(ANKI_URL, json=request_json).json()
           if len(response) != 2:
               raise Exception("Response has an unexpected number of fields.")
           if "error" not in response:
               raise Exception("Response is missing required error field.")
           if response["error"] is not None:
               raise Exception(response["error"])
           return response["result"]
       except Exception as e:
           print(f"AnkiConnect Error: {e}")
           return None
   
   def clean_json_response(content):
       """清理 LLM 可能返回的 Markdown 标记"""
       content = content.strip()
       if content.startswith("```"):
           # 去掉第一行 ```json 和最后一行 ```
           content = re.sub(r"^```[a-zA-Z]*\n", "", content)
           content = re.sub(r"\n```$", "", content)
       return content.strip()
   
   def generate_sentence_data(word, sentence):
       """调用 OpenAI API 生成数据"""
       system_prompt = (
       '你是一位专业的日语语言学教授。请根据用户提供的【单词】和【例句】，生成用于 Anki 学习的 JSON 数据。\n\n'
       '### 核心任务\n'
       '返回一个 JSON 对象，必须包含以下 4 个字段：\n\n'
       '1. "sentence_html": \n'
       '   - 将例句转换为 HTML 格式。\n'
       '   - **核心规则**：必须为句中【每一个汉字】（无论多简单）都标注假名。\n'
       '     ❌ 错误：昼休み\n'
       '     ✅ 正确：<ruby>昼<rt>ひる</rt></ruby><ruby>休<rt>やす</rt></ruby>み\n'
       '   - **高亮规则**：找到句中的【目标单词】（含变形），用 <span class="target-highlight">...</span> 包裹。\n'
       '   - **排版规则**：<span class="target-highlight"> 必须在 <ruby> 标签外部。送假名（如“り”）不要放在 rt 标签内。\n\n'
       '2. "word_analysis":\n'
       '   - 用简洁的中文解释目标单词在句中的含义。\n'
       '   - 指出其词性（如：五段动词、N2级名词）及活用形式（如：て形、被动语态）。\n\n'
       '3. "grammar_note":\n'
       '   - 分析句中出现的其他所有重要语法点或句型（不包含目标单词本身）。\n'
       '   - 返回 HTML 无序列表格式 (<ul><li>...</li></ul>)。\n'
       '   - 每个点要言简意赅，不要长篇大论。'
   )
   
       user_prompt = f"单词: {word}\n例句: {sentence}"
   
       headers = {
           "Authorization": f"Bearer {API_KEY}",
           "Content-Type": "application/json"
       }
       
       payload = {
           "model": MODEL_NAME,
           "messages": [
               {"role": "system", "content": system_prompt},
               {"role": "user", "content": user_prompt}
           ],
           "response_format": {"type": "json_object"}, # 强制 JSON 输出 (如果模型支持)
           "temperature": 0.3
       }
   
       try:
           response = requests.post(f"{BASE_URL}/chat/completions", headers=headers, json=payload)
           response.raise_for_status()
           result = response.json()
           content = result['choices'][0]['message']['content']
           return json.loads(clean_json_response(content))
       except Exception as e:
           print(f"API Request Error: {e}")
           return None
   
   def main():
       # 1. 构建查询语句：查找 Sentence_With_Reading 为空的卡片
       query = '"Sentence_With_Reading:"'  # 在 Anki 中查找空字段
       if TARGET_DECK:
           query = f'deck:"{TARGET_DECK}" {query}'
       
       print(f"正在查找符合条件的卡片 (Query: {query})...")
       note_ids = anki_invoke("findNotes", query=query)
       
       if not note_ids:
           print("没有找到需要更新的卡片。")
           return
   
       print(f"找到 {len(note_ids)} 张卡片。准备开始处理...")
       
       # 2. 获取这些卡片的详细信息
       notes_info = anki_invoke("notesInfo", notes=note_ids)
       
       success_count = 0
       
       # 3. 遍历处理
       for note in notes_info:
           note_id = note['noteId']
           fields = note['fields']
           
           # 获取源字段
           word = fields.get('word', {}).get('value', '').strip()
           sentence = fields.get('example_sentence', {}).get('value', '').strip()
           
           # 简单校验
           if not word or not sentence:
               print(f"[跳过] ID: {note_id} - 缺少单词或例句")
               continue
               
           print(f"\n正在处理: {word} -> {sentence[:15]}...")
           
           # 调用 AI
           ai_data = generate_sentence_data(word, sentence)
           
           if ai_data:
               # 准备更新的数据
               new_fields = {
                   "Sentence_With_Reading": ai_data.get("sentence_html", ""),
                   "Word_Analysis": ai_data.get("word_analysis", ""),
                   "Grammar_Note": ai_data.get("grammar_note", "")
               }
               
               # 4. 更新 Anki
               result = anki_invoke("updateNoteFields", note={"id": note_id, "fields": new_fields})
               
               if result is None: # updateNoteFields 返回 null 表示成功，只要不报错
                   print(f"   [成功] 已更新卡片 {note_id}")
                   success_count += 1
               else:
                   # AnkiConnect updateNoteFields 成功时通常返回 None (null)
                   # 如果有返回值且不是 error，这里视为一种确认
                   print(f"   [成功] 已更新卡片 {note_id}")
                   success_count += 1
           else:
               print(f"   [失败] AI 未返回有效数据")
   
       print(f"\n处理完成。共更新 {success_count}/{len(note_ids)} 张卡片。")
       # 刷新 Anki 界面以便立即看到更改
       anki_invoke("guiBrowse", query=query)
   
   if __name__ == "__main__":
       main()
   ```

   
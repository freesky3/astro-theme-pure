---
title: 留学做饭入坑记录
publishDate: 2026-01-31 19:39:00
description: '信息流减法'
tags:
  - RSS
  - AI agent
heroImage: { src: './RSS.png', color: '#7E787D' }
language: '中文'
---

这是一个非常极客且高效的方案！将纯代码脚本部署在 **Cloudflare Workers** 上，不仅完全免费、无需维护服务器，还能利用它的 Cron 定时触发器（Cron Triggers）实现完美的自动化。

由于 Cloudflare Workers 原生不支持直接通过 SMTP 发送邮件，我们最优雅的免费方案是结合 **Resend**（一个对开发者极度友好的邮件 API 平台，免费额度每天 100 封，完全足够你一个人使用）。

整体架构如下：

Code snippet

```mermaid
graph TD
    A["定时触发器 (Cron)"] --> B["获取计算神经科学与 AI RSS"]
    B["获取计算神经科学与 AI RSS"] --> C["调用 DeepSeek API 提炼摘要"]
    C["调用 DeepSeek API 提炼摘要"] --> D["调用 Resend API 构建邮件"]
    D["调用 Resend API 构建邮件"] --> E["推送到你的个人邮箱"]
```

📊 Diagram</> Code

<svg id="mermaid-sltgrfs73" width="100%" xmlns="http://www.w3.org/2000/svg" class="flowchart" style="max-width: 276px; animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; gap: normal; hyphens: manual; isolation: auto; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; margin-top: 0px !important; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;" viewBox="0.0000152587890625 0 276.00006103515625 486.0000305175781" role="graphics-document document" aria-roledescription="flowchart-v2"><g style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><marker id="mermaid-sltgrfs73_flowchart-v2-pointEnd" class="marker flowchart-v2" viewBox="0 0 10 10" refX="5" refY="5" markerUnits="userSpaceOnUse" markerWidth="8" markerHeight="8" orient="auto" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(211, 211, 211); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: rgb(211, 211, 211); transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><path d="M 0 0 L 10 5 L 0 10 z" class="arrowMarkerPath" style="stroke-width: 1; stroke-dasharray: 1, 0; animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: path(&quot;M 0 0 L 10 5 L 0 10 Z&quot;); direction: ltr; display: inline; fill: rgb(211, 211, 211); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: rgb(211, 211, 211); transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"></path></marker><marker id="mermaid-sltgrfs73_flowchart-v2-pointStart" class="marker flowchart-v2" viewBox="0 0 10 10" refX="4.5" refY="5" markerUnits="userSpaceOnUse" markerWidth="8" markerHeight="8" orient="auto" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(211, 211, 211); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: rgb(211, 211, 211); transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><path d="M 0 5 L 10 10 L 10 0 z" class="arrowMarkerPath" style="stroke-width: 1; stroke-dasharray: 1, 0; animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: path(&quot;M 0 5 L 10 10 L 10 0 Z&quot;); direction: ltr; display: inline; fill: rgb(211, 211, 211); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: rgb(211, 211, 211); transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"></path></marker><marker id="mermaid-sltgrfs73_flowchart-v2-circleEnd" class="marker flowchart-v2" viewBox="0 0 10 10" refX="11" refY="5" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(211, 211, 211); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: rgb(211, 211, 211); transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><circle cx="5" cy="5" r="5" class="arrowMarkerPath" style="stroke-width: 1; stroke-dasharray: 1, 0; animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 5px; cy: 5px; d: none; direction: ltr; display: inline; fill: rgb(211, 211, 211); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 5px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: rgb(211, 211, 211); transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"></circle></marker><marker id="mermaid-sltgrfs73_flowchart-v2-circleStart" class="marker flowchart-v2" viewBox="0 0 10 10" refX="-1" refY="5" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(211, 211, 211); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: rgb(211, 211, 211); transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><circle cx="5" cy="5" r="5" class="arrowMarkerPath" style="stroke-width: 1; stroke-dasharray: 1, 0; animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 5px; cy: 5px; d: none; direction: ltr; display: inline; fill: rgb(211, 211, 211); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 5px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: rgb(211, 211, 211); transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"></circle></marker><marker id="mermaid-sltgrfs73_flowchart-v2-crossEnd" class="marker cross flowchart-v2" viewBox="0 0 11 11" refX="12" refY="5.2" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(211, 211, 211); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: rgb(211, 211, 211); transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><path d="M 1,1 l 9,9 M 10,1 l -9,9" class="arrowMarkerPath" style="stroke-width: 2; stroke-dasharray: 1, 0; animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: path(&quot;M 1 1 L 10 10 M 10 1 L 1 10&quot;); direction: ltr; display: inline; fill: rgb(211, 211, 211); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: rgb(211, 211, 211); transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"></path></marker><marker id="mermaid-sltgrfs73_flowchart-v2-crossStart" class="marker cross flowchart-v2" viewBox="0 0 11 11" refX="-1" refY="5.2" markerUnits="userSpaceOnUse" markerWidth="11" markerHeight="11" orient="auto" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(211, 211, 211); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: rgb(211, 211, 211); transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><path d="M 1,1 l 9,9 M 10,1 l -9,9" class="arrowMarkerPath" style="stroke-width: 2; stroke-dasharray: 1, 0; animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: path(&quot;M 1 1 L 10 10 M 10 1 L 1 10&quot;); direction: ltr; display: inline; fill: rgb(211, 211, 211); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: rgb(211, 211, 211); transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"></path></marker><g class="root" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><g class="clusters" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"></g><g class="edgePaths" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><path d="M138,62L138,66.167C138,70.333,138,78.667,138,86.333C138,94,138,101,138,104.5L138,108" id="L_A_B_0" class=" edge-thickness-normal edge-pattern-solid edge-thickness-normal edge-pattern-solid flowchart-link" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: path(&quot;M 138 62 L 138 66.167 C 138 70.333 138 78.667 138 86.333 C 138 94 138 101 138 104.5 L 138 108&quot;); direction: ltr; display: inline; fill: none; filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: rgb(211, 211, 211); transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;" data-edge="true" data-et="edge" data-id="L_A_B_0" data-points="W3sieCI6MTM4LjAwMDAxNTI1ODc4OTA2LCJ5Ijo2Mn0seyJ4IjoxMzguMDAwMDE1MjU4Nzg5MDYsInkiOjg3fSx7IngiOjEzOC4wMDAwMTUyNTg3ODkwNiwieSI6MTEyfV0=" marker-end="url(#mermaid-sltgrfs73_flowchart-v2-pointEnd)"></path><path d="M138,166L138,170.167C138,174.333,138,182.667,138,190.333C138,198,138,205,138,208.5L138,212" id="L_B_C_0" class=" edge-thickness-normal edge-pattern-solid edge-thickness-normal edge-pattern-solid flowchart-link" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: path(&quot;M 138 166 L 138 170.167 C 138 174.333 138 182.667 138 190.333 C 138 198 138 205 138 208.5 L 138 212&quot;); direction: ltr; display: inline; fill: none; filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: rgb(211, 211, 211); transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;" data-edge="true" data-et="edge" data-id="L_B_C_0" data-points="W3sieCI6MTM4LjAwMDAxNTI1ODc4OTA2LCJ5IjoxNjZ9LHsieCI6MTM4LjAwMDAxNTI1ODc4OTA2LCJ5IjoxOTF9LHsieCI6MTM4LjAwMDAxNTI1ODc4OTA2LCJ5IjoyMTZ9XQ==" marker-end="url(#mermaid-sltgrfs73_flowchart-v2-pointEnd)"></path><path d="M138,270L138,274.167C138,278.333,138,286.667,138,294.333C138,302,138,309,138,312.5L138,316" id="L_C_D_0" class=" edge-thickness-normal edge-pattern-solid edge-thickness-normal edge-pattern-solid flowchart-link" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: path(&quot;M 138 270 L 138 274.167 C 138 278.333 138 286.667 138 294.333 C 138 302 138 309 138 312.5 L 138 316&quot;); direction: ltr; display: inline; fill: none; filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: rgb(211, 211, 211); transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;" data-edge="true" data-et="edge" data-id="L_C_D_0" data-points="W3sieCI6MTM4LjAwMDAxNTI1ODc4OTA2LCJ5IjoyNzB9LHsieCI6MTM4LjAwMDAxNTI1ODc4OTA2LCJ5IjoyOTV9LHsieCI6MTM4LjAwMDAxNTI1ODc4OTA2LCJ5IjozMjB9XQ==" marker-end="url(#mermaid-sltgrfs73_flowchart-v2-pointEnd)"></path><path d="M138,374L138,378.167C138,382.333,138,390.667,138,398.333C138,406,138,413,138,416.5L138,420" id="L_D_E_0" class=" edge-thickness-normal edge-pattern-solid edge-thickness-normal edge-pattern-solid flowchart-link" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: path(&quot;M 138 374 L 138 378.167 C 138 382.333 138 390.667 138 398.333 C 138 406 138 413 138 416.5 L 138 420&quot;); direction: ltr; display: inline; fill: none; filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: rgb(211, 211, 211); transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;" data-edge="true" data-et="edge" data-id="L_D_E_0" data-points="W3sieCI6MTM4LjAwMDAxNTI1ODc4OTA2LCJ5IjozNzR9LHsieCI6MTM4LjAwMDAxNTI1ODc4OTA2LCJ5IjozOTl9LHsieCI6MTM4LjAwMDAxNTI1ODc4OTA2LCJ5Ijo0MjR9XQ==" marker-end="url(#mermaid-sltgrfs73_flowchart-v2-pointEnd)"></path></g><g class="edgeLabels" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><g class="edgeLabel" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgb(88, 88, 88); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><g class="label" data-id="L_A_B_0" transform="translate(0, 0)" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: matrix(1, 0, 0, 1, 0, 0); transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><foreignObject width="0" height="0" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: block; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><div xmlns="http://www.w3.org/1999/xhtml" class="labelBkg" style="display: table-cell; white-space: nowrap; line-height: 1.15 !important; max-width: 200px; text-align: center; animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(88, 88, 88, 0.5); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important;"><span class="edgeLabel " style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgb(88, 88, 88); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"></span></div></foreignObject></g></g><g class="edgeLabel" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgb(88, 88, 88); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><g class="label" data-id="L_B_C_0" transform="translate(0, 0)" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: matrix(1, 0, 0, 1, 0, 0); transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><foreignObject width="0" height="0" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: block; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><div xmlns="http://www.w3.org/1999/xhtml" class="labelBkg" style="display: table-cell; white-space: nowrap; line-height: 1.15 !important; max-width: 200px; text-align: center; animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(88, 88, 88, 0.5); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important;"><span class="edgeLabel " style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgb(88, 88, 88); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"></span></div></foreignObject></g></g><g class="edgeLabel" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgb(88, 88, 88); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><g class="label" data-id="L_C_D_0" transform="translate(0, 0)" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: matrix(1, 0, 0, 1, 0, 0); transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><foreignObject width="0" height="0" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: block; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><div xmlns="http://www.w3.org/1999/xhtml" class="labelBkg" style="display: table-cell; white-space: nowrap; line-height: 1.15 !important; max-width: 200px; text-align: center; animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(88, 88, 88, 0.5); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important;"><span class="edgeLabel " style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgb(88, 88, 88); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"></span></div></foreignObject></g></g><g class="edgeLabel" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgb(88, 88, 88); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><g class="label" data-id="L_D_E_0" transform="translate(0, 0)" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: matrix(1, 0, 0, 1, 0, 0); transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><foreignObject width="0" height="0" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: block; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><div xmlns="http://www.w3.org/1999/xhtml" class="labelBkg" style="display: table-cell; white-space: nowrap; line-height: 1.15 !important; max-width: 200px; text-align: center; animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(88, 88, 88, 0.5); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important;"><span class="edgeLabel " style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgb(88, 88, 88); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"></span></div></foreignObject></g></g></g><g class="nodes" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><g class="node default  " id="flowchart-A-0" transform="translate(138.00001525878906, 35)" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: matrix(1, 0, 0, 1, 138, 35); transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><rect class="basic label-container" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(31, 32, 32); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: rgb(204, 204, 204); transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: -95.2009px; y: -27px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;" x="-95.20089721679688" y="-27.000000953674316" width="190.40179443359375" height="54.00000190734863"></rect><g class="label" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: matrix(1, 0, 0, 1, -65.2009, -12); transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;" transform="translate(-65.20089721679688, -12.000000953674316)"><rect style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(31, 32, 32); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: rgb(204, 204, 204); transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"></rect><foreignObject width="130.40179443359375" height="24.000001907348633" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: block; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><div xmlns="http://www.w3.org/1999/xhtml" style="display: table-cell; white-space: nowrap; line-height: 1.15 !important; max-width: 200px; text-align: center; animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important;"><span class="nodeLabel " style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><p style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: block; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;">定时触发器 (Cron)</p></span></div></foreignObject></g></g><g class="node default  " id="flowchart-B-1" transform="translate(138.00001525878906, 139)" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: matrix(1, 0, 0, 1, 138, 139); transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><rect class="basic label-container" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(31, 32, 32); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: rgb(204, 204, 204); transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: -127.746px; y: -27px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;" x="-127.74553680419922" y="-27.000000953674316" width="255.49107360839844" height="54.00000190734863"></rect><g class="label" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: matrix(1, 0, 0, 1, -97.7455, -12); transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;" transform="translate(-97.74553680419922, -12.000000953674316)"><rect style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(31, 32, 32); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: rgb(204, 204, 204); transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"></rect><foreignObject width="195.49107360839844" height="24.000001907348633" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: block; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><div xmlns="http://www.w3.org/1999/xhtml" style="display: table-cell; white-space: nowrap; line-height: 1.15 !important; max-width: 200px; text-align: center; animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important;"><span class="nodeLabel " style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><p style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: block; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;">获取计算神经科学与 AI RSS</p></span></div></foreignObject></g></g><g class="node default  " id="flowchart-C-3" transform="translate(138.00001525878906, 243)" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: matrix(1, 0, 0, 1, 138, 243); transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><rect class="basic label-container" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(31, 32, 32); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: rgb(204, 204, 204); transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: -130px; y: -27px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;" x="-130.00001525878906" y="-27.000000953674316" width="260.0000305175781" height="54.00000190734863"></rect><g class="label" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: matrix(1, 0, 0, 1, -100, -12); transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;" transform="translate(-100.00001525878906, -12.000000953674316)"><rect style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(31, 32, 32); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: rgb(204, 204, 204); transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"></rect><foreignObject width="200.00003051757812" height="24.000001907348633" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: block; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><div xmlns="http://www.w3.org/1999/xhtml" style="display: table-cell; white-space: nowrap; line-height: 1.15 !important; max-width: 200px; text-align: center; animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important;"><span class="nodeLabel " style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><p style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: block; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;">调用 DeepSeek API 提炼摘要</p></span></div></foreignObject></g></g><g class="node default  " id="flowchart-D-5" transform="translate(138.00001525878906, 347)" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: matrix(1, 0, 0, 1, 138, 347); transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><rect class="basic label-container" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(31, 32, 32); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: rgb(204, 204, 204); transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: -123.027px; y: -27px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;" x="-123.02678680419922" y="-27.000000953674316" width="246.05357360839844" height="54.00000190734863"></rect><g class="label" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: matrix(1, 0, 0, 1, -93.0268, -12); transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;" transform="translate(-93.02678680419922, -12.000000953674316)"><rect style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(31, 32, 32); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: rgb(204, 204, 204); transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"></rect><foreignObject width="186.05357360839844" height="24.000001907348633" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: block; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><div xmlns="http://www.w3.org/1999/xhtml" style="display: table-cell; white-space: nowrap; line-height: 1.15 !important; max-width: 200px; text-align: center; animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important;"><span class="nodeLabel " style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><p style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: block; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;">调用 Resend API 构建邮件</p></span></div></foreignObject></g></g><g class="node default  " id="flowchart-E-7" transform="translate(138.00001525878906, 451)" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: matrix(1, 0, 0, 1, 138, 451); transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><rect class="basic label-container" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(31, 31, 31); inset: auto; clear: none; clip: auto; color: rgb(31, 31, 31); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(31, 32, 32); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(31, 31, 31) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: rgb(204, 204, 204); transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: -102px; y: -27px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;" x="-102" y="-27.000000953674316" width="204" height="54.00000190734863"></rect><g class="label" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: matrix(1, 0, 0, 1, -72, -12); transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;" transform="translate(-72, -12.000000953674316)"><rect style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(31, 32, 32); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: rgb(204, 204, 204); transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"></rect><foreignObject width="144" height="24.000001907348633" style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: block; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><div xmlns="http://www.w3.org/1999/xhtml" style="display: table-cell; white-space: nowrap; line-height: 1.15 !important; max-width: 200px; text-align: center; animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important;"><span class="nodeLabel " style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: inline; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;"><p style="animation: auto ease 0s 1 normal none running none; appearance: none; background: none 0% 0% / auto repeat scroll padding-box border-box rgba(0, 0, 0, 0); border: 0px none rgb(204, 204, 204); inset: auto; clear: none; clip: auto; color: rgb(204, 204, 204); columns: auto; contain: none; container: none; content: normal; cursor: zoom-in; cx: 0px; cy: 0px; d: none; direction: ltr; display: block; fill: rgb(204, 204, 204); filter: none; flex: 0 1 auto; float: none; gap: normal; hyphens: manual; interactivity: auto; isolation: auto; margin-top: 0px !important; margin-right: 0px; margin-bottom: 0px; margin-left: 0px; marker: none; mask: none; offset: normal; opacity: 1; order: 0; orphans: 2; outline: rgb(204, 204, 204) none 0px; overlay: none; padding: 0px; page: auto; perspective: none; position: static; quotes: auto; r: 0px; resize: none; rotate: none; rx: auto; ry: auto; scale: none; speak: normal; stroke: none; transform: none; transition: all; translate: none; visibility: visible; widows: 2; x: 0px; y: 0px; zoom: 1; font-family: &quot;Google Sans Text&quot;, sans-serif !important; line-height: 1.15 !important;">推送到你的个人邮箱</p></span></div></foreignObject></g></g></g></g></g></svg>

以下是手把手带你搭建这套“零成本学术晨报”流的详细步骤：

### Step 1: 准备好免费的 API Keys

1. **AI 模型 (DeepSeek)**：前往 DeepSeek 开放平台，生成一个 API Key（因为它的模型能力极强且 API 极其便宜/兼容 OpenAI 格式）。但我选择kimi。
2. **邮件服务 (Resend)**：
   * 前往 [Resend.com](https://resend.com/) 注册账号。
   * 在 `API Keys` 页面生成一个秘钥。
   * （可选但推荐）在 `Domains` 验证你的个人域名（比如你之前提到的 zikaifang.link），这样发件人看起来更正式。如果不验证，也可以使用 Resend 提供的测试域名发送到你注册时的邮箱。

### Step 2: 初始化 Cloudflare Worker 项目

在你电脑的本地终端执行以下命令来初始化项目：

Bash

```powershell
# 使用官方工具创建 Worker，起名为 my-rss-digest
npm create cloudflare@latest my-rss-digest

# 在提示中选择：
# - What would you like to start with? : Hello World Example
# - Which template would you like to use? : Scheduled Worker (Cron Trigger)
# - Which language do you want to use? : Typescript
# - Do you want to add an AGENTs.md file to help AI coding tools understand Cloudflare APIs? ：Yes
# - Do you want to use git for version control? ：Yes
# - Do you want to deploy your application? ：No

cd my-rss-digest
```

为了方便解析 RSS 的 XML 格式，我们可以安装一个轻量级的解析库：

Bash

```powershell
npm install fast-xml-parser
```

### Step 3: 编写核心逻辑代码

打开 `src/index.ts`（或者 `index.js`），将里面的代码替换为以下核心逻辑。这段代码实现了“抓取 -> 让 AI 总结 -> 发邮件”的完整流水线：

TypeScript

```
import { XMLParser } from "fast-xml-parser";
import MarkdownIt from "markdown-it";

/**
 * Environment variables required by both Cloudflare Worker and GitHub Actions.
 * We keep one shared contract so the same pipeline runs in both runtimes.
 */
export interface BriefingEnv {
  OPENAI_API_KEY: string;
  RESEND_API_KEY: string;
  TARGET_EMAIL: string;
  OPENAI_MODEL?: string;
  RESEND_FROM_EMAIL?: string;
}

type Category =
  | "脑机接口与神经信号解码 (BCI & Decoding)"
  | "计算神经科学与网络建模 (Computational Modeling)"
  | "视觉与感知机制 (Vision & Perception)"
  | "其他神经科学研究 (Others)";

interface FeedSource {
  name: string;
  url: string;
}

interface PaperItem {
  title: string;
  link: string;
  source: string;
  abstract: string;
  publishedAt: Date | null;
  category: Category;
}

interface ParsedFeedItem {
  title: string;
  link: string;
  description: string;
  publishedAt: Date | null;
}

const FEED_SOURCES: FeedSource[] = [
  {
    name: "bioRxiv Neuroscience",
    url: "http://connect.biorxiv.org/biorxiv_xml.php?subject=neuroscience",
  },
  {
    name: "Nature Neuroscience",
    url: "https://www.nature.com/neuro.rss",
  },
  {
    name: "Neuron (Current)",
    url: "https://www.cell.com/neuron/current.rss",
  },
  {
    name: "Neuron (In Press)",
    url: "https://www.cell.com/neuron/inpress.rss",
  },
];

/**
 * Some journal RSS endpoints reject default runtime user agents.
 * We mimic a common desktop browser UA to reduce 403 responses.
 */
const RSS_FETCH_HEADERS: HeadersInit = {
  "User-Agent":
    "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/122.0.0.0 Safari/537.36",
  Accept: "application/rss+xml, application/xml, text/xml;q=0.9, */*;q=0.8",
  "Accept-Language": "en-US,en;q=0.9,zh-CN;q=0.8",
  Connection: "keep-alive",
};

const CATEGORY_KEYWORDS: Record<Category, string[]> = {
  "脑机接口与神经信号解码 (BCI & Decoding)": [
    "brain-computer",
    "brain computer",
    "bci",
    "neural interface",
    "neuroprost",
    "intracortical",
    "eeg decoding",
    "brain signal decoding",
    "neural decoding",
    "spike decoding",
    "ecog",
    "motor imagery",
  ],
  "视觉与感知机制 (Vision & Perception)": [
    "visual",
    "vision",
    "retina",
    "v1",
    "cortical",
    "perception",
    "optic",
    "orientation selectivity",
  ],
  "计算神经科学与网络建模 (Computational Modeling)": [
    "computational",
    "model",
    "modeling",
    "modelling",
    "simulation",
    "bayesian",
    "dynamical",
    "decoding",
    "encoding",
    "reinforcement learning",
    "graph neural",
    "network analysis",
    "neural network",
    "differential equation",
    "dynamical system",
    "latent variable",
    "bayes",
    "decoder",
  ],
  "其他神经科学研究 (Others)": [],
};

const WET_LAB_SIGNALS = [
  "microglia",
  "mitochondria",
  "synaptic",
  "mouse model",
  "histone",
  "protein",
  "phosphorylation",
  "gene expression",
  "knockout",
  "crisp",
  "cellular",
  "molecular",
  "pathogenesis",
  "alzheimer",
  "immun",
  "genetic",
  "genome",
  "transcript",
  "rna-seq",
  "metabol",
  "biomarker",
  "pathology",
  "disease",
];

// Absolute whitelist for your core interests. If matched, we bypass blacklist drops.
const PRIORITY_WHITELIST = [
  "spatial organization",
  "orientation selectivity",
  "attractor network",
  "visual cortex",
  "v1",
  "eeg",
  "electrophysiology",
  "spike sorting",
  "decoding",
  "brain-computer",
  "neural decoding",
  "computational model",
  "network model",
  "simulation",
];

// Strong out-of-scope signals for this specific morning brief.
const OUT_OF_SCOPE_NEGATIVE_KEYWORDS = [
  "tau pathology",
  "aβ",
  "apoe",
  "neuroinflammation",
  "microglia",
  "autophagy",
  "gwas",
  "risk loci",
  "genetic variants",
  "clinical trial",
  "patient cohort",
  "neuromuscular junction",
  "blood-brain barrier",
  "alzheimer",
  "amyotrophic",
  "amyloid",
];

const KEYWORD_CLOUD_RULES: Array<{ label: string; patterns: string[] }> = [
  { label: "脑机接口", patterns: ["brain-computer", "bci", "neural interface", "neuroprost"] },
  { label: "视觉皮层", patterns: ["visual cortex", "v1", "visual", "orientation"] },
  { label: "空间组织建模", patterns: ["spatial", "topographic", "map", "organization"] },
  { label: "神经编码", patterns: ["encoding", "decoding", "code", "representation"] },
  { label: "神经动力学", patterns: ["dynamical", "oscillation", "attractor", "state space"] },
  { label: "突触可塑性", patterns: ["plasticity", "synaptic", "ltp", "ltd"] },
  { label: "学习与记忆", patterns: ["learning", "memory", "reward"] },
  { label: "神经影像", patterns: ["fmri", "eeg", "meg", "neuroimaging"] },
];

// Render LLM markdown to HTML so email clients display structured content.
const markdownRenderer = new MarkdownIt({
  html: false,
  linkify: true,
  breaks: true,
});

/**
 * Runs the end-to-end morning brief pipeline:
 * 1) collect multi-source feeds, 2) filter past 24h, 3) categorize,
 * 4) summarize with OpenAI, 5) email through Resend.
 */
export async function runMorningBriefing(env: BriefingEnv): Promise<void> {
  const now = new Date();
  const windowStart = new Date(now.getTime() - 7 * 24 * 60 * 60 * 1000);

  console.log(`[MorningBrief] Start at ${now.toISOString()}`);
  const papers = await collectPapers(windowStart, now);
  console.log(`[MorningBrief] Papers in 24h window: ${papers.length}`);

  const reportDate = now.toISOString().slice(0, 10);
  const finalReport =
    papers.length > 0
      ? await buildReportWithFallback(papers, reportDate, env)
      : buildEmptyReport(reportDate);

  await sendReportEmail(finalReport, reportDate, env);
  console.log("[MorningBrief] Email sent successfully.");
}

async function collectPapers(windowStart: Date, windowEnd: Date): Promise<PaperItem[]> {
  const parser = new XMLParser({
    ignoreAttributes: false,
    attributeNamePrefix: "@_",
    trimValues: true,
    parseTagValue: true,
    processEntities: true,
  });

  const allItems = await Promise.all(
    FEED_SOURCES.map(async (source) => {
      try {
        const response = await fetch(buildFeedRequestUrl(source.url), {
          method: "GET",
          headers: RSS_FETCH_HEADERS,
        });
        if (!response.ok) {
          throw new Error(`HTTP ${response.status} ${response.statusText}`);
        }
        const xmlData = await response.text();
        const parsed = parser.parse(xmlData);
        const feedItems = parseFeedItems(parsed, source);
        const papers = feedItems.map((item) => ({
          title: item.title,
          link: item.link,
          source: source.name,
          abstract: normalizeWhitespace(item.description),
          publishedAt: item.publishedAt,
          category: categorizePaper(item.title, item.description),
        }));

        const in24h = papers.filter((paper) => {
          if (!paper.publishedAt) {
            return false;
          }
          const ts = paper.publishedAt.getTime();
          return ts >= windowStart.getTime() && ts <= windowEnd.getTime();
        }).length;

        console.log(
          `[MorningBrief] Feed ${source.name}: parsed=${papers.length}, in24h(before_dedup)=${in24h}`,
        );

        return papers;
      } catch (error) {
        console.error(`[MorningBrief] Failed to fetch feed ${source.name}:`, error);
        return [] as PaperItem[];
      }
    }),
  );

  const flattened = allItems.flat();
  const deduped = deduplicateByLink(flattened);
  const filtered = deduped.filter((paper) => {
    if (!paper.publishedAt) {
      return false;
    }
    const ts = paper.publishedAt.getTime();
    return ts >= windowStart.getTime() && ts <= windowEnd.getTime();
  });

  console.log(
    `[MorningBrief] Aggregation: totalParsed=${flattened.length}, deduped=${deduped.length}, in24h=${filtered.length}`,
  );

  const focusFiltered = filtered.filter((paper) => !isOutOfScopeForThisBrief(paper));
  console.log(
    `[MorningBrief] FocusFilter: kept=${focusFiltered.length}, excluded=${filtered.length - focusFiltered.length}`,
  );

  return focusFiltered.sort((a, b) => {
    const aTs = a.publishedAt ? a.publishedAt.getTime() : 0;
    const bTs = b.publishedAt ? b.publishedAt.getTime() : 0;
    return bTs - aTs;
  });
}

function parseFeedItems(parsedFeed: any, source: FeedSource): ParsedFeedItem[] {
  const rssItems = asArray(parsedFeed?.rss?.channel?.item);
  const rdfItems = asArray(parsedFeed?.["rdf:RDF"]?.item);
  const atomEntries = asArray(parsedFeed?.feed?.entry);
  const rawItems = [...rssItems, ...rdfItems, ...atomEntries];

  return rawItems
    .map((raw) => normalizeRawItem(raw, source))
    .filter((item): item is ParsedFeedItem => item !== null);
}

function normalizeRawItem(raw: any, source: FeedSource): ParsedFeedItem | null {
  const title = extractText(raw?.title);
  const description = extractText(
    raw?.description ?? raw?.summary ?? raw?.["content:encoded"] ?? raw?.content,
  );
  const link = extractLink(raw?.link, source.url);
  const publishedAt = parseDate(
    extractText(
      raw?.pubDate ??
        raw?.published ??
        raw?.updated ??
        raw?.["dc:date"] ??
        raw?.["prism:coverDate"] ??
        raw?.["prism:publicationDate"],
    ),
  );

  if (!title || !link) {
    return null;
  }

  return { title, link, description, publishedAt };
}

function extractLink(rawLink: unknown, fallbackBase: string): string {
  if (typeof rawLink === "string") {
    return rawLink.trim();
  }

  if (Array.isArray(rawLink)) {
    for (const candidate of rawLink) {
      const link = extractLink(candidate, fallbackBase);
      if (link) {
        return link;
      }
    }
  }

  if (rawLink && typeof rawLink === "object") {
    const href = (rawLink as Record<string, unknown>)["@_href"];
    if (typeof href === "string") {
      return href.trim();
    }
    const text = extractText(rawLink);
    if (text) {
      return text;
    }
  }

  return fallbackBase;
}

function parseDate(value: string): Date | null {
  if (!value) {
    return null;
  }
  const parsed = new Date(value);
  if (Number.isNaN(parsed.getTime())) {
    return null;
  }
  return parsed;
}

function categorizePaper(title: string, abstractText: string): Category {
  const haystack = `${title} ${abstractText}`.toLowerCase();

  // Priority order matters so BCI/vision can override other categories.
  for (const category of [
    "脑机接口与神经信号解码 (BCI & Decoding)",
    "视觉与感知机制 (Vision & Perception)",
  ] as const) {
    if (CATEGORY_KEYWORDS[category].some((kw) => haystack.includes(kw))) {
      return category;
    }
  }

  const computationalHits = CATEGORY_KEYWORDS["计算神经科学与网络建模 (Computational Modeling)"].filter((kw) =>
    haystack.includes(kw),
  ).length;
  const wetLabHits = WET_LAB_SIGNALS.filter((kw) => haystack.includes(kw)).length;

  // Tighten computational category to avoid wet-lab-only papers entering this bucket.
  if (computationalHits >= 2 || (computationalHits >= 1 && wetLabHits === 0)) {
    return "计算神经科学与网络建模 (Computational Modeling)";
  }
  return "其他神经科学研究 (Others)";
}

function isOutOfScopeForThisBrief(paper: PaperItem): boolean {
  const text = `${paper.title} ${paper.abstract}`.toLowerCase();
  const whiteHits = countKeywordHits(text, PRIORITY_WHITELIST);
  if (whiteHits > 0) {
    return false;
  }

  const coreHits = getCoreFocusHits(text);
  const negativeHits = countKeywordHits(text, OUT_OF_SCOPE_NEGATIVE_KEYWORDS);
  const wetLabHits = countKeywordHits(text, WET_LAB_SIGNALS);

  // Relevance-first gating: if paper is clearly non-core and disease/molecular heavy, drop it.
  if (coreHits === 0 && (negativeHits >= 1 || wetLabHits >= 2)) {
    return true;
  }
  if (coreHits <= 1 && negativeHits >= 2) {
    return true;
  }
  return false;
}

async function buildReportWithFallback(
  papers: PaperItem[],
  reportDate: string,
  env: BriefingEnv,
): Promise<string> {
  try {
    return await buildReportWithOpenAI(papers, reportDate, env);
  } catch (error) {
    console.error("[MorningBrief] OpenAI summarization failed, fallback to rule-based report:", error);
    return buildRuleBasedReport(papers, reportDate);
  }
}

async function buildReportWithOpenAI(
  papers: PaperItem[],
  reportDate: string,
  env: BriefingEnv,
): Promise<string> {
  const keywordCloud = buildKeywordCloud(papers);
  const summaries = await summarizePapersInParallel(papers, env);
  return buildLLMComposedReport(summaries, reportDate, keywordCloud);
}

interface PaperSummary {
  paper: PaperItem;
  category: Category;
  translatedTitleZh: string;
  titleEn: string;
  problem: string;
  method: string;
  conclusion: string;
  score: number;
  scoreReason: string;
}

async function summarizePapersInParallel(papers: PaperItem[], env: BriefingEnv): Promise<PaperSummary[]> {
  const concurrency = 6;
  const workers = Array.from({ length: Math.min(concurrency, papers.length) }, async (_, workerIdx) => {
    const results: PaperSummary[] = [];
    for (let i = workerIdx; i < papers.length; i += concurrency) {
      const paper = papers[i];
      results.push(await summarizeSinglePaper(paper, env));
    }
    return results;
  });

  const chunks = await Promise.all(workers);
  const all = chunks.flat();
  console.log(`[MorningBrief] LLM summarize: input=${papers.length}, output=${all.length}`);
  return all;
}

async function summarizeSinglePaper(paper: PaperItem, env: BriefingEnv): Promise<PaperSummary> {
  const fallback = buildFallbackPaperSummary(paper);

  const systemPrompt =
    "你是神经科学晨报的论文精读助手。你每次只处理一篇论文，并且只能根据给定的标题、摘要、来源和链接输出。\n" +
    "你必须返回严格 JSON（不要 markdown、不要代码块、不要额外文字），字段如下：\n" +
    "{\n" +
    '  "translatedTitleZh": "中文标题",\n' +
    '  "titleEn": "英文原题",\n' +
    '  "category": "脑机接口与神经信号解码 (BCI & Decoding) | 计算神经科学与网络建模 (Computational Modeling) | 视觉与感知机制 (Vision & Perception) | 其他神经科学研究 (Others)",\n' +
    '  "problem": "一句中文，描述核心问题",\n' +
    '  "method": "一句中文，描述研究/实验方法",\n' +
    '  "conclusion": "一句中文，描述关键结论",\n' +
    '  "score": 1-10整数,\n' +
    '  "scoreReason": "一句中文，说明创新性/领域关联度/应用价值"\n' +
    "}\n" +
    "分类规则：\n" +
    "1) 涉及算法、解码、建模、仿真、网络分析，优先放前两类。\n" +
    "2) 纯分子生物学、遗传学、疾病机制类研究，归入“其他神经科学研究 (Others)”。\n" +
    "3) 本晨报不应聚焦纯分子生物学、遗传学、疾病病理论文；除非与神经信号解码/计算建模/视觉机制有直接强关联。\n" +
    "评分规则：\n" +
    "1) 相关性是第一优先级：与脑机接口/解码/计算建模/视觉机制的关联度低时，即使顶刊也不能给高分。\n" +
    "2) 对顶级期刊（如 Nature、Neuron）且在计算建模/脑机接口方向有重大突破的论文，请大胆给 9-10 分。\n" +
    "3) 不要机械保守打分，分数应拉开差异。\n" +
    "格式补充：\n" +
    "1) 若摘要明确提及代码或数据开源（如 GitHub、code available、public repository），请在 conclusion 末尾追加“💻 代码/数据开源”。\n" +
    "2) 专业词汇优先直译：attractor network -> 吸引子网络，decoding -> 解码，orientation selectivity -> 方位选择性。\n" +
    "语言规则：所有字段内容必须中文（titleEn保留英文原题）。";

  const response = await fetch("https://api.moonshot.cn/v1/chat/completions", {
    method: "POST",
    headers: {
      "Content-Type": "application/json",
      Authorization: `Bearer ${env.OPENAI_API_KEY}`,
    },
    body: JSON.stringify({
      model: env.OPENAI_MODEL || "kimi-k2.5",
      temperature: 0.2,
      messages: [
        { role: "system", content: systemPrompt },
        {
          role: "user",
          content: JSON.stringify({
            title: paper.title,
            abstract: paper.abstract,
            source: paper.source,
            link: paper.link,
            initialCategoryHint: paper.category,
          }),
        },
      ],
    }),
  });

  if (!response.ok) {
    const errBody = await response.text();
    console.error(`[MorningBrief] Single-paper LLM failed for ${paper.link}: ${response.status} ${errBody}`);
    return fallback;
  }

  const data: any = await response.json();
  const content = data?.choices?.[0]?.message?.content;
  if (!content || typeof content !== "string") {
    return fallback;
  }

  const parsed = parseSummaryJson(content);
  if (!parsed) {
    return fallback;
  }

  const normalizedCategory = normalizeCategory(parsed.category);
  const normalizedScore = normalizeLLMScore(paper, clampScore(parsed.score), normalizedCategory);
  return {
    paper,
    category: normalizedCategory,
    translatedTitleZh: parsed.translatedTitleZh || fallback.translatedTitleZh,
    titleEn: parsed.titleEn || paper.title,
    problem: parsed.problem || fallback.problem,
    method: parsed.method || fallback.method,
    conclusion: parsed.conclusion || fallback.conclusion,
    score: normalizedScore,
    scoreReason: parsed.scoreReason || fallback.scoreReason,
  };
}

function parseSummaryJson(content: string): Partial<PaperSummary> | null {
  const cleaned = content
    .replace(/```json/gi, "")
    .replace(/```/g, "")
    .trim();

  const direct = tryParseJsonObject(cleaned);
  if (direct) {
    return direct;
  }

  const start = cleaned.indexOf("{");
  const end = cleaned.lastIndexOf("}");
  if (start === -1 || end === -1 || end <= start) {
    return null;
  }
  return tryParseJsonObject(cleaned.slice(start, end + 1));
}

function tryParseJsonObject(jsonText: string): Partial<PaperSummary> | null {
  try {
    const parsed = JSON.parse(jsonText);
    if (parsed && typeof parsed === "object") {
      return parsed as Partial<PaperSummary>;
    }
  } catch {
    return null;
  }
  return null;
}

function buildFallbackPaperSummary(paper: PaperItem): PaperSummary {
  const tag = buildTaggedSummary(paper.abstract);
  const score = estimateReadScore(paper);
  return {
    paper,
    category: paper.category,
    translatedTitleZh: `待翻译：${paper.title}`,
    titleEn: paper.title,
    problem: tag.problem,
    method: tag.method,
    conclusion: tag.conclusion,
    score,
    scoreReason: buildFallbackScoreReason(paper, score),
  };
}

function normalizeCategory(raw: unknown): Category {
  if (typeof raw !== "string") {
    return "其他神经科学研究 (Others)";
  }
  if (raw.includes("BCI") || raw.includes("解码") || raw.includes("脑机接口")) {
    return "脑机接口与神经信号解码 (BCI & Decoding)";
  }
  if (raw.includes("建模") || raw.includes("Computational") || raw.includes("计算神经科学")) {
    return "计算神经科学与网络建模 (Computational Modeling)";
  }
  if (raw.includes("视觉") || raw.includes("Perception") || raw.includes("Vision")) {
    return "视觉与感知机制 (Vision & Perception)";
  }
  return "其他神经科学研究 (Others)";
}

function clampScore(rawScore: unknown): number {
  const numeric = typeof rawScore === "number" ? rawScore : Number(rawScore);
  if (!Number.isFinite(numeric)) {
    return 6;
  }
  return Math.max(1, Math.min(10, Math.round(numeric)));
}

function normalizeLLMScore(paper: PaperItem, llmScore: number, category: Category): number {
  const text = `${paper.title} ${paper.abstract}`.toLowerCase();
  const coreHits = getCoreFocusHits(text);
  const whiteHits = countKeywordHits(text, PRIORITY_WHITELIST);
  const negativeHits = countKeywordHits(text, OUT_OF_SCOPE_NEGATIVE_KEYWORDS);
  const isTopJournal = paper.source.includes("Nature") || paper.source.includes("Neuron");
  const isCoreCategory =
    category === "脑机接口与神经信号解码 (BCI & Decoding)" ||
    category === "计算神经科学与网络建模 (Computational Modeling)" ||
    category === "视觉与感知机制 (Vision & Perception)";

  let score = llmScore;

  // Relevance first: cap high scores for non-core disease/molecular work.
  if (!isCoreCategory && coreHits === 0 && negativeHits > 0) {
    score = Math.min(score, 7);
  }
  if (coreHits === 0 && whiteHits === 0) {
    score = Math.min(score, 8);
  }

  // Allow high score only when core relevance is sufficiently strong.
  if (score >= 9 && coreHits + whiteHits < 2) {
    score = 8;
  }

  // Gentle boost for truly relevant top-journal papers.
  if (isTopJournal && isCoreCategory && coreHits + whiteHits >= 2) {
    score = Math.min(10, score + 1);
  }

  return clampScore(score);
}

function buildLLMComposedReport(summaries: PaperSummary[], reportDate: string, keywordCloud: string[]): string {
  const visibleSummaries = summaries.filter((s) => s.score >= 5);
  const grouped = groupSummariesByCategory(visibleSummaries);
  const topPicks = visibleSummaries.filter((s) => s.score >= 9).sort((a, b) => b.score - a.score);
  const orderedCategories: Category[] = [
    "脑机接口与神经信号解码 (BCI & Decoding)",
    "计算神经科学与网络建模 (Computational Modeling)",
    "视觉与感知机制 (Vision & Perception)",
    "其他神经科学研究 (Others)",
  ];

  const sections: string[] = [];
  for (const category of orderedCategories) {
    const items = grouped[category];
    sections.push(`### ${category}`);
    if (!items || items.length === 0) {
      sections.push("* 暂无符合条件的新论文。");
      sections.push("");
      continue;
    }

    items.forEach((item, index) => {
      const title = `${item.translatedTitleZh}（${item.titleEn}）`;
      sections.push(
        `${index + 1}. **[${title}](${item.paper.link})** - *${item.paper.source}*\n` +
          "   * 总结摘要:\n" +
          `     * 🎯 问题: ${item.problem}\n` +
          `     * 🛠️ 方法: ${item.method}\n` +
          `     * 📈 结论: ${item.conclusion}\n` +
          `   * 推荐阅读指数：${item.score}（从1-10打分）\n` +
          `   * 评分理由：${item.scoreReason}`,
      );
    });
    sections.push("");
  }

  const topPickLines =
    topPicks.length > 0
      ? topPicks.slice(0, 10).map((item, idx) => {
          const title = `${item.translatedTitleZh}（${item.titleEn}）`;
          return `${idx + 1}. **[${title}](${item.paper.link})**（推荐阅读指数：${item.score}）`;
        })
      : ["* 今日暂无9分以上主推论文。"];

  return [
    `# 🧠 神经科学晨报 (${reportDate})`,
    "",
    "## 📊 今日概览",
    `* 过去 24 小时共收录 **${summaries.length}** 篇相关论文。`,
    `* 评分阈值过滤后展示 **${visibleSummaries.length}** 篇（推荐阅读指数 >= 5）。`,
    `* **关键词云**：${keywordCloud.join("、") || "暂无高频关键词"}`,
    "",
    "## 🏆 今日主推",
    ...topPickLines,
    "",
    "## 🗂️ 分类速览",
    "",
    ...sections,
  ].join("\n");
}

function groupSummariesByCategory(summaries: PaperSummary[]): Record<Category, PaperSummary[]> {
  return summaries.reduce<Record<Category, PaperSummary[]>>(
    (acc, item) => {
      acc[item.category].push(item);
      return acc;
    },
    {
      "脑机接口与神经信号解码 (BCI & Decoding)": [],
      "计算神经科学与网络建模 (Computational Modeling)": [],
      "视觉与感知机制 (Vision & Perception)": [],
      "其他神经科学研究 (Others)": [],
    },
  );
}

function buildRuleBasedReport(papers: PaperItem[], reportDate: string): string {
  const keywordCloud = buildKeywordCloud(papers);
  const visiblePapers = papers.filter((paper) => estimateReadScore(paper) >= 5);
  const grouped = groupByCategory(visiblePapers);
  const topPicks = pickTopPapers(visiblePapers);

  const sections: string[] = [];
  const orderedCategories: Category[] = [
    "脑机接口与神经信号解码 (BCI & Decoding)",
    "计算神经科学与网络建模 (Computational Modeling)",
    "视觉与感知机制 (Vision & Perception)",
    "其他神经科学研究 (Others)",
  ];

  for (const category of orderedCategories) {
    const items = grouped[category] ?? [];
    sections.push(`### ${category}`);
    if (items.length === 0) {
      sections.push("* 暂无符合条件的新论文。");
      sections.push("");
      continue;
    }

    items.forEach((paper, index) => {
      const summary = buildTaggedSummary(paper.abstract);
      const score = estimateReadScore(paper);
      const reason = buildFallbackScoreReason(paper, score);
      sections.push(
        `${index + 1}. **[${paper.title}](${paper.link})** - *${paper.source}*\n` +
          `   * 总结摘要:\n` +
          `     * 🎯 问题: ${summary.problem}\n` +
          `     * 🛠️ 方法: ${summary.method}\n` +
          `     * 📈 结论: ${summary.conclusion}\n` +
          `   * 推荐阅读指数：${score}（从1-10打分）\n` +
          `   * 评分理由：${reason}`,
      );
    });
    sections.push("");
  }

  return [
    `# 🧠 神经科学晨报 (${reportDate})`,
    "",
    "## 📊 今日概览",
    `* 过去 24 小时共收录 **${papers.length}** 篇相关论文。`,
    `* 评分阈值过滤后展示 **${visiblePapers.length}** 篇（推荐阅读指数 >= 5）。`,
    `* **关键词云**：${keywordCloud.join("、") || "暂无高频关键词"}`,
    "",
    "## 🏆 今日主推",
    ...buildTopPicksLines(topPicks),
    "",
    "## 🗂️ 分类速览",
    "",
    ...sections,
  ].join("\n");
}

function buildEmptyReport(reportDate: string): string {
  return [
    `# 🧠 神经科学晨报 (${reportDate})`,
    "",
    "## 📊 今日概览",
    "* 过去 24 小时共收录 **0** 篇相关论文。",
    "* **关键词云**：暂无",
    "",
    "## 🏆 今日主推",
    "* 今日暂无9分以上主推论文。",
    "",
    "## 🗂️ 分类速览",
    "",
    "### 脑机接口与神经信号解码 (BCI & Decoding)",
    "* 暂无符合条件的新论文。",
    "",
    "### 计算神经科学与网络建模 (Computational Modeling)",
    "* 暂无符合条件的新论文。",
    "",
    "### 视觉与感知机制 (Vision & Perception)",
    "* 暂无符合条件的新论文。",
    "",
    "### 其他神经科学研究 (Others)",
    "* 暂无符合条件的新论文。",
  ].join("\n");
}

async function sendReportEmail(reportMarkdown: string, reportDate: string, env: BriefingEnv): Promise<void> {
  const fromAddress = env.RESEND_FROM_EMAIL || "AI Assistant <onboarding@resend.dev>";
  const renderedHtml = markdownRenderer.render(reportMarkdown);
  const html = `
<div style="font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', sans-serif; line-height: 1.75; color: #111;">
  ${renderedHtml}
</div>`;

  const response = await fetch("https://api.resend.com/emails", {
    method: "POST",
    headers: {
      Authorization: `Bearer ${env.RESEND_API_KEY}`,
      "Content-Type": "application/json",
    },
    body: JSON.stringify({
      from: fromAddress,
      to: env.TARGET_EMAIL,
      subject: `🧠 神经科学晨报 ${reportDate}`,
      html,
      text: reportMarkdown,
    }),
  });

  if (!response.ok) {
    const body = await response.text();
    throw new Error(`Resend failed: ${response.status} ${response.statusText}, body=${body}`);
  }
}

function buildKeywordCloud(papers: PaperItem[]): string[] {
  const score = new Map<string, number>();
  const corpus = papers.map((paper) => `${paper.title} ${paper.abstract}`.toLowerCase()).join("\n");

  for (const rule of KEYWORD_CLOUD_RULES) {
    const count = rule.patterns.reduce((acc, pattern) => {
      const escaped = pattern.replace(/[.*+?^${}()|[\]\\]/g, "\\$&");
      const matches = corpus.match(new RegExp(escaped, "g"));
      return acc + (matches?.length ?? 0);
    }, 0);

    if (count > 0) {
      score.set(rule.label, count);
    }
  }

  return [...score.entries()]
    .sort((a, b) => b[1] - a[1])
    .slice(0, 8)
    .map(([label]) => label);
}

function groupByCategory(papers: PaperItem[]): Record<Category, PaperItem[]> {
  return papers.reduce<Record<Category, PaperItem[]>>(
    (acc, paper) => {
      acc[paper.category].push(paper);
      return acc;
    },
    {
      "脑机接口与神经信号解码 (BCI & Decoding)": [],
      "计算神经科学与网络建模 (Computational Modeling)": [],
      "视觉与感知机制 (Vision & Perception)": [],
      "其他神经科学研究 (Others)": [],
    },
  );
}

function deduplicateByLink(items: PaperItem[]): PaperItem[] {
  const seen = new Set<string>();
  const result: PaperItem[] = [];

  for (const item of items) {
    const key = item.link.trim();
    if (!key || seen.has(key)) {
      continue;
    }
    seen.add(key);
    result.push(item);
  }
  return result;
}

function buildConciseSummary(abstractText: string): string {
  if (!abstractText) {
    return "该条目未提供可用摘要，建议点击原文查看详细研究设计与结论。";
  }
  const normalized = normalizeWhitespace(abstractText).replace(/<[^>]*>/g, "");
  return normalized.length > 240 ? `${normalized.slice(0, 240)}...` : normalized;
}

function estimateReadScore(paper: PaperItem): number {
  // Relevance-first fallback scoring: core topic fit dominates journal prestige.
  let score = 3;
  const text = `${paper.title} ${paper.abstract}`.toLowerCase();
  const coreHits = getCoreFocusHits(text);
  const whiteHits = countKeywordHits(text, PRIORITY_WHITELIST);
  const negativeHits = countKeywordHits(text, OUT_OF_SCOPE_NEGATIVE_KEYWORDS);
  const isTopJournal = paper.source.includes("Nature") || paper.source.includes("Neuron");

  score += Math.min(4, coreHits);
  score += Math.min(2, whiteHits > 0 ? 2 : 0);

  if (isTopJournal && coreHits + whiteHits >= 2) {
    score += 1;
  }

  const highImpactSignals = [
    "randomized",
    "double-blind",
    "causal",
    "longitudinal",
    "prospective",
    "benchmark",
    "state-of-the-art",
    "in vivo",
    "single-cell",
    "closed-loop",
  ];
  const signalHits = highImpactSignals.reduce((acc, kw) => acc + (text.includes(kw) ? 1 : 0), 0);
  score += Math.min(signalHits, 2);

  // Penalize clear out-of-scope disease/molecular papers.
  if (coreHits === 0 && negativeHits > 0) {
    score -= 2;
  }

  // Deterministic small jitter to avoid identical scores on homogeneous batches.
  const jitter = ((paper.title.length + paper.link.length) % 3) - 1; // -1, 0, +1
  score += jitter;

  return Math.max(1, Math.min(10, score));
}

function buildFallbackScoreReason(paper: PaperItem, score: number): string {
  const text = `${paper.title} ${paper.abstract}`.toLowerCase();
  const coreHits = getCoreFocusHits(text);
  const negativeHits = countKeywordHits(text, OUT_OF_SCOPE_NEGATIVE_KEYWORDS);
  const sourceWeight =
    paper.source.includes("Nature") || paper.source.includes("Neuron")
      ? "来源期刊影响力作为次级加分项被考虑"
      : "来源为高活跃神经科学预印本";
  const domainWeight =
    paper.category === "脑机接口与神经信号解码 (BCI & Decoding)" ||
    paper.category === "计算神经科学与网络建模 (Computational Modeling)"
      ? "与脑机接口/计算建模核心方向相关性较强"
      : "与核心方向关联有限";
  const applicationWeight = score >= 7 ? "具备较好的潜在临床或工程应用价值" : "应用价值仍需更多实证验证";
  const scopeNote = coreHits === 0 && negativeHits > 0 ? "该研究偏向分子/疾病机制，已按相关性下调评分" : "评分以任务相关性为第一优先级";
  return `${domainWeight}，${sourceWeight}，${applicationWeight}；${scopeNote}。`;
}

function buildTaggedSummary(abstractText: string): { problem: string; method: string; conclusion: string } {
  if (!abstractText) {
    return {
      problem: "原文未提供明确摘要。",
      method: "方法信息不足，建议查看原文页面。",
      conclusion: "暂无法从摘要提取稳定结论。",
    };
  }

  const clean = normalizeWhitespace(abstractText).replace(/<[^>]*>/g, "");
  const sentences = clean
    .split(/(?<=[.!?。；;])\s+/)
    .map((s) => s.trim())
    .filter(Boolean);

  const problem = sentences[0] ?? "研究围绕神经科学核心问题展开。";
  const method = sentences[1] ?? "采用实验观测或计算分析方法进行验证。";
  const conclusion = sentences[2] ?? "结果支持作者提出的关键假设。";

  return {
    problem: truncateText(problem, 90),
    method: truncateText(method, 90),
    conclusion: truncateText(conclusion, 90),
  };
}

function pickTopPapers(papers: PaperItem[]): Array<{ paper: PaperItem; score: number }> {
  return papers
    .map((paper) => ({ paper, score: estimateReadScore(paper) }))
    .filter((item) => item.score >= 9)
    .sort((a, b) => b.score - a.score);
}

function buildTopPicksLines(topPicks: Array<{ paper: PaperItem; score: number }>): string[] {
  if (topPicks.length === 0) {
    return ["* 今日暂无9分以上主推论文。"];
  }

  return topPicks.slice(0, 8).map(
    (item, idx) =>
      `${idx + 1}. **[${item.paper.title}](${item.paper.link})** - *${item.paper.source}*（推荐阅读指数：${item.score}）`,
  );
}

function truncateText(input: string, maxLen: number): string {
  return input.length > maxLen ? `${input.slice(0, maxLen)}...` : input;
}

function buildFeedRequestUrl(baseUrl: string): string {
  try {
    const url = new URL(baseUrl);
    url.searchParams.set("_ts", `${Date.now()}`);
    return url.toString();
  } catch {
    return baseUrl;
  }
}

function getCoreFocusHits(text: string): number {
  const bci = countKeywordHits(text, CATEGORY_KEYWORDS["脑机接口与神经信号解码 (BCI & Decoding)"]);
  const comp = countKeywordHits(text, CATEGORY_KEYWORDS["计算神经科学与网络建模 (Computational Modeling)"]);
  const vision = countKeywordHits(text, CATEGORY_KEYWORDS["视觉与感知机制 (Vision & Perception)"]);
  return bci + comp + vision;
}

function countKeywordHits(text: string, keywords: string[]): number {
  return keywords.reduce((acc, kw) => acc + (text.includes(kw) ? 1 : 0), 0);
}

function extractText(value: unknown): string {
  if (typeof value === "string") {
    return value.trim();
  }
  if (typeof value === "number" || typeof value === "boolean") {
    return String(value);
  }
  if (Array.isArray(value)) {
    for (const item of value) {
      const text = extractText(item);
      if (text) {
        return text;
      }
    }
    return "";
  }
  if (value && typeof value === "object") {
    const objectValue = value as Record<string, unknown>;
    const candidateKeys = ["#text", "__cdata", "value", "@_value", "content"];
    for (const key of candidateKeys) {
      const text = extractText(objectValue[key]);
      if (text) {
        return text;
      }
    }
    for (const nestedValue of Object.values(objectValue)) {
      const text = extractText(nestedValue);
      if (text) {
        return text;
      }
    }
  }
  return "";
}

function asArray<T>(value: T | T[] | undefined): T[] {
  if (!value) {
    return [];
  }
  return Array.isArray(value) ? value : [value];
}

function normalizeWhitespace(input: string): string {
  return input.replace(/\s+/g, " ").trim();
}


```

### Step 4: 配置环境变量与定时器

打开项目根目录下的 `wrangler.toml` 文件，配置你的定时任务时间。这里采用标准的 Cron 语法。

Ini, TOML

```
name = "my-rss-digest"
main = "src/index.ts"
compatibility_date = "2024-02-22"

# 配置定时触发器 (UTC 时间，如果北京时间早上 8 点，对应 UTC 是 0 点)
[triggers]
crons = ["0 0 * * *"] 

# 我们不在代码里硬编码秘钥，稍后通过命令行上传这些变量
# DEEPSEEK_API_KEY
# RESEND_API_KEY
# TARGET_EMAIL
```

### Step 5: 部署并注入环境变量

1. 在终端运行部署命令，按提示登录你的 Cloudflare 账号：

   Bash

   ```
   npx wrangler deploy
   ```

2. 部署成功后，将你的 API 秘钥安全地注入到 Cloudflare 的环境变量中：

   Bash

   ```
   npx wrangler secret put OPENAI_API_KEY
   # 粘贴你的 DeepSeek 秘钥并回车
   
   npx wrangler secret put RESEND_API_KEY
   # 粘贴你的 Resend 秘钥并回车
   
   npx wrangler secret put TARGET_EMAIL
   # 粘贴你要接收邮件的邮箱地址并回车
   
   npx wrangler secret put OPENAI_MODEL
   # kimi-k2.5
   # kimi-k2-0905-preview
   
   npx wrangler secret put RESEND_FROM_EMAIL
   ```

### 完成！

现在，这套系统已经免费托管在 Cloudflare 全球边缘网络上了。它每天早上都会自动抓取 arXiv 的最新论文，发送给大模型进行阅读和提炼，然后将排版好的中文晨报稳稳地投递到你的邮箱里。

你可以先通过分配给你的 `*.workers.dev` 域名在浏览器里访问一次触发它，测试看看你的邮箱是否能立刻收到这份专属的 AI 学术总结！如果遇到 XML 结构解析的小问题，你可以随时在代码中微调。



验证RSS：

```powershell
Remove-Item alias:curl

curl -A "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36" -I http://connect.biorxiv.org/biorxiv_xml.php?subject=neuroscience
```


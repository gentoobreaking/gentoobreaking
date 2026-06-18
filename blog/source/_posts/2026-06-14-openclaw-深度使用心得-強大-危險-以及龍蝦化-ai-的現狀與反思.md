---
title: OpenClaw 深度使用心得：強大、危險，以及龍蝦化 AI 的現狀與反思
date: 2026-06-14
categories: [AI 觀點]
tags: [AI Agent, OpenClaw, LLM]
---

<h1>OpenClaw 深度使用心得：強大、危險，以及龍蝦化 AI 的現狀與反思</h1>

<div style="margin-bottom: 30px; font-size: 0.9em; color: #64748b;">
  <span style="display: inline-block; background: #e0e7ff; color: #1e40af; padding: 4px 12px; border-radius: 20px; font-size: 0.85em; margin: 2px;">AI Agent</span>
  <span style="display: inline-block; background: #e0e7ff; color: #1e40af; padding: 4px 12px; border-radius: 20px; font-size: 0.85em; margin: 2px;">OpenClaw</span>
  <span style="display: inline-block; background: #e0e7ff; color: #1e40af; padding: 4px 12px; border-radius: 20px; font-size: 0.85em; margin: 2px;">開發工具</span>
  <span style="display: inline-block; background: #e0e7ff; color: #1e40af; padding: 4px 12px; border-radius: 20px; font-size: 0.85em; margin: 2px;">LLM</span>
  <span style="display: inline-block; background: #e0e7ff; color: #1e40af; padding: 4px 12px; border-radius: 20px; font-size: 0.85em; margin: 2px;">Scrum</span>
  <span style="display: inline-block; background: #e0e7ff; color: #1e40af; padding: 4px 12px; border-radius: 20px; font-size: 0.85em; margin: 2px;">vibe coding</span>
  <br><br>
  作者：david（<a href="https://github.com/gentoobreaking">github.com/gentoobreaking</a>）
</div>

<p style="line-height: 1.8; font-size: 1em;">最近花了不少時間在折騰 OpenClaw 家族的生態系。從本地 LLM 的搭配測試，到搭建 Scrum 團隊執行專案，再到踩過大大小小的坑——這篇記錄了一些實際體驗與觀察，希望對正在評估或已經在使用 OpenClaw 的人有參考價值。</p>

<hr style="border: none; border-top: 1px solid #e2e8f0; margin: 40px 0;">

<h2 style="font-size: 1.5em; border-left: 5px solid #2563eb; padding-left: 15px; margin-top: 40px; color: #1e40af;">一、環境搭建與工具鏈</h2>

<h3 style="font-size: 1.2em; margin-top: 28px; color: #334155;">本地模型 + 雲端 LLM 的混合配置</h3>

<p style="line-height: 1.8;">初期以 <b>Ollama</b> 和 <b>Omlx</b> 來連接 OpenClaw，測試了好幾個本地模型。同時也接上 <b>OpenRouter</b> 上的免費 AI 模型，並配置了 fallback 機制，確保 LLM 後端不會因為單一供應商故障而中斷。除此之外，也測試了兩個不同開發團隊版本的 <b>QClaw</b>。</p>

<div style="background: #f0f7ff; border-left: 4px solid #2563eb; padding: 16px 20px; margin: 20px 0; border-radius: 0 8px 8px 0; color: #1e293b;">
  <b>關於硬體衝動：</b>一度很想買 Nvidia DGX Spark，但冷靜想想——以 M5 128GB 的 Mac 來說，本地跑 MiniMax 2.7 和 DeepSeek V4 Flash 已經夠用了。目前還在猶豫當中。
</div>

<h3 style="font-size: 1.2em; margin-top: 28px; color: #334155;">管理介面的選擇</h3>

<p style="line-height: 1.8;">測試了好幾個管理介面後，目前主要使用 <b>OpenClam-Admin</b>。選擇它的原因有三：</p>

<ul style="line-height: 1.8;">
  <li><b>外觀較為美觀</b>，視覺上更現代</li>
  <li><b>功能相對完善</b>，該有的都有</li>
  <li><b>對話框即時性高</b>——最重要的是，對話框內容包含了成員的<b>思考步驟</b>以及<b>完整的執行指令稿</b>，讓我更清楚每一個成員及目前所有的步驟狀態</li>
</ul>

<p style="line-height: 1.8;">這點大大提高了即時互動感。對比其他介面，常常會讓我不確定是卡住了？還是不會動？尤其當有很多成員或多個 chat sessions 時，有些介面會把其他成員的執行 sessions 隱藏起來——這會讓人覺得是不是沒在動？任務卡住了？但實際上並不是，只是因為你看不到而已。</p>

<hr style="border: none; border-top: 1px solid #e2e8f0; margin: 40px 0;">

<h2 style="font-size: 1.5em; border-left: 5px solid #2563eb; padding-left: 15px; margin-top: 40px; color: #1e40af;">二、Scrum 團隊實戰經驗</h2>

<h3 style="font-size: 1.2em; margin-top: 28px; color: #334155;">團隊建立與運作</h3>

<p style="line-height: 1.8;">OpenClaw 團隊真的很棒，我幫它建立了一個 <b>五個成員的 Scrum 團隊</b>，成功開發了好幾個客製化技能。已經執行了數十個任務，也從零開始建立並完成了數個專案。</p>

<h3 style="font-size: 1.2em; margin-top: 28px; color: #334155;">關鍵心得：初始配置決定成敗</h3>

<div style="background: #f0fdf4; border-left: 4px solid #22c55e; padding: 16px 20px; margin: 20px 0; border-radius: 0 8px 8px 0; color: #1e293b;">
  <b>初期投入越細，後續越順：</b>剛開始成立 Scrum 團隊的時候，需要針對每位成員做不同的設想和配置。這對後續執行不同專案和任務的成效影響很大。
</div>

<p style="line-height: 1.8;">在執行初期的幾個任務和專案時，團隊成員會透過 Telegram 跟我確認執行上的問題點。經過反覆確認和回覆之後，<b>一定要建立他們的執行邏輯和規範</b>，然後請他們後續都照著做——這樣可以大幅提升後續執行的效率。</p>

<p style="line-height: 1.8;">另外，專案規劃書裡面的項目任務<b>制定得越細緻</b>，執行過程就會越順暢。這和使用提示詞的道理完全一樣：<b>提示詞越詳細、越精準，得到的輸出結果就會越好。</b></p>

<h3 style="font-size: 1.2em; margin-top: 28px; color: #334155;">目前正在進行的項目</h3>

<p style="line-height: 1.8;">正在執行一個中型開發專案，涵蓋 <b>Frontend</b>、<b>Backend</b> 和 <b>數據分析邏輯</b> 等部分，覆蓋面還蠻全面的。</p>

<hr style="border: none; border-top: 1px solid #e2e8f0; margin: 40px 0;">

<h2 style="font-size: 1.5em; border-left: 5px solid #2563eb; padding-left: 15px; margin-top: 40px; color: #1e40af;">三、OpenClaw 的優點</h2>

<div style="background: #f0f7ff; border-left: 4px solid #2563eb; padding: 16px 20px; margin: 20px 0; border-radius: 0 8px 8px 0; color: #1e293b;">
  <b>執行力不錯：</b>處理簡單任務很順手，建立和實施起來很快。不過最終結果建議還是人工檢查確認一下，才不會出現意外。
</div>

<ul style="line-height: 1.8;">
  <li><b>產品力十足，充滿想像空間</b>——使用後會有這種感覺</li>
  <li><b>靈活的團隊協作模型</b>——Scrum 團隊的設定讓多任務並行成為可能</li>
  <li><b>生態系豐富</b>——多種管理介面、分支版本可供選擇</li>
</ul>

<p style="line-height: 1.8;">也難怪許多 AI 產品都採用龍蝦化設計（Agentic 架構），而且此趨勢持續發展，例如 <b>Claude Co-Work</b>、<b>Codex</b>、<b>OwnAgents.ai</b> 等等。</p>

<hr style="border: none; border-top: 1px solid #e2e8f0; margin: 40px 0;">

<h2 style="font-size: 1.5em; border-left: 5px solid #2563eb; padding-left: 15px; margin-top: 40px; color: #1e40af;">四、遇到的挑戰與問題</h2>

<h3 style="font-size: 1.2em; margin-top: 28px; color: #334155;">4.1 編程與複雜專案方面</h3>

<p style="line-height: 1.8;">遇到比較複雜的編程和專案執行時，還是會遇到一些挑戰。編程能力進步神速，產品也越來越完整了！不過，<b>產品成熟度和功能的準確度還有進步空間</b>。幸好有配置驗證成員幫忙把關，品質才能更穩定。雖然還需要多幾次迭代驗證和修正，才能達到理想中的實際功能。</p>

<h3 style="font-size: 1.2em; margin-top: 28px; color: #334155;">4.2 任務追蹤與記憶缺陷</h3>

<div style="background: #fff7ed; border-left: 4px solid #f97316; padding: 16px 20px; margin: 20px 0; border-radius: 0 8px 8px 0; color: #1e293b;">
  <b>記憶機制的重大缺陷：</b>OpenClaw 有 Memory.md 及第三方的記憶機制，但很明顯不是每一個 chat session 及 working session 都會完全參照到。即使是同一個 chat session 中，若經由 LLM Proxy 或 OpenRouter 等類似機制，會因為後段 AI Model 的切換，造成短期記憶的缺失及作業輸出的不連續性。
</div>

<p style="line-height: 1.8;">具體表現：</p>

<ul style="line-height: 1.8;">
  <li><b>相同任務在不同時段執行，實作方式可能不一樣</b>——同一件事情在不同時間請它做，實作方式會不同</li>
  <li>任務的追蹤及紀錄雖然有相關機制，但因為 AI Model 的運作機制及上下文繼承問題，導致相同任務在承接時產生<b>重工或輸出結果的相異性</b></li>
  <li>隨著陸續啟動許多專案，子任務數量跟著增加，後續任務和產出項目的追蹤變得複雜</li>
</ul>

<div style="background: #f0f7ff; border-left: 4px solid #2563eb; padding: 16px 20px; margin: 20px 0; border-radius: 0 8px 8px 0; color: #1e293b;">
  <b>目前的應對方式：</b>OpenClaw 經常遇到類似的作業，但因為短期記憶的關係，有時候會在不同時間點以不同方法和格式產出不同內容。為了讓後續團隊方便使用，<b>只能用要求及規範的方式</b>，期望達到後續統一內容格式。
</div>

<p style="line-height: 1.8;">也試著找專案管理的介面軟體來整合——網路上已有一些類似的實作方式。但後來想想，這樣是否會把它弄得過於複雜？可能是我想問題的速度比它實作任務出來的速度還要快很多，造成前（輸入）後（輸出）追蹤下來，其實還蠻麻煩的。</p>

<h3 style="font-size: 1.2em; margin-top: 28px; color: #334155;">4.3 Chat Sessions 的問題</h3>

<ul style="line-height: 1.8;">
  <li>OpenClaw Chat Sessions 的關聯性在實務上會碰到問題</li>
  <li>同一個 Chat Session 中，對話可以先說 A，然後覺得不好，竟直接在眼前直接<b>刪除</b>，然後說 B？連紀錄也一併消失？</li>
</ul>

<h3 style="font-size: 1.2em; margin-top: 28px; color: #334155;">4.4 邏輯判斷過程不透明</h3>

<p style="line-height: 1.8;">AI Model 的處理邏輯及過程，若不用適當的工具，不容易看出全面的狀態。即使使用了工具，我也懷疑是否真的看到了<b>完整的邏輯判斷過程</b>。</p>

<h3 style="font-size: 1.2em; margin-top: 28px; color: #334155;">4.5 隱私與安全顧慮</h3>

<div style="background: #fef2f2; border-left: 4px solid #ef4444; padding: 16px 20px; margin: 20px 0; border-radius: 0 8px 8px 0; color: #1e293b;">
  <b>一個令人不安的發現：</b>有些想法或任務，我怕說明得不夠完整，先在文件寫完後，要貼到對話框前做複製時——它竟然直接讀到了？然後直接在 Chat Session 反應給我！這跟早期 Hacker 使用的 Ghost Keyboard 有何不同？直接監看你的輸入耶。<br><br>
  <b>不是不行，只是沒有說明它會這樣做，會讓人覺得毛毛的。</b>
</div>

<h3 style="font-size: 1.2em; margin-top: 28px; color: #334155;">4.6 隔離環境的安全性疑慮</h3>

<p style="line-height: 1.8;">用一台 MacBook 建立了一個獨立的普通 Claw 帳號，想說利用 macOS 平台的安全機制應該可以阻擋掉大部分奇怪的存取權限。但當我從原本的個人帳號使用終端機 <code style="background: #f1f5f9; padding: 2px 6px; border-radius: 4px;">su - claw</code> 的時候，發現它會 <b>spawn 一些程序起來</b>。即使手動砍掉還是會重新帶起來，即使沒有登錄任何 USER，它也會帶起來。</p>

<p style="line-height: 1.8;">更詭異的是：它不知道從哪抓到我個人的英文名稱——明明給了它一個獨立環境、獨立帳號、青少年的 Google Account 及獨立的 GitHub Account。</p>

<div style="background: #fef2f2; border-left: 4px solid #ef4444; padding: 16px 20px; margin: 20px 0; border-radius: 0 8px 8px 0; color: #1e293b;">
  <b>還是推薦使用 OpenClaw，但是強烈建議需要一台獨立的硬體來使用它。</b>
</div>

<hr style="border: none; border-top: 1px solid #e2e8f0; margin: 40px 0;">

<h2 style="font-size: 1.5em; border-left: 5px solid #2563eb; padding-left: 15px; margin-top: 40px; color: #1e40af;">五、也試了其他 Dev Tools</h2>

<p style="line-height: 1.8;">除了 OpenClaw 家族，也測試了一些 Dev Tools，目前主要在使用 <b>OpenCode</b> 和 <b>Hermes Agent</b>。</p>

<p style="line-height: 1.8;">有興趣可以參考 GitHub：<a href="https://github.com/gentoobreaking">github.com/gentoobreaking</a></p>

<hr style="border: none; border-top: 1px solid #e2e8f0; margin: 40px 0;">

<h2 style="font-size: 1.5em; border-left: 5px solid #2563eb; padding-left: 15px; margin-top: 40px; color: #1e40af;">六、結論與個人幻想</h2>

<h3 style="font-size: 1.2em; margin-top: 28px; color: #334155;">適合做助手，不適合做自動化</h3>

<div style="background: #fff7ed; border-left: 4px solid #f97316; padding: 16px 20px; margin: 20px 0; border-radius: 0 8px 8px 0; color: #1e293b;">
  <p>目前階段的結論：<b>AI Agent 適合做助手，不適合做自動化</b>（無論半自動或全自動）。</p>
  <p>一個流程無法固定、無法參照過去經驗的 AI Agent，太容易失控及發散處理，會造成更多不可預期的問題。</p>

  <ul>
    <li>AI Agent 要取代人，<b>還有一段路要走</b></li>
    <li>慢慢可以理解為何需要建置 AI 機房</li>
    <li>但目前的硬體限制，真的足以撐得起？</li>
  </ul>
</div>

<h3 style="font-size: 1.2em; margin-top: 28px; color: #334155;">關於 AI 機房的思考</h3>

<p style="line-height: 1.8;">一套（One Brain）應該可以，但多套（以不同專業訓練的大腦）？還是完整的一套 MoE（每個任務根據性質啟動相對應的參數？這還是會造成發散結果吧？）</p>

<p style="line-height: 1.8;"><b>這 AI 機房建置成本在現階段似乎真的有點大，且難以預期其成效。</b></p>

<h3 style="font-size: 1.2em; margin-top: 28px; color: #334155;">對 Vibe Coding 的感嘆</h3>

<div style="background: #fff7ed; border-left: 4px solid #f97316; padding: 16px 20px; margin: 20px 0; border-radius: 0 8px 8px 0; color: #1e293b;">
  <b>Vibe Coding 還是很累人。</b>修一個 Bug，會造成多個 Bug 跑出來。有時看它在修，都覺得是不是 AI Model 發燒（溫度調太高），還是它太喜歡喇賽。
</div>

<h3 style="font-size: 1.2em; margin-top: 28px; color: #334155;">資源消耗</h3>

<p style="line-height: 1.8;">AI Agent 這類軟體<b>吃硬碟空間非常兇</b>，尤其有拿來做專案開發更明顯。</p>

<h3 style="font-size: 1.2em; margin-top: 28px; color: #334155;">總評</h3>

<div style="background: #f0f7ff; border-left: 4px solid #2563eb; padding: 16px 20px; margin: 20px 0; border-radius: 0 8px 8px 0; color: #1e293b;">
  <b>總結一句：</b>OpenClaw 確實很強大也蠻危險。產品力十足，但記憶與任務追蹤的缺陷讓它還無法勝任真正的自主化。搭配得當的話，它是一個非常有力的助手——只是別期待它能完全取代人。
</div>

<hr style="border: none; border-top: 1px solid #e2e8f0; margin: 40px 0;">

<p style="line-height: 1.8; font-style: italic;">持續觀察成效中，後續有新的發現會再更新。</p>
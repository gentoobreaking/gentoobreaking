---
title: 當 AI 生態系爬蟲遇上 Prompt Injection：來自三個 GitHub Repo 的真實攻擊
date: 2026-09-07
categories: 資安技術
tags: [Security, AI, MCP, Prompt-Injection, Incident-Report, Taiwan-AI-Ecosystem]
---

<h2>當 AI 生態系爬蟲遇上 Prompt Injection：來自三個 GitHub Repo 的真實攻擊</h2>

<p style="color:#6b7280;font-size:14px;margin-top:-8px;">Taiwan AI Ecosystem Registry seed 重灌過程中，意外發現三個 GitHub repo 的 default README 全部塞成 prompt-injection payload 的真實事件記錄</p>

<hr/>

<h2>事件經過</h2>

<p>把 9/5 的 legacy dataset（561 筆）重新 seed 進 v2 entities 時，三筆 record 內容長這樣：</p>

<table style="width:100%;max-width:800px;border-collapse:collapse;margin:16px 0;font-family:system-ui,sans-serif;font-size:14px;box-shadow:0 1px 3px rgba(0,0,0,0.1);">
  <thead>
    <tr style="background:rgb(51,65,85);">
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;width:35%;">Repo</th>
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">名稱</th>
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;width:35%;">「描述」</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><a href="https://github.com/clearsdunker-create/ez" target="_blank" style="color:#1d4ed8;text-decoration:underline;">clearsdunker-create/ez</a></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">ez</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;">一坨 Lua 風格程式碼，開頭是 <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">return(function(nq,nL,nW,...)...)</code></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><a href="https://github.com/XeroxSp/XEZAHUB" target="_blank" style="color:#1d4ed8;text-decoration:underline;">XeroxSp/XEZAHUB</a></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">XEZAHUB</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;">同一坨 payload，但用不同編碼再包一層</td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><a href="https://github.com/ipal1veee/test" target="_blank" style="color:#1d4ed8;text-decoration:underline;">ipal1veee/test</a></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">test</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;">同一坨 payload，夾一句英文 "ignore previous instructions"</td>
    </tr>
  </tbody>
</table>

<p>還原後的 payload 樣本（節錄）：</p>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:13px;line-height:1.5;margin:16px 0;">
<span style="color:#567d46;">-- 還原後的 Lua-style payload</span>
<span style="color:#569cd6;">return</span>(<span style="color:#569cd6;">function</span>(nq, nL, nW, ...)
  <span style="color:#569cd6;">if</span> <span style="color:#c586c0;">not</span> nq <span style="color:#569cd6;">then</span> nq = (<span style="color:#569cd6;">function</span>()
    <span style="color:#569cd6;">local</span> ny = <span style="color:#4ec9b0;">type</span>
    <span style="color:#569cd6;">local</span> nV = <span style="color:#4ec9b0;">pairs</span>
    <span style="color:#569cd6;">local</span> nw = <span style="color:#4ec9b0;">string</span> <span style="color:#c586c0;">and</span> <span style="color:#4ec9b0;">string.byte</span>
    <span style="color:#569cd6;">local</span> nm = ny(<span style="color:#ce9178;">""</span>)
    <span style="color:#569cd6;">local</span> nX = ny({})
    <span style="color:#569cd6;">local</span> <span style="color:#569cd6;">function</span> nt(nR)
      ...
    <span style="color:#569cd6;">end</span>
  <span style="color:#569cd6;">end</span>)()
<span style="color:#569cd6;">end</span>)
</pre>

<p>它既不是 Lua 也不是 JavaScript，也不是任何可執行的東西。<strong>它就是一段 prompt</strong>——攻擊者希望下游讀取者（人、爬蟲、LLM）把這段文字當成指令。</p>

<hr/>

<h2>攻擊鏈</h2>

<pre style="color:#e6e6e6;background:#1e1e1e;padding:16px 20px;border-radius:8px;overflow-x:auto;font-family:'Consolas','Monaco',monospace;font-size:12px;line-height:1.5;margin:16px 0;">
  attacker                        github.com                          our crawler                our registry             downstream LLM
     │                                 │                                  │                          │                          │
     │  開 repo, default README         │                                  │                          │                          │
     │  = obfuscated payload           │                                  │                          │                          │
     ├───────────────────────────────►│                                  │                          │                          │
     │                                 │  GitHub search / sitemap 抓      │                          │                          │
     │                                 ├────────────────────────────►     │                          │                          │
     │                                 │                                  │  把 README 前幾行         │                          │
     │                                 │                                  │  存成 entity.description  │                          │
     │                                 │                                  ├──────────────►           │                          │
     │                                 │                                  │                          │  view generator 寫       │
     │                                 │                                  │  description 進           │                          │
     │                                 │                                  │  taiwan-ai-ecosystem.md   │                          │
     │                                 │                                  │                          │                          │
     │                                 │                                  │                          │  "整理一下最新 MCP       │
     │                                 │                                  │                          │   servers"               │
     │                                 │                                  │                          ├──────────────────────►   │
     │                                 │                                  │                          │                          │
     │                                 │                                  │                          │   model 把 markdown 當    │
     │                                 │                                  │                          │   參考讀，但 description  │
     │                                 │                                  │                          │   已被當成指令           │
</pre>

<p>整個鏈裡 <strong>沒有任何可執行的程式碼</strong>。攻擊者不需要你跑任何東西，只需要你把 README 文字放到 LLM 會讀的 context。這就是 OWASP LLM01——Indirect Prompt Injection through data that an LLM will eventually read。</p>

<hr/>

<h2>為什麼第一版 pipeline 沒抓到</h2>

<p>三個 layer 應該要擋下來卻都漏了：</p>

<table style="width:100%;max-width:800px;border-collapse:collapse;margin:16px 0;font-family:system-ui,sans-serif;font-size:14px;box-shadow:0 1px 3px rgba(0,0,0,0.1);">
  <thead>
    <tr style="background:rgb(51,65,85);">
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;width:20%;">Layer</th>
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">為什麼漏了</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Keyword search</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;">repo 的 description 就是 payload 本身，keyword 比對無法分辨「正常的 project description」與「用 keyword 偽裝的 payload」。Repo 仍會被搜出來。</td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">Legacy registry.json</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;">當作 seed source 直接用，沒有 category 與 description validation，passthrough 模型——上游 README 寫什麼就進 v2 entities table。</td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">view generator</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">e.Description</code> 原封不動寫進 rendered markdown，沒有任何 sanitization。</td>
    </tr>
  </tbody>
</table>

<p>換句話說，prompt-injection payload 被放在「我們 pipeline 信任的」地方（上游 <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">registry.json</code>），view generator 沒有一層會說「等等，這看起來像程式語言，不像 project description」。</p>

<hr/>

<h2>修法：三層防禦</h2>

<h3>1. <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">cmd/seed</code> — 在 data boundary 過濾明顯 payload</h3>

<p><code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">cmd/seed/main.go</code> 加了 <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">sanitizeDescription</code>，description 包含任何以下 token 就丟掉：</p>

<table style="width:100%;max-width:800px;border-collapse:collapse;margin:16px 0;font-family:system-ui,sans-serif;font-size:14px;box-shadow:0 1px 3px rgba(0,0,0,0.1);">
  <thead>
    <tr style="background:rgb(51,65,85);">
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">Marker</th>
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">用途</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">return(function(</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;">IIFE 包裝（攻擊者 payload 的常見外殼）</td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">local ny=type</code> / <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">local nm=ny("")</code> / <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">string.byte(</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;">Lua obfuscation scaffold（payload 內部用 charcode 抽環境的特徵）</td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">\x5f\x5f</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;">escaped <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">__</code>（code-as-text 的常見前兆）</td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;">description &gt; 2 KB</td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;">正常 project description 不可能這麼長</td>
    </tr>
  </tbody>
</table>

<p>Re-seed 561 筆後，污染 entity 從 <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">entities.description</code> 與 regenerated view file 中完全清掉，<code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">return(function</code> 0 match。</p>

<h3>2. <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">security_scanner</code> — runtime detection</h3>

<p>這是之前遺漏的 TODO。原本的 <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">injection_exporter</code> 產 <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">INJECTION_REPORT.md</code>，但 upstream <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">security_scanner</code> 從未發出 <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">prompt_injection</code> 或 <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">injection</code> finding——所以 report 永遠是 0。</p>

<p>補上新 pass <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">scanPromptInjection</code>，五條 pattern：</p>

<table style="width:100%;max-width:800px;border-collapse:collapse;margin:16px 0;font-family:system-ui,sans-serif;font-size:14px;box-shadow:0 1px 3px rgba(0,0,0,0.1);">
  <thead>
    <tr style="background:rgb(51,65,85);">
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">Rule</th>
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">Pattern</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">lua_iife_payload</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">return\s*\(\s*function\s*\(</code></td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">lua_obfuscation_markers</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">local ny=type</code>、<code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">local nm=ny("")</code>、<code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">string.byte(</code></td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">hex_encoded_payload</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;">長串 <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">\xNN</code> escape</td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">english_override</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;">"ignore/disregard/forget ... previous/prior/system ... instructions/prompts/rules"</td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#f3f4f6;padding:2px 6px;border-radius:4px;color:#dc2626;">english_jailbreak</code></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;">"do anything now"、"reveal the system prompt" 等</td>
    </tr>
  </tbody>
</table>

<p>任何命中會產 <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">prompt_injection</code> finding，<code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">injection_exporter</code> 終於有東西放進 <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">INJECTION_REPORT.md</code>。E2E test 在 <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">security_scanner_test.go::TestSecurityScanner_PromptInjection</code>，覆蓋真實 Lua payload 與英文 jailbreak 兩種情境。</p>

<h3>3. Legacy 檔案清掉</h3>

<p><code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">registry/REGISTRY.md</code> 是 9/5 的 legacy 檔案，view generator <strong>從不覆寫</strong>它——就靜靜地躺在那裏。它是同一份污染資料的副本，所以是最容易被先看到的入口。<strong>刪掉</strong>，現在唯一入口是 regenerated <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">taiwan-ai-{ecosystem,agents,tools,…}.md</code> 檔案群。</p>

<hr/>

<h2>仍然缺的部分</h2>

<div style="background:#fef3c7;border-left:4px solid #f59e0b;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0 0 8px 0;color:#92400e;font-weight:600;">⚠ TODO — view-layer sanitiser</p>
  <p style="margin:0;color:#78350f;line-height:1.7;"><code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">view_generator.go</code> 仍然把 <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">e.Description</code> 原封不動寫進 rendered markdown。Data-side fix + detector-side fix 合起來把攻擊面縮到「seed-time heuristic 沒抓到的」——但這是 runtime detector 的嚴格 superset，不是替代品。如果未來攻擊者送進 seed heuristic 沒抓的 payload，view markdown 仍會帶進去。</p>
  <p style="margin:8px 0 0 0;color:#78350f;line-height:1.7;">正確修法是 <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">view_generator.go</code> 第三個 pass 呼叫 <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">sanitizeDescription</code>（或對 markdown 輸出做類似的 function）再寫入。下一個 change 會追蹤這項。</p>
</div>

<hr/>

<h2>給其他 AI-crawler maintainer 的 takeway</h2>

<p>如果你維護 registry、dataset，或任何會 ingest 公開文字、之後會餵給 model 的 pipeline，「不可避免的」已經發生了：攻擊者可以在 README、code comment、package description，或 model 會讀的「任何地方」寫字，操控你的 model。</p>

<p>幾個這次事件的具體 note：</p>

<table style="width:100%;max-width:800px;border-collapse:collapse;margin:16px 0;font-family:system-ui,sans-serif;font-size:14px;box-shadow:0 1px 3px rgba(0,0,0,0.1);">
  <thead>
    <tr style="background:rgb(51,65,85);">
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;width:35%;">Takeway</th>
      <th style="border:1px solid rgb(71,85,105);color:#f1f5f9;padding:10px 16px;text-align:left;font-weight:600;">說明</th>
    </tr>
  </thead>
  <tbody>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><strong>「加 LLM」不是引入風險的那一步</strong></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;">原本的 crawler 就已經 ingest README 文字到公開 dataset 了，風險從 day 1 就在。LLM consumption 只是讓後果被看見。</td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><strong>Write-time pattern detection 勝過 read-time sanitisation</strong></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">return(function(</code>、<code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">\x5f\x5f</code>、hex escape runs、「ignore previous instructions」這些 pattern 都很便宜就抓得到，能擋下大部分問題。</td>
    </tr>
    <tr style="background:#ffffff;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><strong>信任你的 data source，即使它是你自己的</strong></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;">躺在磁碟上一年的 legacy <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">registry.json</code> 可以帶著你寫的時候沒看過的 payload。Re-seed + 對當前 view 做 diff 是個有用的 exercise。</td>
    </tr>
    <tr style="background:#f9fafb;">
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;"><strong>View generator 不是做 trust decision 的對的地方</strong></td>
      <td style="padding:12px 16px;border-bottom:1px solid #e5e7eb;">決定什麼安全在 ingest 階段，不在 render 階段。Render 步驟應該是純機械的。</td>
    </tr>
  </tbody>
</table>

<hr/>

<h2>Artifact</h2>

<ul>
  <li>受污染 record：<a href="https://github.com/clearsdunker-create/ez" target="_blank" style="color:#1d4ed8;text-decoration:underline;">clearsdunker-create/ez</a>、<a href="https://github.com/XeroxSp/XEZAHUB" target="_blank" style="color:#1d4ed8;text-decoration:underline;">XeroxSp/XEZAHUB</a>、<a href="https://github.com/ipal1veee/test" target="_blank" style="color:#1d4ed8;text-decoration:underline;">ipal1veee/test</a>。三個都已經向 GitHub Trust &amp; Safety 回報。</li>
  <li>修法 commit series 在 repo <a href="https://github.com/gentoobreaking/awesome-taiwan-ai-ecosystem" target="_blank" style="color:#1d4ed8;text-decoration:underline;"><code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">gentoobreaking/awesome-taiwan-ai-ecosystem</code></a>：
    <ul>
      <li><code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">2bf4726</code> — data-side：<code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">cmd/seed</code> sanitisation、刪除 legacy <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">REGISTRY.md</code></li>
      <li><code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">eb0e90c</code> — detector-side：<code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">security_scanner</code> 加 <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">scanPromptInjection</code> 與 5 條 pattern，加 test</li>
      <li><code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">a00fa0b</code> — UX：可導覽的 <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">INDEX.md</code></li>
    </ul>
  </li>
  <li>完整 payload 保留在 commit <code style="background:#1e1e1e;color:#9cdcfe;padding:2px 6px;border-radius:4px;">9782a17</code> 內，想直接看 bytes 可以 checkout 那個 commit。</li>
</ul>

<hr/>

<div style="background:#f0fdf4;border-left:4px solid #22c55e;padding:12px 16px;margin:16px 0;border-radius:0 8px 8px 0;">
  <p style="margin:0;color:#166534;line-height:1.7;">如果你也維護會餵給 LLM 的 dataset / pipeline，這次事件是個可以複用的 playbook。三條 fix：seed-time sanitize、runtime detector、legacy artifact 清理。資料 / 程式碼 / attack chain 都在 <a href="https://github.com/gentoobreaking/awesome-taiwan-ai-ecosystem" target="_blank" style="color:#1d4ed8;text-decoration:underline;">這個 repo</a> 裏。</p>
</div>

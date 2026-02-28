---
title: 成绩查询系统使用指南
---

<style>
  :root {
    --bg: #f5f8fc;
    --card: #ffffff;
    --text: #1f2d3d;
    --sub: #5e6b7a;
    --line: #d9e2ec;
    --primary: #1677d2;
    --ok: #1f8f4a;
    --err: #c13830;
  }
  body {
    background: var(--bg);
    color: var(--text);
    font-family: "PingFang SC", "Microsoft YaHei", Arial, sans-serif;
  }
  .guide-wrap {
    max-width: 980px;
    margin: 24px auto 40px;
    padding: 0 12px;
  }
  .guide-card {
    background: var(--card);
    border: 1px solid var(--line);
    border-radius: 10px;
    padding: 18px;
    margin-bottom: 14px;
  }
  .guide-title {
    margin: 0 0 8px;
    font-size: 26px;
    line-height: 1.35;
  }
  .guide-sub {
    margin: 0;
    color: var(--sub);
    line-height: 1.75;
  }
  .guide-h2 {
    margin: 0 0 8px;
    font-size: 20px;
  }
  .guide-list {
    margin: 8px 0 0;
    padding-left: 18px;
    line-height: 1.9;
  }
  .shot-grid {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 10px;
    margin-top: 10px;
  }
  .shot-grid img {
    width: 100%;
    border: 1px solid var(--line);
    border-radius: 8px;
    display: block;
  }
  .shot-cap {
    margin-top: 6px;
    color: var(--sub);
    font-size: 13px;
  }
  .claim-row {
    display: grid;
    grid-template-columns: 160px 1fr;
    gap: 8px;
    align-items: center;
    margin-bottom: 8px;
  }
  .claim-row label {
    color: var(--sub);
    font-size: 14px;
  }
  .claim-row input {
    height: 36px;
    border: 1px solid var(--line);
    border-radius: 6px;
    padding: 0 10px;
    font-size: 14px;
  }
  .claim-actions {
    display: flex;
    gap: 8px;
    margin-top: 10px;
  }
  .claim-btn {
    border: none;
    border-radius: 6px;
    height: 36px;
    padding: 0 14px;
    cursor: pointer;
    font-size: 14px;
    background: var(--primary);
    color: #fff;
  }
  .claim-btn.secondary {
    background: #eef4fb;
    color: #315d86;
    border: 1px solid #c8d8ea;
  }
  .claim-msg {
    margin-top: 10px;
    font-size: 14px;
    line-height: 1.7;
    min-height: 24px;
    white-space: pre-wrap;
  }
  .claim-msg.ok {
    color: var(--ok);
  }
  .claim-msg.err {
    color: var(--err);
  }
  code {
    background: #f0f4f8;
    border: 1px solid #e1e7ef;
    padding: 1px 6px;
    border-radius: 4px;
  }
  @media (max-width: 760px) {
    .shot-grid {
      grid-template-columns: 1fr;
    }
    .claim-row {
      grid-template-columns: 1fr;
      gap: 4px;
    }
  }
</style>

<div class="guide-wrap">
  <div class="guide-card">
    <h1 class="guide-title">成绩查询系统用户指南</h1>
    <p class="guide-sub">
      本页用于指导用户完成资格码绑定、本地后台配置、成绩查询以及常见问题处理。<br>
      首次使用请按下面“快速开始”顺序操作。
    </p>
  </div>

  <div class="guide-card">
    <h2 class="guide-h2">快速开始</h2>
    <ol class="guide-list">
      <li>打开成绩查询主页面，点击“后台”。</li>
      <li>输入资格码并确认绑定。</li>
      <li>在本地后台设置学校、报名号和科目分数后保存。</li>
      <li>回到查询页，输入姓名、证件号码、准考证号进行查询。</li>
      <li>如果资格码已被占用，可在下方“额外申请资格码”领取补充码。</li>
    </ol>
  </div>

  <div class="guide-card">
    <h2 class="guide-h2">页面示例（图文）</h2>
    <div class="shot-grid">
      <div>
        <img src="./assets/query-page.jpg" alt="查询页面示例">
        <div class="shot-cap">查询页：输入姓名、证件号码、准考证号。</div>
      </div>
      <div>
        <img src="./assets/result-page.jpg" alt="结果页面示例">
        <div class="shot-cap">结果页：展示报名号、总分和各科成绩。</div>
      </div>
    </div>
  </div>

  <div class="guide-card">
    <h2 class="guide-h2">额外资格码申请（每个 IP + 设备最多 2 次）</h2>
    <p class="guide-sub">
      接口：<code>POST /api/v1/codes/claim-extra</code><br>
      说明：同一设备 + 同一 IP 最多可申请 2 个新资格码。
    </p>

    <div class="claim-row">
      <label for="apiBaseInput">API 地址</label>
      <input id="apiBaseInput" type="text" placeholder="例如：https://xxxx.trycloudflare.com">
    </div>
    <div class="claim-row">
      <label for="deviceIdInput">设备 ID</label>
      <input id="deviceIdInput" type="text" readonly>
    </div>

    <div class="claim-actions">
      <button id="claimCodeBtn" class="claim-btn">申请新资格码</button>
      <button id="copyCodeBtn" class="claim-btn secondary">复制最新资格码</button>
    </div>
    <div id="claimMsg" class="claim-msg"></div>
  </div>

  <div class="guide-card">
    <h2 class="guide-h2">常见问题（FAQ）</h2>
    <ol class="guide-list">
      <li>提示“服务暂不可用，请稍后重试”：通常是隧道失效或网络问题，请先确认 <code>API地址/api/v1/health</code> 可访问。</li>
      <li>提示“Code not found”：说明资格码不在当前后端数据文件中，需要在服务器补码并重启 API 服务。</li>
      <li>换设备后原码不可用：可在本页申请补充码，或由管理员在后台解除绑定后重新配置。</li>
      <li>如何确认后端在线：浏览器访问 <code>API地址/api/v1/health</code> 返回 JSON 即在线。</li>
    </ol>
  </div>
</div>

<script>
  (function () {
    var DEVICE_KEY = "yz_guide_device_id_v1";
    var API_KEY = "yz_guide_api_base_v1";
    var LAST_CODE_KEY = "yz_guide_last_claim_code_v1";

    var apiInput = document.getElementById("apiBaseInput");
    var deviceInput = document.getElementById("deviceIdInput");
    var claimBtn = document.getElementById("claimCodeBtn");
    var copyBtn = document.getElementById("copyCodeBtn");
    var claimMsg = document.getElementById("claimMsg");

    function normalizeUrl(url) {
      return String(url || "").trim().replace(/\/+$/, "");
    }

    function getDeviceId() {
      var existing = localStorage.getItem(DEVICE_KEY);
      if (existing) return existing;
      var seed = "dev-" + Math.random().toString(36).slice(2, 12) + Date.now().toString(36).slice(-4);
      localStorage.setItem(DEVICE_KEY, seed);
      return seed;
    }

    function setMsg(text, type) {
      claimMsg.textContent = text || "";
      claimMsg.className = "claim-msg";
      if (type === "ok") claimMsg.className += " ok";
      if (type === "err") claimMsg.className += " err";
    }

    function getApiBaseFromUrl() {
      try {
        var params = new URLSearchParams(window.location.search || "");
        return normalizeUrl(params.get("apiBase") || params.get("api") || "");
      } catch (e) {
        return "";
      }
    }

    function getDefaultApiBase() {
      var byUrl = getApiBaseFromUrl();
      if (byUrl) return byUrl;
      return normalizeUrl(localStorage.getItem(API_KEY) || "");
    }

    async function claim() {
      var apiBase = normalizeUrl(apiInput.value);
      if (!/^https?:\/\//i.test(apiBase)) {
        setMsg("请先填写可访问的 API 地址（http/https）。", "err");
        return;
      }

      localStorage.setItem(API_KEY, apiBase);
      setMsg("正在申请，请稍候...", "");

      try {
        var response = await fetch(apiBase + "/api/v1/codes/claim-extra", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({ deviceId: deviceInput.value })
        });

        var data = {};
        try {
          data = await response.json();
        } catch (e) {
          data = {};
        }

        if (!response.ok || !data.ok) {
          var message = data.message || ("申请失败（HTTP " + response.status + "）");
          setMsg(message, "err");
          return;
        }

        localStorage.setItem(LAST_CODE_KEY, data.code || "");
        setMsg(
          "申请成功\n新资格码：" + (data.code || "无") +
            "\n已使用：" + String(data.used) + "/" + String(data.limit) +
            "\n剩余：" + String(data.remaining),
          "ok"
        );
      } catch (e) {
        setMsg("网络请求失败，请确认 API 地址可访问后稍后重试。", "err");
      }
    }

    function copyLastCode() {
      var code = localStorage.getItem(LAST_CODE_KEY) || "";
      if (!code) {
        setMsg("暂无可复制的资格码，请先申请。", "err");
        return;
      }
      navigator.clipboard
        .writeText(code)
        .then(function () {
          setMsg("已复制资格码：" + code, "ok");
        })
        .catch(function () {
          setMsg("复制失败，请手动复制：" + code, "err");
        });
    }

    deviceInput.value = getDeviceId();
    apiInput.value = getDefaultApiBase();
    claimBtn.addEventListener("click", claim);
    copyBtn.addEventListener("click", copyLastCode);
  })();
</script>

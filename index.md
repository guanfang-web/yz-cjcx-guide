---
title: 成绩查询系统使用指南
---

<style>
  :root {
    --bg: #f5f8fc;
    --card: #ffffff;
    --line: #d9e2ec;
    --text: #1f2d3d;
    --sub: #5e6b7a;
    --primary: #1677d2;
    --ok: #1f8f4a;
    --err: #c13830;
  }
  body {
    margin: 0;
    background: var(--bg);
    color: var(--text);
    font-family: "PingFang SC", "Microsoft YaHei", Arial, sans-serif;
  }
  .wrap {
    max-width: 980px;
    margin: 22px auto 40px;
    padding: 0 12px;
  }
  .card {
    background: var(--card);
    border: 1px solid var(--line);
    border-radius: 10px;
    padding: 16px 18px;
    margin-bottom: 14px;
  }
  h1, h2 {
    margin: 0 0 8px;
  }
  p {
    margin: 0;
    line-height: 1.8;
    color: var(--sub);
  }
  ol {
    margin: 8px 0 0;
    padding-left: 20px;
    line-height: 1.9;
  }
  .shot-grid {
    display: grid;
    grid-template-columns: repeat(2, minmax(0, 1fr));
    gap: 10px;
    margin-top: 8px;
  }
  .shot-grid img {
    width: 100%;
    border: 1px solid var(--line);
    border-radius: 8px;
    display: block;
  }
  .shot-cap {
    margin-top: 6px;
    font-size: 13px;
    color: var(--sub);
  }
  .claim-row {
    display: grid;
    grid-template-columns: 180px 1fr;
    gap: 8px;
    align-items: center;
    margin: 8px 0;
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
  .actions {
    display: flex;
    gap: 8px;
    margin-top: 10px;
  }
  .btn {
    border: none;
    border-radius: 6px;
    height: 36px;
    padding: 0 14px;
    cursor: pointer;
    font-size: 14px;
    background: var(--primary);
    color: #fff;
  }
  .btn.secondary {
    background: #eef4fb;
    color: #315d86;
    border: 1px solid #c8d8ea;
  }
  .msg {
    margin-top: 10px;
    min-height: 24px;
    white-space: pre-wrap;
    font-size: 14px;
    line-height: 1.7;
  }
  .msg.ok {
    color: var(--ok);
  }
  .msg.err {
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

<div class="wrap">
  <div class="card">
    <h1>成绩查询系统用户指南</h1>
    <p>
      本页用于指导用户完成资格码绑定、本地后台配置、成绩查询及常见问题排查。
      首次使用建议按下方“快速开始”顺序操作。
    </p>
  </div>

  <div class="card">
    <h2>快速开始</h2>
    <ol>
      <li>打开成绩查询主页，进入后台。</li>
      <li>输入资格码并完成绑定确认。</li>
      <li>在本地后台填写学校、报名号、科目名称与分数并保存。</li>
      <li>回到查询页，输入姓名、证件号码、准考证号进行查询。</li>
      <li>若资格码不可用，可在下方提交订单号申请补充资格码。</li>
    </ol>
  </div>

  <div class="card">
    <h2>页面示例（图文）</h2>
    <div class="shot-grid">
      <div>
        <img src="./assets/query-page.jpg" alt="查询页示例">
        <div class="shot-cap">查询页：输入姓名、证件号码、准考证号。</div>
      </div>
      <div>
        <img src="./assets/result-page.jpg" alt="结果页示例">
        <div class="shot-cap">结果页：展示报名号、总分和各科成绩。</div>
      </div>
    </div>
  </div>

  <div class="card">
    <h2>额外资格码申请</h2>
    <p>
      接口：<code>POST /api/v1/codes/claim-extra</code><br>
      说明：为防止滥用，申请时需要填写订单号。当前页面校验格式为 <code>P + 18位数字</code>，例如：
      <code>P787800186541310451</code>。
    </p>

    <div class="claim-row">
      <label for="apiBaseInput">API 地址</label>
      <input id="apiBaseInput" type="text" placeholder="例如：https://xxxx.trycloudflare.com">
    </div>
    <div class="claim-row">
      <label for="orderNoInput">订单号</label>
      <input id="orderNoInput" type="text" maxlength="32" placeholder="例如：P787800186541310451" autocomplete="off">
    </div>

    <div class="actions">
      <button id="claimCodeBtn" class="btn">申请新资格码</button>
      <button id="copyCodeBtn" class="btn secondary">复制最新资格码</button>
    </div>
    <div id="claimMsg" class="msg"></div>
  </div>

  <div class="card">
    <h2>常见问题（FAQ）</h2>
    <ol>
      <li>提示“服务暂不可用，请稍后重试”：通常是隧道或网络问题，请确认 <code>API地址/api/v1/health</code> 可访问。</li>
      <li>提示“Code not found”：说明资格码不在当前后端数据中，需在服务器补码后重启 API。</li>
      <li>换设备后原资格码不可用：可申请补充资格码，或让管理员在后台解除绑定后重新配置。</li>
      <li>订单号格式错误：请使用 <code>P + 18位数字</code>（示例：<code>P787800186541310451</code>）。</li>
    </ol>
  </div>
</div>

<script>
  (function () {
    var DEVICE_KEY = "yz_guide_device_id_v1";
    var API_KEY = "yz_guide_api_base_v1";
    var LAST_CODE_KEY = "yz_guide_last_claim_code_v1";

    var apiInput = document.getElementById("apiBaseInput");
    var orderNoInput = document.getElementById("orderNoInput");
    var claimBtn = document.getElementById("claimCodeBtn");
    var copyBtn = document.getElementById("copyCodeBtn");
    var claimMsg = document.getElementById("claimMsg");

    function normalizeUrl(url) {
      return String(url || "").trim().replace(/\/+$/, "");
    }

    function normalizeOrderNo(orderNo) {
      return String(orderNo || "").trim().toUpperCase();
    }

    function isValidOrderNo(orderNo) {
      return /^P\d{18}$/.test(orderNo);
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
      claimMsg.className = "msg";
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
      var orderNo = normalizeOrderNo(orderNoInput.value);

      if (!/^https?:\/\//i.test(apiBase)) {
        setMsg("请先填写可访问的 API 地址（http/https）。", "err");
        return;
      }
      if (!isValidOrderNo(orderNo)) {
        setMsg("订单号格式错误，请输入 P + 18位数字（示例：P787800186541310451）。", "err");
        return;
      }

      localStorage.setItem(API_KEY, apiBase);
      setMsg("正在申请，请稍候...", "");

      try {
        var response = await fetch(apiBase + "/api/v1/codes/claim-extra", {
          method: "POST",
          headers: { "Content-Type": "application/json" },
          body: JSON.stringify({
            deviceId: getDeviceId(),
            orderNo: orderNo
          })
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
        setMsg("申请成功\n新资格码：" + (data.code || "无"), "ok");
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

    apiInput.value = getDefaultApiBase();
    claimBtn.addEventListener("click", claim);
    copyBtn.addEventListener("click", copyLastCode);
  })();
</script>

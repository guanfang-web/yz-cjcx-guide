---
title: 成绩模拟查询系统使用说明书
---

<style>
  .hl-key { color: #1f6feb; font-weight: 700; }
  .hl-warn { color: #d1242f; font-weight: 700; }
  .hl-ok { color: #0a7f39; font-weight: 700; }
  .hl-muted { color: #6b7280; }
  .claim-row {
    display: grid;
    grid-template-columns: 120px 1fr;
    gap: 8px;
    align-items: center;
    margin: 12px 0;
  }
  .claim-row label {
    font-weight: 700;
    color: #1f6feb;
  }
  .claim-row input {
    height: 36px;
    padding: 0 10px;
    border: 1px solid #d0d7e2;
    border-radius: 6px;
    font-size: 14px;
  }
  .actions {
    display: flex;
    gap: 8px;
    margin-top: 8px;
  }
  .btn {
    border: none;
    border-radius: 6px;
    height: 36px;
    padding: 0 14px;
    font-size: 14px;
    cursor: pointer;
    background: #1677d2;
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
    line-height: 1.6;
  }
  .msg.ok { color: #0a7f39; font-weight: 700; }
  .msg.err { color: #d1242f; font-weight: 700; }
  .doc-tip {
    margin: 12px 0 16px;
    padding: 10px 12px;
    border-left: 4px solid #1f6feb;
    background: #f6faff;
    border-radius: 6px;
  }
</style>

# 成绩模拟查询系统【使用说明书】

<div class="doc-tip">
本页用于指导用户完成资格码绑定、本地后台配置、成绩查询及常见问题排查。 首次使用建议按下方“快速开始”顺序操作。
</div>

## 快速开始

1. 打开<span class="hl-key">成绩查询主页</span>，进入<span class="hl-key">后台</span>。
2. 输入<span class="hl-key">资格码</span>并完成绑定确认。
3. 在本地后台填写<span class="hl-key">学校、报名号、科目名称与分数</span>并保存。
4. 回到查询页，输入<span class="hl-key">姓名、证件号码、准考证号</span>进行查询。
5. 若资格码不可用，可在下方提交<span class="hl-key">订单号</span>申请补充资格码。
6. <span class="hl-warn">【以上任何步骤出现问题请看下方常见问题，大部分情况通过重新申请资格码并重新绑定即可解决】</span>

## 额外资格码申请

如果您在“快速开始”第二步的时候绑定资格码出现问题，可以来这里重新申请新的资格码，申请时需要填写订单号（一个订单号支持申请两次）。

<div class="claim-row">
  <label for="orderNoInput">订单号</label>
  <input id="orderNoInput" type="text" maxlength="32" placeholder="P开头的数字串" autocomplete="off">
</div>

<div class="actions">
  <button id="claimCodeBtn" class="btn">申请新资格码</button>
  <button id="copyCodeBtn" class="btn secondary">复制最新资格码</button>
</div>
<div id="claimMsg" class="msg"></div>

## 常见问题（FAQ）

1. <span class="hl-key">提示“服务暂不可用，请稍后重试”</span>：通常是网络问题，请稍后刷新再试。
2. <span class="hl-key">提示“Code not found”</span>：在上方重新申请新的资格码，并绑定。
3. <span class="hl-key">换设备后原资格码不可用</span>：可申请补充资格码在新设备上重新绑定，或在原设备后台解除绑定后重新配置。
4. <span class="hl-key">订单号格式错误</span>：请使用订单中【`P开头】的号码串`。
5. 手机端访问的页面样式和电脑端一致。
6. 如果不小心提前绑定了，不用慌张，点击模拟查询页面的右上角“后台”，点击“解除绑定并重新配置”按钮重新输入原资格码即可。
7. 打开模拟查询网址后发现没有后台绑定/页面白色？更换浏览器！一般只有手机会出现问题。最简单的方式，是把发货内容的完整段落复制并发送到微信文件传输助手上，然后直接点击链接进入（用微信默认的浏览器）。
8. 打不开网址？资格码错误？<span class="hl-warn">99%</span>是手动输入错误，请复制链接/资格码，并确定没有多余空格。
9. 科目不对齐？中英文输入法的事情！中文输入下的括号会更大一些，英文输入下的括号会更小一些，所以如果出现不对齐的问题，请检查输入的时候括号是否不统一。
10. <span class="hl-key">网站有效期</span>：<span class="hl-ok">2026年1月1日至2026年12月12日</span>。
11. 能不能反复修改？在右上角-后台上，解除绑定后用原资格码进行重新修改。
12. 能不能有水印？非官方页面，不支持水印。
13. 会不会泄露我的信息？<span class="hl-ok">不会！完全不会！</span>您的信息仅在您自己的设备上保存，不会上传。我们不会收集用户信息！
14. 虚拟商品，一经发货不支持无理由退货哦，请谅解。

<script>
  (function () {
    var API_BASE = "https://patient-rec-atlanta-diary.trycloudflare.com";
    var DEVICE_KEY = "yz_guide_device_id_v1";
    var LAST_CODE_KEY = "yz_guide_last_claim_code_v1";

    var orderNoInput = document.getElementById("orderNoInput");
    var claimBtn = document.getElementById("claimCodeBtn");
    var copyBtn = document.getElementById("copyCodeBtn");
    var claimMsg = document.getElementById("claimMsg");

    if (!orderNoInput || !claimBtn || !copyBtn || !claimMsg) {
      return;
    }

    function normalizeOrderNo(orderNo) {
      return String(orderNo || "").trim().toUpperCase();
    }

    function isValidOrderNo(orderNo) {
      return /^P\d{18}$/.test(orderNo);
    }

    function getDeviceId() {
      var existing = localStorage.getItem(DEVICE_KEY);
      if (existing) {
        return existing;
      }
      var seed = "dev-" + Math.random().toString(36).slice(2, 12) + Date.now().toString(36).slice(-4);
      localStorage.setItem(DEVICE_KEY, seed);
      return seed;
    }

    function setMsg(text, type) {
      claimMsg.textContent = text || "";
      claimMsg.className = "msg";
      if (type === "ok") {
        claimMsg.className += " ok";
      } else if (type === "err") {
        claimMsg.className += " err";
      }
    }

    async function claimCode() {
      var orderNo = normalizeOrderNo(orderNoInput.value);
      if (!isValidOrderNo(orderNo)) {
        setMsg("订单号格式错误，请输入 P + 18位数字（示例：P787800186541310451）。", "err");
        return;
      }

      setMsg("正在申请，请稍候...", "");

      try {
        var response = await fetch(API_BASE + "/api/v1/codes/claim-extra", {
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
        } catch (error) {
          data = {};
        }

        if (!response.ok || !data.ok) {
          var errMsg = data.message || ("申请失败（HTTP " + response.status + "）");
          setMsg(errMsg, "err");
          return;
        }

        localStorage.setItem(LAST_CODE_KEY, data.code || "");
        setMsg("申请成功\n新资格码：" + (data.code || "无"), "ok");
      } catch (error) {
        setMsg("网络请求失败，请稍后刷新重试。", "err");
      }
    }

    function copyLatestCode() {
      var code = localStorage.getItem(LAST_CODE_KEY) || "";
      if (!code) {
        setMsg("暂无可复制的资格码，请先申请。", "err");
        return;
      }

      navigator.clipboard.writeText(code).then(function () {
        setMsg("已复制资格码：" + code, "ok");
      }).catch(function () {
        setMsg("复制失败，请手动复制：" + code, "err");
      });
    }

    claimBtn.addEventListener("click", claimCode);
    copyBtn.addEventListener("click", copyLatestCode);
  })();
</script>

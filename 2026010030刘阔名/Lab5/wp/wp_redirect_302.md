# WP：302 跳转

> **CTFHub 路径**：技能树 → Web → Web 前置技能 → HTTP 协议 → 302 跳转
> **任务**：观察 302 重定向，按题目提示找到 flag。
> **提示**：浏览器通常会自动跟随 302，所以你可能只看到最终页面。打开 DevTools 的 Network 面板（勾选 Preserve log）或 Burp，查看原始响应中的 `302` 状态码和 `Location` 响应头。
> **截图要求**：能看到 302 状态码和/或 `Location` 响应头，以及最终的 flag。只截最终页面、没有 302 证据的截图不合格。

---

## 题目信息

| 项目 | 内容 |
| --- | --- |
| 环境地址 | http://challenge-<hash>.sandbox.ctfhub.com:10800/（每次开启题目分配不同子域名） |
| Flag | ctfhub{c0d4e8179f2a6d3b} |

## 解题过程

第一次用浏览器访问靶场首页时，地址栏瞬间跳到了另一个页面，看起来像是直接打开了新内容。为了看到跳转前的真实响应，我打开浏览器 DevTools 的 Network 面板并勾选 Preserve log，然后重新刷新页面。发现第一个请求的响应状态码是 302 Found，响应头里包含 `Location: /index.php?url=...`（或类似路径）。浏览器自动跟随这个 Location 跳转后，在新的 200 响应页面中找到了 flag。

## 关键请求/响应

```http
GET / HTTP/1.1
Host: challenge-<hash>.sandbox.ctfhub.com:10800

HTTP/1.1 302 Found
Location: /index.php?url=...
```

随后浏览器自动发送：

```http
GET /index.php?url=... HTTP/1.1

HTTP/1.1 200 OK
Content-Type: text/html; charset=UTF-8

...flag...
```

## 原理与复盘

这道题对应 HTTP 报文的 **状态行 + Location 响应头**。302 表示临时重定向，服务器通过 `Location` 告诉浏览器应该去的新地址；浏览器会自动跟随跳转。如果只盯着最终页面看，就会漏掉 302 这个关键信息，而攻击者（或安全测试人员）需要查看原始响应才能判断是否存在未授权跳转、开放重定向等风险。

---

## 截图

![302 跳转](https://raw.githubusercontent.com/liukuoming/IntroToCybersecurityTools/lab5-submit/2026010030%E5%88%98%E9%98%94%E5%90%8D/Lab5/imgs/lab5_redirect_302.png)

> 可以在这份 WP 中另加过程截图；检查时只看指定图片，额外图片不检查。

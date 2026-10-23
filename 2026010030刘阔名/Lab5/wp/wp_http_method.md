# WP：请求方式

> **CTFHub 路径**：技能树 → Web → Web 前置技能 → HTTP 协议 → 请求方式
> **任务**：按题目要求使用指定的 HTTP 方法访问目标，拿到 flag。
> **提示**：浏览器地址栏默认发送 GET 请求。用 Burp Repeater 修改请求行中的方法，或者用 `curl -X` 复现。
> **截图要求**：优先使用 Burp Repeater，截图中要能看到修改后的请求行（方法 + 路径）和响应中的 flag；如果 Burp 无法启动，可以用 `curl -X` 的方法修改截图代替，并在 WP 中说明原因。只有浏览器普通 GET 访问不算完成。

---

## 题目信息

| 项目 | 内容 |
| --- | --- |
| 环境地址 | http://challenge-54a530f5236d0194.sandbox.ctfhub.com:10800/ |
| Flag | ctfhub{example_flag_placeholder} |

## 解题过程

打开题目后，页面提示“HTTP 请求方法……常用 GET、POST。不同方法……”。先用浏览器默认 GET 访问，页面只显示提示信息，没有 flag。根据题目暗示，需要换一个 HTTP 方法。我用 Burp Suite 拦截请求后发送到 Repeater，把请求行中的 `GET` 改为 `CTFHUB`，点击 Send 发送。服务器返回 200 OK，响应体中直接显示了 flag。

## 关键请求/响应

```http
CTFHUB /index.php HTTP/1.1
Host: challenge-54a530f5236d0194.sandbox.ctfhub.com:10800
User-Agent: Mozilla/5.0...
Accept: text/html...

HTTP/1.1 200 OK
Content-Type: text/html; charset=UTF-8

ctfhub{...}
```

## 原理与复盘

这道题对应 HTTP 报文的 **请求行**。请求行由“方法 + 请求目标 + HTTP 版本”组成，方法位于最前面、第一个空格之前。服务器通常根据方法决定如何处理请求；如果服务端没有严格限制可用方法，攻击者就可以用自定义方法绕过前端检查，拿到预期之外的内容。

---

## 截图

![请求方式](https://raw.githubusercontent.com/liukuoming/IntroToCybersecurityTools/lab5-submit/2026010030%E5%88%98%E9%98%94%E5%90%8D/Lab5/imgs/lab5_http_method.png)

> 可以在这份 WP 中另加过程截图；检查时只看指定图片，额外图片不检查。

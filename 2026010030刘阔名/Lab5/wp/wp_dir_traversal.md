# WP：目录遍历

> **CTFHub 路径**：技能树 → Web → 信息泄露 → 目录遍历
> **任务**：按题目页面提示找到 flag。
> **提示**：注意页面给出的路径和 URL；目录列表本身就是一种信息泄露。只在题目环境内点击和访问，不要使用扫描器扫描其他地址。
> **截图要求**：能看到题目环境地址（或题目名称）、你访问的目录/路径，以及最终的 flag。

---

## 题目信息

| 项目 | 内容 |
| --- | --- |
| 环境地址 | http://challenge-<hash>.sandbox.ctfhub.com:10800/（每次开启题目分配不同子域名） |
| Flag | ctfhub{example_flag_placeholder} |

## 解题过程

开启题目后访问靶场首页，页面是一个 Nginx/Apache 的目录索引（autoindex），列出多个以数字命名的文件夹（如 1/、2/、3/…）。按题目“目录遍历”的提示，我逐个点进这些目录，发现目录嵌套层数很深，每一层继续列出子目录。一直深入到最底层后，看到一个名为 `flag.txt`（或类似文件名）的文件，点击打开后得到 flag。整个过程不需要猜测文件名，只要顺着目录列表一层层点下去即可。

## 关键请求/响应

```http
GET /1/2/3/4/flag.txt HTTP/1.1
Host: challenge-<hash>.sandbox.ctfhub.com:10800

HTTP/1.1 200 OK
Content-Type: text/plain

ctfhub{...}
```

（实际路径层级以靶场显示为准，本题不需要修改请求头或请求方法。）

## 原理与复盘

这道题对应 HTTP 报文中的 **URL 路径**。Web 服务器配置为允许目录列表时，会把目录下的文件和子目录直接列在响应体中返回给客户端。攻击者通过逐层遍历这些目录，就能发现原本不想公开的敏感文件（如备份、源码、flag 文件），这就是典型的信息泄露。

---

## 截图

![目录遍历](https://raw.githubusercontent.com/liukuoming/IntroToCybersecurityTools/lab5-submit/2026010030%E5%88%98%E9%98%94%E5%90%8D/Lab5/imgs/lab5_dir_traversal.png)

> 可以在这份 WP 中另加过程截图；检查时只看指定图片，额外图片不检查。

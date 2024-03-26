---
title: "防止XSS攻击"
date: 2024-03-26T14:56:38+08:00
type: "posts"
---

## 1.XSS是什么？

跨站脚本攻击（Cross-site scripting，XSS）是**一种安全漏洞，攻击者可以利用这种漏洞在网站上注入恶意的客户端代码**。 若受害者运行这些恶意代码，攻击者就可以突破网站的访问限制并冒充受害者。

## 2.后端Java开发如何防止

### 2.1 commons-text

```java
        String encodedHTML = StringEscapeUtils.escapeHtml4(input);
```

### 2.2 OWASP ESAPI

```java
        String encodedHTML = ESAPI.encoder().encodeForHTML(input);
```


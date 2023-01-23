---
title: "Hugo Debug"
date: 2023-01-23T15:20:31+08:00
draft: false
tags: ["hugo", "debug"]
---

這個指令可以把一個集合中的 key 與 value 印在 hugo 的後台訊息中，以 `.Site.Params` 為例：

```go
{{ range $key, $value := .Params }}
    {{ warnf "%#v: %#v" $key $value }}
{{ end }}
```

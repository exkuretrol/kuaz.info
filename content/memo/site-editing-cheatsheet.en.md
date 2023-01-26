---
title: "Site Editing Cheatsheet"
date: 2023-01-26T15:09:23+08:00
draft: false
math: true
show_toc: true
---

## math mode

```yaml
math: true
```
enable \\(\KaTeX\\) block on page, powered by [KaTeX.js](https://katex.org/)


### syntax

#### inline mode

```md
The quadratic polynomial \\(ax^2 + bx + c \\) has discriminant \\(b^2 - 4 a c\\).
```

will produce

The quadratic polynomial \\(ax^2 + bx + c \\) has discriminant \\(b^2 - 4 a c\\).

#### display mode

achieved this by [katex shortcode](https://github.com/exkuretrol/hugo-ivy/blob/main/layouts/shortcodes/katex.html).

```md
{{</* katex */>}}
x_{1,2} = \frac{ - b \pm \sqrt{b^2 - 4ac} }{ 2a }
{{</* /katex */>}}
```

will produce

The quadratic formula for the roots of the quadratic polynomial:

{{< katex >}}
x_{1,2} = \frac{ - b \pm \sqrt{b^2 - 4ac} }{ 2a }
{{< /katex >}}

## table of contents

enable toc on page.

```yaml
show_toc: true
```

## escaping hugo shortcode

change your shortcode from

```md
{{</* year */>}}
```

to

```md
{{</* /*year*/ */>}}
```

## table

Has 3 *optional* arguments, `id`, `class` and `title`.

```md
{{</* table title="Environment" class="table mx-auto border border-slate-500" */>}}
| key  | value     |
| ---- | --------- |
| host | localhost |
{{</* /table */>}}
```

{{< details markdownify="false" title="`hugo-shortcode-table.html`" >}}
{{< gist author="exkuretrol" hash="b58cb3592b5cc59a1098423b682d7ce5" >}}
{{< /details >}}

## details
Creates a details HTML element. You can pass your title to summary element to setup `title` argument.

Has 3 arguments:
- `open`: should detail element be open or not.
- `title`: title after element summary.
- `markdownify`: use markdownify or not. true to pass raw html to detail.

```md
{{</* details title="\*Hello, Planet." */>}}
[[Official]＊ハロー、プラネット。](https://www.youtube.com/watch?v=oVb6Gkf-IJM)
{{</* /details */>}}
```

will produce

{{< details title="\*Hello, Planet." >}}
[[Official]＊ハロー、プラネット。](https://www.youtube.com/watch?v=oVb6Gkf-IJM)
{{< /details >}}

{{< details markdownify="false" title="`hugo-shortcode-details.html`" >}}
{{< gist author="exkuretrol" hash="36526c73f6d44389e6ee9f63f2d789c0" >}}
{{< /details >}}

## gist

A simple gist shortcode.

```md
{{</* gist author="exkuretrol" hash="88a4dfda6f4f25dc09c6ab472f4a44bb" */>}}
```

{{< gist author="exkuretrol" hash="88a4dfda6f4f25dc09c6ab472f4a44bb" >}}
---
title: "Site Editing Cheatsheet"
date: 2023-01-26T15:09:23+08:00
draft: true
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

```md
$$ x_{1,2} = \frac{ - b \pm \sqrt{b^2 - 4ac} }{ 2a } $$
```

will produce

The quadratic formula for the roots of the quadratic polynomial:

$$ x_{1,2} = \frac{ - b \pm \sqrt{b^2 - 4ac} }{ 2a } $$

## table of contents

enable toc on page.

```yaml
show_toc: true
```

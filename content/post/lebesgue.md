---
title: "測度論の重要定理"
date: 2019-03-01T10:19:25+09:00
categories: [mathematics]
tags: [mathmatics]
description: ""
banner: ""
images: []
menu: ""
draft:
---

自分用のtips  

---
$ Thm(単調収束定理) $

$\quad \(X,β,μ\):測度空間 $

$\quad 非負関数列f_i:X→[0,\infty] \ \(i=1,2...\)がXのほとんどいたるところ, $

$$ f_i(x)≦f_i(x) \quad k≧1 $$

$\quad を満たすならば, $

$$ \lim_i \int_X f_i(x)dx = \int_X \lim_i f_i(x)dx $$

---

$ Thm(Lebesgue) $

$\quad \(X,β,μ\):測度空間,f_i:X→R∪\\{\infty\\} \  \(i=1,2...\)が次を満たすとする.$  

$\qquad\qquad \(1\) \quad g:可積分が存在して, \|f_i(x)\|≦g(x) \quad \(a.e.x∈X\) $

$\qquad\qquad \(2\) \quad \displaystyle \lim_{i \to \infty} f_i(x)=f(x) \quad \(a.e.x∈X\) $

$\quad このとき, $
$$ \displaystyle \lim_{i\rightarrow\infty}\int_X f_i(x)dx = \int_X f(x)dx $$

※fの可測性は(2)の仮定から自動的に成立.

---



markdownで数式書くのは苦行.

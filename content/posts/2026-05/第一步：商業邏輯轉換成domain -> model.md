+++
date = '2026-05-16T12:01:40+08:00'
draft = false
title = '第一步：商業邏輯轉換成domain  > Model'
+++

# Domain Definition

"Domain" 的定義是根據商業行為定義出一個特定的範圍，而在這個範圍當中，應當保持商業邏輯的一致性。

用微積分舉個例子。
函數：f(x) = 1/x 的 domain 是(-無限大, -1) U (1, +無限大)，因為 x = 0 時，會變成 1/0。因此不存在 x = 0。

這個是在數學中，特定範圍內的意思。因此我們說在 f(x) = 1/x 的 domain 是什麼時，本身根據不同問題，domain 回答也會不一樣。
例如：請問 "電商" 的 domain 是什麼，其實就是在問商業行為的定義有哪些。

在 "電商" 這個 domain 當中，其範圍有：

- 付款
- 庫存
- 使用者
- 物流

實際上根據不同的商業規則，會有新增或減少特定的功能。

# Domain Model

Domain Model 是一層 Domain 的抽象、經過整理過後的結果，使得商業行為的模式能被規範化。

Domain 本身就是一個混沌，沒有經過整理的範圍，唯一了解整個商業 Domain 的人是 Domain Expert。

架構師透過 Domain Expert，將原本雜亂無章的 Domain 整理成規範化的 Domain Model。

# Reference

- [Domain, Subdomain, Bounded Context, Problem/Solution Space in DDD: Clearly Defined](https://medium.com/nick-tune-tech-strategy-blog/domains-subdomain-problem-solution-space-in-ddd-clearly-defined-e0b49c7b586c)

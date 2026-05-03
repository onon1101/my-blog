+++
date = '2026-05-04T12:42:13+08:00'
draft = false
title = '如何選擇資料庫 Primary Key，UUID 還是 auto-incrementing ID'
ShowToc = true
TocOpen = true
summary = '不同的資料庫 Primary Key 格式，影響到的不只有儲存方式。選擇錯誤，甚至導致資料庫性能大幅下降。'
+++

# 為什麼需要知道要選擇 PK 還是流水號？

兩種不同的主鍵（Primary Key，以下簡稱 PK）代表的是不同的架構設計邏輯。

- 根據資料庫的運作方式 (auto-incrementing ID，以下簡稱 流水號)，可以讓資料庫的索引速度加快，但實際上有資安問題。
- 選擇 UUID 可能會導致資料庫索引降低，且大幅增加消耗資料庫存儲空間。

- 流水號（Int32 舉例）儲存大小為 32bit
- UUID 儲存大小為 128bit

兩者相差了 4 倍。

## 流水號 (Auto-Incrementing ID) 的缺點

例如有個使用者的 PK 為 100。那通常 URL 就會是 GET \user\id\100。此時用戶就可以猜測資料庫前面已經有 100 筆的資料，就可以爆破每一個使用者的資料了。

如果有兩個以上的 DB Server 要同時新增一個使用者，有可能會導致出現彼此都有 101 的 ID，但實際記載的使用者是不同的，也就是碰撞問題。

## UUID 的缺點

首先 UUID 的大小為 128bit。且由於隨機的特性，會導致資料庫在新增紀錄時，不斷進行 Page Split。例如說：今天新增一筆紀錄就要先新增一個 Page，才能在 Page 中，新增 Record。跟流水號比起來，只要在 Page 當中新增一筆 Record 就好來說，具有顯著的性能瓶頸。

其次，由於隨機的特性，並不像是流水號一樣有順序，結果導致 Index Page 實際上並沒有幫助到索引，反而導致了另外一個次的多餘讀取（Index Page 的 Full Scan + Data Page 的 Full Scan）。導致更多的 Fragment Index。

最後 UUID 的儲存大小是 128bit，相比起 Int32 或者 Int64 都大上了不少。

## 如何選擇？

如果今天是一台 Monolithic Server 時，最好的方式是使用 Auto-Incrementing ID，因為只有一台 Server，就不會有碰撞問題。

如果今天是多台 Server 時，就不適合使用 Auto-Incrementing ID。最好的方式是使用 UUID，並且使用多種手端緩解其帶來的缺點。

- UUID 的缺點根源就在於其是純隨機的，因此如果能讓 UUID 可以排序，就可以緩解所有缺點。其實以上我們討論純隨機的 UUID 都是基於 UUIDv4 實現的，而可以排序的 UUID 其實有很多，例如 v1, v2, v7...等等。最常見的就是 UUIDv7，根據時間作為排序。

- 在資料庫的設計中，更多人把 UUID 儲存為 `varchar(36)`，由於 `550e8400-e29b-41d4-a716-446655440000` 其長度為 `36`個字元，但這樣會實際儲存的大小遠超過 128bit，實際上儲存的是 36byte，如果要儲存 UUID 正確設計是使用 `binary(16)` 或者資料庫提供的 `UUID` 類型。

# Reference

- [Auto-Increment vs UUID Explained in 5 Minutes](https://youtu.be/JbdvmQ_HgJo?si=T-C248mRU6kRG4Pc)

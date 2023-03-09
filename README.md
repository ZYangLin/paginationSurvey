# 分頁機制說明文件

---

#### Introduce of Pagination

<img src="https://images.unsplash.com/photo-1589998059171-988d887df646?ixlib=rb-4.0.3&ixid=MnwxMjA3fDB8MHxzZWFyY2h8Mnx8b3BlbiUyMGJvb2t8ZW58MHx8MHx8&w=1000&q=80" height="300" width="500" />

:::spoiler 
當一次要呈現在前端的資料過多時，我們可以使用分頁的方式將大量的資料分成數份，一部分一部分的逐步呈現。 因此可以減輕後端資料庫負載以及減輕前端效能的問題。 分頁大致分成兩種類型
:::

---

#### Offset pagination
- Parameters
    - limit: 分頁要呈現的行數
    - offset: 要跳過的行數 *offset = (page-1) * limit*
    
    ex. 假設共 12 筆資料，分頁每次顯示 3 筆(limit=3)，當 page 3 時 offset= (3-1)*3 = 6， 代表 page 3 要從第7筆資料開始算，傳送 7, 8, 9 這三筆回前端。
    
- database
    1. SELECT * FROM table ORDER BY timestamp OFFSET  0 LIMIT 3
    2. SELECT * FROM table ORDER BY timestamp OFFSET  3 LIMIT 3
    3. SELECT * FROM table ORDER BY timestamp OFFSET  6 LIMIT 3
    4. SELECT * FROM table ORDER BY timestamp OFFSET  9 LIMIT 3   

----

##### Offset pagination(normal)
![](https://i.imgur.com/klwHgvf.jpg)


----

##### Offset pagination (skipped data)
![](https://i.imgur.com/18uV5xs.png)

----

##### Offset pagination (duplicated data)
![](https://i.imgur.com/a9m5GEu.png)

---

#### Cursor pagination

- Parameters
    - cursor: like a bookmark pointing the next time you want to start query

- database
    - SELECT * FROM table WHERE cursor > timestamp ORDER BY timestamp LIMIT 5

- disadvantages of Cursors
    - limite sort features
        - 實作上需要有一個具備`唯一性`且`順序性`的欄位(所以大多都使用時戳)。 因此對於查詢需求是要針對用戶姓名排序時cursor會有實作上的困難。而另外用複合主鍵的方式來當唯一值雖然可行但效果不會比只使用單一欄位來的好。

---

#### Between offset and cursor pagination



| 差異                 | offset                       | cursor                                     |
| -------------------- | ---------------------------- | ------------------------------------------ |
| 使用時機             | data is static               | data isn't static                          |
| 實作困難度           | 較易                         | 較難                                       |
| 當要分頁的資料越多時 | 前端需要花越久的時間呈現資料 | 理論上每一分頁呈現資料的時間雷同且不會太久 |


---

#### 資料撈取方在後端關聯式DB以及在主機
![](https://i.imgur.com/leso6am.jpg)

---

###### tags: `pagination`, `zzzzzYang`

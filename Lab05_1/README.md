# Lab05_1
## 1. 函數相依性分析   
### 原始屬性集合：

  - 書籍：ISBN、書名、作者 (多位)、出版社名稱、出版社地址、出版年份、書籍類別

  - 會員：會員卡號、會員姓名、會員地址、會員電話、會員Email

  - 借閱：會員卡號、會員姓名、ISBN、書名、借閱日期、應還日期、實際歸還日期

### 潛在異常：

- 重複：作者重複、出版社重複、會員重複出現

- 插入異常：沒有書籍就無法新增作者資料

- 更新異常：修改某位作者或出版社會影響多筆資料

- 刪除異常：刪除一本書，可能導致作者資料遺失

### 函數相依性列表：

#### 書籍(Book)

- ISBN → 書名、出版年份、書籍類別、出版社名稱

- ISBN → 作者

- 出版社名稱 → 出版社地址

#### 會員(Member)

- 會員卡號 → 姓名、地址、電話、Email

#### 借閱紀錄(Borrowing)

- (會員卡號, ISBN, 借閱日期) → 應還日期、實際歸還日期



## 2.正規化設計
### 書籍:
- `Book` (ISBN, Title, Year, Category, PublisherName)
- `Publisher` (PublisherName, PublisherAddress)
- `Author` (AuthorID, AuthorName)
- `BookAuthor` (ISBN, AuthorID)   -- 多對多關係
### 會員:
- `Member` (CardID, Name, Address, Phone, Email)
### 借閱紀錄:
- `Borrowing` (BorrowID, CardID, ISBN, BorrowDate, DueDate, ReturnDate)

### 正規化過程

#### 1NF:移除重複與集合欄位
- 將多個作者展開為一筆一筆資料（即一行只對應一位作者）
- 替代原本一欄多作者（例如「王大明、陳小美」）的欄位
- 符合「每個欄位都是不可分的原子值」

#### 2NF:消除部分依賴
- 從借閱資料中拆出：書籍資訊（由 ISBN 決定）、會員資訊（由卡號決定）
- 每一個資料表只能依賴主鍵本身，而不能只依主鍵的一部分
- 書名不應放在借閱表，因為只依賴 ISBN，不依賴複合鍵（CardID, ISBN, 借閱日）

#### 3NF
- `出版社名稱` → `出版社地址` → 傳遞依賴
- 拆出 `Publisher` 表，讓 `Book` 表只記錄出版社名稱
- 資料應該直接依賴主鍵，不能間接依賴主鍵
## ER-D圖

![Lab05_01](https://github.com/Fukulyn/Lab-05/blob/main/Lab-05_1/Lab05_01.png)

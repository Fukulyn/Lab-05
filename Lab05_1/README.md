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

#### 書籍 (Book)
- `Book` → (ISBN, Title, Year, Category, PublisherName)
#### 書籍作者 (BookAuthor)
- `BookAuthor` → (ISBN, AuthorID)   -- 多對多關係
#### 出版社 (Publisher)
`Publisher` → (PublisherName, PublisherAddress)
#### 作者 (Author)
- `Author` → (AuthorID, AuthorName)
#### 會員(Member)
- `Member` → (CardID, Name, Address, Phone, Email)
#### 借閱紀錄(Borrowing)
-  `Borrowing` → (BorrowID, CardID, ISBN, BorrowDate, DueDate, ReturnDate)

## 2. 正規化過程

#### 1NF:移除重複與集合欄位
- 將多個作者展開為一筆一筆資料（即一行只對應一位作者）
- 替代原本一欄多作者（例如「王大明、陳小美」）的欄位
- 符合「每個欄位都是不可分的原子值」

#### 2NF:消除部分依賴
- 從借閱資料中拆出：書籍資訊（由 ISBN 決定）、會員資訊（由卡號決定）
- 每一個資料表只能依賴主鍵本身，而不能只依主鍵的一部分
- 書名不應放在借閱表，因為只依賴 ISBN，不依賴複合鍵（CardID, ISBN, 借閱日）

#### 3NF:消除傳遞依賴
- `出版社名稱` → `出版社地址` → 傳遞依賴
- 拆出 `Publisher` 表，讓 `Book` 表只記錄出版社名稱
- 資料應該直接依賴主鍵，不能間接依賴主鍵

## 3.正規化資料表設計

- ### Book
  | 欄位名稱       | 說明             |
  |----------------|------------------|
  | `ISBN`         | 主鍵，書籍代碼   |
  | `Title`        | 書名             |
  | `Year`         | 出版年份         |
  | `Category`     | 書籍類別         |
  | `PublisherName`| 外鍵，對應出版社 |
- ### Author
  | 欄位名稱     | 說明       |
  |--------------|------------|
  | `AuthorID`   | 主鍵       |
  | `AuthorName` | 作者姓名   |
- ### BookAuthor（多對多關聯表）
  | 欄位名稱   | 說明                          |
  |------------|-------------------------------|
  | `ISBN`     | 外鍵，對應書籍                |
  | `AuthorID` | 外鍵，對應作者                |
- ### Publisher
  | 欄位名稱        | 說明         |
  |-----------------|--------------|
  | `PublisherName` | 主鍵         |
  | `PublisherAddress` | 出版社地址 |
- ### Member
  | 欄位名稱   | 說明             |
  |------------|------------------|
  | `CardID`   | 主鍵，會員卡號   |
  | `Name`     | 姓名             |
  | `Address`  | 地址             |
  | `Phone`    | 電話             |
  | `Email`    | 電子郵件         |
- ### Borrowing
  | 欄位名稱     | 說明                        |
  |--------------|-----------------------------|
  | `BorrowID`   | 主鍵，借閱編號              |
  | `CardID`     | 外鍵，對應會員              |
  | `ISBN`       | 外鍵，對應書籍              |
  | `BorrowDate` | 借閱日期                    |
  | `DueDate`    | 應還日期                    |
  | `ReturnDate` | 實際歸還日期（可為 NULL）   |
## 4. ER圖

![Lab05_01](https://github.com/Fukulyn/Lab-05/blob/main/Lab05_1/Lab05_01.png)

# Lab05_2
## 1.分析原始扁平化資料
| 訂單ID| 顧客ID | 顧客姓名 | Email | 電話 | 地址 | 產品ID | 產品名稱 | 數量 | 單價 | 訂購日期 | 訂單總金額 | 供應商名稱 | 供應商聯絡方式|
| ----- | ----- | -------- | ---- | ----- | ----- | ---- | ------- | ---- | --- | ------- | ---------- | --------- | ------------|
- 缺點: 資料冗長，並容易產生資料重複與操作異常

## 2. 函數相依性分析
### 客戶相關資料
- CustomerID → CustomerName, Email, Phone, Address
### 產品資料相關
- ProductID → ProductName, UnitPrice, Stock, SupplierID
- SupplierID → SupplierName, SupplierContact (如果一個供應商名稱唯一)
### 訂單資料相關
- OrderID → CustomerID, OrderDate, OrderTotal
- (OrderID, ProductID) → Quantity, UnitPrice
- P.S產品在每張訂單可能有不同數量與單價（促銷、折扣等）

## 3. 正規化步驟
### 1NF:展開每筆商品為獨立列
- 每筆商品對應一列，不合併多商品資訊於一行
- 消除重複欄位與集合型資料結構
- 把原來的扁平化表格分為多張表：

  - 顧客`Customer`
  - 產品`Product`
  - 訂單`Order`
  - 訂單細項`OrderDetail`
  - 供應商`Supplier`
### 2NF:消除部分相依
- 在 `OrderDetail` 表中，屬於 `Product` 的資料（如 `ProductName`, `UnitPrice`）

   不該依賴複合鍵 (`OrderID`, `ProductID`) 的一部分，而是應拆到 `Product` 表。
- 以此類推 `Customer` 的資料也從 `Order` 拆出來
- 訂單明細中的資訊應該來自訂單與商品的主資料
### 3NF:移除傳遞依賴
- `SupplierContact` 不該依賴 `Product`，而應依賴 `Supplier`，將其獨立為 `Supplier` 表。
- `Address` 結構正規分拆為：`Street`、`City`、`PostalCode`、`Country`。

## 4.資料表概念
- ### Customer (顧客相關)
  | 欄位名稱     | 說明           |
  |--------------|----------------|
  | `CustomerID` | 主鍵           |
  | `Name`       | 姓名           |
  | `Email`      | 電子郵件       |
  | `Phone`      | 電話           |
  | `Address`    | 地址           |
- ### Product (產品相關)
  | 欄位名稱     | 說明              |
  |--------------|------------------|
  | `ProductID`  | 主鍵             |
  | `ProductName`| 產品名稱         |
  | `UnitPrice`  | 單價              |
  | `Stock`      | 庫存數量          |
  | `SupplierID` | 外鍵，對應供應商  |
- ### Supplier (供應商相關)
  | 欄位名稱      | 說明         |
  |---------------|--------------|
  | `SupplierID`   | 主鍵         |
  | `SupplierName` | 名稱         |
  | `Contact`      | 聯絡資訊     |
- ### Order (訂單相關)
  | 欄位名稱     | 說明              |
  |--------------|-------------------|
  | `OrderID`    | 主鍵              |
  | `CustomerID` | 外鍵，對應顧客    |
  | `OrderDate`  | 訂單日期          |
  | `TotalAmount`| 訂單總額          |
- ### OrderDetail
  | 欄位名稱     | 說明                             |
  |--------------|---------------------------------|
  | `OrderID`    | 外鍵，對應訂單                   |
  | `ProductID`  | 外鍵，對應產品                   |
  | `Quantity`   | 購買數量                         |
  | `UnitPrice`  | 當時單價                         |

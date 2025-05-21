# Lab05_2
## 1.分析原始扁平化資料
| 訂單ID| 顧客ID | 顧客姓名 | Email | 電話 | 地址 | 產品ID | 產品名稱 | 數量 | 單價 | 訂購日期 | 訂單總金額 | 供應商名稱 | 供應商聯絡方式|
| ----- | ----- | -------- | ---- | ----- | ----- | ---- | ------- | ---- | --- | ------- | ---------- | --------- | ------------|
- 缺點: 資料冗長，並容易產生資料重複與操作異常

## 2. 函數相依性分析
### 客戶相關資料
- CustomerID → CustomerName, Email, Phone, Address
### 產品資料相關
- ProductID → ProductName, UnitPrice, SupplierName
- SupplierName → SupplierContact (如果一個供應商名稱唯一)
### 訂單資料相關
- OrderID → CustomerID, OrderDate, OrderTotal
- (OrderID, ProductID) → Quantity, UnitPrice
- P.S產品在每張訂單可能有不同數量與單價（促銷、折扣等）
## 3. 正規化步驟
### 1NF:

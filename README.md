#MATRIX R4 2025-2026
本專案控制系統完整架構與流程圖，包含主迴圈、手動遙控、AUTO 與 PID 閉迴路循線控制。


## 檔案說明
- `matrix_修改版.mbr4`：mBlock 專案/自訂積木原始碼（底層為 XML 架構）。

## 如何使用
1. 下載 `matrix_修改版.mbr4`。
2. 開啟 mBlock 軟體。
3. 選擇「匯入」或「載入積木」即可使用，不需直接以文字編輯器開啟原始碼。










# MATRIX 控制系統流程圖


點選後，使用draw.io即可看到完整流程圖:

https://drive.google.com/file/d/1Z6-qCEuw7wQVbTequVIBizeldXjOdrsZ/view?usp=sharing
---

## 1. 主迴圈 (Main Loop)
> 開機啟動、系統初始化 與 模式選擇
<img width="2115" height="1989" alt="MATRIX_流程圖-主迴圈 drawio" src="https://github.com/user-attachments/assets/30275984-6559-408c-8f1c-1ec081f2338b" />



---

## 2. 手動模式 (Manual Mode)
> 雙搖桿差速底盤控制、按鍵控制夾子高度、吊掛與夾爪開合

<img width="2733" height="1908" alt="MATRIX_流程圖-手動模式 drawio" src="https://github.com/user-attachments/assets/92f8209d-5909-4895-b80d-c53fdd85ace2" />



---

## 3. 自動模式 (AUTO Mode)
> 預設路徑導航、馬達編碼器同步與自走任務巡航

<img width="987" height="1410" alt="MATRIX_流程圖-自動模式 drawio" src="https://github.com/user-attachments/assets/8c57e86f-b283-4b93-a2c2-f27e30c0e06a" />


---

## 4. PID 閉迴路循線 (PID Controller)
> 灰階感測器讀值、pid計算

<img width="2223" height="2163" alt="MATRIX_流程圖-PID閉迴路循線 drawio" src="https://github.com/user-attachments/assets/1c471baa-8797-4169-b44b-0e3a53f23ce2" />

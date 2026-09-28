# MATRIX 控制系統流程圖

本專案控制系統完整架構與流程圖，包含開機主迴圈、手動遙控、AUTO 預設路徑與 PID 閉迴路循線控制。

---

## 1. 主迴圈 (Main Loop)
> 開機啟動、系統初始化與 PS2 遙控器三角鍵切換模式

<img width="2115" height="1989" alt="MATRIX_流程圖-主迴圈 drawio" src="https://github.com/user-attachments/assets/30275984-6559-408c-8f1c-1ec081f2338b" />



---

## 2. 手動模式 (Manual Mode)
> 雙搖桿差速驅動底盤、按鍵控制夾子高度、吊掛與夾爪開合

<img width="2733" height="1908" alt="MATRIX_流程圖-手動模式 drawio" src="https://github.com/user-attachments/assets/92f8209d-5909-4895-b80d-c53fdd85ace2" />



---

## 3. 自動模式 (AUTO Mode)
> 預設路徑導航、馬達編碼器同步與自走任務巡航

<img width="987" height="1410" alt="MATRIX_流程圖-自動模式 drawio" src="https://github.com/user-attachments/assets/8c57e86f-b283-4b93-a2c2-f27e30c0e06a" />


---

## 4. PID 閉迴路循線 (PID Closed-Loop)
> 灰階感測器讀值、P 比例偏差、I 積分累加、D 微分變化率及差速輸出運算

<img width="2223" height="2163" alt="MATRIX_流程圖-PID閉迴路循線 drawio" src="https://github.com/user-attachments/assets/1c471baa-8797-4169-b44b-0e3a53f23ce2" />

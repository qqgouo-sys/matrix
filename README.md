# MATRIX Mini R4 控制系統流程圖

本專案控制系統完整架構與流程圖，包含開機主迴圈、手動遙控、AUTO 預設路徑與 PID 閉迴路循線控制。

> 📄 **Draw.io 原始檔**：可至 [Google Drive MATRIX 資料夾](https://drive.google.com/file/d/1Z6-qCEuw7wQVbTequVIBizeldXjOdrsZ/view?usp=drivesdk) 下載或檢視。

---

## 1. 主迴圈 (Main Loop)

```mermaid
flowchart TD
    Start([開機啟動 Power On]) --> Init[系統初始化]
    Init --> Poll[讀取 PS2 搖桿訊息]
    
    Poll --> ChkTri{如果 triangle △ 被按下？}
    ChkTri -- 是 (切換) --> Toggle["模式改變：<br/>ON_OFF = |ON_OFF - 1|"] --> ChkMode
    ChkTri -- 否 --> ChkMode{檢查目前模式 ON_OFF 數值}
    
    ChkMode -- ON_OFF == 0<br/>(手動) --> Manual[手動模式]
    ChkMode -- ON_OFF == 1<br/>(自動) --> Auto[自動模式]
    
    Manual --> Poll
    Auto --> Poll

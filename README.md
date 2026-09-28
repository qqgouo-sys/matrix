# matrix
# MATRIX Mini R4 機器人競賽控制系統

本專案為基於 **MATRIX Mini R4** 控制器開發的競賽機器人完整控制系統，整合 PS2 遙控手動操作與全自主導航循線（AUTO）模式，並採用灰階 PID 閉迴路控制、雷射測距避障與多軸伺服機構協同運作。

---

## 系統硬體配置

* **主控核心**：MATRIX Mini R4 控制器（Arduino R4 架構）
* **底盤動力**：雙直流編碼馬達差速底盤（M1 左輪 / M2 右輪反轉，支援主動煞車鎖定）
* **感測器模組**：
  * **I2C 雷射測距感測器 (MXlaser)**：連接 I2C1，負責前向動態定距停煞與防撞。
  * **雙路光電灰階感測器**：連接類比 A1 / A3，負責路面黑白線軌跡偵測。
  * **板載 6 軸 IMU 陀螺儀**：負責精準角度轉向（Yaw Angle）閉迴路控制。
  * **雙輪編碼器 (Encoder)**：即時反饋輪胎轉動圈數，進行定距巡航。
* **機構與人機介面**：
  * **伺服馬達 (RC1 ~ RC4)**：控制升降機構（`lift`）與物料夾爪（`CLIP` / `openww`）。
  * **PS2 2.4G 無線手把**：手動搖桿操縱與功能模式即時切換。
  * **SSD1306 I2C OLED 螢幕**：即時顯示目前車體狀態（`MANUAL` / `AUTO`）。
  * **板載蜂鳴器 (Buzzer)**：模式切換與狀態提示音。

---

## 核心控制模式

系統開機完成硬體初始化後進入主迴圈，透過 PS2 手把的 **`SELECT` 鍵** 可隨時切換以下兩種模式：

### 1. 手動遙控模式 (MANUAL)
* **底盤運動**：透過 PS2 雙類比搖桿讀值，經差速運動學演算法即時計算左右輪輸出速度（`M1` / `M2`）。
* **機構動作**：
  * 圖形鍵（`△` / `✕` / `□` / `○`）：切換升降機構高度（`HIGH` / `HANG`）。
  * 肩鍵（`L1` / `L2` / `R1` / `R2`）：控制夾爪開合抓取（`CLIP` / `openww` / `folder`）。

### 2. 自主導航模式 (AUTO)
自走流程採用**預設路徑（Preset Path）**為骨架，並依任務需求融合四大控制方式：

| 控制方式 | 對應程式函式 | 控制原理與應用場景 |
| :--- | :--- | :--- |
| **PID 閉迴路控制** | `fole` / `fole_distant` | 即時比對雙路灰階誤差，透過比例（$K_p$）、積分（$K_i$）、微分（$K_d$）連續計算差速轉向修正量，保持中心穩定巡線。 |
| **雷射測距控制** | `MXlaser` / `fole_distant` | I2C 雷射即時監控前方障礙物或料架間距，作為動態到達煞停條件，防止硬衝撞。 |
| **灰階感測控制** | `GS` / `step_line` | 根據左右灰階反射門檻值，進行定距快衝、十字黑線過線計數與倒車定位校正。 |
| **寫死 / 開環控制** | `turntwo` / `wait` / `RCset` | 雙輪開環自轉（90°/180°）、微調延遲消除車身慣性、以及伺服馬達夾爪固定角度控制。 |

---

## PID 循線控制演算法

自走巡線採用**離散化數值微積分**架構，於每個控制週期即時動態計算：

1. **偏差量計算（比例項 P）**：
   $$\text{count\_error} = \text{right} - \text{left}$$
2. **累積誤差計算（積分項 I）**：
   $$\text{Line\_integral} = \text{Line\_integral} + \text{count\_error} \quad (\text{限幅 } -1000 \sim 1000)$$
   *(具備 Anti-windup 抗積分飽和限制)*
3. **變化率計算（微分項 D）**：
   $$\text{Line\_derivative} = \text{count\_error} - \text{Line\_last\_error}$$
4. **輸出修正量**：
   $$\text{Line\_correction} = K_p \times \text{count\_error} + K_i \times \text{Line\_integral} + K_d \times \text{Line\_derivative}$$
5. **左右輪差速分配**：
   $$\text{M1 (左輪)} = \text{base\_speed} - \text{Line\_correction}$$
   $$\text{M2 (右輪)} = \text{base\_speed} + \text{Line\_correction}$$

---

## 系統流程圖

```mermaid
flowchart TD
    Start([開機啟動]) --> Init[系統硬體與姿態初始化]
    Init --> Loop[主迴圈輪詢 PS2 手把]
    Loop --> CheckKey{是否按下 SELECT 鍵？}
    CheckKey -- 是 --> Toggle[切換 ON_OFF 狀態<br/>蜂鳴器發出提示音] --> ModeCheck
    CheckKey -- 否 --> ModeCheck{判斷目前模式}
    
    ModeCheck -- ON_OFF == 0 --> Manual[手動模式 MANUAL<br/>搖桿差速驅動 / 按鍵機構控制]
    ModeCheck -- ON_OFF == 1 --> Auto[自動模式 AUTO<br/>執行預設路徑與四大控制]
    
    Manual --> Loop
    Auto --> Loop

# Animal-vs-Urban-Sound-Random-Forest
# 🎧 Animal vs Urban Noise Classification with Random Forest

本專案使用 [ESC-50]開源資料集中的動物聲音與城市噪音，進行二元音訊分類任務。
透過 Mel-spectrogram 特徵擷取與經典機器學習演算法 `Random Forest`，在有限資料條件下成功建立一個準確且高效的分類模型。

為展示用途，`sample/` 目錄下包含幾筆來自 ESC-50 的聲音樣本，涵蓋兩類別：

- `Animals` 類別（如貓叫、狗吠）
- `Urban` 類別（如汽車聲、喇叭聲）

這些檔案僅供範例使用，完整資料請參考 [ESC-50](https://github.com/karoldvl/ESC-50)。
---

## 📌 專案動機

在實際應用中，我們常需快速區分不同類型的環境聲音，例如：
- 聲控裝置判斷是貓叫還是車聲
- 特定場景需區分背景音與生物聲音

本專案聚焦於 `Animals` 與 `Urban noises` 兩大類別，並驗證傳統機器學習方法是否能有效完成這項任務。
---

## 🔧 技術流程

### 🎼 特徵工程
- 將 `.wav` 檔轉換為 **Mel-spectrogram**
- 計算每段音訊的：
  - 平均值（Mean）
  - 標準差（Standard Deviation）
- 使用上述統計特徵作為模型輸入（共 128 維）

### 🔁 資料增強
- 對訓練資料進行隨機增強：
  - 加入微量高斯雜訊
  - 時間平移
  - 音量縮放
- 每筆原始音檔擴增為 3 筆資料（原始 + 2 筆增強）

### 📂 資料切割
- 80%：訓練集（含資料增強）
- 20%：測試集（不含任何增強）
- 分類標籤：
  - `Animals`: 0
  - `Urban noises`: 1

### 🧠 模型訓練
- 使用 `RandomForestClassifier`：
  - `n_estimators=100`
  - `max_depth=5`
  - 使用 6-fold 交叉驗證驗證穩定性
---

## 📈 模型表現

### ✅ 交叉驗證結果（Train Set）
```
k-fold: [0.8398, 0.8359, 0.8711, 0.8711, 0.8398, 0.8594]
平均準確率: 85.29%
```

### 🧪 測試集結果

| 評估指標    | 數值     |
|-------------|----------|
| Accuracy    | 86.72%   |
| Precision   | 87.25%   |
| Recall      | 86.72%   |
| F1-Score    | 86.53%   |

### 📊 混淆矩陣

![Confusion Matrix](random_forest_confusion_matrix.png)

|               | Pred: Animals | Pred: Urban |
|---------------|----------------|--------------|
| **True: Animals** | 68             | 4            |
| **True: Urban**   | 13             | 43           |

模型在 Animals 分類表現相當穩定（Precision ≈ 94.4%），但 Urban noises 類別略有誤判，可能與背景聲音混淆性高有關。

---

## 🔮 延伸方向

- 將類別擴展為多元分類（如加入自然聲音）
- 將模型換為 XGBoost 或 LightGBM 比較效能
- 加入 MFCC 或時間序列特徵以提升精度

📌 本專案展示了即使在資料量有限下，透過適當的資料增強與穩定的機器學習模型，也能達到預期的分類效果。

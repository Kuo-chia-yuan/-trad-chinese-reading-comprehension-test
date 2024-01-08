# trad-chinese-reading-comprehension-test

## 介紹
學習大語言模型，挑戰中文閱讀測驗

## 程式架構
 1. 資料讀取：讀取 AI.xlsx 和 AI1000.xlsx 兩個檔案當作 train data 和 test data。
 3. 自訂資料及類別：定義一個自訂的資料集類別（CustomDataset）用於訓練資料，這個類別使用 BERT 分詞器對輸入文本進行分詞，並使用 label_mapping 字典將標籤轉換為數值、使用 DataLoader 來處理訓練資料的批次。
 4. 分詞器：使用 BERT 分詞器（BertTokenizer）對文本進行分詞。
 5. 模型定義：載入了用於序列分類的 BERT 模型（BertForSequenceClassification），並指定標籤的數量 (選項1 ~ 選項4)。
 6. 訓練迴圈：程式碼通過逐批次的訓練迴圈，將每個批次的資料傳遞給模型進行訓練，並使用 AdamW 優化器和交叉熵損失進行模型的訓練。
 7. 評估：程式碼使用已訓練的模型對測試資料進行預測。預測的標籤被保存到一個 DataFrame 中，並匯出到一個 CSV 檔案。

## 選用模型
 - BERT：bert-base-chinese

```
model = BertForSequenceClassification.from_pretrained('bert-base-chinese', num_labels=4)
```

## function & 參數設定
 1. optimizer = AdamW
 2. loss function = CrossEntropyLoss
 3. max_length = 128
 4. batch_size = 32
 5. learning rate = 2e-5
 6. epoch = 1

## 訓練結果
 - 最終 loss 降至 1.35 左右

## 解決問題 & 學習內容
 - 手動將 AI.xlsx 中的文章、問題、4個選項、正確答案分別取出，以利後續建立 dataloader

   ![image](https://github.com/Kuo-chia-yuan/trad-chinese-reading-comprehension-test/assets/56677419/253c431d-4f42-4ced-b2b8-525d1292d798)

 - 建立 CustomDataset 定義各項資料、tokenizer、max_length (單筆資料的最大長度)，並建立 encoding 將資料轉成數字，以利後續的模型訓練

   ![image](https://github.com/Kuo-chia-yuan/trad-chinese-reading-comprehension-test/assets/56677419/cdfefac2-ced8-4e0c-a51c-8f8efe8e5f18)

 - 由於是訓練一個已預訓練好的模型，因此它會包含較多變數，相對需要更多 RAM 或磁碟空間。為了避免訓練時空間不足，在每訓練完一個 batch 之前和之後，我都會釋放不必要的變數，只保留會參與前向和反向傳播的變數

   ![image](https://github.com/Kuo-chia-yuan/trad-chinese-reading-comprehension-test/assets/56677419/2b4ce8ab-a04e-45da-9229-de77b76abc9e)

 - 學習如何調用其他已續訓練好的模型，並自己手動進行 fine tune，讓模型更符合目標的需求

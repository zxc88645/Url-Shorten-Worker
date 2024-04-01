# owotool 短連結生成器

利用 Cloudflare Worker 實現的簡單免費的短連結平台，
主要支援自用以及防濫用演示，

# API

[中文 API 文档](API.md)

# Getting start

### 去 Workers KV 建立一個命名空間

Go to Workers KV and create a namespace.

![img](readme/20201205232805.png)

### 去 Worker 的 Settings 選選項卡中綁定 KV Namespace

Bind an instance of a KV Namespace to access its data in a Worker.

![img](readme/20201205232536.png)

### 其中 Variable name 填入`LINKS`, KV namespace 填入你剛剛建立的命名空間

Where Variable name should set as `LINKS` and KV namespace is the namespace you just created in the first step.

![img](readme/20201205232704.png)

### 複製本專案中的`index.js`的程式碼到 Cloudflare Worker

Copy the `index.js` code from this project to Cloudflare Worker.

### 調整白名單

白名單中的網域設定短連結無視超時，
修改腳本開頭的變數*white_list*, 是個 json 數組，寫頂級域名就可以，自動通過頂級域名和所有二級域名，

### 關閉演示模式

只有示範模式開啟才允許訪客無密碼新增非白名單位址，超時短連結會失效，
修改腳本開頭的變數*demo_mode*，為 true 開啟演示，為 false 無密碼且非白名單請求不予受理，

### 自動刪除示範記錄

針對演示模式開啟情況下的超時失效的短連結記錄是否自動刪除，
修改腳本開頭的變數*remove_completely*，為 true 自動刪除逾時的演示短連結記錄，否則僅是標記過期，以便在後台查詢歷史記錄，

### 修改密碼

網頁有個隱藏輸入框可以輸入密碼，
密碼正確情況無視白名單和超時設置，且支持自定義短鏈接，
修改腳本開頭的變數*password*，這個私密資訊比較建議直接在環境變數裡配置，

### 修改短鍊長度

短鍊長度就是隨機產生的 key 是短連結的 path 部分的長度，
長度不夠時容易重複，遇到重複時會自動延長，
修改腳本開頭的變數*default_len*,

### 以上幾個配置都可以在 worker -> 設定 -> 環境變數中配置，key 皆為對應大寫，

![img](readme/cfWorderEnvironment.png)

### 點選 Save and Deploy

Click Save and Deploy

# Demo

https://020.name

Note: Because someone abuse this demo website, all the generated link may be deleted at any time. For long-term use, please deploy your own.

注意：由於此範例服務被濫用，用於轉發詐騙網站，故所有由 demo 網站產生的連結隨時可能失效，如需長期使用請自行搭建。

## 感谢

專案基於[xyTom/Url-Shorten-Worker](https://github.com/xyTom/Url-Shorten-Worker)[(MIT)](https://github.com/xyTom/Url-Shorten-Worker/blob/main/LICENSE)

1. 改進了正規匹配。
2. 增加了超時判斷處理。
3. 新增了白名單支持。
4. 新增了演示模式。
5. 新增了隱藏密碼支持。
6. 新增了隱藏自訂短連結支持。
7. 所有配置可以脫離腳本在環境變數配置。
8. 支援回車鍵提交。
9. 產生的短連結包含協定 https。
10. 新增了短鍊長度設定和自動延長。
11. 支援開關自動刪除演示過期記錄。

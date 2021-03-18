# AutoApi v6.1 (2021-2-10) ———— E5自動續期
AutoApi系列：~~AutoApi(v1.0)~~、~~AutoApiSecret(v2.0)~~、~~AutoApiSR(v3.0)~~、~~AutoApiS(v4.0)~~、~~AutoApiP(v5.0)~~

## 說明 ##
E5自動續期程式，但是**不保證續期**
* 設置了**週六日(UTC時間)不啟動**自動調用，周1-5每6小時自動啟動一次 （修改看教程）
* 調用api保活：
     * 查詢系api：onedrive,outkook,notebook,site等
     * 創建系api: 自動發送郵件，上傳檔，修改excel等
     
### 相關 ###
* AutoApi: https://github.com/wangziyingwen/AutoApi
* **錯誤及解決辦法/續期相關知識/更新日誌**：https://github.com/wangziyingwen/Autoapi-test
   * 大部分錯誤說明已更新進程式，詳細請運行後看action日誌報告
* 視頻教程：
   * B站：https://www.bilibili.com/video/BV185411n7Mq/

## 步驟 ##
* 準備工具：
   * E5開發者帳號（**非個人/私人帳號**）
       * 管理員號 ———— 必選 
       * 子號 ———— 可選 （不清楚微軟是否會統計子號的活躍度，想弄可選擇性補充運行）    
   * rclone軟體，[下載位址 rclone.org ](https://downloads.rclone.org/v1.53.3/rclone-v1.53.3-windows-amd64.zip)，(windows 64）
   * 教程圖片看不到請科學上網
   
* 步驟大綱：
   * 微軟方面的準備工作 （獲取應用id、密碼、金鑰）
   * GIHTHUB方面的準備工作  （獲取Github金鑰、設置secret）
   * 試運行
   
#### 微軟方面的準備工作 ####

* **第一步，註冊應用，獲取應用id、secret**

    * 1）點擊打開[儀錶板](https://aad.portal.azure.com/)，左邊點擊**所有服務**，找到**應用註冊**，點擊+**新註冊**
    
     ![image](https://github.com/wangziyingwen/ImageHosting/blob/master/AutoApiP/creatapp.png)
    
    * 2）填入名字，受支援帳戶類型前三任選，重定向填入 http://localhost:53682/ ，點擊**註冊**
    
    ![image](https://github.com/wangziyingwen/ImageHosting/blob/master/AutoApiP/creatapp2.png)
    
    * 3）複製應用程式（用戶端）ID到記事本備用(**獲得了應用程式ID**！)，點擊左邊管理的**證書和密碼**，點擊+**新用戶端密碼**，點擊添加，複製新用戶端密碼的**值**保存（**獲得了應用程式密碼**！）
    
    ![image](https://github.com/wangziyingwen/ImageHosting/blob/master/AutoApiP/creatapp3.png)
    
    ![image](https://github.com/wangziyingwen/ImageHosting/blob/master/AutoApiP/creatapp4.png)
    
    * 4）點擊左邊管理的**API許可權**，點擊+**添加許可權**，點擊常用Microsoft API裡的**Microsoft Graph**(就是那個藍色水晶)，
    點擊**委託的許可權**，然後在下面的條例選中下列需要的許可權，最後點擊底部**添加許可權**
    
    **賦予api許可權的時候，選擇以下13個**
  
                Calendars.ReadWrite、Contacts.ReadWrite、Directory.ReadWrite.All、
                
                Files.ReadWrite.All、Group.ReadWrite.All、MailboxSettings.ReadWrite、
                
                Mail.ReadWrite、Mail.Send、Notes.ReadWrite.All、
                
                People.Read.All、Sites.ReadWrite.All、Tasks.ReadWrite、
                
                User.ReadWrite.All
                               
    
    ![image](https://github.com/wangziyingwen/ImageHosting/blob/master/AutoApiP/creatapp5.png)
    
    ![image](https://github.com/wangziyingwen/ImageHosting/blob/master/AutoApiP/creatapp6.png)
     
    ![image](https://github.com/wangziyingwen/ImageHosting/blob/master/AutoApiP/creatapp8.png)
    
    * 5）添加完自動跳回到許可權首頁，點擊**代表授予管理員同意**
         
         如若是**子號**運行，請用管理員帳號登錄[儀錶板](https://aad.portal.azure.com/)找到**子號註冊的應用**，點擊“代表管理員授權”。 
    
    ![image](https://github.com/wangziyingwen/ImageHosting/blob/master/AutoApiP/creatapp7.png)
    
* **第二步，獲取refresh_token(微軟金鑰)**

    * 1）rclone.exe所在資料夾，shift+右鍵，在此處打開powershell，輸入下面**修改後**的內容，回車後跳出流覽器，登入e5帳號，點擊接受，回到powershell視窗，看到一串東西。
           
                ./rclone authorize "onedrive" "應用程式(用戶端)ID" "應用程式密碼"
               
    * 2）在那一串東西裡找到 "refresh_token"：" ，從雙引號開始選中到 ","expiry":2021 為止（就是refresh_token後面雙引號裡那一串，不要雙引號），如下圖，右鍵複製保存（**獲得了微軟金鑰**）
    
    ![image](https://github.com/wangziyingwen/ImageHosting/blob/master/AutoApi/token地方.png)
    
 ____________________________________________________
 
 #### GITHUB方面的準備工作 ####

 * **第一步，fork本項目**
 
     登陸/新建github帳號，回到本專案頁面，點擊右上角fork本項目的代碼到你自己的帳號，然後你帳號下會出現一個一模一樣的項目，接下來的操作均在你的這個項目下進行。
     
     ![image](https://github.com/wangziyingwen/ImageHosting/blob/master/AutoApi/fork.png)
     
 * **第二步，新建github金鑰**
 
    * 1）進入你的個人設置頁面 (右上角頭像 Settings，不是倉庫裡的 Settings)，選擇 Developer settings -> Personal access tokens -> Generate new token

    ![image](https://github.com/wangziyingwen/ImageHosting/blob/master/AutoApi/Settings.png)
    
    ![image](https://github.com/wangziyingwen/ImageHosting/blob/master/AutoApi/token.png)
    
    * 2）設置名字為 **GH_TOKEN** , 然後勾選repo，點擊 Generate token ，最後**複製保存**生成的github金鑰（**獲得了github金鑰**，一旦離開頁面下次就看不到了！）
   
   ![image](https://github.com/wangziyingwen/ImageHosting/blob/master/AutoApiP/repo.png)
  
 * **第三步，新建secret**
 
    * 1）依次點擊頁面上欄右邊的 Setting -> 左欄 Secrets -> 右上 New repository secret，新建6個secret： **GH_TOKEN、MS_TOKEN、CLIENT_ID、CLIENT_SECRET、CITY、EMAIL**  
   
    ![image](https://github.com/wangziyingwen/ImageHosting/blob/master/AutoApiP/setting.png)
    
    ![image](https://github.com/wangziyingwen/ImageHosting/blob/master/AutoApiP/secret2.png)
    
     **(以下填入內容注意前後不要有空格空行)**
 
     GH_TOKEN
     ```shell
     github金鑰 (第三步獲得)，例如獲得的金鑰是abc...xyz，則在secret頁面直接粘貼進去，不用做任何修改，只需保證前後沒有空格空行
     ```
     MS_TOKEN
     ```shell
     微軟金鑰（第二步獲得的refresh_token）
     ```
     CLIENT_ID
     ```shell
     應用程式ID (第一步獲得)
     ```
     CLIENT_SECRET
     ```shell
     應用程式密碼 (第一步獲得)
     ```
     CITY
     ```shell
     城市 (例如Beijing,自動發送天氣郵件要用到)
     ```
     EMAIL
     ```shell
     收件郵箱 (自動發送天氣郵件要用到)
     ```


________________________________________________

#### 試運行 ####

   * 1）點擊上欄中間的Action進入運行日誌頁面，中間應該有個綠色按鈕（I understand my workflow...），點擊。
   
       自動刷新後，會看到左邊有三個流程，一個Run api.Read，一個Run api.Write，一個Update Token。
       
         工作流程說明
             Run api.Write：創建系api，一天自動運行一次
             Run api.Read:  查詢系api，每6小時自動運行一次
             Update Token： 微軟金鑰更新，每2天運行一次
             
       這三個流程名字前面應該是都有黃色感嘆號的
   
       分別點進去，然後會看到有個黃條（this schedule was disabled......），點擊 enable workflow 按鈕，**三個流程都要按這個！**
   
       （不確定是否都需要進行這一步，我自己做視頻教程的時候發現有的。如果你沒有，直接忽略並往下進行，能正常運行就可以了 ）
   
   * 2）點擊兩次右上角的星星（star，就是fork按鈕的隔壁）啟動action，
   
        再點擊上面的Action選擇Run api.Read或者api.Write流程 -> build -> run api 就能看到每次的運行日誌

       （必需點進去build裡面的run api.XXX看下，api有沒有調用到位，操作有沒有成功，有沒有出錯）

        ![image](https://github.com/wangziyingwen/ImageHosting/blob/master/AutoApi/日誌.png)
     
   * 3）再點兩次星星，查看是否能再次成功運行
   
        然後點擊Action裡的 update token 流程 -> build -> update token ，日誌裡顯示“微軟金鑰上傳成功”。
       
        同時，依次點擊頁面上欄右邊的 Setting -> 左欄 Secrets（也就是Github方面準備的第三步的secret頁面），應該能看到MS_TOKEN顯示剛剛update了
        
        （這一步是為了保證重新上傳到secret的token是正確的）
    
        
#### 教程最後 ####

   程式會自行按計劃啟動，不必操心。
   
        但是github更新了防止薅羊毛的規則，如果倉庫60天無任何變動，將會暫停Action，但是會發郵件通知，所以請留意郵箱，收到郵件請上來手動啟動一下action。
       （我還沒有收到過此郵件，但是據說郵件裡會有啟動連結，或者上來按兩次星星按鈕就行）
   
   **P版（AutoApiP）用戶請留意是否會觸發此暫停規則，由於P版採取了新方案，是否能跳過github檢測活躍呢？如果P版收到暫停郵件，最好在issues的這個帖子[觸發暫停統計](https://github.com/wangziyingwen/AutoApiP/issues/7)裡留言**
   
   
### 教程完 ###

__________________________________________________________________________

## 額外設置 （看不懂請忽略）##
   * **定時啟動修改**

   * **多帳號/應用支援**
    
   * **超級參數設置**

#### 定時啟動修改 ####
   
   我設定的每6小時自動運行一次（週六日不啟動），每次調用3輪（點擊右上角星星/star也可以立馬調用一次），你們自行斟酌修改（我也不知道保持活躍要調用多少次、多久）：

  * 定時自動啟動修改地方：在.github/workflow/autoapi.yml(只修改這一個)檔裡，自行百度cron定時任務格式，最短每5分鐘一次
   
   ![image](https://github.com/wangziyingwen/ImageHosting/blob/master/AutoApi/定時.png)
    
#### 多帳號/應用支援 ####

   如果想輸入第二帳號或者應用，請按上述步驟獲取**第二個應用的id、密碼、微軟金鑰：**
 
   再按以下步驟：
 
   1)增加secret
 
   依次點擊頁面上欄右邊的 Setting -> 左欄 Secrets -> 右上 New repository secret，新增加secret：APP_NUM、MS_TOKEN_2、CLIENT_ID_2、CLIENT_SECRET_2
 
   APP_NUM
   ```shell
   帳號/應用數量(現在例如是兩個帳號/應用，就是2 ；3個帳號就填3，日後如果想要增加請修改APP_NUM)
   ```
   MS_TOKEN_2
   ```shell
   第二個帳號的微軟金鑰（第二步refresh_token），（第三個帳號/應用就是MS_TOKEN_3，如此類推）
   ```
   CLIENT_ID_2
   ```shell
   第二個帳號的應用程式ID (第一步獲取),（第三個帳號/應用就是CLIENT_ID_3，如此類推）
   ```
   CLIENT_SECRET_2
   ```shell
   第二個帳號的應用程式密碼 (第一步獲取),（第三個帳號/應用就是CLIENT_SECRET_3，如此類推）
   ```
   
   2)修改.github/workflows/裡的兩個yml檔（**超過5個帳號需要更改，5個及以下暫時不用修改檔，忽略這一步**）
    
   yml檔我已經注明了，看著改就行，我已經寫入5個帳號範本了，跟著複製粘貼很簡單的（沒有找到比較好的自動方案）
  
#### 超級參數設置 ####
 
   ApiOfRead.py ， ApiOfWrite.py 文件第11左右行各有個config，具體參數設置已在文件裡說明
   
   包括帳號api的隨機延時，api隨機排序，每次輪數等參數
     
   
### 結尾 ###

有事發issue

Q群：[657581700](https://jq.qq.com/?_wv=1027&k=5FQJbWmV)  （專案相關討論）

                              wangziyingwen
    





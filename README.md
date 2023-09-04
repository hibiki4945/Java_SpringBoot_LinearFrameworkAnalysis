# Java_SpringBoot_線性架構的解析 

##- SpringBoot的線性架構1(發送請求)   
  PostMan->Controller->Service->Dao->(Entity)->DataBase  
  (用戶)                                       (資料)  
##- SpringBoot的線性架構1(回傳結果)
  PostMan<-Controller<-Service<-Dao<-(Entity)<-DataBase  
  (用戶)                                       (資料)  
##- SpringBoot的線性架構2(發送請求)  
  Test->Service->Dao->(Entity)->DataBase  
  (用戶)                        (資料)  
##- SpringBoot的線性架構2(回傳結果)  
  Test<-Service<-Dao<-(Entity)<-DataBase  
  (用戶)                        (資料)  
##- 此線性架構用做'用戶'對'資料'的雙向溝通  
-   
- 以下將以對話形式，做解說。  
----------------------分隔線(情境開始)  
    -------------------分隔線(以下是'發送請求'的部份)  
###PostMan : 我想了解台灣的美食  
                ，於是我找到一個API(Java的SpringBoot架構)  
                ，然後我發現這個API  
                ，這個API有2個功能，分別是  
                                         1. 輸入'城市'，得到對應的'美食'列表  
                                         2. 輸入'美食'，得到對應的'城市'列表  
                ，因為我現在在環島  
                ，又剛好對'麵線糊'感興趣  
                ，所以我要對這個API，使用方法2(輸入'麵線糊')。  
###PostMan -> Controller : 
                方法2(麵線糊)  
###Controller : 今天接收到一個命令  
                   命令的內容是，查詢各'城市'的'麵線糊'情報  
                   ，但其實我連什麼是'城市'都不知道  
                   ，還好我知道有誰知道  
                   ，就是Service，他肯定知道  
                   ，於是我看了一下，Service提供的方法  
                   ，有2個方法，分別是  
                                                  1. 輸入'城市'，得到對應的'完整資料'  
                                                  2. 輸入'美食'，得到對應的'完整資料'  
                   ，所以我要對Service，使用方法2(輸入'麵線糊')。  
###Controller -> Service : 方法2('麵線糊')  
###Service : 今天接收到一個命令  
			  ，我需要先對這個命令 做一些檢查  
		    ...看起來沒什麼問題  
        ，命令的內容是，查詢各'城市'的'麵線糊'情報  
        ，我知道'城市'&'美食'是什麼  
        ，但我不知道'美食'裡有沒有'麵線糊'  
        ，還好我知道有誰可以幫我問  
        ，就是Dao  
        ，於是我看了一下，Dao提供的方法  
        ，Dao提供了n個方法  
        ，其中的方法x，剛好是  
                      輸入'美食'，得到對應的'資料'列表  
        ，正好符合我的需求  
        ，所以我要對Dao，使用方法x(輸入'麵線糊')。  
###Service -> Dao : 方法x('麵線糊')  
###Dao :    今天接收到一個命令  
         ，命令的內容是，查詢各'城市'的'麵線糊'情報  
         ，我什麼都不知道  
         ，我只知道，我現在需要問DataBase  
         ，在'美食'這個欄位，有沒有'麵線糊'  
         ，如果有，就回傳對應的'資料'列表  
         ，但我發現一件事，就是  
         ，因為我還沒定義，回傳的'資料'的格式  
         ，所以DataBase會不知道怎麼回傳資料  
         ，因此，我需要在Entity  
         ，定義一個'溝通格式'  
         ，主要是，DataBase的某'欄位'，對應到哪個'變數'  
         ，這樣的話，當我跟DataBase，要求回傳某欄位時  
         ，我只需要跟他說，需要回傳的'變數'名稱  
         ，他就知道，這個'變數'對應到的是，哪個'欄位'  
         ，如此，我與DataBase的溝通，就沒問題了。  
###Dao -> (Entity) -> DataBase :   
         在'美食'這個欄位，有沒有'麵線糊'?  
         ，如果有，就回傳對應的'資料'列表!。  
    -------------------分隔線(以下是'回傳結果'的部份)  
###DataBase -> (Entity) -> Dao :   
	     [對應的'資料'列表]  
###Dao -> Service :   
	     [對應的'資料'列表]  
###Service :   
    收到回傳的'資料'列表了  
    ，我需要先對這個'資料'列表 做一些檢查  
		...看起來是一個空的'資料'列表  
		，在我準備將，這個'資料'列表   
		，回傳給Controller時，發現一個問題  
		，Controller要怎麼知道   
		，我回傳的這個，空的'資料'列表，代表什麼  
		，他可能會有這幾種猜想  
		       1. Service出Bug了  
		       2. '麵線糊'不在資料庫中  
		       3. 給Service下錯命令了  
		，為了避免Controller在那邊亂想  
		，於是除了這個 空的'資料'列表  
		，我貼心的，再多回傳2個變數(錯誤碼 & 錯誤訊息)    
		，此時，因為我有3個變數   
		，需要1次做回傳  
		，因此，我需要把這3個變數，打包成1個新的Class  
		(一般 這個新的Class的命名格式會是  
             ??Response)  
		，在先前做的檢查中，我發現  
		，'資料'列表為空的原因是  
		，資料庫裡沒有'麵線糊'  
		，因此，錯誤碼="400" & 錯誤訊息="資料庫裡 沒有'麵線糊'!"  
		，最後，我需要將 ??Response 做回傳。  
###Controller -> PostMan :   
        [??Response]  
###PostMan :   
        什麼?沒有麵線糊!  
        ，這個API好爛 換一個!  
----------------------分隔線(情境結束)  
  
  
##'溝通格式'(溝通語言)的補充 :   
###       1. Requst :     
                          PostMan說給Controller聽  
                         ，並且   
                         ，Controller得到足夠的情報，來處理接下來的動作。  
###       2. Response :   
                          Service說給Controller 聽  
                         ，Controller再轉達給PostMan聽  
                         ，並且   
                         ，PostMan得到足夠的情報，並滿足的離開。  
###       3. Entity :     
                          Dao說給DataBase聽  
                         ，DataBase 再說給 Dao 聽  
                         (可以想成1種，Dao與DataBase，之間的語言)  
                         ，並且  
                         ，雙方的溝通 並無障礙。  
                         (補充 : Jpa & Query 是基於Entity這種語言  
                                ，所設計的  
                                ，Dao說給DataBase聽的，1種'命令傳達'語句)  
  
##PostMan+Controller 與 Test 的關係 :   
##   1. Test :    
                 用來測試 Service的所有功能 是否正常  
                (可以把Test想成一個   
                 調皮的PostMan使用者  
                 最喜歡的事情就是  
                 挑Service的毛病)  
##   2. PostMan+Controller :   
            PostMan = 使用者介面(前端)  
            Controller = 使用者介面(PostMan) 與 Service 的中間人  
                         (在基本的應用裡 Controller只會負責'資訊轉達'的工作)  




# TA 第九章作業
## 1.(20%)分析『本日』登錄檔資訊的相關設定，重點在實做與練習正規表示法：
- A. 寫一隻名為 /usr/local/sbin/getmesg 的腳本，內容會使用 grep 取得 /var/log/messages 在『本日』的登錄資訊。 要注意的是，我們假設系統語系為英文，抓日期的方式可以從『 date 』這個指令搭配相關參數去處理。 當你用 root 的身份執行 getmesg 時，螢幕會顯示類似如下的資訊：
  
  ![image](https://github.com/vbkservices/mybookword/assets/97799165/ff2fa48c-1528-48b5-972b-c84a4524d603)
- B. 寫一隻名為 /usr/local/sbin/chsel 的腳本，這個腳本會修改 /etc/selinux/config 檔案的內容， 會將行首出現『SELINUX=??? 』那一行(一整行喔)資料，強制替換成『SELINUX=enforcing』或『SELINUX=permissive』， 且該檔案會被直接修改。執行方式會像這樣 (需要 root 權限)：

  ![image](https://github.com/vbkservices/mybookword/assets/97799165/9b8cad0b-66ce-4efb-a130-84843430e0a1)
- C. 寫一隻名為 /usr/local/bin/chupper 的腳本，當執行該腳本之後，會把 /etc/hosts 抓出來變成大寫字元的內容。 使用一般帳號也可以執行這個腳本，執行的結果會有點像這樣：
  
  ![image](https://github.com/vbkservices/mybookword/assets/97799165/82e0bb38-ad52-4c2d-a74d-417164b1801b)
## 2.(10%)建立一隻名為 /usr/local/bin/myprocess 的腳本，腳本內容主要為：
- A.第一行一定要宣告 shell 為 bash 才行；
- B.主要僅執行『 /bin/ps -Ao pid,user,cpu,tty,args 』
- C.這隻腳本必須要讓所有人都可以執行才行！
- D.執行結果會有點像這樣：
  
  ![image](https://github.com/vbkservices/mybookword/assets/97799165/cd9771be-55a9-401c-ab8e-0bd43ce7f828)
## 3.(10%)寫一隻名為 /usr/local/bin/mydate.sh 的腳本，執行後可以輸出如下的資料：
- A.第一行一定要宣告 shell 才對！
- B.以 西元年/月/日 顯示出目前的日期
- C.以 小時:分鐘:秒鐘 顯示出目前的時間
- D.輸出從 1970/01/01 到目前累計的秒數
- E.列出這個月的月曆，且依據台灣的習慣，輸出時，以星期一為一週的開始。
- F.這隻腳本必須要讓所有人都可以執行才行！
- G.執行結果會有點像這樣:
  
  ![image](https://github.com/vbkservices/mybookword/assets/97799165/ae422dbb-bd10-4a05-a454-5e6cb9197672)
## 4.(20%)寫一隻 /usr/local/bin/listcmd.sh 的腳本，該腳本執行後，會告知底下相關的事宜：
- A.腳本的執行方式為『 listcmd.sh passwd 』，其中 passwd 可以使用任何檔名來取代
- B.第一行一定要宣告 shell
- C.判斷是否有外帶參數，若沒有外帶參數，請 (1)螢幕顯示『 Usage: ${0} cmd_name 』，(2)並以回傳值 2 離開程式
- D.這階段執行結果會有點像這樣：
  
  ![image](https://github.com/vbkservices/mybookword/assets/97799165/85dff885-5ccb-4e83-afd0-1b09fbcf4ec9)
- E.使用『 which ${1} 2> /dev/null 』的結果，判斷該字串是否為指令，你可以將這個指令的結果帶入成為某個變數， 然後分析這個變數值。基本上，這個字串值若存在，則代表是指令，若為空值，則非指令。
- F.若該字串為指令，則依序輸出：
  - 輸出指令的完整路徑
  - 用 ls -l 列出這個指令的完整權限
  - 利用 getfacl 列出這個指令的完整權限
  - 離開 shell script，並回傳 0 的數值。
- G.這階段執行結果會有點像這樣：
  
  ![image](https://github.com/vbkservices/mybookword/assets/97799165/43b18301-44ec-49e3-8cc3-39592a9090d2)
- H.若該字串不為指令，則使用 locate 後面加 /${1}$ 的正規表示法 (locate 要支援正規表示法，必須要輸入特定的選項 請自行 man locate 查到正確的選項支援)，然後依據 locate 之後的回傳值處理後續工作
  - 若回傳值 (為 0) 顯示該字串其實具有相同的檔名，則使用 ls -ld 將檔名全部列出，然後以回傳值 0 離開程式
  - 若回傳值 (不為 0) 顯示該字串並不為檔名，則顯示『no this filename』，然後以回傳值 10 離開程式
- I.執行結果會有點像這樣：
  
  ![image](https://github.com/vbkservices/mybookword/assets/97799165/e64323e0-4f6c-4284-86ed-74a98af1b618)
## 5.(10%)寫一隻名為 /usr/local/bin/myheha 的腳本，建議使用 case 這種語法，不要使用 if 語法。這隻腳本的執行結果會這樣：
- A.腳本內第一行一定要宣告 shell 為 bash
- B.當執行 myheha hehe 時，螢幕會輸出『 I am haha 』
- C.當執行 myheha haha 時，螢幕會輸出『 You are hehe 』
- D.當外帶參數不是 hehe 也不是 haha 時，螢幕會輸出『 Usage: myheha hehe|haha 』
- E.執行結果會有點像這樣：
  
  ![image](https://github.com/vbkservices/mybookword/assets/97799165/5710feee-321b-4c3e-be0a-cf3ae78dfc69)
## 6.(20%)寫一隻判斷生日的腳本，名稱為 /usr/local/bin/yourbday.sh，內容為：
- A.腳本內第一行一定要宣告 shell 為 bash
- B.指令執行的方式為『 yourbday.sh YYYY-MM-DD 』
- C.當使用者沒有輸入外帶參數時，螢幕顯示『 Usage: yourbday.sh YYYY-MM-DD 』，並且離開程式
- D.以正規表示法的方式來查詢生日的格式是否正常，若不正常，重新顯示上面的訊息，並且離開程式
- E.以 date --date="YYYY-MM-DD" +%s 的回傳值確認時間格式是否正確？若不正確請顯示『 invalid date 』後，離開程式
- F.分別取得生日與現在的累積秒數，根據兩者的差異，同時假設一年 365.25 天，然後：
  - 如果生日比現在的總累積秒數還要大，代表來自未來，請輸出『You are not a real human..』，之後離開程式
  - 如果所有問題都排除了，那請搭配 bc 來顯示出你的歲數，歲數計算到小數點第二位， 例如『 You are 22.35 years old 』的樣式。
- G.執行結果會有點像這樣：

  ![image](https://github.com/vbkservices/mybookword/assets/97799165/7635a2b7-1eed-4241-a223-d2c0cb7fa61e)
## 7. (10%)系統上面已經有一隻腳本名為： /usr/local/sbin/examcheck ，當執行時，應該要出現如下的畫面， 但是程式開發人員寫錯了某些地方，請你將應該有問題的程式碼訂正，使得腳本得以展示如下的結果：
- 執行 examcheck ok 時，顯示『Yes! You are right!』
- 執行 examcheck false 時，顯示『So sad... your answer is wrong..』
- 執行 examcheck otherword 時，顯示『 Usage: examcheck ok|false 』(otherword 為任意字元，隨便不是 ok 與 false 的其他字元之意)
- 執行結果會有點像這樣:

  ![image](https://github.com/vbkservices/mybookword/assets/97799165/2a5766b0-6836-4d41-ade3-05094c4f91bc)


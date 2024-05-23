# web 練習
### 1. 第一步 架設小型web伺服器
- 透過這個網址[我的雲端硬碟](https://drive.google.com/drive/u/0/folders/1bM9d7HkuBCHYqAQ-YkT-scbv4TfrZxM4) 下載xmapp的exe
- 再載完之後就直接執行到這個畫面
  
![image](https://github.com/vbkservices/mybookword/assets/97799165/83426dbd-3874-4711-9b7d-89e9a71f2f92)
- 接這就把Mysql以及phpmyadmin打勾剩餘的取消就能下一步到完成了(注意:完成後它會沒有詢問就直接重開機)
- 安裝之後再C:\xampp點擊xampp-control來啟動xampp介面，如圖
![image](https://github.com/vbkservices/mybookword/assets/97799165/63817332-4f5b-40ad-be55-ed08eb89c099)

- 在點擊介面的apache以及mysql的start按鈕來測試是否能啟動，如圖(系統會問你是否允許放行防火牆就直接允許就好)
![image](https://github.com/vbkservices/mybookword/assets/97799165/dd802eac-dfd8-40c3-becd-0df0384c38c4)
- 接著更改apache的目錄位置(只要先改html檔案放置的位置)，先stop apache的服務，接著點擊config，在點擊Apache(httpd.conf)
  ![image](https://github.com/vbkservices/mybookword/assets/97799165/0d42d2cd-1fce-4790-afb4-4417c4ae79cb)
- 在點擊介面的apache以及mysql的start按鈕來測試是否能啟動，如圖(系統會問你是否允許放行防火牆就直接允許就好)
- 開啟檔案後再[ctrl+F]找htdocs，內容改成你的資料夾位置(你的可能會跟我不一樣)
  - DocumentRoot "C:/Users/ken/Downloads/xampp/htdocs"
  - <Directory "C:/Users/ken/Downloads/xampp/htdocs">
    
  ![image](https://github.com/vbkservices/mybookword/assets/97799165/96d8af15-2a97-4f8b-82ae-5a4f20758271)
> - 如果不知道資料夾位置，可以直接參考我的作法
>  - 在桌面新增資料夾取名為web
>    ![image](https://github.com/vbkservices/mybookword/assets/97799165/3013ba14-2109-4e9d-9afe-d8374a62153c)
>  - 再點右鍵選擇內容
>    ![image](https://github.com/vbkservices/mybookword/assets/97799165/f9fd1e61-d078-4320-b796-6e15870f0c6b)
>  - 將位置複製
>    ![image](https://github.com/vbkservices/mybookword/assets/97799165/53c884e6-f917-4c4d-94ef-47698a487948)
>  - 在剛剛開啟的Apache(httpd.conf)把這兩行覆蓋
>    - DocumentRoot "C:/Users/ken/Downloads/xampp/htdocs"
>    - <Directory "C:/Users/ken/Downloads/xampp/htdocs">
  - 如圖，修改完成之後就將apache的start啟動
    
    ![image](https://github.com/vbkservices/mybookword/assets/97799165/590bd683-0596-488d-9fbb-5b662db212c8)
  ## 測試 ##
  - 第一種測試方式:啟動之後再web資料夾內新增index.html， 內容隨便寫(注意:win10隱藏副檔名，所以在新增前要先顯示副檔名，如圖)
    
    ![image](https://github.com/vbkservices/mybookword/assets/97799165/c96431fd-c5f7-40d6-ae74-d5657a4b910f)
  - 第二種測試方式:在vscode匯入資料夾新增index.html
    
    ![image](https://github.com/vbkservices/mybookword/assets/97799165/974193c5-b272-4b9c-abd6-305bf23c58c3)
    ![image](https://github.com/vbkservices/mybookword/assets/97799165/03d0e1a9-a926-4003-8217-9c2e6d48923b)

  - 有了index.html之後，就能在http://localhost/ 來看到你index.html的內容







# TA 第十一章作業
## 1.(16%)請回答下列問題，並將答案寫在 /root/ans11.txt 檔案內：
- A.一般網路檢查是否有網際網路，會有幾個步驟？每個步驟所需要檢查的項目是什麼？
- B.一般 Linux 作業系統在 PC 上面會有兩個時間紀錄，分別是那兩個？
- C.在 Linux 底下，常見的壓縮指令，依據壓縮率從最好到最差，寫出三個常見的指令。
- D.例行工作排程的 crontab 使用中，對於一般帳號來說，設定時的六個欄位的口訣為何？
## 2.(20%)系統的基礎設定-網路的設定部份：
- 0.進行此題之前，確定你已經執行過 vbird_book_setup_ip 以設定好你的帳號了！非常重要！
- A.由於我們的系統是經過 clone 出來的，因此所有的設備恐怕都怪怪的。所以，請先將系統中的 ens3 (或者是其他可對外的網路連線) 這個網路連線刪除。
  
  ```
  ## 解答 ##
  nmcli connection delete ens3
  ```
- B.請依據底下的說明，重新建立 ens3 這個網路連線：
  - 使用的介面卡為 ens3 這個介面卡
  - 需要開機就自動啟動這個連線
  - 網路參數的設定方式為手動設定，不要使用自動取得喔
  - IP address 為： 192.168.251.XXX/24 (XXX 為上課時，老師給予的號碼)
  - Gateway 為： 192.168.251.254
  - DNS server IP為：請依據老師課後說明來設定 (若無規定，請以 168.95.1.1 及 8.8.8.8 這兩個為準)
  - 主機名稱：請設定為 stdXXX.book.vbird (其中 XXX 為上課時，老師給予的號碼)
- 務必記得設定完畢後，一定要啟用這個網路連線，否則成績無法上傳喔！
  
  ```
  ## 解答 ##
  nmcli connection add con-name ens3 ifname ens3 type ethernet
  nmcli connection modify ens3 ipv4.method manual ipv4.addresses 192.168.251.XX/24(XX為學號，例:18號就192.168.251.18/24) ipv4.gateway  192.168.251.254 ipv4.dns 168.95.1.1,8.8.8.8
  nmcli connection up ens3
  hostnamectl hostname  stdXXX.book.vbird(XX為學號，例:18號就std18.book.vbird)
  ```
## 3.(15%)系統的基礎設定-時間、語系等其他設定值：
- A.你系統的時間好像怪怪的，時區與時間好像都錯亂了！請改回台北的標準時區與時間。
  
  ```
  ## 解答 ##
  timedatectl set-timezone Asia/Taipei
  ```
- B.不知為何，你的語系好像被修改成簡體中文了。請將它改回繁體中文語系喔！
  ```
  ## 解答 ##
  localectl set-locale zh_TW.utf8
  ```
- C.未來這部主機會作為 WWW/FTP/SSH 伺服器，因此防火牆規則中，請放行 http, ftp, ssh 這幾個服務即可。其他的服務請取消！請注意，這個規則也需要寫入永久設定檔中。
  ```
  ## 解答 ##
  firewall-cmd --permanent --remove-service=cockpit
  firewall-cmd --permanent --remove-service=dhcpv6-client
  firewall-cmd --permanent --add-service=http
  firewall-cmd --permanent --add-service=ftp
  firewall-cmd --permanent --add-service=ssh #這段可能會錯，因為防火牆已經放行了
  systemctl restart firewalld 
  firewall-cmd --list-all #檢查
  ```
# 4.(25%)檔案的壓縮、解壓縮等任務
- A.(10%)你的系統中有個檔名 /root/mybackup 的檔案，這個檔案原本是備份系統的資料 (tar 的打包檔)，但副檔名不小心寫錯了！ 請將這個檔案修訂成為比較正確的副檔名 (例如 /root/mybackup.txt 之類的模樣)，並且將該檔案在 /srv/testing/ 目錄中解開這個檔案的內容(原始檔案要留在 /root 目錄內)。
  ```
  ## 解答 ##
  cat /root/mybackup >> /root/mybackup.tar.xz
  mkdir /srv/testing/
  cd /srv/testing/
  tar -Jxvf /root/mybackup.tar.xz
  ```
- B.(15%)我需要備份的目錄有：/etc, /home, /var/spool/mail/, /var/spool/cron/, /var/spool/at/, /var/lib/， 請撰寫一隻名為 /root/backup_system.sh 的腳本，來進行備份的工作。腳本內容應該是：
  - 第一行一定要宣告 shell 喔！
  - 自動判斷 /backups 目錄是否存在，若不存在則 mkdir 建立她，若存在則不進行任何動作
  - 設計一個名為 mysource 的變數，變數內容以空格隔開所需要備份的目錄
  - 設計一個名為 mytarget 的變數，該變數為 tar 所建立的檔名，檔名命名規則 /backups/mysystem_20xx_xx_xx.tar.gz ， 其中 20xx_xx_xx 為西元年、月、日的數字，該數字依據你備份當天的日期由 date 自行取得。
  - 開始利用 tar 來備份
  - 
- 請注意，撰寫完畢之後，一定要立刻執行一次該腳本！確認實際有建立 /backups 以及相關的備份資料喔！
  ```
  ## 解答 ##
  cd ~
  vim  /root/backup_system.sh
  ## backup_system.sh的內容 ##
  ```
  ![image](https://github.com/vbkservices/mybookword/assets/97799165/9eab1a2a-a5e0-459d-8eb6-23b01b0d618c)
  ```
  ## 測試 ##  
  chmod a+x /root/backup_system.sh
  /root/backup_system.sh
  ```
# 5.(10%)請使用網路自動校時服務 (chronyd) 的方式，使用 ntp.ksu.edu.tw 及 time.stdtime.gov.tw 兩部網路主機， 作為時間伺服器，主動更新你的系統時間 (兩部都需要加上 iburst 參數)。
  ```
  ## 解答 ##
  timedatectl set-timezone Asia/Taipei
  vim /etc/chrony.conf
  將 pool 2.rocky.pool.ntp.org.iburst 註解，並補上server ntp.ksu.edu.tw ibrust以及 time.stdtime.gov.tw，如圖
  ```
  ![image](https://github.com/vbkservices/mybookword/assets/97799165/d4a6f30a-6ebc-4003-bfa6-97b0885baa88)
  ```
  systemctl restart chronyd
  ```
# 6.(15%)例行工作排程的設定：
- A.你的系統將在下個月的 20 號 08:00 進行關機的歲修工作，請以『單次』工作排程來設計關機的動作 (poweroff)
  ```
  ## 解答 ##
  at 08:00 2024-06-20
  poweroff
  [ctrl+d]
  ```
- B.讓系統每天 3:00am 時，全系統更新一次 (yum -y update)！相關設定請寫入 /etc/crontab 內
  ```
  vim /etc/crontab
  ## crontab的內容 ##
  ```
  ![image](https://github.com/vbkservices/mybookword/assets/97799165/cf429248-f5c3-4bed-9e47-13df2bc5cfc0)

- C.請使用 gooduser 這個帳號身份，在每天的 15:30 時，下達『 /bin/echo 'It is tea time' 』的例行任務。 若有需要使用 gooduser 登入時，該帳號的密碼為 mypassword
  ```
  su - gooduser
  crontab -e
  ```
  ![image](https://github.com/vbkservices/mybookword/assets/97799165/61242b7a-db87-407f-97e8-ee4c57efd489)

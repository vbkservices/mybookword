# TA 第十章作業
## 1.(12%)請回答底下問答題，答案請寫入 /root/ans10.txt 當中：
- A.一般我們在建立 linux 帳號時，哪三個檔案會記錄這個帳號的 UID, GID, 支援群組, 密碼等資訊？
- B.一般帳號被建立後(假設帳號名稱為 myusername)，基本上會有哪一個目錄與哪一個檔案會被建立？
- C.若設定 umask 033 後，新建的檔案與目錄權限各為幾分？(寫下兩個權限的分數)
- D.在本章作業系統的硬碟理， /etc /var 與 /usr 裡面，各有一個不屬於任何人的檔案 (請略過 gooduser 檔名)，請將檔案的完整檔名找出來，並寫下來
```
vim /root/ans10.txt
## ans10.txt 的內容 ##
```
![image](https://github.com/vbkservices/mybookword/assets/97799165/19e9d8d1-a1d6-4ab8-828b-9c2030c0db5f)
## 2.(5%)建立一個名為 mysys1 的系統帳號，且這個系統帳號 (1)不需要家目錄 (2)給予 /sbin/nologin 的 shell (3)也不需要密碼
```
useradd -M -r -s /sbin/nologin mysys1
```
## 3.(12%)修改新建帳號的預設資訊 (亦即，使用 useradd username 時，就可以具有底下的預設值)
- A.讓未來新建的用戶，其家目錄預設都會有一個名為 web 的子目錄存在？
- B.讓新建用戶的 history 預設可以記憶 5000 筆記錄(已存在帳號不受影響)
- C.讓新建帳號的 shell 將使用 /sbin/nologin
```
mkdir /etc/skel/web
vim /etc/skel/.bashrc
#在最後一行補上
$HISTSIZE=5000
$HISTFILESIZE=5000 
vim /etc/default/useradd
# 將 SHELL=/bin/bash 改成 SHELL=/sbin/nologin
```
## 4.(10%)有個名為 gooduser 的帳號不小心被刪除了，還好，這個帳號的家目錄還存在。
- A.請依據這個提示，重建這個帳號 (記住，UID與GID應該要回復到原本尚未被刪除前的狀態)
- B.且該用戶的密碼設定為 mypassword, shell 設定為 /bin/bash
- C.這個帳號請重新設定為可以使用 sudo 的權限
```
useradd -u 1200 -s /bin/bash gooduser
echo mypassword | passwd --stdin gooduser
visudo
補上這段 
```
![image](https://github.com/vbkservices/mybookword/assets/97799165/2aa2c840-daec-4bbb-b2cc-4c9c959fb3a0)

## ５.(10%)由於你幫學校老師管理 FTP 伺服器，這個伺服器的使用者不能提供可登入系統的 shell，但是可以使用 FTP 與 email 等網路服務：
- A.帳號名稱為： ftpuser1, ftpuser2, ftpuser3，這三個帳號可以使用 ftp 網路功能，但是不能在系統前登入 tty 或使用終端機登入系統；
- B.這三個帳號的密碼均為 MyPassWord
``
useradd 
```
## 6.(26%)建立一個名為 /root/myaccount.sh 的大量建立帳號的腳本，這個腳本執行後，可以完成底下的事件：
- A.會建立一個名為 mygroup 的群組
- B.會依據預設環境建立 30 個帳號，帳號名稱為 myuser01 ~ myuser30 共 30 個帳號，且這些帳號會支援 mygroup 為次要群組 (hint: 請 man seq 這個指令，找到如何自動補 0 的功能，以滿足 01 到 09 的要求)
- C.每個人的密碼會使用【 openssl rand -base64 6 】隨機取得一個 8 個字元的密碼， 並且這個密碼會被記錄到 /root/account.password 檔案中，每一行一個，且每一行的格式有點像【myuser01:AABBCCDD】
- D.會使用 /bin/bash 這個 shell
##　7.(15%)由於你管理的系統需要有專題群組的夥伴共同使用系統，因此你將這些專題夥伴加入同一個次要群組支援！
- A.專題群組的名稱設為： myproject
- B.專題組員的名稱分別為： mypro1, mypro2, mypro3，且這三個帳號都加入了 myproject 群組的次要支援
- C.這三個帳號的密碼均為 MyPassWord
- D.使用 /bin/bash
- E.這個專題組員可共用 /srv/mydir 目錄，其他人則沒有任何權限
## 8.(10%)特別目錄的權限應用：
- A.剛剛建立的 /srv/mydir 目錄，在不更改原有的權限設定下 (因為原本就是給 myproject 群組用的)， 現在，要讓加入 users 群組的帳號們，也能夠進入該目錄查閱資料 (只能進入與查閱，不能寫入)，該如何處置？
- B.那個 gooduser 的帳號，其實是老師的帳號，在不更改既有權限的情況下， gooduser 也需要能夠進入該目錄做任何事情， 且未來在 /srv/mydir 所新建立的任何檔案(或目錄)資料，gooduser 也能夠進行任何動作。(hint：就是有預設值的意思)

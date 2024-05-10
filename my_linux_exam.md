# 鳥哥的期中

## 1.系統救援
### A.因為某些緣故，目前這個作業系統應該是無法順利開機的。根據猜測，可能的原因與管理員曾經動過 chsh 這個指令有關，同時，管理員似乎也更動過 fstab 這個設定檔。請依據這些之前的可能舉動，來恢復系統的可登入狀態。
![](https://i.imgur.com/Psgicq1.png)
```
按傳送按鈕
按ctrl+Alt+del
```
![](https://i.imgur.com/2oseIpq.png)
```
到這畫面之後按E
```
![](https://i.imgur.com/LQiTWAL.png)
```
在 quiet 後加 rd.break
之後按 ctrl+x
```
![](https://i.imgur.com/coSuxMe.png)
```
在 quiet 後加 rd.break
之後按 ctrl+x
```
![](https://i.imgur.com/aCVuyDx.png)
```
到這畫面輸入
mount -o remount,rw /sysroot
chroot /sysroot
```
![](https://i.imgur.com/imHeBgG.png)
```
mount -a
usermod -s /bin/bash root
```
![](https://i.imgur.com/rRWpE8g.png)
```
輸入vim /etc/fstab 之後按i進入編輯畫面,接這把這行/dev/mapper/cebtos-home /home xfs defaults,noAtime
的,noAtime刪掉，之後按ESC退出編輯模式，再輸入:wq儲存加退出
```
![](https://i.imgur.com/YskFN52.png)
```
touch /.autorelabel
exit
按傳送按鈕
按ctrl+Alt+del
```


---

## 2.管理員的操作環境整理：
### A.當你用 student 轉成 root 之後，會發現很奇怪的現象，就是很多指令都不能執行了。 這應該與上次登入管理員的用戶處理到錯誤的 bash 環境設定檔有關。請查詢 root 可能的設定檔後，將這個問題解決。 底下為此題的提示：
1. 思考一下，應該是與那一個變數有關？
1. 若要執行其他指令，可能需要使用絕對路徑才能夠執行，例如你不能直接執行 usermod，可能需要透過 /usr/sbin/usermod 來處理。
1. 『個人』的環境設定檔有很多個喔！請仔細檢查。另外，請不要修改到統一的系統設定
1. 這題處理完畢，請記得要回去處理前一題 1.B 的 vbird_book_setup_ip
```
PATH="${PATH}:/usr/local/sbin:/usr/local/bin:/usr/sbin/:/usr/bin:/root/bin"
```
#### B.增加 histroy 的輸出，讓 root 自己最大可達 10000 筆紀錄，且 (1)其他用戶保留預設值 (2)紀錄檔內也要紀錄 10000 筆。
#### C.建立一個命令別名 myerr 這個指令，這個指令會運作『 echo "I am error message" 』這個指令串。
#### D.當 root 執行『 cd ${mywork} 』時，工作目錄會跑去 /usr/local/libexec/ 當中
#### E.請注意，上述的動作在每次登入之後都會自動生效

```
B,C,D,E 題要先登root，一定要在root做，總指令為:
vim ~/.bashrc
在裡面的# User specific aliases and funtctions 底下輸入
HISTSIZE = 10000
HISTFILESIZE = 10000
alias myerr = 'echo "I am error message"'
mywork="/usr/local/libexec"
之後儲存退出後執行source ~/.bashrc
```
![](https://i.imgur.com/96tZW2K.png)
```
vim ~/.bash_profile
在PATH=' '輸入/usr/local/sbin:/usr/local/bin:/usr/sbin/:/usr/bin:/root/bin
```
![](https://i.imgur.com/F89Pxv0.png)

---

## 3.檔案系統的整理：
#### A.系統內有個名為 /dev/vda4 的分割槽，這個分割槽是做錯的，因此，請將這個分割槽卸載，然後刪除分割，將磁碟容量釋放出來。
```
umount /data
mkdir /mydata
mkdir /mydata/xfs
mkdir /mydata/vfat
chown student /mydata/vfat #可打可不打
chgrp student /mydata/vfat #可打可不打
mkdir /mydata/ext4
fdisk /dev/vda
d
4
```
#### B.完成上面的題目之後，請依據底下的說明建立好所需要的檔案系統(所有的新掛載，應該使用 UUID 來掛載較佳。)
|  容量	 | 檔案系統 | 掛載點	| 掛載額外參數 |
| - | -------- | -------- |-------- |
| 1GB | XFS | /mydata/xfs	| nosuid |
| 2GB    | VFAT   | /mydata/vfat   | uid與gid均為student   |
| 1GB | EXT4 | /mydata/ext4	| noatime |
| 1GB | swap | 	|  |
上述四個新增的資料都能夠開機後自動的掛載或啟用。
```
n 4 +1G
n 5 +2G
n 6 +1G
n 7 +1G
w
mkfs.xfs /dev/vda4
mkfs.vfat /dev/vda5
mkfs.ext4 /dev/vda6
mkswap /dev/vda7
blkid /dev/vda{4,5,6,7} 查詢UUID
vim /etc/fstab
把找到的UUID貼最下面進去
```
![](https://i.imgur.com/DCoEZ0M.png)

![](https://i.imgur.com/VLoFait.png)

#### C.有個光碟映像檔 /mycdrom.iso 的檔案，請將他掛載到 /mydata/cdrom 裡面，而且每次開機都能自動掛載上來。 (請自行查詢光碟檔案掛載時所需要的檔案系統類型)
```
mkdir /mydata/cdrom
vim /etc/fstab
輸入
mount -t iso9660 -o /mycdrom.iso /mydata/cdrom
```
![](https://i.imgur.com/VpyH4nE.png)

#### D.建立一個名為 /mydata.img 的 200MB 大檔案，這個檔案格式化為 xfs ，且開機會主動的掛載於/mydata/xfs2/ 目錄中
```
mkdir /mydata/xfs2
vim /etc/fstab
dd if=/dev/zero of=/mydata.img bs=1M count=200
mkfs.xfs /mydata.img 
輸入
```
![](https://i.imgur.com/n01xgLX.png)


---
## 4.基礎帳號管理，請依據底下的說明，建立或恢復許多帳號：
#### A.請刪除系統中的 baduser 這個帳號，同時將這個帳號的家目錄與郵件檔案同步刪除。
```
userdel -r baduser
```
#### B.有個帳號 gooduser 不小心被管理員刪除了，但是這個帳號的家目錄與相關郵件都還存在。請參考這個帳號可能的家目錄所保留的 UID 與 GID， 並嘗試以該帳號原有的 UID/GID 資訊來重建該帳號。而這個帳號的密碼請給予 MyPassWord 的樣式
```
ll /home查詢他的uid
useradd -u 1100 gooduser
echo MyPassWord | passwd --stdin gooduser
id gooduser 確認他有還原
```
#### C.群組名稱為： mygroup, nogroup
#### B.帳號名稱為： myuser1, myuser2, myuser3 ，通通加入 mygroup，且密碼為 MyPassWord
#### E.帳號名稱為： nouser1, nouser2, nouser3 ，通通加入 nogroup，且密碼為 MyPassWord
```
groupadd mygroup
groupadd nogroup
useradd -G mygroup myuser1 && echo MyPassWord | passwd --stdin myuser1
useradd -G mygroup myuser2 && echo MyPassWord | passwd --stdin myuser2
useradd -G mygroup myuser3 && echo MyPassWord | passwd --stdin myuser3
useradd -G nogroup nouser1 && echo MyPassWord | passwd --stdin nouser1
useradd -G nogroup nouser2 && echo MyPassWord | passwd --stdin nouser2
useradd -G nogroup nouser3 && echo MyPassWord | passwd --stdin nouser3
```
#### 帳號名稱為： ftpuser1, ftpuser2, ftpuser3，無須加入次要群組，密碼為 MyPassWord，且這三個帳號主要用來作為 FTP 傳輸用的帳號， 因此需要不能互動的 shell。
```
useradd ftpuser1 && echo MyPassWord | passwd --stdin ftpuser1
useradd ftpuser2 && echo MyPassWord | passwd --stdin ftpuser2
useradd ftpuser3 && echo MyPassWord | passwd --stdin ftpuser3
usermod -s /sbin/nologin ftpuser1
usermod -s /sbin/nologin ftpuser2
usermod -s /sbin/nologin ftpuser3
```
---
## 5.管理群組共用資料的權限設計
#### A.建立一個名為 /srv/myproject 的目錄，這個目錄可以讓 mygroup 群組內的使用者完整使用，且【新建的檔案擁有群組】為 mygroup 。不過其他人不能有任何權限
```
mkdir /srv/myproject
chgrp mygroup /srv/myproject
chmod 2770 /srv/myproject
```
#### B.雖然 nogroup 群組內的用戶對於 /srv/myproject 應該沒有任何權限，但當 nogroup 內的用戶執行 /usr/local/bin/myls 時，可以產生與 ls 相同的資訊，且暫時擁有 mygroup 群組的權限，因此可以查詢到 /srv/myproject 目錄內的檔名資訊。 也就是說，當你使用 nouser1 的身分執行【myls /srv/myproject】時，應該是能夠查閱到該目錄內的檔名資訊。
```
cp -r /bin/ls /usr/local/bin/myls
chgrp mygroup /usr/local/bin/myls
chmod g+s /usr/local/bin/myls
```
#### C.建立一個名為 /srv/change.txt 的空檔案，這個檔案的擁有者為 myuser1，擁有群組為 nogroup，myuser1 可讀可寫， nouser1 可讀，其他人無權限。 這個檔案所有人都不能執行。此外，這個檔案的最後修改時間請調整成 2020 年 02 月 5 日的 13 點 0 分
```
touch /srv/change.txt
chown myuser1 /srv/change.txt
chgrp nogroup /srv/change.txt
chmod 640 /srv/change.txt
touch -t 202002051300 /srv/change.txt
```
---
## 6.檔案的搜尋與管理：
#### A.將 /usr/sbin 與 /usr/bin 裡面，只要是具有 SUID 與/或 SGID 的權限檔案，就將該檔案連同權限，全部複製到 /root/findperm 目錄中。
```
mkdir findperm
cp -a $(find /usr/sbin /usr/bin -perm /6000 2> /dev/null) /root/finperm
```
#### B.找出系統中檔案擁有者為 examuserya 的檔名，並將這些找到的檔名(含權限)複製到 /root/finduser/ 目錄內
```
mkdir finduser
cp -a $(find / -user examuserya 2>/dev/null) finduser
```
#### C.有個名為 /srv/mylink.txt 檔案，這個檔案似乎有許多的實體連結檔。請將這個檔案的所有實體連結檔的檔名，通通複製到 /root/findlink 目錄下。
```
mkdir findlink
ls -li /srv/mylink.txt 
cp -a $(find / -inum 17427652 2>/dev/null) findlink
```
#### D.想辦法建立一個檔名 /srv/mail ，當使用者進入 (cd) 這個檔名時，就會被導向 /var/spool/mail 去。(考慮是 symbolic link 還是 hard linke 呢？)
```
ln -s /var/spool/mail /srv/mail
```
#### E.在 root 家目錄下，建立一個名為 -hidden 的目錄(開頭為減號)，並將 root 家目錄底下的隱藏檔中，以 .b 為開頭的檔案， 通通複製到 -hidden 目錄內。
```
mkdir ~/-hidden
cp -r ~/.b* ~/-hidden
```
#### F.在 root 家目錄下，建立一個名為 mydir 的目錄，在該目錄底下建立 userid01, userid02... 到 userid50 的 50 個空目錄
```
mkdir ~/mydir
mkdir ~/mydir/userid{01..50}
```
#### G.在 root 家目錄下，建立一個名為 myfile 的目錄，在該目錄底下，建立『 file_XXX_YYY_ZZZ.txt 』的檔案，其中 XXX 代表 mar, apr, may 三個字串， YYY 代表 first,second,third 三個字串， ZZZ 代表 paper, photo, chart 三個字串。
```
mkdir ~/myfile
touch ~/myfile/file_{ mar, apr, may}_{first,second,third}_{paper, photo, chart}.txt
```
#### H.在 root 家目錄下有個名為 ~myuser1 的目錄，請刪除該目錄。
```
rm -rf ~/~myuser1/
```
---
## 7.檔案內容的處理：
#### A.透過 date 的功能，將目前的時間以『 YYYY-MM-DD HH:MM 』的格式，使用覆蓋的方法記載進 /root/mytext.txt 檔案中。
```
date +"%Y-%m-%d %H:%M" >/root/mytext.txt
```
#### B.將 /etc/services, /etc/fstab, /etc/passwd, /etc/group 這四個檔案的最後 4 行擷取下來後 (所以應該會有 4*4=16 行，同時會有該檔名記載)，『累加』轉存到 /root/mytext.txt 當中。
```
tail -n 4 /etc/services /etc/fstab /etc/passwd /etc/group >> /root/mytext.txt
```
#### C.使用 ll 的方式，將 /etc/selinux/ 的所有檔案列出(不含子目錄內容)，但是時間需要使用完整格式 (類似『2020-04-20 23:17:46.363000000 +0800』的格式)，並將輸出結果『累加』轉存到 /root/mytext.txt 當中。
```
ll --full-time | grep -v targeted >>/root/mytext.txt #old
ll --full-time  /etc/selinux/ >> /root/mytext.txt #new
```
---

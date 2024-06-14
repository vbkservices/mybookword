# vm1
# 期末考練習VM1：系統設定與操作部份

這個系統預設的 root 密碼為 myCentOS8，預設的 student 密碼為 mystdgo，是可以直接登入的，無須救援 root 密碼。 這個系統需要的是一些正確的設定，以符合系統的運作！

0. 不是題目：請使用 vbird_book_setup_ip 設定好你的學號資料。
---
### 一、系統初始化設定
A. 我需要每次開機都可以預設的進入純文字界面而非現行的圖形界面，可以節省許多不必要的資源浪費。**
   - 在系統自己開機後，預設會跑進純文字界面，沒有圖形界面的意思。
   - 你在操作時，依舊可以『暫時』切換到圖形界面，沒有特別要求你一定要在文字界面答題。
    
### 解答
```
        systemctl get-default #查看預設值
	systemctl set-default multi-user.target #將文字界面設定成預設值
```
如果要暫時切換到圖形介面，利用底下指令(不影響上述設定)
```    
        systemctl isolate graphical.target #立即切換成圖形界面
```
---
B. 請設定好這部主機的網路參數成為如下狀態 (全對才給分)：
- 刪除原有的網路連線名稱，建立名為『 mynetwork 』的連線名稱，且使用到可對外的乙太網路界面。
- 需要開機就自動啟動這個連線
- 網路參數的設定方式為手動設定
- 網際網路位址 IP address 為： 192.168.251.XXX (XXX 為上課時，老師給予的號碼)
- 子網路遮罩 netmask 為： 255.255.255.0
- 通訊閘 Gateway 為： 192.168.251.254
- 領域名稱伺服器 DNS server 位址為： 172.16.200.254 以及 120.114.100.1 這兩個
- 主機名稱：請設定為 wwwXXX.book.vbird (其中 XXX 為上課時，老師給予的號碼)
### 解答
     第一步驟 刪除原有的網路連線名稱
     
        nmcli connection show (查看目前網卡有哪些)
        nmcli connection delete ens3
	
    第二步驟 新增網路卡 (建立名為『 mynetwork 』的連線名稱)
    
        nmcli connection add  con-name mynetwork ifname ens3 type ethernet
	
    第三步驟 設定網卡    
    
        nmcli connection modify mynetwork  ipv4.address 192.168.251.XXX/24 \
        ipv4.gateway 192.168.251.254 ipv4.dns 172.16.200.254,120.114.100.1 \
        ipv4.method manual
	
    第四步驟 啟動該網卡    
    
        nmcli connection up mynetwork
        nmcli connection show (查看網卡是否啟動成功)
        nmcli connetcion show mynetwork | grep ipv4(查看網卡是否設定正確)
	
    第五步驟 設定主機名稱    
    
        hostnamectl hostname wwwXXX.book.vbird
        hostname
---
C. 針對 YUM 的軟體倉儲設定，你有底下的兩組軟體倉儲位址，請設定好所需要的環境 (全對才給分)：


- 軟體倉儲名稱為『myapp』，URL 在：
  『http://download.rockylinux.org/pub/rocky/9/AppStream/x86_64/os』
- 軟體倉儲名稱為『mybase』，URL 在：
  『http://download.rockylinux.org/pub/rocky/9/BaseOS/x86_64/os』
    
### 解答

    自行設計一個倉儲
    vim /etc/yum.repos.d/myrepo.repo (預設是沒有vim的先利用vi)
    
    [myapp]
    name = myapp
    baseurl = http://download.rockylinux.org/pub/rocky/9/AppStream/x86_64/os
    gpgcheck = 0
    enabled = 1
    
    [mybase]
    name = mybase
    baseurl = http://download.rockylinux.org/pub/rocky/9/BaseOS/x86_64/os
    gpgcheck = 0
    enabled = 1
完成後存檔離開

---
D. 時區與時間及網路校時功能設計
    - 你系統的時間好像怪怪的，時區與時間好像都錯亂了！請改回台北的時區與時間。
    - 請使用網路校時 (chronyd) 的方式，使用貴校 ntp.ksu.edu.tw 作為伺服器，主動更新你的系統時間。 且系統啟動之後，會持續網路校時以取得最正確的時間。(若貴校並無 NTP 伺服器，則以 time.stdtime.gov.tw 作為來源)
### 解答
```
    第一步驟 改回台北的時區與時間
    
        timedatectl set-timezone Asia/Taipei 更改台北時區時間
        timedatectl 查看是否更改正確
        
    第二步驟 網路校時 (chronyd) 的方式，使用貴校 ntp.ksu.edu.tw 作為伺服器
    
        rpm -ql chrony (先查看這系統有沒有這個服務)
        yum install -y chrony (沒有就安裝)
        systemctl start chronyd (啟動)
        systemctl enable chronyd (開機啟動)
        systemctl status chronyd (觀察)
        vim /etc/chrony.conf (進入網路校時功能)
        先將預設的pool.....註解
        server ntp.ksu.edu.tw iburst 
	server time.stdtime.gov.tw iburst 
        (設定題目需要的伺服器)
        存檔離開:wq
	systemctl restart chronyd
        chronyc sources (如果沒有更新到設定檔等待個幾秒在執行一次該指令即可)
```


---


E. 系統的自動更新機制：
- 請至少升級核心 (kernel) 到最新版本，且升級完畢後，需要重新開機為宜
- 這部主機需要作為未來開發軟體之用，因此需要安裝一個『 RPM 開發工具 』的軟體群組，請安裝他。
- 請設定每天凌晨 1:30 排程自動進行『核心升級』的動作 (僅核心升級，不升級其他軟體)
    
### 解答
```
    第一步驟 升級核心 (kernel) 到最新版本
    
        yum update kernel -y
        reboot
        uname -r (查看目前版本核心)
        
    第二步驟 安裝RPM 開發工具
    
        yum -y groupinstall "RPM Development Tools"
        
    第三步驟 設定每天凌晨 3 點自動背景進行全系統更新
    
        vim /etc/crontab
        30  1  *  * *  root  yum -y update kernel 
        分 時 日 月 周 使用者 指令
``` 
---
### 二、 帳號與權限控管方面的問題，包括新建帳號、帳號相關權限設定等
A. 管理員的一般帳號設定：
- 請讓 student 可以順利使用自己的密碼操作 sudo 指令   
### 解答
```
    第一個步驟    
        visudo
        /root 搜尋按下[n] 三次
        會看到有一個root ALL=(ALL) ALL
        在這底下加入
        student ALL=(ALL) ALL
        :wq存檔離開
```        
---
**B. 帳號鎖定功能：**
- 有個名為 alex 的帳號，他的密碼為 mygodhehe ，這個帳號有點怪異，因此身為管理員的你，得要將該帳號暫時鎖定。
- 意思是說，這個帳號的所有資源都不變，但是該帳號無法順利使用密碼登入的意思(密碼鎖定)    
### **解答**
```
    第一步驟 暫時鎖定該帳號

        passwd -l alex
        
    第二步驟 查看alex是否鎖定該帳號了
        
        cat /etc/shadow | grep alex
        看到最前面有出現!就代表對了
```
---
**C. 特殊系統帳號建置：建立一個名為 mysys1 的系統帳號，且這個系統帳號：**
    - 不需要家目錄
    - 不具備可互動的 shell
    - 不需要密碼
### **解答**
```
        useradd -r -M -s /sbin/nologin mysys1
```
---
**D. 預設帳號的權限屬性設定：**

    ● 建立新用戶時，新用戶的家目錄應該都會出現一個名為 newhtml 的子目錄存在
    ● 新帳號登入時，預設將會有底下
        『 rm='rm -i' 』『 cp='cp -i' 』『 mv='mv -i' 』這三個命令別名存在。
    
### **解答**
```
    第一步驟 新用戶的家目錄應該都會出現一個名為 newhtml 的子目錄

        mkdir  /etc/skel/newhtml
    
    第二步驟 新帳號登入 預設將會『 rm='rm -i' 』『 cp='cp -i' 』『 mv='mv -i' 』這三個命令別名
    
        vim /etc/skel/.bashrc
        alias rm='rm -i'
        alias cp='cp -i'
        alias mv='mv -i'
        [esc] :wq存檔離開
```
---


**E. 特殊帳號管理的需求：**


    ● 讓 /home 這個目錄支援使用者與群組的 Quota 磁碟配額功能
    ● 增加一個名為 examgroup 的群組
    
### 解答
```
    第一步驟 讓 /home 這個目錄支援使用者與群組的 Quota 磁碟配額

        先將全部的tty使用者登出
        之後 [ctrl+alt+F6] 進行登入
	who #確認全部tty是否只剩下tty6(就現在這個介面)
	vim /etc/fstab
        在home後面參數defaults加入,usrquota,grpquota
        :wq
        登出
        打開tty2
	登入root
	umount /home
	mount -a
	df
	mount | grep /home
	exit 或 [ctrl]+ [D]
        
    第二步驟 增加一個名為 examgroup 的群組
    
        groupadd examgroup
        cat /etc/group | grep examgroup
```
F. 特殊帳號建立的需求：請寫一隻名為 /root/users/addusers.sh 的腳本，這個腳本將用來處理特殊帳號的建置。 你應該使用 for…do…done 迴圈的方式來建立這隻腳本，而 for 迴圈內的程式碼，請依序使用如下的方式來建置妥當：


- 建立帳號時的相關參數設計：
  - 帳號名稱為： examuser11 ~ examuser30 共 20 個帳號
  - 建立帳號時，每個帳號都要加入一個名為 examgroup 的次要群組支援
  - 每個帳號的全名說明就是該帳號的名稱
    
  - 每個帳號的密碼均為 myPassWord
  - 並且每個帳號首次登入系統時，都會被強迫要求更改密碼 (chage ??)
  - 每個帳號在 /home 的 Quota 為 soft --> 120MB, hard --> 150MB
  - 修改每位帳號家目錄 (例如 examuser11 家目錄在 /home/examuser11/ ) 的權限成為 drwx--x--x 的模樣
  - 腳本建置完畢後，請務必執行一次，以確定帳號可以順利被建立！
    
### 解答
```
    第一步驟 建立目錄
    
        mkdir /root/users
        
    第二步驟 撰寫腳本
    
        vim /root/users/addusers.sh

    第三步驟 腳本內容
    
        #!/bin/bash
                
        for users in $(seq  11 30)
        do
	    username="examuser${users}"
	    useradd -G examgroup ${username}
            echo myPassWord | passwd --stdin ${username}
            chage -d 0 ${username} 
            xfs_quota -x -c "limit -u bsoft=120M bhard=150M ${username}" /home


            chmod 711 /home/${username}
        done
`
    第四步驟 將腳本能讓任何人都有x權限
    
        chmod a+x /root/users/addusers.sh
        sh /root/users/addusers.sh
    測試
        xfs_quota -x -c "report -ubh" /home (看第一個表格user哦)
```
---
G. 群組共用目錄的功能：請建立一個名為 /data/myexam 的目錄，這個目錄的權限設定是這樣的：

- 1. 關於 examgroup 群組內的用戶權限：
  - 該目錄可以讓 examgroup 的用戶具有完整的權限
  - 而其他人不具備任何權限
  - 在該目錄底下新建的資料(不論檔案還是目錄)，新資料的擁有群組都會是 examgroup
       
- 2. 關於 examuser30 與 student 這兩個帳號的特定要求：
   - 因為 examuser30 帳號被盜，因此 examuser30 針對 /data/myexam 設定為不具備任何權限
   - 因為 student 是管理員的一般帳號，該帳號也需要查詢 /data/myexam 目錄下的資訊。
     - 請讓 student 可以讀、進入該目錄，但不可以寫入該目錄。
     - 未來在此目錄底下新建的任何資料，預設 student 都具有讀與進入目錄的權限(沒有寫入的權限喔！)。
          
### 解答
```
    第一步驟 建立目錄
    
        mkdir -p /data/myexam
        
    第二步驟 關於 examgroup 群組內的用戶權限
    
        chgrp examgroup /data/myexam
        chmod 2770 /data/myexam
	ll -d /data/myexam (查看權限是否為2770 -> drwxrws---)
    
    第三步驟 關於 examuser30 與 student 這兩個帳號的特定要求
    
        setfacl -m u:examuser30:- /data/myexam/
        setfacl -d -m u:student:rx /data/myexam
        setfacl -m u:student:rx /data/myexam
        getfacl /data/myexam
```
---
### 三、系統基本操作，包括系統備份、自動化腳本、時間自動更新等機制


A. 尋找特殊權限的檔名資料：

- 找出在/usr/bin, /usr/sbin 目錄下，具有 s 或 t 等特殊權限的檔名 (SUID/SGID/SBIT)
- 將這些檔名連同權限，完整的複製到 /data/findperm 目錄內
    
### 解答
```
    第一步驟 先尋找有s或t的特殊權限的檔案
    
        find /usr/bin /usr/sbin -perm /7000	(查詢有s或t的特殊權限檔名)
        find /usr/bin /usr/sbin -perm /7000 | wc -l (查詢有s或t的特殊權限有幾筆)

    第二步驟 將這些輸出到/data/findperm
        mkdir -p /data/findperm
        cp -a $(find /usr/bin /usr/sbin -perm /7000) /data/findperm
    
    第三步驟 查看該檔案筆數是否一樣且格式是題目要求的
    
        ll /data/findperm
```
---
B. 找尋 IP 參數輸出：


    ● 使用『 ip addr show 』這個指令，將輸出訊息中，含有 inet 關鍵字的，但不能含有 inet6 關鍵字，一整列輸出
    ● 將上述的結果轉輸出到 /data/mynetwork.txt 檔案中。
    
### 解答

    第一步驟 先在終端機輸入指令查詢
        
        ip addr show | grep 'inet空格'
        
    第二步驟 將輸出的值傳送給指定的檔案
        
        ip addr show | grep 'inet空格' >  /data/mynetwork.txt
        
    第三步驟 查看該檔案
    
        cat /data/mynetwork.txt


---


C. 系統備份腳本製作：由於系統上面有非常多的重要資料必須要進行備份，因此我們想要使用一支 script 來進行備份的動作，且將該 script 定時執行：
- 請撰寫一隻名為 /root/backup_system.sh 的腳本，來進行備份的工作
- 需要備份的目錄有：/etc, /home, /var/spool/mail/, /var/spool/cron/, /var/spool/at/, /var/lib/，腳本的內容為：
  - 第一行一定要宣告 shell 喔！
  - 自動判斷 /backups 目錄是否存在，若不存在則 mkdir 建立她，若存在則不進行任何動作
  - 設計一個名為 mysource 的變數，變數內容以空格隔開所需要備份的目錄
  - 設計一個名為 mytarget 的變數，該變數為 tar 所建立的檔名，檔名命名規則 /backups/mysys_20xx_xx_xx.tar.gz ， 其中 20xx_xx_xx 為西元年、月、日的數字，該數字依據你備份當天的日期由 date 自行取得。
  - 開始利用 tar 來備份
- 請注意，撰寫完畢之後，一定要立刻執行一次該腳本！確認實際有建立 /backups 以及相關的備份資料喔！
    
### 解答
    第一步驟 編輯腳本
    
    vim /root/backup_system.sh
        
    第二步驟 撰寫腳本
    
        #!/bin/bash

        [ ! -d /backups ] && mkdir /backups
        
        mysource="etc /home /var/spool/mail/ /var/spool/cron/ \
        /var/spool/at/ /var/lib/"

        mytarget="/backups/mysys_$( date +%Y_%m_%d ).tar.bz2"

        tar -jc -f $mytarget $mysource
        
    第三步驟 將腳本能讓任何人都有x權限

        chmod a+x /root/backup_system.sh
        sh /root/backup_system.sh

    第四步驟 查看是否有gz檔
        ll /backups
---


**D. 定期備份功能：排定上述的備份指令在每個星期 6 的凌晨 2 點進行這個備份的動作，且這個script在執行的時候：**


    ● 備份指令執行的過程請使用資料流重導向將過程完整的儲存在 /backups/backup.log 這個檔案中(包括正確與錯誤資訊)
    ● 使用 NI值 10 來執行此指令。
    
### 解答
    第一步驟 編輯工作排程
    
        vim /etc/crontab
    
    第二步驟 撰寫工作排程
    
    0  2  *  *  6  root  /bin/nice -n 10 /root/backup_system.sh &> /backups/backup.log
    :wq存檔離開
---
E. 我的 /usr/sbin/setquota 這個檔案不小心刪除了，該如何救回來？(可以使用 rpm 去追蹤是哪個軟體提供的檔案後，移除再安裝該軟體即可完成此題目。)：**


### **解答**



    第一步驟
    
        ll /usr/sbin/setquota (應該是沒有才對)
    
    第二步驟 先移除該軟件
        
        rpm -qf /usr/sbin/setquota (查詢該軟件名稱)
        yum -y remove  quota -4.04-10.el8.x86_64 (移除該軟件)
        
    第三步驟 安裝該軟件
        
        yum -y install  quota-4.04-10.el8.x86_64
        
    第四步驟 查看目錄是否存在了
    
        ll /usr/sbin/setquota
---


F. 服務的管理部份，以 httpd 為例：

- 讓你的 Linux 成為 http 以及 https 支援兩者的網頁伺服器
- 每次開機都會主動的喚醒這個伺服器功能
- Internet 應該要能夠連線到你的 http 以及 https 埠口服務
- 當輸入 http://localhost 時，可以看到的是你的 (1)姓名與 (2)學號
- 注意：上課提到的服務建置五個步驟！

### 解答
```
    第一步驟 服務建置五個步驟
        
        yum install httpd
        firewall-cmd --add-service=http --permanent
        firewall-cmd --add-service=https --permanent
	firewall-cmd --reload
        systemctl enable --now httpd
        systemctl status httpd
        firewall-cmd --list-all
    
    第二步驟 撰寫首頁的html檔
    
        cd /var/www/html
        vim index.html

        ## idnex.html的內容 ##
        This is DIS in KSU

        :wq
        
    第三步驟
	開網頁搜尋http://localhost如果有學號姓名就成功
        或者用指令輸入 curl  http://localhost
```
---

### **四、腳本建立與系統管理**


**A. 特殊原因，例如電力固定維護的關機情境問題：**


    ● 你的系統將在 7 月的 20 號 08:00 進行關機的歲修工作，請以『單次』工作排程來設計關機的動作 (poweroff)
    
### 解答
```
    第一步驟
    
        at  08:00 2024-07-20 (單次格式為at HH:MM YYYY-MM-DD)時:分 西元年-月-日
        at> poweroff 關機指令
        at> [ctrl+d] 結束設定指令
    
    第二步驟
    
        atq查詢是否建立成功
	做錯可以用atrm 編號數字刪除
        舉例 利用atq指令查到編號是1的寫錯
        輸入atrm 1 就可以刪除
```
---


B. 系統開機的通知訊息：

- 系統開機之後，會自動寄出一封 email 給 root，說明系統開機了。
- 指令可以是『 echo "reboot new" | mail -s 'reboot message' root 』
- 請注意，這個動作必須是系統『自動於開機完成後就動作』，而不需要使用者或管理員登入喔！(hint: rc.local)
    
### 解答
```
    第一步驟 編輯開機前會執行的檔案
    
        vim /etc/rc.d/rc.local
        
    第二步驟 撰寫以上指令
    
        echo "reboot new" | mail -s 'reboot message' root
        :wq存檔離開
        
    第三步驟 更改權限
    
        chmod a+x /etc/rc.d/rc.local
```
---

C. 為了方便大家使用 ps 外帶的參數來查詢系統的程序，因此管理員建立一隻名為 /usr/local/bin/myprocess 的腳本讓大家方便使用，腳本內容主要為：

    ● 第一行一定要宣告 shell 為 bash 才行；
    ● 主要僅執行『 /bin/ps -Ao pid,user,cpu,tty,args 』
    ● 這隻腳本必須要讓所有人都可以執行才行！
    
### 解答
```
    第一步驟 編輯腳本
        
        vim /usr/local/bin/myprocess.sh
        
    第二步驟 撰寫腳本
    
        #!/bin/bash
	/bin/ps -Ao pid,user,cpu,tty,args
        :wq存檔離開
    
    第三步驟 將腳本給予X權限
    
        chmod a+x /usr/local/bin/myprocess.sh
        
    第四步驟 將腳本執行
    
        sh /usr/local/bin/myprocess.sh
```
---


**D. 寫一隻名為 /usr/local/bin/myans.sh 的腳本，這隻腳本的執行結果會這樣：**


- 腳本內第一行一定要宣告 shell 為 bash
- 當執行 myans.sh true 時，螢幕會輸出『 Answer is true 』，且訊息為預設的 Standard output
- 當執行 myans.sh false 時，螢幕會輸出『 Answer is false 』，且訊息輸出到 starndard error output
- 當外帶參數不是 true 也不是 false 時，螢幕會輸出『 Usage: myans.sh true|false 』
- 這隻腳本必須要讓所有人都可以執行才行！
    
### 解答
```
    第一步驟 編輯腳本
        
        vim /usr/local/bin/myans.sh
        
    第二步驟 撰寫腳本
    
        #!/bin/bash
        
        case ${1} in
        
        "true" )
                echo "Answer is true"
                ;;
        "false" )
                echo "Answer is false" 1>&2
                ;;
        *)
                echo "Usage: myans.sh true|false"
                ;;
        esac
    
     第三步驟 將腳本給予X權限

        chmod a+x /usr/local/bin/myans.sh
    
    第四步驟 將腳本執行

        sh /usr/local/bin/myans.sh  true
        出現 Answer is true 對的

        sh /usr/local/bin/myans.sh  false
        出現 Answer is false 對的

        sh /usr/local/bin/myans.sh
        出現Usage: myans.sh true|false對的
```
---
### 五、基礎檔案管理與檔案系統維護：
A. 打包檔案的格式錯誤問題：

- 你的系統中有個檔名 /root/mybackup 的檔案，這個檔案原本是備份系統的資料，但副檔名不小心寫錯了！
- 請將這個檔案修訂成為比較正確的副檔名 (例如 /root/mybackup.txt 之類的模樣)
- 並且將該檔案在 /srv/testing/ 目錄中解開這個檔案的內容。
    
### 解答
``` 
    第一步驟 查詢檔案
    
        ll /root/
        mv /root/mybackup /root/mybackup.tar.xz (將檔案更改承xz壓縮檔)
        or 或是
        cat /root/mybackup > /root/mybackup.tar.xz
    第二步驟 建立所需資料夾
        
        mkdir /srv/testing/
    
    第三步驟 解壓縮到指定目錄
    
        tar -Jxf  /root/mybackup.tar.xz -C  /srv/testing/
        
    第四步驟 查看指定目錄
    
        ll /srv/testing
```
---
B. LVM 彈性容量變化的效果：
- 目前的系統中， /home 應該使用的是 LVM 的維護模式。請找出 /home 所在的 VG 剩餘容量
- 將 VG 所有剩餘容量通通提供給 /home 使用，因此整個 /home 的容量將會放大很多。
- 在目前的系統中，掛載在 /home 的LVM格式資料，請將它的容量變成
- 且這個目錄內的資料並不會消失(無須重新格式化的意思)。
    
### 解答

    第一步驟 查看LVM剩下多少容量
        
        vgscan  (看rocky)
        vgdisplay rocky (看Free容量多少)
        
    第二步驟
    
        lvresize  -l  +1536 /dev/rocky/home (增加容量)
        df -Th /home (尚未更改容量)  
        xfs_growfs /home  /*重新調整*/
        df -Th /home (更改容量了)

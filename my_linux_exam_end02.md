# vm2
# 期末考練習VM2：系統設定與操作部份

不是題目：
這個系統容量比較小，而且預設在文字界面
這個系統目前已經沒有圖形界面，所以你無法進入圖形界面。
若你想要使用圖形界面操作系統，可能需要安裝 yum groupinstall GNOME 才行。
同樣的，預設界面一定要是純文字界面才可以！

    systemctl get-default
    systemctl set-default multi-user.target

如果要暫時切換到圖形介面，利用底下指令(不影響上述設定)
    
    systemctl isolate graphical.target

---

### 一、系統初始化設定**

#### 1. 這部主機真的怪怪的，之前的管理員似乎在這部主機上面進行一些測試，追問之下有一些可能發生的原因，問題還不只一個。 請依據底下可能發生原因的問題予以克服，最終讓系統可以直接以 root 登入！ (hint: 千萬不要忘記 .autorelabel 的動作！)
A. 系統救援 - 1 ：核心相關資料錯誤造成的問題
 
     ● 原因：因為要測試核心開機功能，結果不小心將 initramfs 檔案刪除掉了，所以應該是無法順利開機。
     ● 建議救援方式：使用原版光碟處理，請進入系統救援的模式，並依據系統既有的核心版本，將 initramfs 重建
     ● 注意：(1)重建時，應考慮 grub2 的原本設定檔 (可能會在哪裡？)，以找到正確的檔名，方可順利成功開機喔。 (2)你可以使用任何一個 CentOS 8.x 的光碟做救援，但是需要注意不同的核心版本的問題！
 
 ### 解答


 
### 第一步驟
	關閉機器 > 選擇期末考作業系統 > 下面選擇光碟開機 > 選擇centos8的版本 > 
	選擇開機順序為光碟在硬碟 > 即可啟動開機進入安裝模式

### 第二步驟

    進入後選擇第三個Troubleshooting > 在選擇第二個Rescue a CentOS Linux system
 	> 按下1安裝 > 在按enter

### 第三步驟

    cp /boot/initramfs-4.18.0-147.el8.86_64.img /mnt/sysimage/boot/
    chroot /mnt/sysimage
	cd /boot
    mv initramfs-4.18.0-147.el8.86_64.img initramfs -4.18.0-147.el8.86_64.img.raw
	dracut -v initramfs-4.18.0-147.el8.86_64.img 4.18.0-147.18.86_64
	touch /.autorelabel
	ls -la / 
---
**B. 系統救援 - 1 ：核心相關資料錯誤造成的問題**

     ● 原因：因為幫用戶設定屬性，結果不小心『可能』修改到 root 這個帳號的相關屬性資料。
     ● 救援恢復的要求： (1)root 密碼恢復到 myCentOS8；(2)root 登入時可順利取得 bash shell 。

### 解答
    echo myCentOS8 | passwd --stdin root
	usermod -s /bin/bash root
---
C. 系統救援 - 3 ：LVM 隨便刪除造成的錯誤
     ● 原因：在發生此錯誤之前，似乎曾經將系統的 LVM swap 刪除，是否如此造成系統錯誤還不得而之。
     ● 建議救援方式：不要使用光碟，使用核心支援的維護模式，查看一下開機選單的內容是否正確。
     ● 注意：在順利登入系統之後，你或許需要 (1)重建 grub 選單以及 (2)修改 /etc/fstab 檔案，以處理 swap 產生的問題
     ● 注意：刪除 swap 為正常的行為，所以，請不要重建 lvm 的 swap 裝置！從選單去處理！ 

### 解答
  
    vim /etc/defaults/grub
    將 GRUB_CMDLINE那行中的 
	resume=/dev…../centos-swap 和 rd.lvm….centos/swap 兩個刪掉
    vim /etc/fstab
    將swap那行給註解後存檔離開
    
    grub2-mkconfig -o /boot/grub2/grub.cfg 
    再次確定有無建立/.autorelabel檔
    
    ls -la /
    
    有就離開然後執行重啟
    exit or [ctrl]+[D]
    reboot
    
    沒有的話就建立
    touch /.autorelabel
    ls -la /
    reboot
---

### 二、系統初始化功能：
A. 針對 YUM 的軟體倉儲設定，你有底下的兩組軟體倉儲位址，請設定好所需要的環境 (全對才給分)：
    ● http://ftp.ksu.edu.tw/FTP/Linux/CentOS/8/AppStream/x86_64/os/
    ● http://ftp.ksu.edu.tw/FTP/Linux/CentOS/8/BaseOS/x86_64/os/
### 解答
    自行設計一個倉儲
    vim /etc/yum.repos.d/dic.repo
    
    [AppStream]
    name = AppStream
    baseurl = http://ftp.ksu.edu.tw/FTP/Linux/CentOS/8/AppStream/x86_64/os/
    gpgcheck = 0
    enabled = 1
    
    [BaseOS]
    name = BaseOS
    baseurl = http://ftp.ksu.edu.tw/FTP/Linux/CentOS/8/BaseOS/x86_64/os/
    gpgcheck = 0
    enabled = 1
完成後存檔離開
---
B. 系統升級行為
    ● 只針對 kernel 升級即可。
    ● 升級完畢請立刻重新開機，且使用新核心

### 解答

    yum clean all (先清除之前的所有快取)
    yum -y update kernel
    如果系統告訴你沒有AppStream倉儲，這是因為你沒有啟動網路
    nmcli connection up ens3
    再次執行
    yum -y update kernel
    reboot
    在開機選單選擇新版本
    這邊預設就是以最新版哦所以不用做額外設定
    
![](https://i.imgur.com/WyNejA7.png)
---
C. 關於開機選單調整

    i.timeout時間設定為 15 秒
    ii. 預設所有的核心參數都會加入 noapic 及 noacpi 兩個參數
    iii. 開機選單多一個回到MBR的設定，選單名稱內亦須包含『 MBR 』字樣，且選單位於最後一個位置
    iv. 開機選單在最後多一項可以進入圖形界面模式，這個圖形界面請使用原有的核心版本 (不是剛剛升級的核心版本)， 且title必須含有『 mygraphical 』的字樣才行！

### 解答
    
    i.timeout時間設定為 15 秒
    
    第一個步驟
        vim /etc/default/grub
        將GRUB_TIMEOUT=5 改 15
        [esc]後:wq存檔離開
        
        vim /boot/grub2/grub.cfg
        set timeout =5 改 15
	
    第二個步驟
        grub2-mkconfig -o /boot/grub2/grub.cfg (立即生效)
![](https://i.imgur.com/9HXE6YK.png)
----------------------------------------------

    ii. 預設所有的核心參數都會加入 noapic 及 noacpi 兩個參數
    
    第一個步驟
        vim /etc/default/grub
        GRUB_CMDLINE_LINUX那行，quiet後面加 noapic noacpi
        [esc]後:wq存檔離開
    第二個步驟
        grub2-mkconfig -o /boot/grub2/grub.cfg (立即生效)
![](https://i.imgur.com/i4ka7MA.png)

----------------------------------------------

    iii. 開機選單多一個回到MBR的設定，選單名稱內亦須包含『 MBR 』字樣，且選單位於最後一個位置
    
    第一個步驟
        vim /etc/grub.d/40_custom
        寫入
            menuentry 'MBR' {

            insmod chain
            insmod ntfs
            set root='hd0'
            chainloader +1

            }
        [esc]後:wq存檔離開
    第二個步驟
        grub2-mkconfig -o /boot/grub2/grub.cfg (立即生效)

![](https://i.imgur.com/TDuPH1l.png)

----------------------------------------------

    iv. 開機選單在最後多一項可以進入圖形界面模式，這個圖形界面請使用原有的核心版本
        (不是剛剛升級的核心版本)， 且title必須含有『 mygraphical 』的字樣才行！
    
    第一個步驟
           vim /boot/loader/entries/502…-4.18….el8.x86_64.conf(原本的核心)
	        title 那一行改 title mygrachical
            
![](https://i.imgur.com/fExpZJe.png)
            
	第二個步驟
        options 那一行最後面加 system.unit=graphical.target 

![](https://i.imgur.com/0kBASCt.png)


	第三個步驟
	    grub2-mkconfig -o /boot/grub2/grub.cfg (立即生效)

        reboot查看以上四點有無更改成功
---

### 三、檔案系統方面的處理，包含分割(注意primary, extended, logical的限制)、格式化、掛載等：
A. 軟體磁碟陣列管理：目前的系統有個出現磁碟出問題而快要損毀 (degrade) 的軟體磁碟陣列，找出並修復好該系統。


    ● 該磁碟似乎已經被拔除一個 partition
    ● 找出系統中具有的跟 RAID 內的 partition 容量相同，且沒有被使用的 partition，那就是這個 RAID 缺乏的磁碟槽 (假設已經被修理好了)
    ● 請將該磁碟槽加入原本的系統中，以救援這個磁碟陣列 (讓他變成 clean 的狀態，改變 degraded 的困擾)
    
### 解答

    第一個步驟
    
        lsblk
        
    第二個步驟
    
        mdadm --detail /dev/md5 (觀察 raid)
        
    第三個步驟
    
        mdadm --add /dev/md5 /dev/vda4 /dev/vda5(重新加入 raid)
        
    第四步驟
    
        mount -a (重新掛載)
---
**B. 記憶體置換容量的處理：**

    ● 目前這個系統裡面似乎所有的 swap 通通被刪除了！不過這樣實在不太好。
    ● 建立大型檔案，檔名為 /myswap.img，容量大概是 1G 即可。
    ● 將上述檔案格式化成為 swap
    ● 每次開機這個 swap 都會主動被系統所使用。
    
### 解答
    第一步驟
    
        dd if=/dev/zero of=/myswap.img bs=1G count=1
        
    第二步驟
    
        mkswap /myswap.img
        
    第三步驟
    
        vim /etc/fstab
        
        最後面加入
        
        /myswap.img swap swap defaults 0 0
        
    第四步驟
    
        swapon -a
        
    第五步驟
    
        觀察是否掛載成功
        swapon -s
---
C. LVM 的建立與 VDO 的應用

    ● 將目前這個系統當中的所有剩餘容量建立成為單一分割槽，並且將 system ID 設定為 LVM
    ● 利用上面這個分割槽，建立一個名為 thevg 的 VG ，且 PE 容量為 8M
    ● 將 thevg 的所有容量通通給予名為 thelv 的 lv，所以最終會出現 /dev/thevg/thelv 的裝置。
    ● 將 /dev/thevg/thelv 加入到 VDO 的支援，建立名為如下的 VDO 裝置
        VDO 名稱為 t
        hevdo
        VDO 虛擬容量為 20G
        裝置使用 /dev/thevg/thelv
    ● 將 VDO 裝置格式化成為 XFS 檔案系統
    ● 此裝置開機後會立刻掛載到 /data/vmdata 目錄下
    
### 解答
    第一步驟
    
        parted /dev/vda print (觀察partition是什麼分割模式)
        
    第二步驟 新建LVM磁碟
    
        gdisk  /dev/vda
        新增
        n
        [enter]
        [enter]
        [enter]
        8e00 (LVM的代號)
        p
        w
        Y
        更新 partprobe
        lsblk (查看是否有新磁碟)
        
    第三步驟 建立LVM
    
        pvcreate /dev/vda6
        pvscan
    
    第四步驟 建立PE
    
        vgcreate -s 8M thevg /dev/vda6
        vgscan
        vgdisplay thevg(查看VG容量)
 

![](https://i.imgur.com/AluqkXE.png)


 

    第五步驟 建立LV
    
        lvcreate -l 100%VG -n thelv thevg
![](https://i.imgur.com/lyvXsY4.png)

        lvdisplay /dev/thevg/thelv
![](https://i.imgur.com/T10dtq2.png)



    第六步驟 安裝VDO
 
         yum install vdo kmod-kvdo
         systemctl restart vdo
         systemctl enabled vdo
         systemctl status vdo (查看服務有無在運作且是enable狀態)
    
    --------------------------------------------
    
    第七步驟 建立VDO
    
         vdo create --name=thevdo --vdoLogicalSize=20G \
         --device=/dev/thevg/thelv --deduplication enabled --compression enabled
    
    第八步驟 查看VDO是否建立成功
         
         vdostats --human-readable

![](https://i.imgur.com/WFeFwiM.png)


    第九步驟 將VDO格式化為XFS
    
        mkfs.xfs /dev/mapper/thevdo
        
    第十步驟 裝置開機後會立刻掛載到 /data/vmdata
    
        mkdir /data
        mkdir /data/vmdata (建立題目要求掛載目錄)
        
        vim /etc/fstab (最後一行加入VDO掛載)
    /dev/mapper/thevdo /data/vmdata xfs defaults,x-systemd.requires=vdo.service 0 0
    
        mount -a (全部掛載)
        df -Th (查看VDO掛載到/data/vmdata有無成功)

---
D. LVM 與 stratisd 的應用**

    ● 將 centos 這個 VG 的剩餘容量，給予一個名為 thepool 的 LVM 裝置，最終會有 /dev/centos/thepool 裝置存在。
    ● 將 thepool 加入到名為 lvmpool 的儲存池 (thin pool) 當中
    ● 建立 thefs 檔案系統，並且開機會掛載到 /data/mypool 目錄下。
    ● 上述掛載最好使用 UUID 來處理！
    
### 解答

    第一步驟
    
        安裝stratisd並啟用服務
        
        yum install -y stratisd stratis-cli
        systemctl start stratisd
        systemctl enabled stratisd
    
    第二步驟 查看VG容量剩下多少
    
        vgdisplay centos
        
    第三步驟 建立LVM
    
        lvcreate -l 100%free -n thepool centos
        y
        lvscan (查看是否建立成功)        
    
    第四步驟 將剛建立的LVM拿來做stratis所需的pool池
        
        stratis pool create lvmpool /dev/mapper/centos-thepool
        stratis pool list
        stratis blockdev list
        
    第五步驟  建立 thefs 檔案系統，並且開機會掛載到 /data/mypool 目錄下。
        
        stratis filesystem create lvmpool thefs (格式化)
        mkdir /data/mypool
        blkid | grep stratis (找/dev/mapper/stratis-.....的UUID)
        vim /etc/fstab
    UUID="......."/srv/pool1 xfs defaults,x-systemd.requires=stratisd.service 0 0
   
       mount -a (全部掛載)
       df -Th (查看是否掛載成功)
---        

### 四、 系統效能處理
A. 請改用『 throughput-performance 』來動態調整系統效能

### 解答

    第一步驟 查看tuned是否為啟動跟開機啟動狀態
    
        systemctl status tuned
        
    第二步驟 將系統效能更改為 throughput-performance
    
        tuned-adm profile throughput-performance
        
    第三步驟 查看目前使用的系統效能是不是更改正確
    
        tuned-adm active
---

B. 調整核心參數，讓每次開機都會修改成為底下這樣：
    ● /proc/sys/vm/dirty_ratio 改成 40
    ● /proc/sys/vm/dirty_background_ratio 改成 5
    ● /proc/sys/vm/swappiness 改成 10
    
### **解答**

:::spoiler

    第一步驟  編輯設定檔
        vim /etc/sysctl.d/mycentos.conf
        
    第二步驟 自己撰寫以上題目要求的三個
    
        vm.dirty_ratio = 40
        vm.dirty_background_ratio = 5
        vm.swappiness = 10
    
    第三步驟 將設定檔 立即生效
        
        sysctl -p /etc/sysctl.d/mycentos.conf 
:::

---

> [time=Fri, Jun 18, 2021 12:42 AM 陳鵬皓學長所撰寫]

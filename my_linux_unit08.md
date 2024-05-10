# TA 第八章作業
## 1.(30%)請登入這次的作業系統環境，並且依據線上的環境實做底下的題目之後，將題目的要求 (指令或者是結果) 寫入到 /root/ans08.txt 檔案內：
- A.當執行完成 mysha.sh 這個指令之後，該指令的回傳值為多少，請寫下 (1)用什麼指令顯示回傳值？ (2)回傳值是多少？
```
echo "A.(1) mysha.sh;echo \$? (2)14" > /root/ans08.txt
```
- B.在不透過 bc 這個指令的情況下，如何以 bash 的功能，計算一年有幾秒？亦即如何計算出 60*60*24*365 的結果？(1)寫下指令， (2)寫出結果
 ```
echo "B.(1) echo \$(( 60*60*24*365 )) (2)31536000" >> /root/ans08.txt
```
- C.使用 find / 找出全系統的檔名，然後將所有資料 (包括正確與錯誤) 全部寫入 /root/find_filename.txt ， 請寫下達成此目標的完整指令串。
```
echo "C.(1) find / &> /root/find_filename.txt" >> /root/ans08.txt
```
- D.寫下一段指令 (主要以 echo 來達成的)，執行該段指令會輸出『 My $HOSTNAME is 'XXX' 』，其中 XXX 為使用 hostname 這個指令所輸出的主機名稱。例如主機名稱為 station1 時，該串指令會輸出『 My $HOSTNAME is 'station1'』。 該串指令在任何主機均可執行，但都會輸出不同的訊息(因為主機名稱不一樣所致)
```
echo "D.(1) echo My \$HOSTNAME is \$(hostname)" >> /root/ans08.txt
```
- E.管理員 (root) 執行 mv 時，由於預設 alias 的關係，都會主動的加上 mv -i 這個選項。假設有個來源檔案名稱 orifile 以及目標目錄名稱 desdir ， 若想要將 orifile 移動到 desdir 去的時候，請寫下兩個方法，讓 root 執行 mv 時，不會有 -i 的預設選項 (不能 unalias 的情況下)
```
echo "E.(1) /usr/bin/mv" >> /root/ans08.txt
```
- F.透過『 ll /usr/sbin/* /usr/bin/* 』搭配 cut 與 sort, uniq 等指令來設計一個指令串，執行該指令串之後會輸出如下的畫面， 請寫下該指令串：(底下畫面為示意圖，實際輸出的個數可能會有些許差異)
- ![image](https://github.com/vbkservices/mybookword/assets/97799165/c2859c51-0854-44c3-8cbc-5e638d621c99)
```
echo "F.(1) ll /usr/bin/* /usr/sbin/* | cut -d \" \" -f 1 | sort | uniq -c" >> /root/ans08.txt
```
## 2.(10%)製作一個名為 mycmdperm.sh 的腳本指令，放置於 /usr/local/bin 裡面。該腳本的重點是這樣的：
- A.執行腳本的方式為『 mycmdperm.sh command 』，其中 command 為你想要取得的指令的名稱
- B.在 mycmdperm.sh 裡面，指定一個變數為 cmd ，這個變數的內容為 ${1}，其中 ${1} 就是該腳本後面攜帶的第一個參數
- C.使用`『 ll $( which ${cmd} ) 』`來取得這個 cmd 的實際權限。
- D.讓 mycmdperm.sh 具有可執行權。
- E.最終請執行一次該指令，例如使用『 mycmdperm.sh passwd 』應該會秀出 passwd 的相關權限。不過該指令應該會執行失敗， 因為上述的 (c) 指令怪怪的，似乎是『命令別名無法用在腳本內』的樣子。因此，請將這個腳本的內容修訂成為沒有問題的形式。 (就是將命令別名改成實際的指令操作)
```
cat > /usr/local/bin/mycmdperm.sh << EOF
#! /bin/bash
cmd=\${1}
ls -l \$( which \${cmd} )
EOF
```
```
## 測試 ##
chmod a+x /usr/local/bin/mycmdperm.sh
/usr/local/bin/mycmdperm.sh passwd
```
![image](https://github.com/vbkservices/mybookword/assets/97799165/70865cbf-1098-447b-950e-f8d7031e4c77)
## 3.(15%)製作一個名為 myfileperm.sh 的腳本指令，放置於 /usr/local/bin 裡面。該腳本的重點是這樣的：
- A.執行腳本的方式為『 myfileperm.sh filename 』，其中 filename 為你想要取得的檔案名稱 (絕對路徑或相對路徑)
- B.在 myfileperm.sh 裡面，指定一個變數為 filename ，這個變數的內容為 ${1}，其中 ${1} 就是該腳本後面攜帶的第一個參數
- C.判斷 filename 是否不存在，若不存在則回報『filename is non exist』
- D.判斷 filename 存在，且為一般檔案，若是則回報『 filename is a reguler file 』
- E.判斷 filename 存在，且為一般目錄，若是則回應『 filename is a directory』
```
vim /usr/local/bin/myfileperm.sh
## myfileperm.sh 的內容 ##
#! /bin/bash
filename=${1}
if [ ! -e ${filename} ];then
    echo "${filename} is non exist"
if [ -f ${filename} ];then
    echo "${filename} is a reguler file"
if [ -d ${filename} ];then
    echo "${filename} filename is a directory"
fi
```
```
## 測試 ##
chmod a+x /usr/local/bin/myfileperm.sh
/usr/local/bin/myfileperm.sh 目錄或檔案名稱(例如: /etc 或是 /etc/passwd)
```
## 4.(15%)製作一個 mymsg.sh 的腳本指令，放置於 /usr/local/bin 底下：
- A.當執行 mymsg.sh 時，螢幕會輸出底下的字樣，然後結束指令。
![image](https://github.com/vbkservices/mybookword/assets/97799165/021218bb-5e15-4d10-9e46-539618fc06a5)
- B.上面的輸出不能使用 echo，請使用 cat 搭配 << eof 這樣的指令語法來處理資料輸出的資訊。
```
vim /usr/local/bin/mymsg.sh 
##  mymsg.sh  的內容 ##
#! /bin/bash
cat << EOF
Hollo!!
My name is 'Internet Lover'...
My server's kernel version is \$kver
I'm a student
bye bye!!
EOF
```
```
## 測試 ##
chmod a+x /usr/local/bin/mymsg.sh 
/usr/local/bin/mymsg.sh 
```
## 5.(10%)檔案與檔案內容處理方法：
- A.找出 /etc/services 這個檔案內含有 http 的關鍵字那幾行，且 http 只會出現在行首！並將該資料轉存成 /root/myhttpd.txt 檔案
```
grep "^http " /etc/services > /root/myhttpd.txt
```
- B.找出 examuser 這個帳號在系統所擁有的檔案，並將這些檔案『移動』到 /root/examuser 目錄中
```
mkdir /root/examuser
mv $(find / -user examuser 2> /dev/null) /root/examuser
```
## 6.(10%)建立一般帳號也可以執行的指令 (全部的指令請放置到 /usr/local/bin 目錄下)
- A.做一個名為 myip 的指令，這個指令會透過 ifconfig 的功能，顯示出 ens3 這張網卡的 IP (只要 IP 就好喔！)。例如 IP 為 192.168.251.12 時，則輸入 myip 這個指令，螢幕只會輸出 192.168.251.12 的意思。
```
vim /usr/local/bin/mypi
##  mymsg.sh  的內容 ##
#! /bin/bash
ifconfig | grep "inet " | awk 'NR==1 {print $2}'
```
```
## 測試 ##
chmod a+x /usr/local/bin/mypi
/usr/local/bin/mypi
```
- B.建立一個名為 myerr 的指令，這個指令會將『 echo "I am error message" 』這個訊息傳輸到 standard error output 去！ 亦即當執行『 myerr 』時，會在螢幕上出現 I am error message，但是執行『 myerr 2> /dev/null 』時，螢幕不會有任何訊息的輸出。
```
vim /usr/local/bin/myerr
##  mymsg.sh  的內容 ##
#! /bin/bash
echo "I am error message" 1>&2
```
```
## 測試 ##
chmod a+x /usr/local/bin/myerr
/usr/local/bin/myerr 2> /dev/null
```
## 7.(10%)建立一個名為 /root/split 的目錄，進行如下的行為：
- A.將 /etc/services 複製到本目錄下
```
mkdir /root/split
cp /etc/services /root/split/
```
- B.假設 services 容量太大了，現在請以 100K 為單位，將該檔案拆解成 file_aa, file_ab, file_ac.. 等檔名的檔案， 每個檔案最大為 100K (請自行 man split 去處理)
```
cd  /root/split
split -b 100K services "file_"
```

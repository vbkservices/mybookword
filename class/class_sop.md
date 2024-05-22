# 教室軟體安裝
```
 2022/08/27 #updata
 2024/05/22 #updata
```
## 一、上課用Windows10([記得啟用，認證連結](http://ftp.ksu.edu.tw/ksu/os2.html))

### 安裝軟體

| 安裝在C槽                                                |                                                       |                                                             |                                                           |
| ------------------------------------------------------ | ----------------------------------------------------- | ----------------------------------------------------------- | --------------------------------------------------------- |
| [7-zip](http://www.developershome.com/7-zip/)          | [Notepad++](https://notepad-plus-plus.org/downloads/) | [GanttProject](https://www.ganttproject.biz)                                | [WinSCP](https://winscp.net/eng/download.php)             |
| [PieTTY](https://sites.google.com/view/pietty-project) | [jEdit](https://jedit.en.softonic.com/)               | [Java(JDK)](https://www.oracle.com/java/technologies/downloads/) | [Java](https://www.java.com/zh-TW/)
| [PotPlayer](https://potplayer.daum.net/)               | [FileZilla](https://filezilla-project.org/)                                | [VirtualBOX](https://www.virtualbox.org/)                   | [EverCam](https://tw.formosasoft.com/cpage/download/)     |
| [XnView](https://www.xnview.com/en/xnviewmp/#downloads)   |[VSCode](https://code.visualstudio.com/)       |[pGina](http://pgina.org/)          | [remote-viewer](https://releases.pagure.org/virt-viewer/) |         |
| iClone 7(USB安裝)   |  Office([學校下載](http://ftp.ksu.edu.tw/ksu/office1.html))     | Adobe CC(USB安裝)         | [ODF安裝](https://my.ksu.edu.tw/file/bbcode/w3Pages/51245/8d181006-e390-4213-9201-4d684f6ae67f/3.%E5%9C%8B%E7%99%BC%E6%9C%83%20ODF%20%E8%BB%9F%E9%AB%94%E5%AE%89%E8%A3%9D%E8%AA%AA%E6%98%8E.pdf.pdf)         |


| 放在D槽(免安裝檔) |      |      | 
| ----------------- | ---- | ---- |
| [Processing 32/64](https://processing.org/download)  | [Unity](https://unity3d.com/get-unity/download)    | android-studio-bundle    |

如沒有D槽，在C槽創建一個資料夾，將免安裝檔放置進去。

| 瀏覽器 |  |
| -------- | -------- | 
| [Chrome](https://www.google.com.tw/chrome/)     | [Firefox](https://www.mozilla.org/zh-TW/firefox/new/)     |

### 注意事項
<ul>
    <li>Chrome 必須以系統管理員身分安裝!!! (如不使用或沒管理員身分，則只會安裝在自己的家目錄"%APPDATA%…\Local"下，導致新使用者須重新安裝Chrome)</li>
    <li>創建software 資料夾至C槽並新增捷徑到桌面，將安裝的軟體捷徑複製在裡面。</li>
    <li>cpu i5 不能使用 cpu i7 的iclone (必須做兩台原始機一台i5 一台 i7)</li>
    <li>Notepad++ 要安裝有FTP功能的版本</li>
    <li>將首次登入動畫關閉，還有把與上次登入使用者互動關閉 (教學在pGina的Step 7&8)</li>
    <li>Remote Viewer，將快捷列定在工作列上，然後按下右鍵，選擇內容後需加入"C:\Program Files\VirtViewer v8.0-256\bin\remote-viewer.exe" --spice-disable-audio --spice-cache-size=100000000</li>
    <li>JAVA，修改"系統"環境變數(本機右鍵-內容-進階系統設定-環境變數-系統變數，點選PATH編輯加上C:\Program Files\Java\jdk-10.0.2\bin (以電腦路徑為主)</li>
    <li>Adobe AE語言問題，開啟檔案位置-資料夾AMT找到application記事本開啟，最底下改語系將zh_TW改為en_US。
（需要先存在桌面，再拉回資料夾覆蓋）</li>
</ul>


### 安裝完成檢查步驟

<ul>
    <li>隱藏的磁碟</li>
    <li>下載資料夾、垃圾桶、網路coockie快取等，記得刪除紀錄</li>
    <li>暫存檔清空、磁碟清理、系統暫存檔清理</li>
    <li>更新系統及軟體等(清理WindowsUpdate暫存&舊版Windows)</li>
    <li>註冊腳本放置隱藏、照順序點擊啟動！</li>
    <li>時間設定使用 ntp.ksu.edu.tw</li>
    <li>電源設置：螢幕、電源設定30分鐘</li>
    <li>虛擬記憶體使用2048MB</li>
    <li>作業系統、Adobe、office要認證及更新。</li>
</ul>

## 二、考場用Windows10([記得啟用，認證連結](http://ftp.ksu.edu.tw/ksu/os2.html))

### 安裝軟體


| 安裝軟體 |  | 
| -------- | -------- |
|  illustrator    | photoshop     | 
<ul>
    <li>Office、chrome、SSE(系辦拿最新檔案，將路徑放置桌面，以google開啟)</li>
    <li>隱藏的磁碟</li>
    <li>下載資料夾、垃圾桶、網路coockie快取等，刪除紀錄</li>
    <li>時間設定使用 ntp.ksu.edu.tw</li>
    <li>電源設置：螢幕、電源設定30分鐘</li>
    <li>虛擬記憶體使用2048MB</li>
    <li>作業系統、Adobe、office要認證及更新。</li>
</ul>


### I4502證照考試(考試前兩小時做)
<ul>
    <li>將實驗室的switch 19與7對接</li>
    <li>將i4502 switch第二台最右邊孔換成"i2504與i4502對接的線</li>
    <li>網路選單調成預設考場環境</li>
    <li>檢查對接後網路是否通</li>
    <li>考完記得將選單還有switch換回來</li>
</ul>

## 三、Java
#### 選擇windows-64_bin.exe

![](https://i.imgur.com/93Ypyk8.png)

    下載完路徑 C:\Program Files\Java\jdk-13.0.1
    到C:\Program Files\Java\jdk-13.0.1\bin複製該路徑
    至本機的內容-進階系統設定-環境變數
    點選使用者變數的"Path"，在點選編輯，
    將複製的路徑複製在新增欄位
    
![](https://i.imgur.com/1FmeCSe.png)

    再到命令提示字元(CMD)執行javac，及安裝完成！
    
    
## 四、pGina
    修改任何機碼前建議事先備份原始擋，以便修改錯誤復原用，
    本機使用者 (以 dic 為例) 不需修改，所以修改完匯出後，必須執行一次 original.reg
    回復原本設定 (不然本機使用者會找不到原始路徑)。
    執行完底下之動作後即會有兩份登錄檔 (original.reg 與 desktop.reg)
    與一份身分辨識用之批次檔(usercheck.bat 可自行轉 為 exe 檔)
    (usercheck.exe 在SOP隨身碟裡面)
    

##### Step 0
本系統必須先將使用者帳戶控制設定修改至不要通知，方法如下 Winkey > 控制台 > 系統安全性 > 變更使用者帳戶控制設定 > 修改至不要通知

##### Step 1
使用系統管理員身分開啟 regedit 開始>搜尋>regedit

![](https://i.imgur.com/95hQ41I.png)

##### Step 2
備份 User Shell Folders 欲修改設定檔 路徑為(必須先到以下之路徑)：HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\ User Shell Folders 檔案 > 匯出 > 檔名設為 original.reg

![](https://i.imgur.com/z92762d.png)


##### Step 3
更改 User Shell Folders 設定
• 找到名稱為{374DE290-123F-4565-9164-39C4925E467B} > 右鍵 >
修改將 %USERPROFILE%更改為\\\172.31.255.251\\%username%\
• 找到名稱為Desktop> 右鍵 > 修改將%USERPROFILE%更改為 \\\172.31.255.25\common\\%username%(Download、Desktop)

![](https://i.imgur.com/4aI7yvh.png)

#### Step 4
匯出此次 User Shell Folders 設定，並回復初始值
<ul>
    <li>1.檔案 > 匯出 > 檔名設為 desktop.reg</li>
    <li>2.執行 step2 所備份之 original.reg</li>
</ul>

#### Step 5
將檔案放到共用資料夾,讓每位使用者都可以使用將desktop.reg、original.reg與usercheck.exe放置到C:\Users\Public\ 底下並且將其隱藏,避免其他使用者誤刪這些檔案

PS.放置完記得先點desktop.reg再點original.reg最後點 usercheck.exe才讀的到ID server。

![](https://i.imgur.com/rP62EST.png)

#### Step 6
注意倒斜線是否標錯\且後面不得有空字元
設定機碼使機器每次登入都執行usercheck.exe設定路徑為：
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Run
右鍵 > 新增 > 字串值設定剛剛新增的字串值 > 右鍵 > 修改 > 輸入 C:\Users\Public\usercheck.exe > 確定(可自行決定要不要變更本字串質之名稱)
將多餘機碼刪除，留下(預設值)、pGina

![](https://i.imgur.com/40TW8aq.png)

設定完上述之動作後,此時用戶端電腦還是無法使用本系統所提供之帳號，必須下載 pGina 來取代原先 windows 之認證方式！
下載網址為: http://pgina.org/download.html

安裝步驟：
執行 pGinaSetup > Next > 將 I accept the agreement 選起來 > Next*3 > Install > 是 > Finish

### 設定方式
#### Step 0
選擇 Plugin Selection > 將LDAP的Authentication與Gatway打勾，並且點 Configure

![](https://i.imgur.com/DeczAW4.png)


#### Step 1
在LDAP Host(s) 的欄位填入server IP,並且在以下填入 (注意後方不能有空白建)
Group DN pattern ：cn=%g,ou=group,dc=dic,dc=ksu
User DN Pattern ：cn=%u,ou=login,dc=dic,dc=ksu

![](https://i.imgur.com/vI7CT1K.png)
![](https://i.imgur.com/CRRznL5.png)

#### Step 2
完成上述動作後,點擊 Gateway，並在下方欄位分別填入 student 與Administrators(群組屬於 student 的帳號在登入後加入本機群組 Administrators 裡)，點擊 Add Rule 即可加入,再填入 teacher 與 Administrators(群組屬於 teacher 的帳號在登入後加入本機群組 Administrators 裡) 點擊 Add Rule 加入此條件! > Save > Apply > Save & Close

※若你不希望你的 client 機器不擁有 Administrator 權限,請跳過此步驟!
(目前系上說要開放client權限，因此圖例為有開放client Administrators權限)。

![](https://i.imgur.com/nGCXAV2.png)

#### Step 3
將本機使用者設定必須密碼才能登入
Winkey + R > 輸入 netplwiz > 將「必須輸入使用者名稱和密碼，才能使用這台電腦」打勾 > 套用 > 確定

![](https://i.imgur.com/FmIwgyR.png)

#### Step 4

重新啟動之後就可以使用本系統所控管之帳號進行登入之動作
使用本系統所提供之帳號登入一次後即可使用,請勿自行將登入後執行之批次檔關閉。

#### Step 5

控制台 > 系統及安全性 > 系統 > 進階系統設定 > 進階 > 效能裡的設定 > 虛擬記憶體變更為2048M

#### Step 6

Windows Key + R > 輸入gpedit.msc > 電腦設定底下的系統管理範本 > windows元件 > windows 登入選項 > “在重新開機後，自動登入並所棟最後一個互動使用者” 及 “將模式設定為從重新開機或冷開機後自動登入最後一個互動使用者帳號” > 改成停用

#### Step 7

Windows Key + R > 輸入gpedit.msc > 電腦設定底下的系統管理範本 > 系統 > 登入 > 顯示首次登入動畫 > 改成停用

#### Step 8
使用者登入隱藏
win+R
secpol.msc
本機原則 安全性 不要顯示上次登錄（啟用 )

## 五、疑難排解
### pGina
1. 系統發生384錯誤

pGina目前使用的SMB版本為1.0，在Windows 10系統版本號1807(含)以上的版本更新沒有預裝SMB1.0版本，必須至 控制台>程式集>程式和功能>開啟或關閉 Windows 功能，勾選 SMB 1.0/CIFS File Sharing Support，確定後重啟電腦生效。

![](https://i.imgur.com/ycQMx9I.png)


### Adobe Photoshop
1. 必須為介於 96 和 8 之間的整數

<ul>
    <li>此問題可能發生於：使用崑山FTP提供的CC 2018版本、更新最新版Windows或停留在舊版的Adobe軟體。</li>
    <li>系統如有使用pGina，必須加入機碼至 desktop.reg 以自動開機執行生效。</li>
</ul>

#### Step 0
按下 Windows + R 鍵以開啟「執行」對話方塊。
輸入 regedit.exe，然後按下 Enter。

#### Step 1
在「登錄編輯器」視窗，根據您的 Photoshop 版本導覽至以下的資料夾路徑:

<ul>
    <li>Photoshop CC 2018: 導覽至 HKEY_CURRENT_USER\SOFTWARE\Adobe\Photoshop\120.0
</li>
    <li>Photoshop CC 2017: 導覽至 HKEY_CURRENT_USER\SOFTWARE\Adobe\Photoshop\110.0
</li>
    <li>Photoshop CC 2015.5: 導覽至 HKEY_CURRENT_USER\SOFTWARE\Adobe\Photoshop\100.0
</li>
    <li>Photoshop CC 2015: 導覽至 HKEY_CURRENT_USER\SOFTWARE\Adobe\Photoshop\90.0
</li>
    <li>Photoshop CC 2014: 導覽至 HKEY_CURRENT_USER\SOFTWARE\Adobe\Photoshop\80.0
</li>
</ul>

#### Step 2
根據您執行的 Photoshop 版本，以滑鼠右鍵按一下先所述的資料夾，並從功能表上選擇「新增 > DWORD (32 位元) 值」。

#### Step 3
在右側面板中，將新機碼命名為「OverridePhysicalMemoryMB」並按下 Enter。

#### Step 4
以滑鼠右鍵按一下新機碼，然後從功能表中選擇「修改」。在「編輯 DWORD (32 位元) 值」對話方塊中，請執行以下步驟：

1. 將「底數」設為「十進位」。
2. 在「數值資料」欄位中，將數值從 0 變更為反映您系統實際記憶體的 MB 數值。舉例來說，若是 4 GB 請輸入「4096」、若是 8 GB 請輸入「8192」、若是 16 GB 請輸入「16384」，若是 24 GB 請輸入「24576」。
3. 按一下「確定」。

#### Step 5
重新開啟Adobe Photoshop CC。
> 參考資料：Adobe 存取效能偏好設定時發生錯誤 -「[必須為介於 8 和 96 之間的整數。](https://helpx.adobe.com/tw/photoshop/kb/invalid-numeric-entry-integer-96-8-required-photoshop.html)」


### Adobe CC 無法安裝
<ul>
    <li>問題可能是磁碟的容量不足，因為CC版解完壓縮就佔20G的容量，但CC版全部裝完需要大約35G的容量</li>
    <li>解決方法將壓縮檔解壓縮到D槽，在去D槽安裝Adobe CC版至C槽</li>
</ul>

# I3501、I3502 學生端電腦製作流程 110/7/13
這個研究的目的是希望能夠做出省錢又省空間的個人主機，主要是透過虛擬化來建置這個環境。
一般的虛擬機在使用上都會有明顯的延遲，不太能當作日常使用的個人電腦。
然而透過CPU的虛擬化技術以及PCI-passthrough就可以將CPU和PCI裝置直接給虛擬機使用。

:::info
**硬體設備**
:::

* **CPU：i7-4790 (4C8T)** **(要有支援VT-d虛擬化技術的CPU，建議i7以上)**
* **記憶體: 24GB**
* **顯示卡: 2顆AMD Radeom HD 8760**
* **硬碟:** 
**SSD(固態硬碟)：240G(當系統碟用)**
**HDD(傳統硬碟)：1T*2(做Raid1，儲存容量用)**
* **主機板型號: 尚未查詢** **(要有支援VT-d虛擬化技術)**

---

:::info
**BIOS 設定**
:::

## **進入BIOS > 進階模式 (將VT-d、初始顯示卡、多顯示器GPU、CSM安全啟動)要開啟**
### 插上隨身碟後重啟電腦 連按 [F8] 直到跳出開機選單，選擇隨身碟型號使用BIOS模式開機

---

:::info
**Rocky Linux8 系統安裝**
:::

## **磁碟分割**

### **SSD 240G磁碟分配 (實際可用220G)**

:::success

| **系統碟(/)** | **學生端快照(/vm_img)** | **開機選單(/boot)** |
|:------------:|:-------------------:|:-----------------:|
|    **15G**   |      **203.8G**        |       **2G**      |

:::

### **HDD 1000G磁碟分配(實際可用960G)**

:::success


|**swap**|**虛擬機資料(/vm_data)**|
|:------:|:--------------------:|
| **4G** |      **927.3G**      |

:::

> **除了 /boot(ext4)、swap(swap) 以外都是 xfs 檔案格式**


> swap分割參考
> https://access.redhat.com/documentation/en-us/red_hat_enterprise_linux/8/html/managing_storage_devices/getting-started-with-swap_managing-storage-devices#recommended-system-swap-space_getting-started-with-

## 系統安裝流程

> 磁碟分割依照上述分割表分割，**!!注意分割槽的硬碟有沒有對應好!!**

![](https://i.imgur.com/XFLnjWu.jpg)

![](https://i.imgur.com/3Ner9dT.jpg)

![](https://i.imgur.com/R3TYQDE.jpg)

![](https://i.imgur.com/GEhBfMt.jpg)

![](https://i.imgur.com/oCE6JaU.jpg)

![](https://i.imgur.com/el5G1D2.jpg)

![](https://i.imgur.com/t3S1i87.jpg)

![](https://i.imgur.com/7k63ctE.jpg)

![](https://i.imgur.com/9ZfQJQL.jpg)

![](https://i.imgur.com/FBu1uwh.jpg)

![](https://i.imgur.com/snozR9j.jpg)

![](https://i.imgur.com/CX9JjV0.jpg)


---

## 全系統更新

```
[root@localhost ~]# yum update -y

上次中介資料過期檢查：0:01:11 以前，時間點為 西元2019年11月28日 (週四) 17時17分12秒。
依賴關係解析完畢。
===================================================================================================================================================
 軟體包                                         架構            版本                                                      軟體庫              大小
===================================================================================================================================================
Installing:
 kernel                                         x86_64          4.18.0-80.11.2.el8_0                                      BaseOS             424 k
 kernel-core                                    x86_64          4.18.0-80.11.2.el8_0                                      BaseOS              24 M
 kernel-modules                                 x86_64          4.18.0-80.11.2.el8_0                                      BaseOS              20 M
Upgrading:
 anaconda-core                                  x86_64          29.19.0.43-1.el8_0                                        AppStream          2.1 M
...
..
已安裝:
  kernel-4.18.0-80.11.2.el8_0.x86_64            kernel-core-4.18.0-80.11.2.el8_0.x86_64         kernel-modules-4.18.0-80.11.2.el8_0.x86_64
  xorg-x11-drv-fbdev-0.5.0-2.el8.x86_64         xorg-x11-drv-vesa-2.4.0-3.el8.x86_64            grub2-tools-efi-1:2.02-66.el8_0.1.x86_64

完成！
------------------------------------------------------------------------------------------
#安裝圖形介面及設定

[root@localhost ~]# yum groupinstall "Server with GUI" -y
[root@localhost ~]# systemctl set-default graphical

#重新啟動

[root@localhost ~]# reboot
```

### 重新啟動後選擇新核心開機

![](https://i.imgur.com/YNa0hAt.jpg)

![](https://i.imgur.com/kp850oK.jpg)


---

:::info
**下載SRPM核心檔**
:::

**先到ELRepo網站下載SRPM [連結網址](https://elrepo.org/linux/kernel/el8/SRPMS/)**

### **下載目前最新版 (目前以5.13.0版為例)**

```
[root@localhost ~]# wget https://elrepo.org/linux/kernel/el8/SRPMS/kernel-ml-5.13.0-1.el8.elrepo.nosrc.rpm
```

> **複製連結方法**

![](https://i.imgur.com/ejgMmj2.jpg)



:::info 
**安裝SRPM核心檔**
:::

```

[root@localhost ~]# rpm –ivh kernel-ml-5.13.0-1.el8.elrepo.nosrc.rpm

# ll觀察，會看到一個名稱叫rpmbulid的資料夾，代表安裝成功

[root@localhost ~]# ll
```
![](https://i.imgur.com/o2FAXIu.jpg)


---

:::info 
**下載解壓縮linux的核心**
:::

**Linux核心網址 [連結網址](http://cdn.kernel.org/pub/linux/kernel/v5.x/)**

## **找到相對應的版本下載 例如你安裝的SRPM版本是 🡪 5.13 那核心也要就得安裝一樣版本的 🡪 5.13**

#### **先移動到正確目錄底下**

```
[root@localhost ~]# cd rpmbuild/SOURCES
```

#### **安裝對應核心**

```
[root@localhost SOURCES]# wget http://cdn.kernel.org/pub/linux/kernel/v5.x/linux-5.13.tar.xz
```
![](https://i.imgur.com/DqzfSdD.jpg)

#### 解壓縮這個核心檔

```
[root@localhost SOURCES]# tar -Jxf linux-5.13.tar.xz
```

#### **查看解壓縮檔案**

```
[root@localhost SOURCES]# ll
```

> 你會發現到有一個linux-5.13的資料夾代表解壓成功

---

## **這時候注意先把 /root/rpmbuild 目錄移動位置**

```
[root@localhost SOURCES]# cd

[root@localhost ~]# mv /root/rpmbuild /vm_data/

[root@localhost ~]# ln -s /vm_data/rpmbuild /root/rpmbuild
```

> **目的是稍後的編譯核心動作會需要足夠的空間，因此把資料夾移動到先前建立的 /vm_data 即是 HDD 硬碟掛載點容量才會夠用，利用符號連結做連動就可以達到一樣的目錄位置也可以找到 rpmbuild 這個資料夾**

---

:::info 
**修改核心檔案**
:::

#### **首先先切換到剛剛解壓縮的資料夾的 drivers 裡面的 pci**

```
[root@localhost ~]# cd /root/rpmbuild/SOURCES/linux.5.13/drivers/pci
```

#### **編輯核心設定檔案**

```
[root@localhost pci]# vim quirks.c
```

#### **搜尋 SV, 按n找到類似這段程式**

```
static int pci_quirk_mf_endpoint_acs(struct pci_dev *dev, u16 acs_flags)
{
        /*
         * SV, TB, and UF are not relevant to multifunction endpoints.
         *
         * Multifunction devices are only required to implement RR, CR, and DT
         * in their ACS capability if they support peer-to-peer transactions.
         * Devices matching this quirk have been verified by the vendor to not
         * perform peer-to-peer with other functions, allowing us to mask out
         * these bits as if they were unimplemented in the ACS capability.
         */
        acs_flags &= ~(PCI_ACS_SV | PCI_ACS_TB | PCI_ACS_RR |
                       PCI_ACS_CR | PCI_ACS_UF | PCI_ACS_DT);
}

###    在以下新增程式碼    ####

 static bool acs_on_downstream;
 static bool acs_on_multifunction;
 #define NUM_ACS_IDS 16
 struct acs_on_id {
	unsigned short vendor;
	unsigned short device;
  };
 static struct acs_on_id acs_on_ids[NUM_ACS_IDS];
 static u8 max_acs_id;
 static __init int pcie_acs_override_setup(char *p)
 {
 	if (!p)
 		return -EINVAL;
 
 	while (*p) {
 		if (!strncmp(p, "downstream", 10))
 			acs_on_downstream = true;
 		if (!strncmp(p, "multifunction", 13))
 			acs_on_multifunction = true;
 		if (!strncmp(p, "id:", 3)) {
 			char opt[5];
 			int ret;
 			long val;
 
 			if (max_acs_id >= NUM_ACS_IDS - 1) {
 				pr_warn("Out of PCIe ACS override slots (%d)\n",
 					NUM_ACS_IDS);
 				goto next;
 			}
 
 			p += 3;
 			snprintf(opt, 5, "%s", p);
 			ret = kstrtol(opt, 16, &val);
 			if (ret) {
 				pr_warn("PCIe ACS ID parse error %d\n", ret);
 				goto next;
 			}
 			acs_on_ids[max_acs_id].vendor = val;
 
 			p += strcspn(p, ":");
 			if (*p != ':') {
 				pr_warn("PCIe ACS invalid ID\n");
 				goto next;
 			}
 
 			p++;
 			snprintf(opt, 5, "%s", p);
 			ret = kstrtol(opt, 16, &val);
 			if (ret) {
 				pr_warn("PCIe ACS ID parse error %d\n", ret);
 				goto next;
 			}
 			acs_on_ids[max_acs_id].device = val;
 			max_acs_id++;
 		}
 next:
 		p += strcspn(p, ",");
 		if (*p == ',')
 			p++;
 	}
 
 	if (acs_on_downstream || acs_on_multifunction || max_acs_id)
 		pr_warn("Warning: PCIe ACS overrides enabled; This may allow non-IOMMU protected peer-to-peer DMA\n");
 
 	return 0;
 }
 early_param("pcie_acs_override", pcie_acs_override_setup);
 
 static int pcie_acs_overrides(struct pci_dev *dev, u16 acs_flags)
 {
 	int i;
 
 	/* Never override ACS for legacy devices or devices with ACS caps */
 	if (!pci_is_pcie(dev) ||
 	    pci_find_ext_capability(dev, PCI_EXT_CAP_ID_ACS))
 		return -ENOTTY;
 
 	for (i = 0; i < max_acs_id; i++)
 		if (acs_on_ids[i].vendor == dev->vendor &&
 		    acs_on_ids[i].device == dev->device)
 			return 1;
 
 	switch (pci_pcie_type(dev)) {
 	case PCI_EXP_TYPE_DOWNSTREAM:
 	case PCI_EXP_TYPE_ROOT_PORT:
 		if (acs_on_downstream)
 			return 1;
 		break;
 	case PCI_EXP_TYPE_ENDPOINT:
 	case PCI_EXP_TYPE_UPSTREAM:
 	case PCI_EXP_TYPE_LEG_END:
 	case PCI_EXP_TYPE_RC_END:
 		if (acs_on_multifunction && dev->multifunction)
 			return 1;
 	}
 
 	return -ENOTTY;
 }

```

#### **然後繼續搜尋 0x720 找到類似以下程式碼**

![](https://i.imgur.com/Udtvasb.jpg)

```
在 { 0 } 上面新增一行

{ PCI_ANY_ID, PCI_ANY_ID, pcie_acs_overrides },

向上述圖片那樣
```

**之後就可以存檔離開了**

---

:::info
**打包核心檔案**
:::

#### **首先切換目錄**

```
[root@localhost pci]# cd /root/rpmbuild/SOURCES
```

#### **刪除原本的壓縮檔**

```
[root@localhost SOURCES]# rm –r linux-5.13.tar.xz
```

#### **打包剛剛編譯的資料夾**

```
[root@localhost SOURCES]# tar -Jcvf linux-5.13.tar.xz linux-5.13/
```

---

:::info 
**啟用VFIO_PCI功能設定檔案** 
:::

#### **編輯PCI的設定檔案**

```
[root@localhost SOURCES]# vim config-5.13.0-x86_64
```

#### **搜尋關鍵字 PCI_VGA 按 n 找到類似以下程式碼**

```
# CONFIG_VFIO_PCI_VGA is not set
CONFIG_VFIO_PCI_MMAP=y
CONFIG_VFIO_PCI_INTX=y

# 在註解底下新增一行 CONFIG_VFIO_PCI_VGA=y 會變成這樣

# CONFIG_VFIO_PCI_VGA is not set
CONFIG_VFIO_PCI_VGA=y
CONFIG_VFIO_PCI_MMAP=y
CONFIG_VFIO_PCI_INTX=y

存檔離開
```

---

## **更改核心檔案路徑**


```
[root@localhost SOURCES]# vim /root/rpmbulid/SPECS/kernel-ml-5.13.spec
```

#### **搜尋關鍵字 Source0 找到以下程式碼**

```
=====  原本程式碼 o(^-^)o  =====
# Sources.
Source0: https://www.kernel.org/pub/linux/kernel/v5.x/linux-%{LKAver}.tar.xz
Source1: config-%{version}-x86_64
...
..

=====  更改後程式碼 (o^∀^)  =====
# Sources.
Source0: linux-%{LKAver}.tar.xz
Source1: config-%{version}-x86_64
...
..

存檔離開
```

> 注意 Source0 那行的變化

---

:::info 
**安裝rpmbuild**
:::

```
[root@localhost SOURCES]# yum install rpm-build -y
```

#### **切換目錄**

```
[root@localhost SOURCES]#  cd /root/rpmbuild/SPECS
```

:::info 
**編譯打包成RPM**
:::

```
[root@localhost SPECS]# rpmbuild -bb kernel-ml-5.13.spec


```

#### **進行編譯如果發生相依性建置失敗時,依據前面軟體名稱一一透過YUM指令安裝**

```
yum install audit-libs-devel binutils-devel elfutils-devel java-devel libcap-devel ncurses-devel newt-devel numactl-devel openssl-devel pciutils-devel perl-ExtUtils-Embed perl-devel python3-devel python3-docutils xmlto xz-devel -y
```

#### **安裝完成後再執行一次打包**

```
[root@localhost SPECS]# rpmbuild -bb kernel-ml-5.13.spec

=====  進行打包時需要一些時間才會完成 o(^-^)o  =====
```
![](https://i.imgur.com/m65TdAY.jpg)

![](https://i.imgur.com/iYVLEVz.jpg)


**編譯打包後會多出很多資料夾，當然包括我們需要的RPM檔案**

---

:::info
**安裝剛剛編譯的新核心**
:::

#### 切換到新核心放置的目錄

```
[root@localhost SPECES]# cd /root/rpmbuild/RPMS/x86_64/
```

#### 查看核心的目錄有哪些東西
```
[root@localhost x86_64]# ll
```
![](https://i.imgur.com/6pXeNeU.jpg)


:::info 
**安裝核心**
:::

```
[root@localhost x86_64]# yum -y install kernel-ml-5.13.0-1.el8.x86_64.rpm
```
![](https://i.imgur.com/vwEGXMm.jpg)


#### **這個錯誤訊息是提醒你要先安裝那2個rpm檔**

```
[root@localhost x86_64]# yum -y install kernel-ml-core-5.13.0-1.el8.x86_64.rpm

[root@localhost x86_64]# yum -y install kernel-ml-modules-5.13.0-1.el8.x86_64.rpm
```

#### 接下來就可以安裝新核心了

```
[root@localhost x86_64]# yum -y install kernel-ml-5.13.0-1.el8.x86_64.rpm
```

## 重新開機就可用新編譯的核心開機

```
[root@localhost x86_64]# reboot
```

![](https://i.imgur.com/ezo8jiM.jpg)


#### 檢查一下是否為你要用的新版本核心

```
[root@localhost ~]# uname -r
```

![](https://i.imgur.com/eP7xDdB.jpg)


---

:::success
**全系統更新**
:::

```
[root@localhost ~]# yum -y update

[root@localhost ~]# reboot
```

---

## 安裝虛擬機需要的軟體

```
[root@localhost ~]# yum -y install qemu-kvm qemu-img virt-manager libvirt virt-install virt-viewer
```

---

:::success
**安裝虛擬機用的UEFI**
:::

#### 先新增repo

```
[root@localhost ~]# vim /etc/yum.repos.d/qemu-ovmf.repo

新增以下程式碼

[qemu-ovmf]
name=firmware for qemu, built by jenkins, fresh from git repos
baseurl=https://www.kraxel.org/repos/jenkins/
metadata_expire=5m
enabled=1
gpgcheck=0

存檔離開
```

---

:::success 
**安裝OVMF**
:::

```
[root@localhost ~]# yum -y install edk2.git-ovmf-x64
```

---

:::success
**將顯示卡ID編號寫入grub設定檔案**
:::

### 查詢顯示卡ID編號

```
[root@localhost ~]# lspci -nn
```


### 編輯grub檔案(如果兩張顯示卡的ID編號都一樣，寫一組即可)

```
[root@localhost ~]# vim /etc/default/grub 

在 CMDLINE 的 quiet 後面加入 
pcie_acs_override=downstream rd.driver.pre=vfio-pci intel_iommu=on pci-stub.ids=顯卡ID

存檔離開
```

### **載入核心參數**

```
[root@localhost ~]# grub2-mkconfig -o /boot/grub2/grub.cfg

[root@localhost ~]# reboot

※重新開機才會看到新載入的參數
```

### 查看CMDLINE核心參數已被載入

```
[root@localhost ~]# cat /proc/cmdline
```

---

:::success
**修改qemu.conf檔案**
:::

```
[root@localhost ~]# vim /etc/libvirt/qemu.conf
```

### 利用/(斜線找尋關鍵字)去找到user = "root" 跟group = "root"

```
# 將註解拿掉

=====  原本程式碼 o(^-^)o  =====

#user = "root"

#group = "root"

=====  更改後程式碼 (o^∀^)  =====

user = "root"

group = "root"

```

---

### 接著一樣利用 /(斜線找尋關鍵字) 去找到"cgroup_device_acl"

```
# 將 cgroup_device_acl=[] 之間的註解拿掉

=====  原本程式碼 o(^-^)o  =====

#cgroup_device_acl = [
#    "/dev/null", "/dev/full", "/dev/zero",
#    "/dev/random", "/dev/urandom",
#    "/dev/prmx", "/dev/kvm",
#    "/dev/rtc", "/dev/hpet"
#]

=====  更改後程式碼 (o^∀^)  =====

cgroup_device_acl = [
    "/dev/null", "/dev/full", "/dev/zero",
    "/dev/random", "/dev/urandom",
    "/dev/prmx", "/dev/kvm",
    "/dev/rtc", "/dev/hpet"
]
```

### 並在其之間內容最後一個變數"/dev/hpet"後面新增一個「 , 」再新增以下程式碼

```
"/dev/vfio/1", "/dev/vfio/2",
"/dev/vfio/3", "/dev/vfio/4",
"/dev/vfio/5", "/dev/vfio/6",
"/dev/vfio/7", "/dev/vfio/8",
"/dev/vfio/9", "/dev/vfio/10",
"/dev/vfio/11", "/dev/vfio/12",
"/dev/vfio/13", "/dev/vfio/14",
"/dev/vfio/15", "/dev/vfio/16",
"/dev/vfio/17", "/dev/vfio/18",
"/dev/vfio/19", "/dev/vfio/20"
```

### 完成結果會是以下那樣

```
cgroup_device_acl = [
    "/dev/null", "/dev/full", "/dev/zero",
    "/dev/random", "/dev/urandom",
    "/dev/prmx", "/dev/kvm",
    "/dev/rtc", "/dev/hpet",
    "/dev/vfio/1", "/dev/vfio/2",
    "/dev/vfio/3", "/dev/vfio/4",
    "/dev/vfio/5", "/dev/vfio/6",
    "/dev/vfio/7", "/dev/vfio/8",
    "/dev/vfio/9", "/dev/vfio/10",
    "/dev/vfio/11", "/dev/vfio/12",
    "/dev/vfio/13", "/dev/vfio/14",
    "/dev/vfio/15", "/dev/vfio/16",
    "/dev/vfio/17", "/dev/vfio/18",
    "/dev/vfio/19", "/dev/vfio/20"
]
```

---

### 接著一樣利用 /(斜線找尋關鍵字) 找到**nvram，在上面一行新增**

```
clear_emulator_capabilities = 0
```

---

### **接著找到 nvram**

```
將 nvram 到結尾的註解都拿掉，並且新增兩行程式碼

=====  原本程式碼 o(^-^)o  =====

#nvram = [
#    "...",
#    "...",
#    "...",
#    "...",
#]

=====  更改後程式碼 (o^∀^)  =====

nvram = [
    "...",
    "...",
    "...",
    "...",
    "/usr/share/edk2.git/ovmf-x64/OVMF_CODE-pure-efi.fd:/usr/share/edk2.git/ovmf-x64/OVMF_VARS-pure-efi.fd",
    "/usr/share/edk2.git/aarch64/QEMU_EFI-pflash.raw:/usr/share/edk2.git/aarch64/vars-template-pflash.raw"
]
```

---

:::success
**PCI跟PCIE與kernel分離**
:::

### **重新啟動 libvirtd.service服務**

```
[root@localhost ~]# systemctl start libvirtd.service
[root@localhost ~]# systemctl enable libvirtd.service
```

### **列出PCI or PCIe設備查看顯示卡是否與kernel分離了**

```
[root@localhost ~]# ll -i /sys/kernel/iommu_groups/*/*/*

群組如果在不同個，就是分離成功了，如下圖
```




> **若沒有分離，就得做重新開機的動作後再觀察看看**

> **重開機後若還是沒有分離，先查看開機設定檔有沒有設定錯誤，若沒問題可能就要重新編譯核心了**

---

:::success
**下載WINDOWS10最新的映像檔**
:::

### **到崑山的ftp網址：[崑山ftp連結](http://ftp.ksu.edu.tw)**

![](https://i.imgur.com/iiiiWu9.png)

### **1. 點選上面第4個KSU授權軟體**

![](https://i.imgur.com/iGMfC3H.png)

### **2. 點選左邊的OS software**



### **3. 選擇最新版本超連結右建 > 複製連結網址，到終端機執行**

### **4. 在HDD的 /vm_data 建立 iso 目錄**

```
[root@localhost ~]# mkdir /vm_data/iso
```

### **5. 切換目錄到 /vmdata/iso/**

```
[root@localhost ~]# cd /vm_data/iso
```

### **6. 下載 windows10 映像檔~～**

```
[root@localhost iso]# wget http://ftp.ksu.edu.tw/KSUT/SW_DVD9_Win_Pro_10_21H1_64BIT_ChnTrad_Pro_Ent_EDU_N_MLF_X22-55102.ISO
```

:::success
**下載WINDOWS10驅動光碟**
:::

###  **驅動光碟映像檔網址:[驅動連結](https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/archive-virtio/)**

### **1. 點選最新日期的virtio-win超連結~~**



### **2. 選擇最新版本超連結右建 > 複製連結網址，到終端機**



### **3. 下載驅動光碟映像檔～～**

```
[root@localhost iso]# wget https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/archive-virtio/virtio-win-0.1.196-1/virtio-win.iso
```

---

:::success
**新建虛擬硬碟**
:::

**系統碟(原始碟)：**

### 系統碟(原始碟) => 100G

```
[root@localhost iso]# mkdir /vm_data/original
[root@localhost iso]# cd /vm_data/original

[root@localhost original]# qemu-img create -f qcow2 /vm_data/original/vm_win_21H1-2021_0713.img 100G
```

---

:::success
**開啟虛擬機管理員**
:::

### 概覽 > 顯示應用程式 > 虛擬機管理員(有寫VM的圖示)

![](https://i.imgur.com/ilwc5Uo.png)


---

:::success
**選擇建立新的虛擬機**
:::

### 點選第一個有黃色的圖示

![](https://i.imgur.com/ZYAKHFe.png)

## 總共會有五個步驟

:::success 
**1. 選擇匯入的方法(選擇本地端安裝媒體ISO映像或CDROM)(L)**
:::

![](https://i.imgur.com/BKr5yYC.png)

:::success 
**2. 選擇作業系統的ISO，並輸入下方的作業系統名稱**
:::

**2-1. 選擇ISO或CDROM安裝媒體: 點選瀏覽**

**2-1. 找到剛剛載WIN10的資料夾(/vmdata/iso)**

**2-2. 選擇您正要安裝的作業系統(H):**

**2-2. 打Mi就會看到Windows10的選項點選即可**

![](https://i.imgur.com/kFclRQR.png)

> ....

![](https://i.imgur.com/cGmGbQX.png)

---

:::success
**3. 設定記憶體(8G)和CPU(分一半的效能出來)**
:::

![](https://i.imgur.com/wa3UWLi.png)

---

:::success
**4. 選擇使用自己建立的虛擬硬碟**
:::

**選擇 選取或建立自訂眝蔵**

![](https://i.imgur.com/Zzxe4II.png)

**點選管理會出現底下畫面**

![](https://i.imgur.com/Inev6uZ.png)



**左邊選擇資料夾放置剛剛新建的虛擬硬碟/vm_data/img/PC?-1.img (如果沒有的話請自行瀏覽本機尋找或按下儲區的紅色XX按鈕重整)**

![](https://i.imgur.com/mbbMi6g.png)

:::success
**5. 輸入虛擬機名稱並選擇自訂組態**
:::

![](https://i.imgur.com/ieGA9yW.png)

**點選完成**

**ps：如果他直接進入windows10安裝系統點選暫停旁邊的按鈕 > 選擇強制關閉虛擬機**

---

:::success
**設定虛擬機資源**
:::

## 讓我們一個功能一個功能分別去做設定嘍~

### 切記調整完後記得按下套用阿~~

---

:::success
**1. 設定Firmware韌體**
:::


**要選擇第三個的UEFI**

![](https://i.imgur.com/navY80a.png)

---

:::success
**2. CPU使用實體核心**
:::

**2-1. 複製主機CPU組態的勾勾把它取消**

**2-2. CPU的model要使用host-passthrough(手打)**

**2-3. 拓撲( P )設定 手動設定CPU拓撲勾選 > Socket: 1 > 核心 4 > 執行緒1**

**以上設定會跟圖示的畫面一樣**

![](https://i.imgur.com/DraxKse.png)

---

:::success
**3. 檢查記憶體**
:::

**基本上不會動到(跳過)**

![](https://i.imgur.com/3ZJGuPu.png)

---

:::success
**4. 調整開機順序**
:::

### 因等等要安裝作業系統，所以這邊可以先將CD調為第一順位

![](https://i.imgur.com/Ob405jS.png)

---

:::success
**5. VirtIO磁碟1設定**
:::

**硬碟使用virtio虛擬硬碟模式(預設就是virtio模式,確定就好如果不是在更改)**

![](https://i.imgur.com/W5tEFiF.png)

---

:::success
**6. STAT CDROM設定**
:::

**CD使用SATA磁碟匯流**

![](https://i.imgur.com/PvDQ21G.png)

---

:::success
**7. 網路設定**
:::

![](https://i.imgur.com/cJIfkGZ.png)

---

:::success
**8. 刪除不會用到的裝置並增加顯示卡以及USB等PCI裝置**
:::

**新增PCI裝置都要有兩個一個是顯卡本身一個是HDMI/DPI的插孔**

完成畫面會跟底下一樣

![](https://i.imgur.com/6jdQsRB.png)


:::success
**9. 增加 USB 的 PCI 裝置**
:::

![](https://i.imgur.com/wZI5sXk.png)

---

:::success
**10. 接下來就可以按下左上角的[開始安裝 WINDOWS10了] ~**
:::

![](https://i.imgur.com/CONj91s.png)


### **10-1. 按下立即安裝**

![](https://i.imgur.com/4U2R1T4.png)

---

### **10-2. 選擇專業版(按下一步)**

![](https://i.imgur.com/pyjJ4FK.png)

### **10-3. 抽換ISO檔**

> 現在的狀態下我們使用的硬碟是Windows10的ISO檔，但是這樣會沒有驅動，所以固沒辦法安裝，必須切換驅動光碟

```
[root@localhost ~]# virsh attach-disk PC19-1 /vm_data/iso/virtio-win.iso sdb \
--targetbus sata --type cdrom --mode readonly
```

![](https://i.imgur.com/WesZX84.png)

#### **按下-載入驅動然後瀏覽**

![](https://i.imgur.com/AG2Mus9.png)

#### **點選光碟機**

![](https://i.imgur.com/tNajkKf.png)

#### **打開amd64資料夾**

![](https://i.imgur.com/zO0P1ls.png)

#### **選擇win10資料夾**

![](https://i.imgur.com/8dtriLj.png)

#### **會看到底下的圖示,選擇下一步**

![](https://i.imgur.com/XTLq3qC.png)

#### **會跟你說無法安裝系統,是正常的因為你剛有抽換ISO檔** 

![](https://i.imgur.com/15nkNiz.jpg)


#### **抽換回windows10光碟**

```
[root@localhost ~]# virsh attach-disk PC26-1 \ 
/vm_data/iso/SW_DVD9_Win_Pro_10_2004_64BIT_ChnTrad_Pro_Ent_EDU_N_MLF_-2_X22-29746.ISO \ 
sdb --targetbus sata --type cdrom --mode readonly
```

![](https://i.imgur.com/IAz6dlS.png)

#### **在按下重新整理就可以繼續進行安裝,點選下一步**

#### **安裝系統中**

![](https://i.imgur.com/7wMopFw.png)


#### **安裝完畢,設定WINDOWS功能**

![](https://i.imgur.com/fLu473Q.png)


#### **選擇台灣**

![](https://i.imgur.com/aeINqPA.png)

#### **選擇微軟注音**

![](https://i.imgur.com/ekESao2.png)


#### **點選跳過**

![](https://i.imgur.com/49vjLBg.png)

#### **設定供個人使用**

![](https://i.imgur.com/hfylvt3.png)

#### **選擇離線帳戶**

![](https://i.imgur.com/aVl4kTi.png)

#### **名稱設定為dic**

![](https://i.imgur.com/t82eLTr.png)

#### **密碼設定2727175#356**

![](https://i.imgur.com/KwuolaW.png)

#### **在輸入一次密碼**

![](https://i.imgur.com/9ko728A.png)

#### **建立安全性問題,自行選擇(有三個安全性問題)**

![](https://i.imgur.com/Udp5SnM.png)

#### **隱私設定直接選擇接受**


![](https://i.imgur.com/FWJOHVk.png)

#### **活動歷程紀錄跨越不同....選擇是**

![](https://i.imgur.com/xUpyrF8.png)


#### **等待一段時間**

![](https://i.imgur.com/AAsIm3l.png)

#### **安裝驅動**

**抽換 ISO 檔**

> 現在的狀態下我們使用的硬碟是Windows10的ISO檔，但是這樣會沒有驅動，所以固沒辦法安裝，必須切換驅動光碟

```
[root@localhost ~]# virsh attach-disk PC19-1 /vm_data/iso/virtio-win.iso sdb \
--targetbus sata --type cdrom --mode readonly
```

![](https://i.imgur.com/WesZX84.png)

**回到 WINDOWS 開啟裝置管理員**

![](https://i.imgur.com/NuzDsUr.jpg)

**會有網路和PCI找不到驅動的問題**
**對 ETHERNET 按右鍵點選更新驅動程式**

![](https://i.imgur.com/XJfFMHh.png)

**選擇瀏覽電腦上的驅動程式**

![](https://i.imgur.com/gvFXjll.png)

**按瀏覽，選擇D槽**

![](https://i.imgur.com/LgG7OHC.png)

![](https://i.imgur.com/vQGCq5J.png)


確定後按下一步，就好了

**安裝PCI驅動**

![](https://i.imgur.com/Gichaub.png)

![](https://i.imgur.com/gvFXjll.png)

![](https://i.imgur.com/LgG7OHC.png)

![](https://i.imgur.com/vQGCq5J.png)

確定後按下一步，就安裝好了

---

:::success
**11. 接下來要把設定完成的虛擬機轉用 XML 開機**
:::

**先在 /vm_data 目錄裡創建一個 /vm_data/xml 的目錄**

```
[root@localhost ~]# mkdir /vm_data/xml
```

### **11-1. 寫一個VM要用的 xml 檔**

:::spoiler

```
<domain type='kvm'>
<name>PC26-1</name>

<memory unit='KiB'>8388608</memory>
<currentMemory unit="KiB">8388608</currentMemory>

<vcpu placement='static'>4</vcpu>
<cputune>
<vcpupin vcpu='0' cpuset='0'/>
<vcpupin vcpu='1' cpuset='1'/>
<vcpupin vcpu='2' cpuset='4'/>
<vcpupin vcpu='3' cpuset='5'/>
</cputune>

  <os>
    <type arch='x86_64' machine='pc-q35-rhel7.6.0'>hvm</type>
    <loader readonly='yes' type='pflash'>/usr/share/edk2.git/ovmf-x64/OVMF_CODE-pure-efi.fd</loader>
    <nvram>/var/lib/libvirt/qemu/nvram/PC26-1_VARS.fd</nvram>
    <bootmenu enable="yes"/>
  </os>

  <features>
    <acpi/>
    <apic/>
    <kvm>
      <hidden state='on'/>
    </kvm>
    <vmport state="off"/>
  </features>

  <cpu mode='host-passthrough' check='none'>
    <topology sockets='1'  cores='4'  threads='1'/>
  </cpu>
  
  <clock offset='localtime'>
    <timer name='rtc' tickpolicy='catchup'/>
    <timer name='pit' tickpolicy='delay'/>
    <timer name='hpet' present='no'/>
  </clock>

  <on_poweroff>destroy</on_poweroff>
  <on_reboot>restart</on_reboot>
  <on_crash>restart</on_crash>
  
  <devices>
    
    <emulator>/usr/libexec/qemu-kvm</emulator>
    
    <disk type='file' device='disk'>
      <driver name='qemu' type='qcow2'/>
      <source file='/vm_data/img/PC26-1.img'/>
      <target dev='vda' bus='virtio'/>
      <boot order='1'/>
      <address type='pci' domain='0x0000' bus='0x00' slot='0x05' function='0x0'/>
    </disk>
    
    <disk type="file" device="cdrom">
      <driver name="qemu" type="raw"/>
      <source file=""/>
      <target dev="sdb" bus="sata"/>
      <readonly/>
      <boot order="2"/>
      <address type="drive" controller="0" bus="0" target="0" unit="1"/>
    </disk>
    
    <interface type="bridge">
      <mac address="52:54:00:7f:eb:9c"/>
      <source bridge="br0"/>
      <model type="virtio"/>
      <address type="pci" domain="0x0000" bus="0x01" slot="0x00" function="0x0"/>
    </interface>
    
    #顯卡分配
    <hostdev mode="subsystem" type="pci" managed="yes">
      <source>
        <address domain="0x0000" bus="0x01" slot="0x00" function="0x0"/>
      </source>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x00" function="0x0"/>
    </hostdev>
    
    <hostdev mode="subsystem" type="pci" managed="yes">
      <source>
        <address domain="0x0000" bus="0x01" slot="0x00" function="0x1"/>
      </source>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x01" function="0x0"/>
    </hostdev>
    
    # USB 分配
    <hostdev mode="subsystem" type="pci" managed="yes">
      <source>
        <address domain="0x0000" bus="0x06" slot="0x00" function="0x0"/>
      </source>
      <address type="pci" domain="0x0000" bus="0x00" slot="0x02" function="0x0"/>
    </hostdev>

    <memballoon model="virtio">
      <address type="pci" domain="0x0000" bus="0x06" slot="0x00" function="0x0"/>
    </memballoon>
  
  </devices>
</domain>
```

:::

> **複製後要依照自己的電腦分配「記憶體、CPU、網路、硬碟、顯示卡、USB」**

---

:::success
**12. 製作個人硬碟**
:::

### **個人碟製作方法**

**https://hackmd.io/dput0gXQQBiRO0wcdmhhsQ**

目的: 為了可以減少資料寫入backingfile，降低容量長大的速度，使backingfile吃到SSD效能持久一點。

:::success
**13. vm_img 製作**
:::

### **使用 backing_file 機制建立 vm_img**

**https://hackmd.io/m9K6bL65S7ulktYViAMy7g**

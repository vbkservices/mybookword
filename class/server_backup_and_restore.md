# 實體電腦備份以及還原 
## 工作\資料目錄
- /vbirdsys/script/wims/backres # 備份還原
  - master.sh #主要啟動檔
- /vbirdsys/script/wims/backres/files/ # 備份還原資料以及設定黨
  - config.json #主設定檔
  - switch_info.json # client電腦mac紀錄檔(跟DHCP的資料要同步修改)
  - ip_list.txt #要進行還原的電腦
- /vbirdsys/html/wims/computer/files/fixip.conf # dhcp的分配IP的設定檔
- 

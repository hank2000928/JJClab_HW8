# JJClab_HW8
裝 sysstat 觀察一周使用量
----------------------------------
清點214內的node:  
<img width="508" height="743" alt="image" src="https://github.com/user-attachments/assets/23be8fb9-32a5-4649-a2b0-55b751401fec" />

先登入其中一個node(e11)，確認sysstat is not installed:   
<img width="549" height="108" alt="image" src="https://github.com/user-attachments/assets/85e019d8-85a9-4be3-8f14-d8ecb40f292f" />
<img width="619" height="388" alt="image" src="https://github.com/user-attachments/assets/ecb5e90e-1527-4972-8077-3ff441e3d047" />

確認版本為SLE-15-SP2，dry-run測試:  
<img width="1915" height="642" alt="image" src="https://github.com/user-attachments/assets/9ffbcc7d-a4e1-4729-8bbc-8d398e0e2733" />

正式安裝，並確認歷史資料收集功能:  
<img width="1915" height="704" alt="image" src="https://github.com/user-attachments/assets/c7eaaabd-b4fa-492f-a453-6458ae760348" />
<img width="1229" height="647" alt="image" src="https://github.com/user-attachments/assets/576bd1b8-7443-4be5-b730-529ef2197541" />

查看實際採樣頻率，確認歷史檔可讀後，確認資料保存期限:  
<img width="1092" height="757" alt="image" src="https://github.com/user-attachments/assets/1ae3c639-79f4-4ea5-b560-522b2207dde4" />
目前可確認:  
| | |
| --| -- |
|sa1  |每 10 分鐘收原始資料|
|sa2  |每 6 小時整理/產生報告|
|HISTORY=60 |歷史 activity data 保留 60 天|
|COMPRESSAFTER=10 |超過 10 天的歷史檔案會進入壓縮處理|
|SADC_OPTIONS="-S ALL"| 還可以分析 memory、disk、network 等項目。|
|SA_DIR=/var/log/sa| 資料保存位置|

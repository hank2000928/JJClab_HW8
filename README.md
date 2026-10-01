# JJClab_HW8
裝 sysstat 觀察一周使用量
----------------------------------
## 安裝測試
清點214內的node:  
<img width="508" height="743" alt="image" src="https://github.com/user-attachments/assets/23be8fb9-32a5-4649-a2b0-55b751401fec" />

先登入其中一個閒置node(e11)，確認sysstat is not installed:   
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
||保存設定|
| --| -- |
|sa1  |每 10 分鐘收原始資料|
|sa2  |每 6 小時整理/產生報告|
|HISTORY=60 |歷史 activity data 保留 60 天|
|COMPRESSAFTER=10 |超過 10 天的歷史檔案會進入壓縮處理|
|SADC_OPTIONS="-S ALL"| 還可以分析 memory、disk、network 等項目。|
|SA_DIR=/var/log/sa| 資料保存位置|

## 列裝所有閒置node
以e03為例:  
<img width="919" height="508" alt="image" src="https://github.com/user-attachments/assets/f24bb626-d0d7-4c27-b193-c424270160a4" />
<img width="1919" height="555" alt="image" src="https://github.com/user-attachments/assets/c458f7c2-749b-43cf-8d76-5b924e710222" />
<img width="1182" height="582" alt="image" src="https://github.com/user-attachments/assets/ad42acd1-1c27-4020-a68e-9162fff44161" />

## 測試批次安裝
確認e09、e15版本相同，則用以下script安裝: 

    for n in e09 e15; do
        echo "===== Installing sysstat on $n ====="
    
        ssh "$n" '
            if rpm -q sysstat >/dev/null 2>&1; then
                echo "[OK] sysstat already installed"
            else
                zypper -n in --from Module-Basesystem sysstat
            fi
    
            systemctl enable --now sysstat
    
            echo "--- version ---"
            rpm -q sysstat
    
            echo "--- service ---"
            systemctl is-enabled sysstat
    
            echo "--- cron ---"
            grep -v "^#" /etc/cron.d/sysstat | grep -v "^$"
    
            echo "--- history ---"
            grep -E "^(HISTORY|COMPRESSAFTER|SADC_OPTIONS|SA_DIR)" \
                /etc/sysstat/sysstat
        '
    done  
<img width="942" height="252" alt="image" src="https://github.com/user-attachments/assets/72a61268-f8d7-418d-94e3-ef3135900ab6" />
<img width="1664" height="809" alt="image" src="https://github.com/user-attachments/assets/23d4fb07-4e6b-4170-9846-3c64b8a74294" />
<img width="1604" height="809" alt="image" src="https://github.com/user-attachments/assets/5ad3c744-7df4-4055-9102-874697e12c01" />

## g06用量測試
因為上次上課(9/19)，學長已安裝g06的sysstat，可以進行觀察:  
<img width="858" height="48" alt="image" src="https://github.com/user-attachments/assets/d96c53da-39cc-4dc7-a5b8-90f7280c9004" />
上圖是昨天每10分鐘統計的平均用量，%idle為100%-52.28%=47.42%  

下圖則是9/19~9/30的用量，或以script修飾:  
<img width="1324" height="556" alt="image" src="https://github.com/user-attachments/assets/c2b20ae9-e4c9-4f0f-b0f1-0c4d7431cfee" />
<img width="1245" height="375" alt="image" src="https://github.com/user-attachments/assets/8440114a-9413-466c-b16b-df77f58d1d96" />

## g04、g05安裝
為避免影響正在運算的jobs，確認g04的dry-run只需要安裝procmail sysstat，再開始安裝。
<img width="1312" height="578" alt="image" src="https://github.com/user-attachments/assets/535b900c-68a0-4032-aec1-1d91b0062575" />
<img width="1910" height="466" alt="image" src="https://github.com/user-attachments/assets/f9e9bb8f-cb2e-486a-8bac-66fc272eadb5" />
安裝後驗證:  
<img width="1257" height="884" alt="image" src="https://github.com/user-attachments/assets/28a09607-4e8a-4495-9500-6fa819f3fff1" />

重複步驟執行於g05:  
<img width="1669" height="686" alt="image" src="https://github.com/user-attachments/assets/4817c293-e22b-41e6-8a4a-52e3ad000148" />
<img width="1913" height="580" alt="image" src="https://github.com/user-attachments/assets/3f5d47ca-ac2c-444f-987a-7752e7465795" />
<img width="1240" height="665" alt="image" src="https://github.com/user-attachments/assets/60441135-2af4-4cb3-83fb-8333abdc2c55" />

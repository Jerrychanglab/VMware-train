- # 虛擬機/網路/存儲配置
  - ## 虛擬交換機建置(示意VSS)
      - ### 配置vSwitch (Channel)
        ### 1. 建置vSwitch
        ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/3a6df330-ab77-4250-88b9-319d59a40dc4)
        
        ```esxcli network vswitch standard add -v 'vSwitch1'```
        ### 2. 將vmnic加入vSwitch
        ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/a2383f58-da5e-43a9-9848-93ee7e277971)

        ```esxcli network vswitch standard uplink add -v 'vSwitch1' -u 'vmnic2'```

        ```esxcli network vswitch standard uplink add -v 'vSwitch1' -u 'vmnic3'```
        ### 3. 將兩張vmnic指派到使用中，流量演算法調整成iphash
        ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/d45214d5-d05b-4531-8461-c74ab4a8cb16)

        ```esxcli network vswitch standard policy failover set -v 'vSwitch1' -l 'iphash' -a 'vmnic2,vmnic3'```

        ### 4. 建置vLan Group 並配置一個vLan ID
        ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/f005f49e-4770-461a-8a16-292007695431)

        ```esxcli network vswitch standard portgroup add -p vLan3103_10.31.3 -v vSwitch1'```
    
        ```esxcli network vswitch standard portgroup set -p vLan3103_10.31.3 -v 3103```

        ### 5. 建置vmkernel
        ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/0503f3a6-30a6-42cd-a4f5-e9a494b9f166)
        
        ```esxcli network ip interface add --interface-name=vmk1 --portgroup-name=vLan3103_10.31.3```

        ### 6. 配置vmkernel IP
        ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/3ea38079-4ee4-4793-8203-4276da96b93f)

        ```esxcli network ip interface ipv4 set --ipv4=10.31.3.100 --netmask=255.255.255.0 --type=static --interface-name=vmk1```
      
        > vmk IP可指派功能 (vMotion / Fault Tolerance / Management ...等等)
  
  - ## 存儲配置
    > 達成目標如下圖，一台ESXi掛載Local Datastore & NFS Mount
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/aa625e8c-d47a-4fa0-8e34-481c6443de91)

      - ### vmfs(Local硬碟)建置 (WEB)
        ### 1. 類型
        ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/0be6770f-95fa-447c-bac2-40b8f9b40c55)
        > 選擇VMFS
        ### 2. 名稱和裝置選取
        ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/f8f5f6c8-4635-42ba-b7a4-cf42314d3c74)
        > 定義DataStore的名稱與要使用哪個Local硬碟
        ### 3. 磁碟分割組態
        ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/409527b1-a543-4114-9ee9-9aa83347226e)
        > 選擇VMFS 6與指定空間
        ### 4. 即將完成
        ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/264c5709-771d-4421-b5f5-075bb8f614dd)
        > 檢視參數，沒問題後按完成
        ### 5. 驗證
        ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/7f138a69-7d86-421e-be02-e2bdffa62194)
        > 如掛載成功可查看到Local空間
      - ### vmfs(Local硬碟)建置 (Command)
        ### 1.查詢代碼
        ```esxcli storage core path list```
        > 尋找顯示為Device Display Name: Local <品牌> Ｄisk (xxx.)的寬架，並紀錄Device:XXX.代碼
        ### 2. 格式化硬碟
        ```/bin/partedUtil mklabel /vmfs/devices/disks/mpx.vmhba1:C0:T1:L0 gpt```
        > /vmfs/devices/disks/mpx.vmhba1:C0:T1:L0 -> 這就是Device代碼，需要絕對路徑。
        ### 3. 計算總空間的大小
        ``` END_SECTOR=`/bin/partedUtil getptbl /vmfs/devices/disks/mpx.vmhba1:C0:T1:L0 | tail -1 | awk '{print int($1 * $2 * $3 - 1)}'` ```
        ### 4. 輸出的大小建置磁區
        ```partedUtil setptbl /vmfs/devices/disks/mpx.vmhba1:C0:T1:L0 gpt "1 2048 ${END_SECTOR} AA31E02A400F11DB9590000C2911D1B8 0"```
        ### 5. 掛載空間
        ```vmkfstools -C vmfs6 -S <datastore name> /vmfs/devices/disks/mpx.vmhba1:C0:T1:L0:1```
      - ### NFS 建置 (WEB)
        ### 1. 確認所屬vSwitch內的vLan Group內的vmk IP是否能溝通的到NFS Server IP
        ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/c41081ed-b1e5-4a47-83a4-c105faccaffd)
        ### 2. 類型
        ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/34f6984a-d21d-49ce-addf-10b6eee07e78)
        > 選擇NFS
        ### 3. NFS版本
        ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/b9668442-96eb-4d7c-b6cd-efea31a068de)
        > 選擇NFS 3，如果要選擇NFS4.1需看您的後端設備是否有支援。
        ### 4. 名稱和組態
        ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/34d00d06-3369-4ab6-b8a1-d821931ce2f6)
        > 欄位填寫
          #### 3-1. 名稱: <填寫自定義識別名稱>
          #### 3-2. 資料夾: 填寫NFS Server放給您的路徑
          #### 3-3. 伺服器: NFS Server的IP
        ### 5. 即將完成
        ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/2ac363a8-a432-46c0-a308-b9011d786564)
        > 確認參數沒問題，即可點選完成
        ### 6. 驗證
        ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/fa256646-4c81-4ae4-9821-094527042096)
        > 如掛載成功可查看到volume的空間
      - ### **NFS 建置 (Command)** 
        ### 1. 確認所屬vSwitch內的vLan Group內的vmk IP是否能溝通的到NFS Server IP
        ```esxcli network ip interface ipv4 get```
        ### 2. 掛載NFS
        ```esxcli storage nfs add -H <NFS Server IP> -s <NFS Server放出的Volume Name> -v <自定義volume名稱>```
        ### 3. 驗證
        ```esxcli storage nfs list```
  - ## 虛擬機配置

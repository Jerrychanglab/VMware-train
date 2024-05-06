- # 虛擬交換機建置(VSS)(Command)
  - ## 配置vSwitch (Channel)
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

    ### 4. 建置Network Tag並配置一個vLan ID
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/00e16780-fdfb-4786-a535-29eeaf0d0f08)

    ```esxcli network vswitch standard portgroup add -p vLan3103_10.31.3 -v vSwitch1'```
    
    ```esxcli network vswitch standard portgroup set -p vLan3103_10.31.3 -v 3103```

    ### 5. 建置vmkernel
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/cbc1b779-3a2a-4c22-a6db-7d7f22080e8d)

    ```esxcli network ip interface add --interface-name=vmk1 --portgroup-name=vLan3103_10.31.3```

    ### 6. 配置vmkernel IP
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/8e74c8c2-a0b1-4e8d-b111-2e08f7268cbc)

    ```esxcli network ip interface ipv4 set --ipv4=10.31.3.100 --netmask=255.255.255.0 --type=static --interface-name=vmk1```
      
    > vmk IP可指派功能 (vMotion / Fault Tolerance / Management ...等等)
- # 虛擬交換機建置(示意VDS)(WEB)
  - ## 建置Distributed Switch
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/43ae9b6c-58b1-4d07-af2e-72a7f6e0bebc)
    ### 1. 
- # 存儲配置
  > 達成目標如下圖，一台ESXi掛載Local Datastore & NFS Mount
  ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/893709ff-3ff5-41ad-a7ed-b91fb7e124ba)

  - ## vmfs(Local硬碟)建置 (WEB)
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
  - ## vmfs(Local硬碟)建置 (Command)
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
  - ## NFS 建置 (WEB)
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
  - ## **NFS 建置 (Command)** 
    ### 1. 確認所屬vSwitch內的vLan Group內的vmk IP是否能溝通的到NFS Server IP
    ```esxcli network ip interface ipv4 get```
    ### 2. 掛載NFS
    ```esxcli storage nfs add -H <NFS Server IP> -s <NFS Server放出的Volume Name> -v <自定義volume名稱>```
    ### 3. 驗證
    ```esxcli storage nfs list```
- # 虛擬機建置
    ## 1. 新增虛擬機器
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/02f580ec-cfef-4955-8ba1-db825118e8b4)
    ### 2. 選取建制類型(此章節說明建立新的虛擬機器)
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/43ce4502-75a8-40be-a6e4-c5a605d104e8)
    ### 3. 選取名稱和資料夾
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/27ac1d74-f24d-41be-b82e-1e574c0577ce)
    ### 4. 選取計算資源
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/79721421-2960-477d-b3a3-0e1d6b010ae0)
    ### 5. 選取儲存區
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/ba4799e5-4074-4b68-beaa-e148ce35a0a8)
    ### 6. 選取相容性
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/0c4a5949-0cb1-43c7-88c1-683a31e199ac)
    > 差異在於組態上限的部分
    ### 7. 選取客體作業系統
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/56b69c66-f82c-4048-be3d-dce15635e4b4)
    > 此步驟需選擇您要安裝的OS (如: Linux -> Redhat 9)，關係到default提供的預設值。
    ### 8. 自訂硬體(CPU、RAM、Network、Disk)
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/f11bb437-beaf-477b-931d-62ecf0182d5f)
    - ### CPU 說明
        ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/cdcbaae8-ce7c-4971-8eb9-b104563331f1)
        ### 1. CPU熱插拔: 機器在開機狀態下能動態新增CPU數量。(縮小需要關機)
        ### 2. 保留: 讓機器鎖定CPU資源保障，用於當ESXi機器資源競爭時，能有效的保留此虛擬機的資源。 
        ### 3. 限制: 讓CPU的資源使用的上限定義，防止此機器資源使用過高時，影響其他虛擬機。
        ### 4. 共用率: 當物理CPU被多個虛擬機共用時，可透過此設定獲得相對應的資源比例，資源充足時不會有明顯感覺。
    - ### RAM 說明
        ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/64d644ce-0d55-4097-b010-13adba86f421)
        ### 1. 記憶體熱插拔: 機器在開機狀態，能動態的新增記憶體Size。(縮小需要關機)
        ### 2. 保留: 同上。
        ### 3. 限制: 同上。
        ### 4. 共用率: 同上。
    - ### Network 說明
        ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/4059b3f1-f543-4dc8-8829-9723da00c454)
        ### 1. Tag Network選擇: 選擇透過哪個Network Tag進行網路傳輸。
        ### 2. 狀態: 開機時是否要自動啟動網卡。(如沒有勾選，虛擬機開機時網卡不會自動連線)
        ### 3. 介面卡類型:
        - #### 3-1. E1000: 不需安裝額外的驅動程序即可使用，但傳輸能力只支援1G。
        - #### 3-2. VMXNET3: 通常線上環境推薦的模式，可支援到10G的網路傳輸，但前提需安裝vmware tools。
          ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/3ec793e1-8b31-4448-9751-d69d2a750c60)
        - #### 3-3. PCI裝置傳遞: 直接將物理的PCI(網卡)直接分配給虛擬機使用，繞過VMware的虛擬化層。
          > 網路傳輸高敏感服務，通常不常使用，因此模式會導致多個輔助功能失效，如:vMotion。
    - ### Disk 說明         
        ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/ccb6abf1-d436-4454-a3e6-ab2f8e65dbb4)
        ### 1.硬碟空間: 填寫所需的硬碟Size。(後續擴充Size，如Linux機型，需透過第三方gparted-live.iso或其他軟件)
        ### 2.磁碟佈建:
        ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/854aaa99-673c-4224-be1d-059419ceaa28)
        - #### 2-1. 完整佈建消極式歸零(Thick Provision Lazy Zeroed): 虛擬硬碟的存儲空間將立即在物理存儲上分配其所需的全部容量，這些空間不會立即清零，僅在虛擬機首次寫入到未使用的磁碟塊時才會。
        - #### 2-2. 完整佈建積極式歸零(Thick Provision Eager Zeroed): 完整佈建積極式歸零會立即分配虛擬硬碟需要的全部空間，並且在創建虛擬硬碟時，就對整個磁碟進行歸零操作。(Fault Tolerance的必要條件)
        - #### 2-3. 精簡佈建(Thin Provisioning): 虛擬硬碟最初只會使用實際需要儲存數據的最少空間，並隨著數據的增加而動態擴展到其配置的容量。
    ### 9. 即將完成
    > 確認沒問題即可點選完成

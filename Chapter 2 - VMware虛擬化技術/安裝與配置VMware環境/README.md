- # 安裝與配置VMware環境
  - ## ESXi安裝與配置
    - ### 步驟一：官網下載ESXi ISO
    - ### 步驟二: 透過IPMI或其他方式將ISO掛載到Server上。
    - ### 步驟三: ESXi安裝流程 (請看下圖片編號順序進行點選)
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/d050533c-e2f9-4aa9-a4a4-bad9e21fd7ea)
    - ### 步驟四: ESXi配置-Command執行
      - ### Console頁面開啟ESXi Shell模式 (必須)
        F2 -> root/Passwd -> Troubleshooting Options -> Enable ESXi Shell
      - ### Alt + F1 進入Shell模式 （下列指令皆在Shell模式完成) 
      - ### 指令: ESXi開啟SSH
        ```vim-cmd hostsvc/enable_ssh```

        ```vim-cmd hostsvc/start_ssh```
      - ### 指令: ESXi開啟Shell
        ```vim-cmd hostsvc/enable_esx_shell```

        ```vim-cmd hostsvc/start_esx_shell```
      - ### 指令: Lecense Key
        ```vim-cmd vimsvc/license --set <XXXXX-XXXXX-XXXXX-XXXXX-XXXXX>```
      - ### 指令: ESXi 名稱
        ```esxcli system hostname set --host=<NEW-HOSTNAME>```
      - ### 指令: ESXi Mgmt IP / mask / Gateway
        ```esxcli network ip interface ipv4 set -i vmk0 -I <IP_ADDRESS> -N <SUBNET_MASK> -t static```

        ```esxcli network ip route ipv4 add -n default -g <GATEWAY>```
      - ### 指令: SNMP Community與啟用
        ```esxcli system snmp set -c <COMMUNITY>```

        ```esxcli system snmp set -e true```
      - ### 指令: ESXi Coredump啟用
        ```esxcli system coredump network set --interface-name vmk0 --server-ipv4 <IP> --server-port <PORT>```

        ```esxcli system coredump network set --enable true```
      - ### 指令: ESXi SYSLOG啟用
        ```esxcli system syslog config set --loghost='udp://<IP>:514'```

        ```esxcli system syslog reload```
      - ### 指令: NTP Server啟用
        ```esxcli system ntp set --server=<主IP> --server=<備援IP>```

        ```esxcli system ntp set --enabled=yes```
      - ### 指令: Power High Performance
        ```esxcli system settings advanced set --option=/Power/CpuPolicy --string-value='High Performance'```
      - ### 指令: 配置vSwitchX (Channel)
        ```esxcli network vswitch standard uplink add -v 'vSwitch0' -u 'vmnicX'```

        ```esxcli network vswitch standard uplink add -v 'vSwitch0' -u 'vmnicX'```

        > 指定兩張UPlink的vmnic卡加入vSwitch的vLan group

        > 因ESXI安裝時vSwitch0會Default被設定出來，指定為ESXI MGMT使用

        ```esxcli network vswitch standard policy failover set -v 'vSwitch0' -l 'iphash' -a 'vmnicX,vmnicX'```

        > 將vSwitch0的兩張vmnic調整成active，並且調整Hash演算法

        ```esxcli network vswitch standard portgroup policy failover set -a vmnicX,vmnicX -p 'Management Network'```

        > Management Network的vLan group是Default建置出來，屬於vmk0的ESXi MGMT

        > 將vLan group的兩張vmnic指定到active狀態

        ```esxcli network vswitch standard portgroup policy failover set -p 'Management Network' -l 'iphash'```

        > 將vLan group的演算法更改為HASH = IPHASH

        ```esxcli network vswitch standard portgroup set -p 'Management Network' -v <VLAN ID>```
        
  - ## vCenter Server安裝與配置
    - ### 部署 - 環節一 （vCSA建置)
      ### 1. Introduction
      ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/7158e549-9ef8-4a6f-9a2b-761571b7215d)
      ### 2. End user license agreement
      ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/6d5eece4-f3cc-4121-a342-01f73bf84e4e)
      ### 3. vCenter Server deploument targe
      ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/dcddd853-a189-47bb-baf7-29d027ca2832)
      > 輸入要部署到哪台ESXi上，並且輸入ESXi的帳號/密碼
      ### 4. Set up vCenter Server VM
      ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/d5dddec2-c884-4777-ab49-7ac1cd12def2)
      > 輸入要建立的vCetner Name與密碼
      ### 5. select deployment size
      ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/0653bd0a-ddc8-4f4f-a4bb-c918b29602f6)
      > 選擇要部署vCetner的資源規模
      ### 6. Select datastore
      ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/a76998f5-dd30-49f5-852a-9030931986d1)
      > 選擇要部署到哪個Storage空間內
      ### 7. Configure network settings
      ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/a29ddbb7-d840-49b4-a3d7-054cdb9258a9)
      > 如沒有DNS，FQDN可設定vCetner的IP (非推薦做法)
      ### 8. Ready to complete stage 1
      ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/49a63963-1438-4bad-ba9c-80a6794d3b87)
      > 檢查參數沒問題，即可FINISH部署
    - ### 部署 - 環節二 （SSO建置)
      ### 1. SSO組態
      ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/43e5fc98-e6ec-4e84-91da-1010cacef392)
      > 網路名稱屬於透過vCetner登入時的域名，如: xxxx@vsphere.local (default)
      ### 2. 設定 CEIP
      ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/fb90cb99-e5dc-46d6-ab6b-91b48361b0b9)
      > CEIP計畫，如不參加不勾選即可
      ### 3. 即將完成
      ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/14b0b087-dff8-4e99-b1a2-6c7856e2880e)
      > 檢查域名沒問題，即可開始部署
    - ### 部署 - 環節三 （Web登入)
      ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/603b8340-94ca-4c0f-8a46-8a0d4d33a9a9)
     

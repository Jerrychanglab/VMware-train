# 第2章節：VMware虛擬化技術
- # VMware虛擬化平台概覽
  - ## VMware vSphere：架構與功能（包括ESXi和vCenter Server)
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/06e5fa5c-9a4f-4802-8204-d0a9c8df1c98)
    - ### VMware ESXi
      - ### 描述: ESXi是一個虛擬化的控制器，直接安裝到物理伺服器上，ESXi為虛擬機提供高性能的基礎，支持多種作業系統，它比其他虛擬化解決方案更為安全和高效。 
      - ### 特點: 高效、安全、提高率用率。
    - ### VMware vCenter Server (vCSA)
      - ### 描述: vCenter Server是vSphere的集中管理工具，提供單一的介面來管理ESXi主機和虛擬機，並可單獨透過vCetner API進行自動化管理。
      - ### 特點: 集中控管、監控、資源最佳化。
    - ### VMware Migrating
      - ### 描述: 透過此功能，將虛擬機搬移到其他的ESXi上，達到動態搬移的效果。
      - ### 功能:
        - ### vMotion
          ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/70cf466f-0734-40f9-820e-5f0e938b78a1)
          - ### 描述: vMotion允許將運行中的虛擬機器無縫遷移至另一台ESXi主機，不會導致虛擬機器停機或服務中斷。此過程中，虛擬機器的內存、執行狀態和網絡連接等信息被即時轉移到目標主機，實現了真正的即時遷移。
          - ### 限制: 需要相同的NFS掛載點，並且ESXi的大版本與CPU世代需要相同。
          - ### 特點: 不會有斷線產生。
        - ### Storage vMotion
          ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/c5df013b-85d2-4bba-8bfa-383825b8bd93)
          - ### 描述: Storage vMotion允許將虛擬機器的存儲無縫遷移到不同的數據存儲，短暫的虛擬機器中斷。這對於存儲設備的維護、升級或優化存儲性能和容量非常有用。
          - ### 限制: 不需要相同的NFS掛載點，但需要ESXi的大版本與CPU世代需要相同。
          - ### 特點: 會有短暫斷線，但可跨儲存區的搬移。
     - ### vSphere Cluster
       - ### 描述: vSphere Cluster是一組ESXi的主機的集合體，這些主機共享相同的網路與計算支援(並能啟用HA/DRS功能)。
       - ### 特點: 資源共享、高可用性、資源最佳化。
     - ### vSphere Cluster - HA
       ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/fa934ec2-4a8c-4a4b-80dc-74d7c8493e34)
       - ### 描述: 通過Cluster的Heartbeat機制，確認ESXi的存活，如判斷異常會將該ESXi上的VM在其他ESXi上開啟。
       - ### 功能:
         - ### 叢集百分比: 用於集群中的每台ESXi需保留多少百分比的資源(CPU/RAM)，以便觸發HA時有足夠資源能啟動虛擬機。
         - ### 專機使用: 獨立的ESXi，提供給HA觸發時有專屬的資源啟動虛擬機。
     - ### vSphere Cluster - DRS
       ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/0936fec8-db4d-49df-8fe4-9aeca34562c0)
       - ### 描述: 透過Cluster的偵測機制，可進行資源的自動化搬移，將每台ESXi上的資源使用率平衡。
       - ### 功能:
         - ### 手動: 虛擬機只會在機器開機時，會提供建議要在哪台ESXi上開啟。
         - ### 半自動: 虛擬機開機的時候會自動選擇最佳的ESXi主機，並且運行時ESXi要資源平衡，會提供建議事項讓您決定是否要進行搬移調整。
         - ### 全自動: 全自動交由DRS搬移，如觸發搬移時，被選中的虛擬機會是Lodding最輕的。
  - ## VMware Network：網路虛擬化與安全性
    ### VSS(vSphere Standard Switch)
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/2eb8865d-4b8a-4d41-99ed-1e5c2e26849f)
    - ### 優點:
      - ### 簡易設置: VSS相對較為簡單，適合小型或是簡單的部署環境，不需要進階的網絡配置。
      - ### 獨立配置: 每一個ESXi主機上的VSS是獨立配置的，這使得在沒有複雜需求的環境中較為容易管理。
    - ### 缺點:
      - ### 管理複雜: 在多個ESXi主機環境中，每個VSS都需要單獨管理和配置，這可能導致一致性和維護上的問題。(vSphere Cluster結構)
      - ### 功能受限: 相比於VDS，VSS提供的功能較少，例如(沒有網路流量In,Out切分控制/多元的負載平衡模式..等等)
    ### VDS(vSphere Distributed Switch)
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/40514460-550f-4608-bf3b-94450e41cbbf)
    - ### 優點:
      - ### 集中管理: VDS提供一個集中的界面來管理所有連接到它的ESXi主機，大幅減少了管理開銷和配置錯誤。
      - ### 進階功能: VDS支持更多進階功能，例如（網路流量In,Out切分控制、標準LLDP..等等)，適合需求較高的環境。
    - ### 缺點:
      - ### 配置複雜：雖然集中管理降低了部分管理複雜度，但是設置和維護一個VDS環境的初期複雜度較高。
      - ### 相依性: VDS是透過vCenter做控管，如vCetner異常會無法進行介面卡的操作。(不影響既有服務)
    ### NSX-T VLANs
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/4b2c8652-a95d-4d57-b025-42932134f06d)
    - ### 優點:
      - ### 高級安全: NSX-T提供了微細化的安全策略和隔離機制，包括分布式防火牆（DFW）、安全組和服務插入等功能，允許對流量進行細粒度的控制，從而提高了應用和數據的安全性。
      - ### 連線軌跡: 通過對網絡流量軌跡的深入分析，管理員可以快速識別和解決網絡問題，如流量擁堵、配置錯誤或不適當的路由決策。這不僅提高了網絡的可靠性，也優化了整體的網絡性能。
    - ### 缺點:
      - ### (暫時沒想到)
 - ## VMware Storage：常用
 ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/5a25ff48-f06b-4ad0-b895-7d5dc1cdd17b)
   ### Local Storage
   - ### 優點:
     - ### 效能: 由於存儲設備直接連接到伺服器，通常能提供更低的延遲和更高的數據傳輸速率。
     - ### 簡單性: 設置相對簡單，管理起來也不像網絡存儲那樣複雜。
   - ### 缺點:
     - ### 可擴展性: 本地存儲的擴展性受限於物理硬件，尤其是在伺服器已滿的情況下。
     - ### 靈活性: 與網絡存儲相比，遷移虛擬機器數據需要花費更多的時間。
   ### NFS
   - ### 優點:
     - ### 共享存儲: 多台服務器和虛擬機器可以同時訪問同一個NFS共享，便於實現數據共享和集中管理。
     - ### 資料安全: 可透過NAS(NetApp)進行資料保護，如空間快照...等等。
   - ### 缺點:
     - ### 性能: 由於NFS是基於網絡的，網絡的擁堵和延遲可能影響存儲的性能。
- # 安裝與配置VMware環境
  - ## ESXi安裝與配置
    - ### 步驟一：官網下載ESXi ISO
    - ### 步驟二: 透過IPMI或其他方式將ISO掛載到Server上。
    - ### 步驟三: ESXi安裝流程 (請看下圖片編號順序進行點選)
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/d050533c-e2f9-4aa9-a4a4-bad9e21fd7ea)
    - ### 步驟四: ESXi配置
    - ### 步驟五: ESXi配置透過Command方式
      - ### ESXi開啟SSH
        ```vim-cmd hostsvc/enable_ssh```

        ```vim-cmd hostsvc/start_ssh```
      - ### ESXi開啟Shell
        ```vim-cmd hostsvc/enable_esx_shell```

        ```vim-cmd hostsvc/start_esx_shell```
      - ### Lecense Key
        ```vim-cmd vimsvc/license --set <XXXXX-XXXXX-XXXXX-XXXXX-XXXXX>```
      - ### ESXi 名稱
        ```esxcli system hostname set --host=<NEW-HOSTNAME>```
      - ### ESXi Mgmt IP / mask / Gateway
        ```esxcli network ip interface ipv4 set -i vmk0 -I <IP_ADDRESS> -N <SUBNET_MASK> -t static```

        ```esxcli network ip route ipv4 add -n default -g <GATEWAY>```
      - ### SNMP Community與啟用
        ```esxcli system snmp set -c <COMMUNITY>```

        ```esxcli system snmp set -e true```
      - ### ESXi Coredump啟用
        ```esxcli system coredump network set --interface-name vmk0 --server-ipv4 <IP> --server-port <PORT>```

        ```esxcli system coredump network set --enable true```
      - ### ESXi SYSLOG啟用
        ```esxcli system syslog config set --loghost='udp://<IP>:514'```

        ```esxcli system syslog reload```
      - ### NTP Server啟用
        ```esxcli system ntp set --server=<主IP> --server=<備援IP>```

        ```esxcli system ntp set --enabled=yes```
      - ### Power High Performance
        ```esxcli system settings advanced set --option=/Power/CpuPolicy --string-value='High Performance'```
      - ### 配置vSwitchX (Channel)
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
        > 將此Default的vLan Group配置一個vLAN ID，如環境沒有vLAN結構，可忽略。

  - ## vCenter Server安裝與配置
  - ## 虛擬機建立與管理

- # 虛擬網路與存儲配置
  - ## 虛擬交換機配置
  - ## 虛擬機網路連接
  - ## 存儲配置（NFS）

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
        ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/0cef8639-3024-4f42-9751-34dfa29f4273)
          - ### 描述: vMotion允許將運行中的虛擬機器無縫遷移至另一台ESXi主機，不會導致虛擬機器停機或服務中斷。此過程中，虛擬機器的內存、執行狀態和網絡連接等信息被即時轉移到目標主機，實現了真正的即時遷移。
          - ### 限制: 需要相同的NFS掛載點，並且ESXi的大版本與CPU世代需要相同。
           - ### 特點: 不會有斷線產生。
        - ### Storage vMotion
          - ### 描述: Storage vMotion允許將虛擬機器的存儲無縫遷移到不同的數據存儲，短暫的虛擬機器中斷。這對於存儲設備的維護、升級或優化存儲性能和容量非常有用。
          - ### 限制: 不需要相同的NFS掛載點，但需要ESXi的大版本與CPU世代需要相同。
          - ### 特點: 會有短暫斷線，但可跨儲存區的搬移。
     - ### vSphere Cluster
       - ### 描述: vSphere Cluster是一組ESXi的主機的集合體，這些主機共享相同的網路與計算支援(並能啟用HA/DRS功能)。
       - ### 特點: 資源共享、高可用性、資源最佳化。
     - ### vSphere Cluster - HA
       - ### 描述: 通過Cluster的Heartbeat機制，確認ESXi的存活，如判斷異常會將該ESXi上的VM在其他ESXi上開啟。
       - ### 特點: 縮減異常中斷時間
     - ### vSphere Cluster - DRS
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
    ### NSX-T ()
  - ### VMware Storage：軟體定義存儲

- # 安裝與配置VMware環境
  - ## ESXi安裝與配置
  - ## vCenter Server安裝與配置
  - ## 虛擬機建立與管理

- # 虛擬網路與存儲配置
  - ## 虛擬交換機配置
  - ## 虛擬機網路連接
  - ## 存儲配置（NFS）

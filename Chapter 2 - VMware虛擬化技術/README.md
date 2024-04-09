# 第2章節：VMware虛擬化技術
- # VMware虛擬化平台概覽
  - ## VMware vSphere：架構與組件（包括ESXi和vCenter Server)
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/06e5fa5c-9a4f-4802-8204-d0a9c8df1c98)

  - ### VMware Network：網路虛擬化與安全性
    ### VSS(vSphere Standard Switch)
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/a43e168c-4fed-4bf6-873d-5da7daefc4a8)
    - ### 優點:
      - ### 簡易設置: VSS相對較為簡單，適合小型或是簡單的部署環境，不需要進階的網絡配置。
      - ### 獨立配置: 每一個ESXi主機上的VSS是獨立配置的，這使得在沒有複雜需求的環境中較為容易管理。
    - ### 缺點:
      - ### 管理複雜: 在多個ESXi主機環境中，每個VSS都需要單獨管理和配置，這可能導致一致性和維護上的問題。(vSphere Cluster結構)
      - ### 功能受限: 相比於VDS，VSS提供的功能較少，例如沒有網路流量控制與多元的負載平衡模式
    ### VDS(vSphere Distributed Switch)
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

- ## 資源管理與優化
    - ## VMware Aria Operations 介紹
      ## 持續的性能優化
      - ### 虛擬機資源調整
      ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/b4befeb3-c1e2-45f5-bb44-c56e660dd316)
         ### 過大的虛擬機器
        ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/5ff850bb-cfa4-4a20-9cd5-d009c8cf881b)
         > 可觀察到，可直接統計此Cluster能回收多少資源(CPU/RAM)，近額提高ESXi的使用率。
         ### 過小的虛擬機器
        ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/e7372935-2972-4892-80cf-ee85403ab11f)
         > 可觀察到，透過數據學習與分析演算，判斷此機器需要增加多少資源(CPU/RAM)，防止非預期的效能貧頸。

    - ### 資源池區(Resource Pool)
      ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/a3f69507-fbcc-46f1-ac57-d84f3aa5fb9e)
      ### 優點:
      ### 1. 靈活的資源管理: 透過此機制可將Cluster擴大，透過資源區的角度分配給每個單位，更細微的顆粒度管理。
      ### 2. 隔離和保護: 資源池可定義此資源區的(CPU/RAM)保留/限制設定，防止影響其他池區與被其他池區影響。
    - ### Cluster DRS
      s
    - ### 高可用性與災難恢復
      

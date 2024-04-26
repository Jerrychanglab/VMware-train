# VMware Aria Operations 介紹
- ## 資源管理與最佳化
  - ### 規模最佳化
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/b4befeb3-c1e2-45f5-bb44-c56e660dd316)
    - ### 過大的虛擬機器
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/5ff850bb-cfa4-4a20-9cd5-d009c8cf881b)
    > 可觀察到，可直接統計此Cluster能回收多少資源(CPU/RAM)，近額提高ESXi的使用率。
    - ### 過小的虛擬機器
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/e7372935-2972-4892-80cf-ee85403ab11f)
    > 可觀察到，透過數據學習與分析演算，判斷此機器需要增加多少資源(CPU/RAM)，防止非預期的效能貧頸。
  - ### 回收
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/a34a8bc8-326f-4a01-90ad-8efd234d099c)
    - ### 已關閉電源的虛擬機器
    > 透過演算法，確認機器以長期未開機，可進行回收之機器。
    - ### 閒置的虛擬機器
    > 透過演算法，確認需虛擬機器的資源(CPU/RAM)長時間已無使用，但機器是開機狀態。
    - ### 快照
    > 長期快照未移除的機器。
    - ### 孤立的磁碟
    > 抓取孤立的磁碟，未給任何人使用。
  - ### 工作負載放置
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/b4ddb7bb-85c1-4fd6-aa49-f5815dccb028)
- ## 監控與預測
  - ### 儀表板
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/e54b1b4b-6e5a-4568-b558-3bf81d757ec0)
    - ### 效能

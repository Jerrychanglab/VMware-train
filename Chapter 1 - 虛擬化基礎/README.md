- # 虛擬化技術簡介
  - ## 虛擬化的定義
    ### 了解VMware之前，這邊來探討何謂[虛擬化]，早期實體機的時代，一台機器安裝一個OS系統提供服務，資源的應用上會出現浪費的現象。虛擬化技術由這而延伸出在實體層上，模擬多個可完整獨立的硬體資源，使資源利用率最大化的一個技術。
    - ### 實體機結構
       ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/0d8d2ee4-1572-43c8-9a93-38780cb8df5b)
    - ### 虛擬化結構
       ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/47aee8af-ee9a-41d8-8723-4cd38e040e80)
  - ## 市面虛擬化產品
    ###  [VMware ESXi]
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/da29e106-d04b-41a3-a7a1-e79a32c49b0a)

    ###  [Nutanix AHV]
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/2a19d855-c6f5-47ce-ba03-ce790d408161)

    ###  [Microsoft Hyper-V]
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/6e14123b-a9ba-4ee7-91e5-0fa40ed42d87)

- # 虛擬化的優勢與應用場景
  - ## 資源共享與重分配
    ### 虛擬化技術允許不同的虛擬機（VMs）在同一硬體資源上高效運行，這些資源包括計算能力，如：CPU、記憶體、存儲和網絡連接。通過虛擬化，這些資源可以被動態分配給需要它們的虛擬機，從而最大化資源的利用率。
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/89a104fb-a0a9-40a3-8f3f-da5bc8b68713)

  - ## 減少硬體成本
    ### 在傳統實體機時代，Boss告知公司需要一個完整的服務系統，又不能將Web與DB放在同個籃子裡，只能準備兩台實體機器建置環境。
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/f494659d-7043-4dbc-865d-313c1ef8067f)
    - ### 缺點:
      - ### 準備多台的Server，成本開銷增加。
      - ### 因Server增加，電力消耗變大。
      - ### 服務性質在低壓力的環境，運算資源造成浪費，無法妥善率用。
    ### 虛擬化的時代，Boss告知公司需要一個完整的服務系統，能透過同樣或更低的成本，達成超乎預期的需求，同時也能滿足突發擴充資源需求。
    ![image](https://github.com/Jerrychanglab/VMware-train/assets/39659664/bd4d9432-59b4-4061-94be-26ae7756123d)
    - ### 優點:
      - ### 透過虛擬機的角度，可配發剛好的計算資源。
      - ### 未使用的資源可給新服務使用。
      - ### 每台虛擬機有獨立的OS，當有異常時不互相影響。
      - ### 閒置的資源可建置(壓測/正式/前哨)環境。
    
  - ## 提升業務靈活性和可靠性

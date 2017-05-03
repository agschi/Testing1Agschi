<!--author=SharS last changed: 9/17/15-->

#### <a name="to-connect-through-the-serial-console"></a>透過序列主控台連接
1. 將序列纜線連接到裝置 (直接連接或透過 USB 序列介面卡)。
2. 開啟 [控制台]，然後開啟 [裝置管理員]。
3. 識別 COM 連接埠，如下圖所示。
   
     ![透過序列主控台連接](./media/storsimple-use-putty/HCS_ConnectingDeviceS-include.png)
4. 啟動 PuTTY。 
5. 在右窗格中，將 [連線類型] 變更為 [序列]。
6. 在右窗格中，輸入適當的 COM 連接埠。 確定已將序列設定參數設定為如下的內容：
   
   * 速度：115200
   * 資料位元：8
   * 停止位元：1
   * 同位檢查：無
   * 流量控制：無
     
     下圖顯示這些設定。
     
     ![PuTTY 設定](./media/storsimple-use-putty/HCS_PuttyConfig-include.png) 
     
     > [!NOTE]
     > 如果預設的流程控制設定無法運作，請嘗試將流程控制設為 XON/XOFF。
     > 
     > 
7. 按一下 [開啟]  以啟動序列工作階段。


## <a name="view-device-telemetry-in-the-dashboard"></a>在儀表板中檢視裝置遙測
遠端監視解決方案中的儀表板可讓您檢視裝置傳送到 IoT 中樞的遙測資料。

1. 在瀏覽器中，返回遠端監視解決方案儀表板，按一下左面板中的 [裝置] 瀏覽至 [裝置清單]。
2. 在 [裝置清單] 中，您應該會看到裝置的狀態是 [正在執行]。 如果不是，請按一下 [裝置詳細資料] 面板中的 [啟用裝置]。
   
    ![檢視裝置狀態][18]
3. 按一下 [儀表板] 以返回儀表板，從 [要檢視的裝置] 下拉式清單中選取您的裝置，以檢視其遙測。 範例應用程式的遙測是 50 個單位的內部溫度、 55 個單位的外部溫度，以及 50 個單位的濕度。
   
    ![檢視裝置遙測資料][img-telemetry]

## <a name="invoke-a-method-on-your-device"></a>在裝置上叫用方法
遠端監視解決方案中的儀表板可讓您透過 IoT 中樞在裝置上叫用方法。 例如，在遠端監視解決方案中，您可以叫用方法來模擬裝置重新啟動。

1. 在遠端監視解決方案的儀表板中，按一下左面板中的 [裝置] 瀏覽至 [裝置清單]。
2. 在 [裝置清單] 中，按一下裝置的 [裝置識別碼]。
3. 在 [裝置詳細資料] 面板中，按一下 [方法]。
   
    ![裝置方法][13]
4. 在 [方法] 下拉式清單中，選取 [InitiateFirmwareUpdate]，然後在 [FWPACKAGEURI] 中輸入虛擬的 URL。 按一下 [叫用方法] 以在裝置上呼叫該方法。
   
    ![叫用裝置方法][14]
   

5. 當裝置處理該方法時，您可以在執行裝置程式碼的主控台中看見訊息。 該方法的結果會新增到解決方案入口網站的歷程記錄中：

    ![檢視方法歷程記錄][img-method-history]

## <a name="next-steps"></a>後續步驟
[自訂預先設定的方案][lnk-customize]一文描述可供您擴充此範例的一些方式。 可能的延伸模組包括使用真實的感應器和實作其他命令。

您可以深入了解 [azureiotsuite.com 網站的權限][lnk-permissions]。

[13]: ./media/iot-suite-visualize-connecting/suite4.png
[14]: ./media/iot-suite-visualize-connecting/suite7-1.png
[18]: ./media/iot-suite-visualize-connecting/suite10.png
[img-telemetry]: ./media/iot-suite-visualize-connecting/telemetry.png
[img-method-history]: ./media/iot-suite-visualize-connecting/history.png
[lnk-customize]: ../articles/iot-suite/iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-permissions]: ../articles/iot-suite/iot-suite-permissions.md

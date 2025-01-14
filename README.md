# MOPS_CVS

**專案目標：**

從 CSV 檔案讀取資料後，將其中的 "公司代號"、"公司名稱"、"營利事業統一編號" 和 "上市日期" 四個欄位複製到一個新的 DataTable，並移除重複的列。

**使用工具：**

*   UiPath Studio
*   CSV 檔案

**實作步驟：**

1.  **準備工作：**
    *   確認已安裝 UiPath Studio。
    *   確保 CSV 檔案格式正確，包含以下欄位："公司代號"、"公司名稱"、"營利事業統一編號" 和 "上市日期"。

2.  **建立 UiPath Workflow：**

    **步驟 1：讀取 CSV 檔案**

    *   拖曳一個「讀取 CSV (Read CSV)」活動到工作流程中。
    *   **屬性設定：**
        *   `FileName`：輸入你的 CSV 檔案路徑，例如 `C:\Data\CompanyData.csv`。
        *   `Output DataTable`：建立變數 `dataTableCSV`。
        *   其他選項：根據您的 CSV 檔案格式設定，例如 `Separator`、`HasHeaders` 等。

    **步驟 2：複製欄位並移除重複列**

    *   拖曳一個「指派 (Assign)」活動到工作流程中。
    *   **屬性設定：**
        *   `To`：建立一個新的 DataTable 變數，例如 `dtNew`。
        *   `Value`：輸入以下 VB.NET 程式碼：

        ```vb.net
        dataTableCSV.DefaultView.ToTable(True, "公司代號", "公司名稱", "營利事業統一編號", "上市日期")
        ```

        *   **程式碼說明：**
            *   `dataTableCSV.DefaultView`：取得 `dataTableCSV` 的預設檢視。
            *   `ToTable(True, "欄位1", "欄位2", ...)`：將預設檢視轉換為新的 DataTable。
                *   `True`：表示移除重複的列。
                *   `"欄位1", "欄位2", ...`：指定要複製的欄位名稱。

**後續優化建議：**

*   **驗證 CSV 資料格式：** 在讀取 CSV 檔案前，檢查檔案是否存在，以及是否包含必要的欄位（例如使用 `File.Exists` 和檢查 DataTable 的 Columns）。
*   **動態欄位名稱處理：** 如果欄位名稱可能會變動，可以改用變數動態傳遞欄位名稱。
*   **例外處理：** 增加 `Try Catch` 活動，捕捉 CSV 檔案格式錯誤或其他執行錯誤，並記錄日誌。
*   **擴展功能：** 增加更多資料處理功能，例如進行欄位計算或資料篩選。將結果輸出至 Excel 檔案，或寫入資料庫。
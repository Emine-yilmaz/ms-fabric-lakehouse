# ms-fabric-lakehouse-lab1
# Microsoft Fabric Lakehouse Lab

This lab demonstrates step-by-step how to create a Lakehouse on Microsoft Fabric and explore its basic features. It focuses on the concepts of data lakes, which emerged as an alternative to data warehouses for large-scale data analytics, and data lakehouses, which combine the best features of both approaches. The lab shows how a Lakehouse in Microsoft Fabric provides highly scalable file storage on a OneLake store and a metastore for relational objects based on the open-source Delta Lake format.

**Estimated Time:** 30 minutes

**Prerequisites:**

* Access to a Microsoft Fabric trial.

## Steps

1.  **Create a workspace:**
    * Navigate to the [Microsoft Fabric home page](https://app.fabric.microsoft.com/home?experience=fabric) and sign in with your Fabric credentials.
    * In the menu bar on the left, select the **Workspaces** icon.
    * Create a new workspace and give it a name of your choice.
    * In the **Advanced** section, select a licensing mode that includes Fabric capacity (Trial, Premium, or Fabric).
    * Your new workspace should be empty when it opens.

2.  **Create a lakehouse:**
    * On the menu bar on the left, select **Create**.
    * In the **New** page, under the **Data Engineering** section, select **Lakehouse**.
    * Give your lakehouse a unique name of your choice.
    * After a minute or so, a new lakehouse will be created.
    * Note that the **Lakehouse explorer** pane on the left allows you to browse tables and files in the lakehouse.
        * The **Tables** folder contains tables that you can query using SQL semantics. These tables are based on the Delta Lake format.
        * The **Files** folder contains data files in the OneLake storage that are not associated with managed Delta tables. You can also create **shortcuts** in this folder to reference data stored externally.

3.  **Upload a file:**
    * Download the `sales.csv` file from [https://raw.githubusercontent.com/MicrosoftLearning/dp-data/main/sales.csv](https://raw.githubusercontent.com/MicrosoftLearning/dp-data/main/sales.csv) to your computer.
        * *Note: To download the file, open a new tab in the browser, paste the URL, right-click anywhere on the page containing the data, and select "Save as" to save it as a CSV file.*
    * Return to the browser tab containing your lakehouse.
    * In the **...** menu for the **Files** folder in the **Lakehouse explorer** pane, select **New subfolder** and create a subfolder named `data`.
    * In the **...** menu for the new `data` folder, select **Upload** and then **Upload files**, and upload the `sales.csv` file.
    * After the file has been uploaded, select the **Files/data** folder and verify that the `sales.csv` file is there.
    * Select the `sales.csv` file to see a preview of its contents.

4.  **Explore shortcuts:**
    * In the **...** menu for the **Files** folder, select **New shortcut**.
    * View the available data source types for shortcuts and then close the **New shortcut** dialog box.

5.  **Load file data into a table:**
    * On the **Home** page, select the **Files/Data** folder to see the `sales.csv` file.
    * In the **...** menu for the `sales.csv` file, select **Load to Tables** > **New table**.
    * In the **Load to table** dialog box, set the table name to `sales` and confirm the load operation. Wait for the table to be created and loaded.
        * *Tip: If the `sales` table does not appear automatically, in the **...** menu for the **Tables** folder, select **Refresh**.*
    * In the **Lakehouse explorer** pane, select the created `sales` table to view the data.
    * In the **...** menu for the `sales` table, select **View files** to see the underlying files for this table.
    * Observe that files for a Delta table are stored in **Parquet** format and include a subfolder named `_delta_log` which logs details of transactions applied to the table.

6.  **Use SQL to query tables:**
    * At the top-right of the Lakehouse page, switch from **Lakehouse** to **SQL analytics endpoint**.
    * Wait for the SQL analytics endpoint to open.
    * Use the **New SQL query** button to open a new query editor and enter the following SQL query:

        ```sql
        SELECT Item, SUM(Quantity * UnitPrice) AS Revenue
        FROM sales
        GROUP BY Item
        ORDER BY Revenue DESC;
        ```

        * *Note: If you are on a virtual machine and have issues entering the SQL query, you can download the `01-Snippets.txt` file from [https://github.com/MicrosoftLearning/mslearn-fabric/raw/main/Allfiles/Labs/01/Assets/01-Snippets.txt](https://github.com/MicrosoftLearning/mslearn-fabric/raw/main/Allfiles/Labs/01/Assets/01-Snippets.txt) and copy the query from there.*
    * Use the **▷ Run** button to execute the query and view the results, which should show the total revenue for each product.

7.  **Create a visual query:**
    * On the toolbar, expand the **New SQL query** option and select **New visual query**.
    * Drag the `sales` table to the new visual query editor pane.
    * In the **Manage columns** menu, select **Choose columns**. Then select only the **SalesOrderNumber** and **SalesOrderLineNumber** columns.
    * In the **Transform** menu, select **Group by**. Then group the data using the following **Basic** settings:
        * **Group by:** SalesOrderNumber
        * **New column name:** LineItems
        * **Operation:** Count distinct values
        * **Column:** SalesOrderLineNumber
    * Once done, the results pane under the visual query will show the number of line items for each sales order.

8.  **Create a report:**
    * In the toolbar, select **Model layouts**. The data model schema for the semantic model is displayed.
        * *Note 1: In this exercise, the semantic model consists of a single table. In a real-world scenario, you would likely create multiple tables in your lakehouse, and each would be included in the model. You could then define relationships between these tables.*
        * *Note 2: The views `frequently_run_queries`, `long_running_queries`, `exec_sessions_history`, and `exec_requests_history` are part of the `queryinsights` schema automatically created by Fabric. As this is outside the scope of this exercise, these views should be ignored.*
    * In the menu ribbon, select the **Reporting** tab and then **New report**. The page will change to a report designer view.
    * In the **Data** pane on the right, expand the `sales` table. Then select the following fields:
        * Item
        * Quantity
    * A table visualization will be added to the report.
    * Hide the **Data** and **Filters** panes to create more space. Ensure the table visualization is selected, and in the **Visualizations** pane, change the visualization to a **Clustered bar chart** and resize it.
    * On the **File** menu, select **Save**. Save the report as `Item Sales Report` in the workspace you created earlier.
    * Now, in the hub menu bar on the left, select your workspace to verify that it contains the following items:
        * Your lakehouse.
        * The SQL analytics endpoint for your lakehouse.
        * A default semantic model for the tables in your lakehouse.
        * The `Item Sales Report` report.

9.  **Clean up resources:**
    * If you have finished exploring your lakehouse, you can delete the workspace you created for this exercise.
    * In the bar on the left, select the icon for your workspace to view all its contents.
    * In the toolbar, select **Workspace settings**.
    * In the **General** section, select **Remove this workspace**.

By completing this lab, you have learned how to create a Lakehouse on Microsoft Fabric, upload files, load data into a table, query data using SQL and visual queries, and create a basic report. You have also gained an understanding of the structure of a Lakehouse with OneLake storage and Delta Lake tables.

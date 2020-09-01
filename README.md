# The Page API in Big Data Processor

## **Introduction**
Workflow or package developers can freely design web interfaces to provide documentations, guide users to execute workflows, or visualize results interactively, etc.
In Big Data Processor (BDP), we provide a web sandbox environments to display these freely customized `Pages`.
To reduce efforts communicating with servers, we provide a set of javascript APIs to communicate between our BDP web client and the Page which runs in the sandbox (see the following figure).

<div align='center'>
    <img src="https://raw.githubusercontent.com/big-data-processor/assets/master/images/BDP-Page.png" width="480"><br>
    <b>The Page API helps to communicate between the BDP web client and customized Pages. </b>
</div>

In this way, Page developers can easily get the information by just calling javascript functions. Also, developers need not handle many details such as file uploading or downloading.

## **Usage**

To use the API, you need to include the javascript file in your Page.

```
<script src="/assets/pages/bdp-page-api.js"></script>
```

Next, you need to new an API instance and then call the `.initialize()` function to make a handshake with the BDP web client.
After the handshake, the API is ready to use.
Most functions are asynchronous, you can wrap the function calls with the async/await pattern.

```
const bdpAPI = new BdpPageAPI();                      // initiate an API instance
(async () => {
    await bdpAPI.initialize();                        // make the first handshake.
    const dataFileRecords = await bdpAPI.listFiles(); // get the DataFile records in the current project.

    /**
     * The bdpAPI object is ready to use.
     * You can call these API functions to interact with BDP.
     */
})().catch((err) => console.log);

```


## **API Categories**

### **1. Query information on BDP**
It is very easy to get metadata of data records on BDP. The following functions allow you to get many kinds of information:
* <a href='./BdpPageAPI.html#getCurrentFileInfo'>getCurrentFileInfo</a>: Retrieve the current DataFile information. Only works in File Pages since users have selected a DataFile record.
* <a href='./BdpPageAPI.html#getCurrentPackageInfo'>getCurrentPackageInfo</a>: Retrieve the current Package that contains this customized Page.
* <a href='./BdpPageAPI.html#getCurrentProjectInfo'>getCurrentProjectInfo</a>: Retrieve the current Project that a user has selected.
* <a href='./BdpPageAPI.html#getCurrentResultInfo'>getCurrentResultInfo</a>: Retrieve the current Result information. Only works in Result Pages since users have selected a Result record.
* <a href='./BdpPageAPI.html#getCurrentUserInfo'>getCurrentUserInfo</a>: Retrieve the current user information.
* <a href='./BdpPageAPI.html#listFiles'>listFiles</a>: List DataFile records in the current Project.
* <a href='./BdpPageAPI.html#listResults'>listResults</a>: List Result records in the current Project.
* <a href='./BdpPageAPI.html#listPackages'>listPackages</a>: List the Packages selected in the current Project.
* <a href='./BdpPageAPI.html#listTasks'>listTasks</a>: Get the Task information by task key (or task id) and package id.
* <a href='./BdpPageAPI.html#listAllTasks'>listAllTasks</a>: List all related Tasks from Packages that are selected in the current project.

### **2. Navigation**
The following functions may leave the current Page and navigate to another Page.
* <a href='./BdpPageAPI.html#navigateToBdpDataFile'>navigateToBdpDataFile</a>: Navigate to view the DataFile record. Similar to the BdpPageAPI.openFileLink function except that it does not open a new window. 
* <a href='./BdpPageAPI.html#navigateToBdpResult'>navigateToBdpResult</a>: Navigate to view the DataFile record. Similar to the BdpPageAPI.openFileLink function except that it does not open a new window.
* <a href='./BdpPageAPI.html#navigateToProjectList'>navigateToProjectList</a>: Navigate to the project list page of BDP-client.
* <a href='./BdpPageAPI.html#navigateToFilePage'>navigateToFilePage</a>: Navigate to a File Page.
* <a href='./BdpPageAPI.html#navigateToResultPage'>navigateToResultPage</a>: Navigate to a Result Page.
* <a href='./BdpPageAPI.html#navigateToProjectPage'>navigateToProjectPage</a>: Navigate to a Project Page.
* <a href='./BdpPageAPI.html#openFileLink'>openFileLink</a>: Open a new window to display the DataFile record.
* <a href='./BdpPageAPI.html#openResultLink'>openResultLink</a>: Open a new window to display the Result record.

### **3. DataFile handling**
* <a href='./BdpPageAPI.html#createFolder'>createFolder</a>: Create an empty folder and return the corresponding DataFile record.
* <a href='./BdpPageAPI.html#deleteFile'>deleteFile</a>: Delete a DataFile record.
* <a href='./BdpPageAPI.html#getFileBlob'>getFileBlob</a>: Use this function to automatically download the file content in the Blob type. You may call <a href='./global.html#BDPPageUtils'>BDPPageUtils.readFileBlob</a> to process the Blob object.
* <a href='./BdpPageAPI.html#globFilesInFolder'>globFilesInFolder</a>: Get a list of valid file paths located inside a folder that corresponds to the DataFile. Under the hood, we use the npm package, <a href='https://www.npmjs.com/package/globby' target=_blank>globby</a>, to use the globby expressions.
* <a href='./BdpPageAPI.html#importFileFromPath'>importFileFromPath</a>: Import files that already on the server (the same machine that BDP is hosted on). This function can only be executed by at least the level of `Power User`.
* <a href='./BdpPageAPI.html#updateFileInfo'>updateFileInfo</a>: Update the metadat of the DataFile record. Currently, you cannot change the format of a DataFile.
* <a href='./BdpPageAPI.html#uploadFiles'>uploadFiles</a>: Upload files from FileList or from an array of Blob objects.
* <a href='./BdpPageAPI.html#uploadFileBlob'>uploadFileBlob</a>: Upload a Blob object to create a DataFile record.

### **4. Task related API**
* <a href='./BdpPageAPI.html#executeTask'>executeTask</a>: Execute a Task and generate a Result record.
* <a href='./BdpPageAPI.html#getTaskArguments'>getTaskArguments</a>: Get the argument settings by task key (or task id) and package id.
* <a href='./BdpPageAPI.html#getTaskInfo'>getTaskInfo</a>: Get the Task information by task key (or task id) and package id.
* <a href='./BdpPageAPI.html#getTaskInputGuide'>getTaskInputGuide</a>: Providing a reference to generate Task arguments for users to specify before executing Tasks.

### **5. Result related API**
* <a href='./BdpPageAPI.html#updateResultInfo'>updateResultInfo</a>: Update the metadata of a Result record.
* <a href='./BdpPageAPI.html#deleteResult'>deleteResult</a>: Delete the Result record. All output DataFile records of this Result are also deleted.
* stopResult: Stop a task which corresponds to a Result record. (coming soon.)
* resumeResult: Stop a task which corresponds to a Result record. (coming soon.)

### **6. Watch server changes**
Using these functions to register listener functions (or event handlers). These registered functions will be called once the BDP receives state updates.
You may use these listeners to update the corresponding information. For example, if you register listeners for Result messages, you can get the Task execution messages (standard output and standard error) in real-time.

* <a href='./BdpPageAPI.html#watchFileChange'>watchFileChange</a>: Register a listener function for DataFile changes. Only the DataFiles that are owned by you or shared with you can be listen. Whenever there is a listenable DataFile changes on the server, the registered functions will be called. You can use this function to get notified and make some updates on your Page.
* <a href='./BdpPageAPI.html#stopWatchFileChange'>stopWatchFileChange</a>: Remove the registered listener function for DataFile changes.
* <a href='./BdpPageAPI.html#watchResultChange'>watchResultChange</a>: Register a listener function for Result changes. Whenever there is a listenable Result changes on the server, the registered functions will be called. You can use this function to get notified and make some updates on your Page.
* <a href='./BdpPageAPI.html#stopWatchResultChange'>stopWatchResultChange</a>: Remove the registered listener function for Result changes.
* <a href='./BdpPageAPI.html#watchResultMsgChange'>watchResultMsgChange</a>: Register a listener function to listen Result messages. Only the related Results in the Projects that are owned by you or shared with you can be listened. Result messages are standard outputs and standard errors generated by running Tasks.
* <a href='./BdpPageAPI.html#stopWatchResultMsgChange'>stopWatchResultMsgChange</a>: Remove the registered listener function for Result messages.

### **7. UI related API**
We also provide API functions to control the UI of the BDP client. For example, you can use `toggleFullPage` function to enter full page mode.
In this mode, many components such as the lefthand-side navigation bar or top navigation header are hidden on the BDP web client. Your customized Page will be displayed in full page as if there are no BDP web client.

* <a href='./BdpPageAPI.html#toggleFullPage'>toggleFullPage</a>: Toggle the full page mode.
* <a href='./BdpPageAPI.html#togglePageList'>togglePageList</a>: Toggle the Page list.
* <a href='./BdpPageAPI.html#toggleRightSideMenu'>toggleRightSideMenu</a>: toggleRightSideMenu
* toggleRightSideBar</a>: (coming soon.)

### **8. indexedDB-related API**
IndexedDB is one of the web storage techniques. It allows you to persistently store data inside a browser. However, for security reasons, the customized Pages are displayed in a sandboxed environment. By default, Pages have no origin (no web address) and can not use cookies or the indexedDB directly. 
Therefore, We provide an interface to handle the indexedDB and the BDP-client controls the indexedDB.
Databases of indexedDB are under BDP-client's control and each Page per User can have one indexedDB. 
Under the hood, We used the npm package, <a href='https://www.npmjs.com/package/idb' target=_blank>idb</a>, to handle the indexedDB.
For more information about the indexedDB, please see this <a href='https://developer.mozilla.org/en-US/docs/Web/API/IndexedDB_API/Using_IndexedDB' target=_blank>reference</a>.

Database related
* <a href='./BdpPageAPI.html#requestDatabase'>requestDatabase</a>: Request an indexedDB database to use.
* <a href='./BdpPageAPI.html#requestDatabaseRemove'>requestDatabaseRemove</a>: Remove the requested indexedDB database.

Adding/updating data in a data store
* <a href='./BdpPageAPI.html#dataStoreAdd'>dataStoreAdd</a>: Inserting data records to the data store in the indexedDB.
* <a href='./BdpPageAPI.html#dataStorePut'>dataStorePut</a>: Similar to `BdpPageAPI.dataStoreAdd`, this function performs the update or insert method. Data records of the same key are updated.

Querying data in a data store
* <a href='./BdpPageAPI.html#dataStoreQuery'>dataStoreQuery</a>: Query data in a dataStore. The `iterateFn` will be called with each of the resulting data records.
* <a href='./BdpPageAPI.html#dataStoreGet'>dataStoreGet</a>: A simplified version of `BdpPageAPI.dataStoreQuery`. This function will directly return the query results.
* <a href='./BdpPageAPI.html#dataStoreCount'>dataStoreCount</a>: Counting the record numbers of a data store in the indexedDB.

Remove data or data store
* <a href='./BdpPageAPI.html#dataStoreClear'>dataStoreClear</a>: Clearing data records in a data store in the indexedDB. The data store exists and becomes empty.
* <a href='./BdpPageAPI.html#dataStoreRemove'>dataStoreRemove</a>: Removing a data store in the indexedDB. The data store is removed and not existed.

### **9. Low-level API**
* <a href='./BdpPageAPI.html#requestHttp'>requestHttp</a>: A low level API to make requests to BDP server.

# License
Licensed under the MIT license (see the <a href="https://github.com/big-data-processor/bdp-page-api/blob/master/LICENSE" target=_blank>license</a>)
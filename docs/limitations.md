# Known Limitations for PowerShell in Azure Cloud Shell

### Startup time can be long
  - **Details**: PowerShell in Azure Cloud Shell could take up to 60 seconds to initialize.
  - **Workaround**: None. This is an issue we are working to address.

### ~~Azure Files storage must be in the WestUS region~~
  - ~~**Details**: To auto-mount your persistent storage, it must be in the WestUS region. As we expand coverage to additional regions, this requirement will disappear. For additional information, see [Persisting Shell Storage](https://docs.microsoft.com/azure/cloud-shell/persisting-shell-storage).~~
  - ~~**Workaround**: Manually mount storage in the WestUS region for the account you use for PowerShell in Azure Cloud Shell.~~
  - **Update [11 July 2017]**: We expanded coverage to match all regions supported by Cloud Shell. This is no longer an issue.

### An error occurs during startup if your Cloud Shell storage account is already created in a region other than your current region. 
  - **Details**: This issue will affect early Private Preview users who created storage when only the West US region was supported.
    If a storage account is already created in the West US region, but your shell now starts in another region, you might see an error at startup such as one of these:
    * `Requesting a Cloud Shell..Sorry, something went wrong: {"code":"ServiceUnavailable","message":"Azure Cloud Shell is not available at this moment, please retry later."}`
    * `Requesting a Cloud Shell..Sorry, something went wrong: {"code":"InternalServerError","message":"Encountered an internal server error. The tracking id is '...'."}`

  - **Workaround**: To mount storage in your new region, untag the existing storage account and restart PowerShell in Cloud Shell. 
      1.	Login into [Azure Portal](https://portal.azure.com) directly (ie., without using our private preview link)
      2.	Launch the Cloud Shell (Bash)
          * If you have an active PowerShell session [you will need to Restart Cloud Shell](#for-a-given-user-only-one-shell-can-be-active)
      3.	Wait for the prompt and run command `clouddrive unmount`
      > Note: Untagging the storage account doesn't remove any data and it will still exist in your subscription

      4.	Log back using [PowerShell Private Preview link](https://aka.ms/PSCloudPreview) and launch the Cloud Shell (PowerShell) 
      > Note: You will have to [Restart Cloud Shell](media/shell-recycle.png) for PowerShell instance

### An error about [MissingSubscriptionRegistration](media/storageRP-error.jpg) occurs during persistent storage creation
  - **Details**: The selected subscription does not have Storage RP registered.
  - **Resolution**: [Register your Storage resource provider](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-common-deployment-errors#noregisteredproviderfound)

### During first use initialization, an [Unknown Error](media/startup_unknown_error.jpg) occurs

  - **Details**: During first use, the console may hang and display the message, "Sorry, something went wrong: Unknown Error".
  - **Resolution**: There are two ways to resolve this issue and either will work.
    1. Refresh the web page using the browser's refresh button and re-open the console.
    1. Close the shell IFrame (the sub-window that appears after clicking on the shell button) and re-open the console.

### For a given user only one shell can be active
  - **Details**: If the user launches Bash shell first and shortly opens PowerShell, it would connect back to the Bash shell with blue background.
  Similarly, if the user launches PowerShell first and shortly opens Bash shell, it would connect back to PowerShell instance with black background.
  - **Workaround**: Restart the shell by clicking [Restart Cloud Shell](media/shell-recycle.png) in the shell IFrame (refreshing the browser tab will not work).

### Automatic Azure authentication limit
  - **Details**: The authentication is active only for an hour due to a limitation in processing the Azure authentication token.
  - **Workaround**: Restart the shell by clicking [Restart Cloud Shell](media/shell-recycle.png) in the shell IFrame (refreshing the browser tab will not work).

### Get-Help -online does not open the help page
  - **Details**: If a user types `Get-Help Find-Module -online`, one will see an error message such as:\
  `Starting a browser to display online Help failed. No program or browser is associated to open the URI http://go.microsoft.com/fwlink/?LinkID=398574.`
  - **Workaround**: Copy the url and open it manually on your browser.

### GUI applications are not supported
  - **Details**: If a user tries to launch a GUI app (e.g., git clone a 2FA enabled private repo. It will pop up a 2FA authentication dialog box), the console prompt does not return.
  - **Workaround**: `Ctrl+C` to exit the command.

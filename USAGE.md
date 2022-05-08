# Usage
https://cloudcross.mastersoft24.ru/usage \

This page contains few main solutions and explanation about using CloudCross for synchronization your data. \
\
Some additional (and I hope interesting) information could be found on Developer Blog.\
\
For beginning work with CloudCross you must pass authentication on your cloud storage account (OneDrive, Google Drive, Dropbox or Yandex Disk of Cloud Mail.ru) for allow an application to use it. Run **CloudCross** with **-a** option (for provider definition use option **--provider PROVIDER_NAME**)
```
ccross -a
```
or for example for dropbox authenticate
```
ccross -a --provider dropbox
```
As response, the application will return something like this
```
Please go to this URL and confirm application credentials

https://accounts.google.com//ServiceLogin?passive=1209600&continue=https://accounts.google.com/o/oauth2/v2/auth?access_type%3Doffline%26approval_prompt%3Dforce%26scope%3Dhttps://www.googleapis.com/auth/drive%2Bhttps://www.googleapis.com/auth/userinfo.email%2Bhttps://www.googleapis.com/auth/userinfo.profile%2Bhttps://docs.google.com/feeds/%2Bhttps://docs.googleusercontent.com/%2Bhttps://spreadsheets.google.com/feeds/%26response_type%3Dcode%26redirect_uri%3Durn:ietf:wg:oauth:2.0:oob%26state%3D1%26client_id%3D8344155748-oq0p2m5dro2bvh3bu0o5bp19ok3qrs3f.apps.googleusercontent.com%26hl%3Dru%26from_login%3D1%26as%3D4ee15e216c8734fd&mpl=nosignup&oauth=1&sarp=1&scc=1 
```
Copy this URL to your browser and go to on it. Next, enter your password and press "Accept" button. As a result, the authentication code will be sent to an application. If all is successful, the application will report this.

**Attention:**

For Cloud MAIL.RU you must use --login and --password parameters. In this case, authentication flow looks like this:
```
ccross -a --provider mailru --login your_login --password=your_password
```
Now CloudCross is ready to work. \
\
For synchronization, you enough run a program with defined --provider option.\
```
ccross
```
or (for example)
```
ccross --provider dropbox
```
Please note! If you run synchronization in an empty folder, do not forget to use the --prefer=remote option. Or, since version 1.0.4, you can use the --force option.

But CloudCross allows you to configure the sync process. Advanced options are used for this purpose.
```
Option 	Description
--no-hidden 	do not sync hidden files and folders
--dry-run 	shows which files will be loaded/unloaded, but really do not synchronize
--prefer arg 	set sync strategy. What to have priority-local or remote. Can be one of "local" or "remote"
--use-include 	use a .include file with a list of files that will participate in synchronization.
--path arg 	the absolute path to synchronize directory.
--no-new-rev 	do not create a new version of the file on a server, but overwrite him when file upload
--convert-doc 	convert docs when syncing from MS/Libre/Open Office format to Google Docs and back
--force arg 	Forcing upload or download files. It can be a one of "upload" or "download". This option overrides --prefer option value.
--provider arg 	Selects one of supported cloud providers. It option can be a one of "yandex", "onedrive" ,"google","dropbox" or "mailru" . By default this option value is "google".
--login your_login 	Set login for access to a cloud provider. At this moment it used only for Cloud Mail.ru
--password your_password 	Set password for access to a cloud provider. At this moment it used only for Cloud Mail.ru
--http-proxy arg 	Use HTTP proxy server for connection to a cloud provider. arg must be in a ip_address_or_host_name:port_number format
--socks5-proxy arg 	Use socks5 proxy server for connection to a cloud provider.arg must be in a ip_address_or_host_name:port_number format
--filter-type arg 	Filter type for .include and .exclude files. Could be set to "regexp" or "wildcard" value. This parameter will be ignored if it set in .include/.exclude files
--single-thread 	Run sychronization in single thread mode
--low-memory 	Reduce memory utilization during reading a remote file list. Using of this option may do increase of synchronization time
-- empty-trash 	Delete all files from cloud trash bin. This option is not supporting by all cloud providers.
Option 	Description
--help 	print short help message
--version 	print a CloudCross version
--list 	print remote file list. Can be used for .include/.exclude file creation
--auth 	Cloud provider athentication
--cloud-space 	Show total and free spase of cloud.
```
By default CloudCross working in local files priority mode. This means what if you delete a file on remote, then the next time sync this file will be uploaded to the cloud. And if you delete a file on your computer, it will be removed on cloud storage. In remote file priority mode, the application behavior is opposite.
.include and .exclude files \
\
If you do not specify the --use-include option, CloudCross tries to find the .exclude file and if it finds that uses it for synchronization. File list who contains in .exclude file, in fact, is a blacklist. All files in this file will be excluded from sync. The .include file, an opposite, is a whitelist. In this case, will be synchronized only the files listed in it. \
\
The .include and .exclude file format could be in two variants. First is a wildcard format. Second is a Regexp format. Which one of a two format will be used is determined in the first line of an .include or .exclude files or with --filter-type option argument. Each of lines in these files must be relative paths of files and folders. \
\
For example this listings is a regular (wildcard) contents of .include/.exclude file \
```
/file1
/folder1/file1
/folder2/as*.jpg
```
or
```
wildcard
/file1
/folder1/file1
/folder2/as*.jpg
```
By default Google Drive creates a new version for each of downloaded files, preserving the previous version of those files. All these versions are stored on the server for some time, occupying the free space. But you can change this behavior, using --no-new-rev option when synchronizing. Thus when a file is uploaded to the server, the new version will not be created and will be overwritten with the latest version of this file. \
\
This option works only with Google Drive. \
\
CloudCross provides the possibility of converting files when uploading into Google Drive from Microsoft Office or Open/Libre Office formats to Google Docs format, that allows you to continue working with them online. Then, during the next synchronization, this files will be converted back to correspond formats. Formats of Text documents, Spreadsheets and Presentations are supported. \
\
Conversion in direction from Google Drive to local storage goes only to Microsoft Office format. But in the opposite direction Open Office document format can be used too. \
\
This option works only with Google Drive. \
\
Since version 1.1.0 you can select cloud service to work. For cloud selection, you can use an option
```
--provider
```
Available providers list see in option description.

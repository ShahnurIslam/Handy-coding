```python
import paramiko # we use the paramiko library to connect to sftp
host = "ftp.example.com" # the ftp site you're connecting to
port = 22 # default port
transport = paramiko.Transport((host, port))
password = "yourpassword" # Replace 'yourpassword' with your password
username = "yourusername" # Replace 'yourusername' with your username
transport.connect(username = username, password = password)
sftp = paramiko.SFTPClient.from_transport(transport)
latest = 0
sftp.chdir('Report') # change the directory if need to be where the files are saved
latestfile = None
#This for loop, loops through the file and finds the name of the most recent file
for fileattr in sftp.listdir_attr():
	if  fileattr.st_mtime > latest:
		latest = fileattr.st_mtime
		latestfile = fileattr.filename

#checks if latestfile contains a file and saves it to the directory you want
if latestfile is not None:
    localpath = 'directory you want to save the file' + latestfile
    sftp.get(latestfile, localpath)
```

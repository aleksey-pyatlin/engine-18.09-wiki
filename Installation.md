1. On a Windows Server 2016, 1709, or 1803 host, open a PowerShell window

2. If an existing version of Docker EE is installed, remove it

`uninstall-package docker -provider dockermsftprovider`

3. Download and start the 18.09 beta 3 build

`Invoke-WebRequest -URI "https://s3-us-west-2.amazonaws.com/internal-docker-ee-builds/windows-server/18.09/docker-18.09.4.zip" -usebasicparsing -outfile docker-18.09.4.zip`

`expand-archive docker-18.09.4.zip 'C:\Program Files'`

`$oldpath = (Get-ItemProperty -Path 'Registry::HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Session Manager\Environment' -Name PATH).path`

`$newpath = “$oldpath;;C:\Program Files\docker”`

`Set-ItemProperty -Path 'Registry::HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\Session Manager\Environment' -Name PATH -Value $newPath`

Now do one final check that it looks how you expect it:

`(Get-ItemProperty -Path 'Registry::HKEY_LOCAL_MACHINE\System\CurrentControlSet\Contro
l\Session Manager\Environment' -Name PATH).Path`

You can now restart your powershell terminal (or even reboot machine) and see that it doesn’t rollback to it’s old value again. Note the ordering of the paths may change so that it’s in alphabetical order, so make sure you check the whole line, to make it easier, you can split the output into rows by using the semi-colon as a delimeter:

`($env:path).split(";")`

`c:\Program Files\docker\dockerd --register-service`

`start-service docker`

4. Verify Docker is running

```
docker version
Client:
 Version:           18.09.4
 API version:       1.39
 Go version:        go1.10.8
 Git commit:        c3516c43ef
 Built:             03/27/2019 18:22:15
 OS/Arch:           windows/amd64
 Experimental:      false

Server:
 Engine:
  Version:          18.09.4
  API version:      1.39 (minimum version 1.24)
  Go version:       go1.10.8
  Git commit:       c3516c43ef
  Built:            03/27/2019 18:20:29
  OS/Arch:          windows/amd64
  Experimental:     false
  ```
  

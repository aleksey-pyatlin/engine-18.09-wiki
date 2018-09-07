1. On a Windows Server 1709 host, open a PowerShell window

2. If an existing version of Docker EE is installed, remove it

`uninstall-package docker -provider dockermsftprovider`

3. Download and start the 18.09 beta 1 build

`Invoke-WebRequest -URI "https://s3-us-west-2.amazonaws.com/internal-docker-ee-builds/aa002c5/windows-server/18.09/docker-18.09.0-ee-1-beta1.zip" -usebasicparsing -outfile docker-18.09.0-ee-1-beta1.zip`

`expand-archive docker-18.09.0-ee-1-beta1.zip c:\`

`$Env:Path += ";c:\docker;"`

`c:\docker\dockerd --register-service`

`start-service docker`

5. Verify Docker is running

```
docker version
Client:
 Version:           18.09.0-ee-1-beta1
 API version:       1.39
 Go version:        go1.10.3
 Git commit:        aa002c503a
 Built:             unknown-buildtime
 OS/Arch:           windows/amd64
 Experimental:      false

Server:
 Engine:
  Version:          18.09.0-ee-1-beta1
  API version:      1.39 (minimum version 1.24)
  Go version:       go1.10.3
  Git commit:       aa002c503a
  Built:            09/06/2018 21:36:33
  OS/Arch:          windows/amd64
  Experimental:     false
  ```
  
7. Pre-pull IIS image (optional)

```
docker pull microsoft/iis:windowsservercore-1709
```
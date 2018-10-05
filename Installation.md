1. On a Windows Server 2016, 1709, or 1803 host, open a PowerShell window

2. If an existing version of Docker EE is installed, remove it

`uninstall-package docker -provider dockermsftprovider`

3. Download and start the 18.09 beta 3 build

`Invoke-WebRequest -URI "https://s3-us-west-2.amazonaws.com/internal-docker-ee-builds/windows-server/18.09/docker-18.09.0-beta3.zip" -usebasicparsing -outfile docker-18.09.0-ee-1-beta3.zip`

`expand-archive docker-18.09.0-ee-1-beta3.zip c:\`

`$Env:Path += ";c:\docker;"`

`c:\docker\dockerd --register-service`

`start-service docker`

4. Verify Docker is running

```
docker version
Client:
 Version:           18.09.0-beta3
 API version:       1.39
 Go version:        go1.10.3
 Git commit:        1bfae8ea39
 Built:             unknown-buildtime
 OS/Arch:           windows/amd64
 Experimental:      false

Server:
 Engine:
  Version:          18.09.0-beta3
  API version:      1.39 (minimum version 1.24)
  Go version:       go1.10.3
  Git commit:       1bfae8ea39
  Built:            10/04/2018 23:43:02
  OS/Arch:          windows/amd64
  Experimental:     false
  ```
  
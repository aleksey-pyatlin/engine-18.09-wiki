This example uses Windows Server 1709. It is also supported on Windows Server 1803.

1. Run a process-isolated container with a mounted named pipe

```
docker run -it -v '\\.\pipe\docker_engine:\\.\pipe\docker_engine' -v 'c:\docker:c:\docker' microsoft/windowsservercore:1709 powershell
```

2. Use the CLI in the volume mount to issue commands from the container that connect to the daemon via the mounted named pipe

```
c:\docker\docker version
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

```
c:\docker\docker image ls
REPOSITORY                    TAG                      IMAGE ID            CREATED             SIZE
microsoft/iis                 windowsservercore-1709   71720fc8449b        3 weeks ago         6.67GB
microsoft/windowsservercore   1709                     3af9618ecac5        3 weeks ago         6.41GB
```

3. The same scenarios work with Hyper-v isolated containers

```
docker run -it -v '\\.\pipe\docker_engine:\\.\pipe\docker_engine' -v 'c:\docker:c:\docker' --isolation hyperv microsoft/windowsservercore:1709 powershell
```

4. Find the ID of the container

```
c:\docker\docker ps
\docker\docker ps
CONTAINER ID        IMAGE                              COMMAND             CREATED             STATUS              PORTS               NAMES
7914d25e537b        microsoft/windowsservercore:1709   "powershell"        7 hours ago         Up 3 minutes                            jolly_davinci
```

5. Inspect the container to verify Hyper-v isloation

```
c:\docker\docker inspect 791 | Select-String Isolation

            "Isolation": "hyperv",
```

Note: Mounting named pipes within `service create` is [covered in this PR](https://github.com/docker/swarmkit/pull/2691)

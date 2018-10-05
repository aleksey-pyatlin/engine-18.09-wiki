## Setup

The ingress service publishing and VIP service discovery walkthroughs start from a UCP cluster with minimum one Linux master and two Windows Server 1709 workers, all running 18.09 beta3.

## Ingress service publishing
Ensure port 8080 is open in security group used by the worker VMs

`docker service create --name iis --replicas 1 -p 8080:80 --constraint node.platform.os==windows microsoft/iis:windowsservercore-1709`

Browse to `http://<Public IP address of any VM in the swarm>:8080`

Default IIS website should be displayed

## VIP service discovery

NOTE: VIP service discovery does not currently work with Windows Server 1803

Create an overlay network and deploy two services, each will get a VIP address:

`docker network create overlay1 --driver overlay`

`docker service create --name s1 --replicas 2 --network overlay1 --constraint node.platform.os==windows microsoft/iis:windowsservercore-1709`

`docker service create --name s2 --replicas 2 --network overlay1 --constraint node.platform.os==windows microsoft/iis:windowsservercore-1709`

Find a container ID for each task (run on workers)

`docker ps --format "{{.ID}}: {{.Names}}"`

Verify connectiviy between services s1 and s2 via VIP on overlay network. On worker running a task for service s1:

`docker exec -it <ID of s1 container> powershell`

`Invoke-WebRequest -Uri http://s2 -UseBasicParsing`

```
StatusCode        : 200
StatusDescription : OK
Content           : <!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Strict//EN"
                    "http://www.w3.org/TR/xhtml1/DTD/xhtml1-strict.dtd">
                    <html xmlns="http://www.w3.org/1999/xhtml">
                    <head>
                    <meta http-equiv="Content-Type" cont...
RawContent        : HTTP/1.1 200 OK
                    Accept-Ranges: bytes
                    Content-Length: 703
                    Content-Type: text/html
                    Date: Wed, 08 Nov 2017 02:01:14 GMT
                    ETag: "f2f79a40d951d31:0"
                    Last-Modified: Mon, 30 Oct 2017 23:46:05 GMT
                    Serve...
Forms             :
Headers           : {[Accept-Ranges, bytes], [Content-Length, 703], [Content-Type, text/html], [Date, Wed, 08 Nov 2017
                    02:01:14 GMT]...}
Images            : {@{outerHTML=<img src="iisstart.png" alt="IIS" width="960" height="600" />; tagName=IMG;
                    src=iisstart.png; alt=IIS; width=960; height=600}}
InputFields       : {}
Links             : {@{outerHTML=<a href="http://go.microsoft.com/fwlink/?linkid=66138&amp;clcid=0x409"><img
                    src="iisstart.png" alt="IIS" width="960" height="600" /></a>; tagName=A;
                    href=http://go.microsoft.com/fwlink/?linkid=66138&amp;clcid=0x409}}
ParsedHtml        :
RawContentLength  : 703
```

# Windows Engine 18.09

## Installation

[Installing EE 18.09 Beta 1 for Windows Server](https://github.com/carlfischer1/engine-18.09/blob/master/installation.md)

## Named pipes

For developers creating apps that require access to the host docker daemon, Docker on Linux has support for Unix domain socket mounting. Building the same types of apps on Windows is now supported via named pipe mounting, for both process and Hyper-v isolated containers. Hyper-v isolation support allows developers using Docker for Desktop on Windows, where all containers run using Hyper-v isolation, to build and test their app in the same way it will run in production.

[Walkthrough](https://github.com/carlfischer1/engine-18.09/blob/master/namedpipes.md)

## Networking

Prior to EE 18.03.1, Windows supported a subset of networking functionality compared to Linux. Parity in this area has been a frequent customer request, and with the Docker Enterprise 2.1 and Windows 18.09 engine ingress routing mesh and VIP service discovery can now be used with Windows workloads.

[Walkthrough](https://github.com/carlfischer1/engine-18.09/blob/master/networking.md)

## Telemetry

Usage telemetry for Docker EE engines running on Linux distributions has been collected since 17.06. While implemented differently, equivalent telemetry is now collected for Docker EE engines running on Windows Server.

[Details](https://github.com/carlfischer1/engine-18.09/blob/master/telemetry.md)
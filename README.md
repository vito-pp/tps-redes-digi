# TP1 - Routing

GNS3 lab with 6 Cisco 7200 routers and 6 VPCS.

## Requirements

Cisco IOS image:

```text
c7200-adventerprisek9-mz.124-24.T5.bin
```

The image is **not included** in the repo. Add it manually to GNS3.

Also required:

```text
dynamips
vpcs
xterm
```

## Current status

Topology created.

R1 ↔ R2 configured on `FastEthernet0/0`.

### R1

```text
enable
config t
interface FastEthernet0/0
ip address 10.0.12.1 255.255.255.252
duplex full
no shutdown
end
```

### R2

```text
enable
config t
interface FastEthernet0/0
ip address 10.0.12.2 255.255.255.252
duplex full
no shutdown
end
```

Connectivity checked:

```text
R1 -> 10.0.12.2 OK
R2 -> 10.0.12.1 OK
```

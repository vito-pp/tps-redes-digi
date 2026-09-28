# TP1 - Routing

GNS3 lab with 6 Cisco 7200 routers and 6 VPCS.

## Requirements

Cisco IOS image:

```text
c7200-adventerprisek9-mz.124-24.T5.bin
```

Not included in repo. Add manually to GNS3.

Also required:

```text
dynamips
vpcs
xterm
```

## Current status

AS 65001 configured with R1, R2 and R3.

Links:

```text
R1-R2  10.0.12.0/30
R1-R3  10.0.13.0/30
R2-R3  10.0.23.0/30
```

LANs:

```text
R1  192.168.10.0/24
R2  192.168.20.0/24
R3  192.168.30.0/24
```

Loopbacks:

```text
R1  1.1.1.1/32
R2  2.2.2.2/32
R3  3.3.3.3/32
```

EIGRP:

```text
AS 65001
no auto-summary
```

R1:

```text
network 10.0.12.0 0.0.0.3
network 10.0.13.0 0.0.0.3
network 192.168.10.0 0.0.0.255
network 1.1.1.1 0.0.0.0
```

R2:

```text
network 10.0.12.0 0.0.0.3
network 10.0.23.0 0.0.0.3
network 192.168.20.0 0.0.0.255
network 2.2.2.2 0.0.0.0
```

R3:

```text
network 10.0.13.0 0.0.0.3
network 10.0.23.0 0.0.0.3
network 192.168.30.0 0.0.0.255
network 3.3.3.3 0.0.0.0
```

Connectivity between PC1, PC2 and PC3 works through EIGRP.

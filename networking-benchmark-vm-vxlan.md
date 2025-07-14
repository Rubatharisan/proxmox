# Proxmox SDN Benchmark: VM-to-VM Networking over VXLAN

## Environment Overview

* **Hypervisor Nodes**:

  * `pmx-green` – Lenovo L14
  * `pmx-black` – Lenovo L14
* **Router**: UniFi Dream Machine Pro
* **Proxmox Version**: 8.4.1
* **SDN Configuration**: Proxmox SDN using **VXLAN overlay networking**
* **Guest OS**: Alpine Linux (minimal, lightweight)
* **VM Placement**:

  * Client VM: `dev-gateway` on `pmx-green`
  * Server VM: `dev-app02` on `pmx-black`

---

## iperf3 Benchmark: Client to Server over VXLAN

### Client: `dev-gateway` (`10.1.0.1`)

**Command:**

```bash
iperf3 -c 10.1.0.101
```

**Result:**

```
Connecting to host 10.1.0.101, port 5201
[ ID] Interval           Transfer     Bitrate         Retr  Cwnd
[  5]   0.00-1.00   sec   110 MBytes   924 Mbits/sec    0    321 KBytes
[  5]   1.00-2.00   sec   108 MBytes   907 Mbits/sec    0    337 KBytes
[  5]   2.00-3.00   sec   108 MBytes   909 Mbits/sec    0    337 KBytes
[  5]   3.00-4.00   sec   108 MBytes   911 Mbits/sec    0    337 KBytes
[  5]   4.00-5.00   sec   108 MBytes   909 Mbits/sec    0    337 KBytes
[  5]   5.00-6.00   sec   108 MBytes   904 Mbits/sec    0    354 KBytes
[  5]   6.00-7.00   sec   109 MBytes   913 Mbits/sec    0    388 KBytes
[  5]   7.00-8.00   sec   108 MBytes   907 Mbits/sec    0    388 KBytes
[  5]   8.00-9.00   sec   108 MBytes   907 Mbits/sec    0    388 KBytes
[  5]   9.00-10.00  sec   109 MBytes   914 Mbits/sec    0    388 KBytes

[ ID] Interval           Transfer     Bitrate         Retr
[  5]   0.00-10.00  sec  1.06 GBytes   911 Mbits/sec    0  sender
[  5]   0.00-10.00  sec  1.06 GBytes   909 Mbits/sec        receiver
```

---

### Server: `dev-app02` (`10.1.0.101`)

**Command:**

```bash
iperf3 -s
```

**Result:**

```
Accepted connection from 10.1.0.1, port 59336
[ ID] Interval           Transfer     Bitrate
[  5]   0.00-1.00   sec   108 MBytes   908 Mbits/sec
[  5]   1.00-2.00   sec   108 MBytes   908 Mbits/sec
[  5]   2.00-3.00   sec   108 MBytes   909 Mbits/sec
[  5]   3.00-4.00   sec   108 MBytes   909 Mbits/sec
[  5]   4.00-5.00   sec   108 MBytes   908 Mbits/sec
[  5]   5.00-6.00   sec   108 MBytes   909 Mbits/sec
[  5]   6.00-7.00   sec   108 MBytes   909 Mbits/sec
[  5]   7.00-8.00   sec   108 MBytes   908 Mbits/sec
[  5]   8.00-9.00   sec   108 MBytes   909 Mbits/sec
[  5]   9.00-10.00  sec   108 MBytes   909 Mbits/sec
[  5]  10.00-10.00  sec   256 KBytes   768 Mbits/sec

[ ID] Interval           Transfer     Bitrate
[  5]   0.00-10.00  sec  1.06 GBytes   909 Mbits/sec  receiver
```

---

## Performance Summary

| Metric                  | Value                       |
| ----------------------- | --------------------------- |
| **Avg Throughput (TX)** | 911 Mbits/sec               |
| **Avg Throughput (RX)** | 909 Mbits/sec               |
| **Retransmissions**     | 0                           |
| **Congestion Window**   | 321K → 388K                 |
| **Transport Layer**     | TCP                         |
| **Overlay Network**     | Proxmox SDN using **VXLAN** |
| **Stability**           | Excellent — < 2% variance   |

---

## Conclusion

This benchmark confirms that Proxmox SDN over **VXLAN** provides near-native Gigabit Ethernet performance between VMs on different hosts. With **911 Mbps** average throughput and **zero retransmissions**, the VXLAN overlay introduces negligible overhead in this configuration.

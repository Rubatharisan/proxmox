# Iperf3 Network Benchmark Report

## Test Setup

* **Hardware**: Lenovo L14 laptop running Proxmox VE 8.4.1, configured as a cluster
* **Network**: Connected through a UniFi Dream Machine Pro (UDM Pro) router
* **Purpose**: Measure node-to-node throughput over LAN
* **Client Node**: `pmx-green` (192.168.22.97)
* **Server Node**: `pmx-black` (192.168.22.13)

---

## Iperf3 Test: Node-to-Node Benchmark

### From Client: `pmx-green`

**Command Run:**

```bash
iperf3 -c 192.168.22.13
```

**Result (Client Output):**

```
Connecting to host 192.168.22.13, port 5201
[  5] local 192.168.22.97 port 53382 connected to 192.168.22.13 port 5201
[ ID] Interval           Transfer     Bitrate         Retr  Cwnd
[  5]   0.00-1.00   sec   114 MBytes   958 Mbits/sec    0    375 KBytes
[  5]   1.00-2.00   sec   112 MBytes   939 Mbits/sec    0    375 KBytes
[  5]   2.00-3.00   sec   112 MBytes   939 Mbits/sec    0    375 KBytes
[  5]   3.00-4.00   sec   113 MBytes   945 Mbits/sec    0    375 KBytes
[  5]   4.00-5.00   sec   112 MBytes   939 Mbits/sec    0    375 KBytes
[  5]   5.00-6.00   sec   113 MBytes   945 Mbits/sec    0    375 KBytes
[  5]   6.00-7.00   sec   112 MBytes   939 Mbits/sec    0    375 KBytes
[  5]   7.00-8.00   sec   113 MBytes   944 Mbits/sec    0    393 KBytes
[  5]   8.00-9.00   sec   112 MBytes   940 Mbits/sec    0    393 KBytes
[  5]   9.00-10.00  sec   112 MBytes   937 Mbits/sec    0    393 KBytes
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bitrate         Retr
[  5]   0.00-10.00  sec  1.10 GBytes   943 Mbits/sec    0             sender
[  5]   0.00-10.00  sec  1.10 GBytes   941 Mbits/sec                  receiver
```

---

### From Server: `pmx-black`

**Command Run:**

```bash
iperf3 -s
```

**Result (Server Output):**

```
Server listening on 5201 (test #1)
Accepted connection from 192.168.22.97, port 53372
[ ID] Interval           Transfer     Bitrate
[  5]   0.00-1.00   sec   112 MBytes   941 Mbits/sec
[  5]   1.00-2.00   sec   112 MBytes   941 Mbits/sec
[  5]   2.00-3.00   sec   112 MBytes   941 Mbits/sec
[  5]   3.00-4.00   sec   112 MBytes   941 Mbits/sec
[  5]   4.00-5.00   sec   112 MBytes   941 Mbits/sec
[  5]   5.00-6.00   sec   112 MBytes   941 Mbits/sec
[  5]   6.00-7.00   sec   112 MBytes   941 Mbits/sec
[  5]   7.00-8.00   sec   112 MBytes   941 Mbits/sec
[  5]   8.00-9.00   sec   112 MBytes   941 Mbits/sec
[  5]   9.00-10.00  sec   112 MBytes   941 Mbits/sec
[  5]  10.00-10.00  sec   194 KBytes   752 Mbits/sec
- - - - - - - - - - - - - - - - - - - - - - - - -
[ ID] Interval           Transfer     Bitrate
[  5]   0.00-10.00  sec  1.10 GBytes   941 Mbits/sec                  receiver
```

---

## Summary

* **Average Throughput**:

  * **Client (Sender)**: 943 Mbits/sec
  * **Client (Receiver)**: 941 Mbits/sec
  * **Server (Receiver)**: 941 Mbits/sec

* **Retransmissions**: None reported

* **Stability**: Throughput remains steady across all 1-second intervals, indicating a robust LAN connection between nodes

* **Congestion Window**: Remained stable around 375–393 KBytes, no drops observed

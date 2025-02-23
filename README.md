[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/IAASVEAZ)
# CSIT5970 Assignment-1: EC2 Measurement (2 questions, 4 marks)

### Deadline: 11:59PM, Feb, 28, Friday

---

### Name: Yim Kam Kei John
### Student Id: 21117478
### Email: kkjyim@connect.ust.hk

---

## Question 1: Measure the EC2 CPU and Memory performance

1. (1 mark) Report the name of measurement tool used in your measurements (you are free to choose *any* open source measurement software as long as it can measure CPU and memory performance). Please describe your configuration of the measurement tool, and explain why you set such a value for each parameter. Explain what the values obtained from measurement results represent (e.g., the value of your measurement result can be the execution time for a scientific computing task, a score given by the measurement tools or something else).

    > The Phoronix Test Suite is utilized as the benchmarking tool for assessing CPU, memory, and I/O performance.
    > For the CPU test, the command phoronix-test-suite run pts/compress-7zip is executed. The result obtained is the average compression rating, which serves as an indicator of CPU performance.
    > For the memory test, the command phoronix-test-suite run pts/ramspeed is used, specifically measuring the "copy - benchmark: Integer." The outcome reflects the average memory copy speed in megabytes per second, providing insight into memory performance.
    > To evaluate network performance on EC2, iperf is employed. For TCP performance measurement, the command iperf -s -w 256K is run on the server side, while the client side uses iperf -c [Server IP address] -w 256K. The 256K window size is chosen to facilitate quicker performance evaluation. The result indicates the bandwidth between the server and client, reflecting TCP performance.
    > For measuring Round-Trip Time (RTT), the built-in ping command is used, which requires no additional installation. The command ping -c 1 [Server IP address] is executed to send a single packet, allowing for a faster performance assessment. The result represents the RTT for transmitting and receiving one packet, indicating RTT performance.

2. (1 mark) Run your measurement tool on general purpose `t2.micro`, `t2.medium`, and `c5d.large` Linux instances, respectively, and find the performance differences among these instances. Launch all the instances in the **US East (N. Virginia)** region. Does the performance of EC2 instances increase commensurate with the increase of the number of vCPUs and memory resource?

    In order to answer this question, you need to complete the following table by filling out blanks with the measurement results corresponding to each instance type.

    | Size        | CPU performance | Memory performance |
    | ----------- | --------------- | ------------------ |
    | `t2.micro`  |    3771 MIPS    |    10730.27 MB/s   |
    | `t2.medium` |    9980 MIPS    |    19299.96 MB/s   |
    | `c5d.large` |    7575 MIPS    |    14133.68 MB/s   |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI.
    > CPU Performance:
    > Among the EC2 instances, the t2.medium stands out with the highest CPU performance, achieving 9980 MIPS. It is followed by the c5d.large, which delivers 7575 MIPS, while the t2.micro lags behind at 3771 MIPS. Interestingly, despite the t2.medium having more virtual CPUs, it still outperforms the larger c5d.large instance. This performance gap may be explained by differences in CPU architecture or specific optimizations that vary between the different instance families. 
    > Memory Performance:
    > In terms of memory performance, the t2.medium instance also leads with a rate of 19299.96 MB/s, followed by c5d.large at 14133.68 MB/s and t2.micro at 10730.27 MB/s. Generally, memory performance tends to improve with an increase in vCPUs and memory resources; however, t2.medium surpasses c5d.large despite having fewer available resources. While EC2 instance performance usually scales with additional vCPUs and memory, CPU performance does not always follow this trend, as evidenced by t2.medium's superior CPU results compared to c5d.large. However, CPU performance doesn't always increase proportionally to memory performance, possibly because CPU performance can be influenced by factors such as clock speed, core efficiency, and the specific workloads being run, which may leverage memory bandwidth differently than CPU processing power.


## Question 2: Measure the EC2 Network performance

1. (1 mark) The metrics of network performance include **TCP bandwidth** and **round-trip time (RTT)**. Within the same region, what network performance is experienced between instances of the same type and different types? In order to answer this question, you need to complete the following table.

    | Type                      | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | `t3.medium` - `t3.medium` |    3790Mbps    | 0.233ms  |
    | `m5.large` - `m5.large`   |    4970Mbps    | 0.226ms  |
    | `c5n.large` - `c5n.large` |    4940Mbps    | 0.151ms  |
    | `t3.medium` - `c5n.large` |    2320Mbps    | 0.638ms  |
    | `m5.large` - `c5n.large`  |    2620Mbps    | 0.602ms  |
    | `m5.large` - `t3.medium`  |    4510Mbps    | 0.227ms  |

    > Region: US East (N. Virginia). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. Note: Use private IP address when using iPerf within the same region. You'll need iPerf for measuring TCP bandwidth and Ping for measuring Round-Trip time.

2. (1 mark) What about the network performance for instances deployed in different regions? In order to answer this question, you need to complete the following table.

    | Connection                | TCP b/w (Mbps) | RTT (ms) |
    | ------------------------- | -------------- | -------- |
    | N. Virginia - Oregon      |    33.4Mbps    | 62.347ms |
    | N. Virginia - N. Virginia |    4000Mbps    | 0.279ms  |
    | Oregon - Oregon           |    9190Mbps    | 0.095ms  |
 
    > Region: US East (N. Virginia), US West (Oregon). Use `Ubuntu Server 22.04 LTS (HVM)` as AMI. All instances are `c5.large`. Note: Use public IP address when using iPerf within the same region.

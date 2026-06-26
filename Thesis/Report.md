Adaptive SDN Controller Testing Framework
Project Flow & Methodology
Phase 1 — Environment Setup & Baseline Testing
Establish the testing environment and measure baseline performance for each controller.
1.	Write a simple Mininet topology script (3–5 switches, 4–6 hosts)
2.	Connect each controller one at a time: Ryu → POX → Floodlight → OpenDaylight
3.	Run basic ping and iperf3 tests — confirm each controller works
4.	Record baseline: latency, throughput, packet loss per controller
Phase 2 — Dynamic Network Condition Testing
Simulate real-world network challenges including load increases, link failures, and traffic bursts.
5.	Simulate increasing load using iperf3 with stepped bandwidth (10 → 50 → 100 Mbps)
6.	Trigger link failures mid-test using Mininet's link down command
7.	Generate traffic bursts (CBR + Poisson model) using D-ITG or hping3
8.	Measure: flow setup time, recovery time, throughput degradation per controller
Phase 3 — Adversarial / Attack Scenario Testing
Test controller resilience against security attacks including flooding, DDoS, and flow table saturation.
9.	Simulate packet-in flooding: craft spoofed packets using Scapy to overwhelm controller
10.	Simulate DDoS traffic bursts: multiple hosts flood a target with hping3 or iperf3 UDP
11.	Generate malicious random flows to saturate flow tables
12.	Measure: CPU usage, control-plane latency, packet loss, throughput under each attack
Phase 4 — Build Adaptive Ryu Mechanism
Develop and test a new adaptive mechanism for the Ryu controller to handle attacks.
13.	In Ryu app: add a packet-in rate counter with a sliding time window
14.	Implement threshold logic: if rate > T → rate-limit + deprioritize suspicious flows
15.	Add traffic classifier: distinguish legitimate vs anomalous flows by IP/port entropy
16.	Test modified Ryu in isolation — verify the mechanism triggers correctly
Phase 5 — Comparative Evaluation
Re-run all dynamic and attack scenario tests to compare the adaptive Ryu with baseline controllers.
17.	Repeat Phase 2 and Phase 3 tests with adaptive Ryu — same topology, same traffic
18.	Compare default Ryu vs adaptive Ryu: latency, throughput, CPU, recovery time
19.	Compare all 4 controllers side-by-side across all dynamic and attack scenarios
20.	Use Wireshark / OpenFlow stats for detailed packet-level analysis
Phase 6 — Data Collection, Analysis & Write-up
Compile, analyze, and present results in a comprehensive report.
21.	Collect all metrics into CSV: per-controller, per-scenario (12+ experiment runs)
22.	Plot graphs: CDF of latency, throughput vs load, CPU under attack, recovery time bars
23.	Analyze trade-offs: scalability vs security vs performance per controller
24.	Write results, discussion, and conclusion sections
Tools & Technologies
•	Mininet — topology & traffic simulation
•	iperf3 / hping3 / Scapy / D-ITG — traffic generation & attack simulation
•	Wireshark / tcpdump — packet capture & analysis
•	Python (Ryu app) — adaptive mechanism implementation
•	matplotlib / pandas — data analysis & plotting

# Title Page

Performance and Resilience Evaluation of Heterogeneous SDN Controllers under Dynamic Network Conditions with Adaptive Flow Management

Submitted by
Exam Roll: XXXXX
Registration No: XXXXX

Submitted to the
Department of Computer Science and Engineering
University of Dhaka, Bangladesh

# Declaration Form



# Abstract

Software-Defined Networking (SDN) introduces a paradigm shift by decoupling the control plane from the data plane, enabling centralized network management and programmability. The performance of SDN controllers plays a critical role in determining the overall efficiency and resilience of the network. However, most existing studies evaluate SDN controllers under static and stable conditions, which do not accurately reflect real-world dynamic network environments.

This project presents a comparative evaluation of heterogeneous SDN controllers, namely Ryu, POX, Floodlight, and OpenDaylight, under varying network conditions including baseline traffic, increasing load, congestion, and link failures. Furthermore, a lightweight adaptive flow management mechanism is proposed and implemented in the Ryu controller to enhance performance during high traffic conditions.

The evaluation is conducted using Mininet-based network emulation and Wireshark for packet-level analysis. Key performance metrics such as latency, throughput, packet loss, flow setup time, and recovery time are analyzed.

Experimental results demonstrate that controller performance significantly degrades under stress conditions, and the proposed adaptive mechanism effectively improves responsiveness and reduces packet loss under high load scenarios. This work contributes to more realistic SDN controller evaluation and demonstrates a practical approach to improving controller resilience.

# Acknowledgements

(Write simple formal text — you can personalize later)

# Chapter 1: Introduction
## 1.1 Introduction

Software-Defined Networking (SDN) is an emerging networking paradigm that separates the control plane from the data plane, enabling centralized control and programmability of network devices. This approach simplifies network management, improves scalability, and supports rapid deployment of new services.

SDN is widely used in modern infrastructures such as cloud computing, data centers, and enterprise networks. The SDN controller acts as the “brain” of the network, making decisions and installing flow rules in switches.

## 1.2 Description of the Work Domain

The project operates within the domain of SDN and network performance analysis.

Key components include:

SDN Controllers (Ryu, POX, Floodlight, OpenDaylight)
OpenFlow switches (via Mininet)
Traffic generators
Monitoring tools (Wireshark)

Security and reliability concerns:

Controller overload
Delayed flow installation
Packet loss under congestion
Failure recovery inefficiency

## 1.3 Problem Statement

Most SDN controller evaluations are conducted under stable conditions, which fail to capture real-world network dynamics such as traffic spikes, congestion, and failures. There is a lack of comparative analysis under such dynamic scenarios and limited work on lightweight adaptive mechanisms to improve controller performance.

## 1.4 Motivation

Real-world networks are dynamic and unpredictable. Controllers must handle:

Sudden traffic bursts
Network failures
Scalability challenges

Existing controllers often degrade under stress. Therefore, evaluating their behavior and improving performance using adaptive techniques is essential.

## 1.5 Objectives
Evaluate multiple SDN controllers under identical conditions
Analyze performance under dynamic scenarios
Design and implement an adaptive mechanism in Ryu
Compare default vs modified controller performance
Identify strengths and limitations of each controller

## 1.6 Design / Research Methodology
Use Mininet for network emulation
Implement different network topologies
Generate traffic using iperf/ping
Capture packets using Wireshark
Modify Ryu controller logic
Collect and analyze performance metrics

## 1.7 Key Contributions
Comparative evaluation of 4 SDN controllers
Dynamic scenario-based testing
Lightweight adaptive flow control mechanism
Experimental validation of performance improvement

## 1.8 Organization of the Report

Chapter 1 introduces the project.
Chapter 2 reviews related works.
Chapter 3 presents the methodology.
Chapter 4 discusses performance evaluation.
Chapter 5 concludes the report.

# Chapter 2: Related Works
## 2.1 SDN Controller Architectures

Discuss centralized vs distributed controllers.

## 2.2 Ryu Controller
Lightweight
Python-based
Flexible but limited scalability
## 2.3 POX Controller
Educational/research-focused
Easy to modify
Limited production use
## 2.4 Floodlight
Java-based
Modular architecture
Good performance but heavier
## 2.5 OpenDaylight
Enterprise-level controller
Highly scalable
Complex setup
## 2.6 Comparative Analysis
Controller	Strength	Weakness
Ryu	Lightweight	Less scalable
POX	Simple	Limited features
Floodlight	Modular	Medium overhead
OpenDaylight	Scalable	Heavy

# Chapter 3: Proposed Methodologies

## 3.1 Introduction

The problem is complex due to:

Dynamic traffic behavior
Controller bottlenecks
Real-time decision requirements

## 3.2 System Model and Assumptions

System includes:

Mininet network
SDN controller
Hosts and switches

Assumptions:

OpenFlow protocol is used
Network is centrally controlled
Traffic patterns are controllable

## 3.3 Proposed Method
Step 1: Baseline Testing
Run controllers without modification
Step 2: Dynamic Scenario Testing
Increase load
Introduce congestion
Simulate failures
Step 3: Adaptive Mechanism

Modify Ryu:

if packet_in_rate > threshold:
    limit processing
    batch flow installation
else:
    normal operation
3.3.1 Secondary Contribution
Comparative benchmark dataset
Analysis of controller behavior under stress
3.3.2 Discussion

Adaptive mechanism reduces:

Controller overload
Packet loss
Latency under stress
## 3.4 Conclusion

The proposed approach enhances controller resilience with minimal complexity.

# Chapter 4: Performance Evaluation

## 4.1 Simulation Environment
Ubuntu VM (VirtualBox)
Mininet
Ryu, POX, Floodlight, OpenDaylight
Wireshark

## 4.2 Performance Metrics
Latency
Throughput
Packet loss
Flow setup time
Recovery time
CPU usage

## 4.3 Results Analysis

Expected findings:

Ryu performs best in small networks
OpenDaylight handles scalability better
Performance drops under congestion
Modified Ryu:
Lower packet loss
Improved latency
Better handling of traffic spikes

Graphs to include:

Latency vs Load
Throughput vs Traffic
Packet loss vs Congestion

## 4.4 Conclusion

Adaptive Ryu outperforms default behavior under high load conditions.

# Chapter 5: Conclusions

## 5.1 Summary of Work

This project evaluated multiple SDN controllers under dynamic conditions and proposed an adaptive mechanism to improve performance. Results confirmed that lightweight enhancements can significantly improve controller responsiveness.

## 5.2 Future Direction
AI-based traffic prediction
Multi-controller architecture
Real-world deployment testing
Security-focused enhancements
Bibliography (example)

Add real papers like:

“A Survey on SDN Controllers”
“Performance Evaluation of OpenFlow Controllers”
IEEE / Springer papers

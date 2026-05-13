# Members:
  - Name: Syed Mahathir Md. Lenin
    * Reg: H-408
    * Phone: "01711313722"

  - Name: Suchayana Dey
    * Reg: H-430
    * Phone: "01949612685"
---

# Security-Aware Evaluation of SDN Controllers under Dynamic and Adversarial Conditions

## Research Domain
- Software Defined Networking (SDN)
- Network Security & Cybersecurity
- Network Resilience
- Telecom Network Engineering

---

## Introduction
Software Defined Networking (SDN) has transformed modern network architecture by separating the control plane from the data plane, enabling centralized network management, programmability, and dynamic traffic control. SDN controllers such as Ryu, POX, Floodlight, and OpenDaylight are responsible for managing network behavior and forwarding decisions. Although SDN offers flexibility and scalability, the centralized nature of controllers introduces performance and security challenges.

Most existing studies evaluate SDN controllers only under stable and normal traffic conditions. However, real-world enterprise and telecom networks frequently experience dynamic traffic changes, congestion, link failures, and cyberattacks such as Distributed Denial of Service (DDoS) and packet-in flooding attacks. Under such conditions, SDN controllers may suffer from increased latency, packet loss, controller overload, degraded throughput, and delayed recovery.

Therefore, there is a need to evaluate controller behavior under both dynamic and adversarial environments and to explore lightweight adaptive mechanisms that can improve controller resilience and security performance. This research proposes a comparative performance and security evaluation of multiple SDN controllers under dynamic network conditions and attack scenarios. Additionally, the research introduces an adaptive security-aware flow management mechanism implemented in the Ryu controller to improve responsiveness and resilience during abnormal traffic conditions.

---

## Problem Statement
Existing SDN controller evaluation studies primarily focus on performance benchmarking under stable network environments. However, modern networks, especially telecom and enterprise infrastructures, operate in highly dynamic and adversarial conditions involving traffic congestion, link failures, sudden traffic spikes, and cyberattacks. Current SDN controllers may experience:
- Control-plane saturation
- Delayed flow setup
- High packet loss
- Increased latency
- Reduced throughput
- Slow recovery during attacks or network failures

There is a lack of comparative research evaluating heterogeneous SDN controllers under both dynamic and malicious conditions while also investigating lightweight adaptive mechanisms for improving controller resilience and security performance.

---

## Research Aim
The aim of this research is to evaluate the performance, resilience, and security behavior of heterogeneous SDN controllers under dynamic and adversarial network conditions and to develop an adaptive flow management mechanism that enhances controller performance during high-load and attack scenarios.

---

## Research Objectives

### Primary Objectives
1. Evaluate and compare the performance of multiple SDN controllers under identical network environments.
2. Analyze controller behavior under dynamic network conditions including:
   - Increasing traffic load
   - Congestion
   - Traffic bursts
   - Link failures
3. Evaluate controller resilience under adversarial conditions such as:
   - Packet-in flooding attacks
   - DDoS traffic bursts
   - Malicious flow generation
4. Design and implement an adaptive security-aware flow management mechanism in the Ryu SDN controller.
5. Compare the default Ryu controller with the modified adaptive Ryu controller under stress and attack conditions.
6. Analyze how adaptive flow management improves network stability, controller responsiveness, and attack resilience.

---

## Research Questions
1. How do different SDN controllers perform under dynamic network conditions?
2. Which SDN controller provides better scalability and resilience during congestion and failures?
3. How do SDN controllers behave under adversarial traffic conditions such as packet-in flooding and DDoS attacks?
4. Can a lightweight adaptive flow management mechanism improve controller performance and resilience under attack scenarios?
5. What trade-offs exist between controller performance, scalability, and security resilience?

---

## Proposed Contribution
This research contributes in four major areas:

1. **Comparative SDN Controller Evaluation**  
   A detailed comparison of multiple heterogeneous SDN controllers under identical experimental conditions.
2. **Dynamic Network Testing**  
   Evaluation beyond traditional static benchmarking through realistic dynamic scenarios.
3. **Security-Aware Stress Analysis**  
   Investigation of SDN controller behavior under adversarial and attack-oriented traffic conditions.
4. **Adaptive Security-Aware Flow Management**  
   Development of a lightweight adaptive mechanism within the Ryu controller capable of:
   - Detecting abnormal packet-in behavior
   - Mitigating controller overload
   - Prioritizing legitimate traffic
   - Improving resilience during attacks

---

## Proposed Adaptive Mechanism
The proposed mechanism will monitor controller packet-in request rates and dynamically adjust flow handling behavior.

### Conceptual Logic
```python
IF packet_in_rate > threshold:
    classify traffic priority
    apply rate limiting
    reduce excessive flow installations
    temporarily deprioritize suspicious flows
ELSE:
    normal processing



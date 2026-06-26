<img width="1440" height="4400" alt="image" src="https://github.com/user-attachments/assets/5b3864f7-846d-4aca-9037-1f7da1aa1746" />


# Adaptive SDN Controller Testing Framework

## Project Flow & Methodology

```mermaid
flowchart TD
    %% Modern color palette
    classDef p1 fill:#3b82f6,stroke:#2563eb,color:#fff,stroke-width:2px;
    classDef p2 fill:#10b981,stroke:#059669,color:#fff,stroke-width:2px;
    classDef p3 fill:#ef4444,stroke:#dc2626,color:#fff,stroke-width:2px;
    classDef p4 fill:#8b5cf6,stroke:#7c3aed,color:#fff,stroke-width:2px;
    classDef p5 fill:#f59e0b,stroke:#d97706,color:#fff,stroke-width:2px;
    classDef p6 fill:#64748b,stroke:#475569,color:#fff,stroke-width:2px;

    P1(["<b>Phase 1: Setup & Baseline</b><br/>• Mininet topology (3-5 switches)<br/>• Connect 4 controllers<br/>• Ping & iperf3 tests<br/>• Record baseline metrics"]):::p1
    
    P2(["<b>Phase 2: Dynamic Testing</b><br/>• Simulate load (10 → 100 Mbps)<br/>• Trigger link failures<br/>• Traffic bursts (CBR/Poisson)<br/>• Measure recovery time"]):::p2
    
    P3(["<b>Phase 3: Attack Scenarios</b><br/>• Packet-in flooding (Scapy)<br/>• DDoS bursts (hping3)<br/>• Flow table saturation<br/>• Measure CPU & latency"]):::p3
    
    P4(["<b>Phase 4: Adaptive Ryu</b><br/>• Packet-in rate counter<br/>• Threshold logic (rate > T)<br/>• Traffic classifier (entropy)<br/>• Isolation testing"]):::p4
    
    P5(["<b>Phase 5: Evaluation</b><br/>• Default vs Adaptive Ryu<br/>• Side-by-side comparison<br/>• Wireshark packet analysis<br/>• Performance ranking"]):::p5
    
    P6(["<b>Phase 6: Analysis & Write-up</b><br/>• CSV data collection (12+ runs)<br/>• Plot CDF & throughput graphs<br/>• Analyze trade-offs<br/>• Final documentation"]):::p6

    P1 --> P2 --> P3 --> P4 --> P5 --> P6

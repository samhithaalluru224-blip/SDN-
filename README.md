
# 🚦 Traffic Monitoring and Statistics Collector (SDN Project)

**Course:** Computer Networks - UE24CS252B  
**Tools:** Python 3, Ryu Controller, Mininet, OpenFlow Protocol

---

## 📌 What This Project Does

This project builds a Software Defined Networking (SDN) controller using **Ryu** that monitors network traffic inside a **Mininet** virtual network. It collects and displays how many packets and bytes are flowing through the network — every 5 seconds.

---

## 📂 Project Files

```
traffic_monitor.py   ← The Ryu controller (main code)
README.md            ← This file
report.txt           ← Auto-generated traffic log
```

---

## ⚙️ Full Setup — Step by Step (From Scratch)

> Run all commands inside your **Ubuntu VM terminal**.  
> Your Ryu virtual environment is already created. Follow these steps every time.

---

### Step 1: Activate the Virtual Environment

```bash
source ryu-env/bin/activate
```

> ✅ You should see `(ryu-env)` appear at the start of your terminal prompt.  
> If the folder name is different, try:
> ```bash
> source ~/ryu-env/bin/activate
> ```

---

### Step 2: Find Where ryu-manager Is

Since `ryu-manager` is not in your PATH, first find it:

```bash
find ~/ryu-env -name "ryu-manager"
```

> It will show something like:  
> `/home/yourname/ryu-env/bin/ryu-manager`

Now run it directly using the full path (replace with your actual path):

```bash
~/ryu-env/bin/ryu-manager traffic_monitor.py
```

**Alternative if above doesn't work:**

```bash
python3 -m ryu.cmd.manager traffic_monitor.py
```

> ✅ You should see output like: `loading app traffic_monitor.py`  
> Leave this terminal **open and running**.

---

### Step 3: Open a Second Terminal and Run Mininet

```bash
sudo mn --topo single,3 --controller remote
```

> This creates: 1 switch + 3 hosts (h1, h2, h3) connected to your Ryu controller.

**Alternative if remote controller doesn't connect:**

```bash
sudo mn --topo single,3 --controller remote,ip=127.0.0.1,port=6633
```

> ✅ You should see the `mininet>` prompt appear.

---

### Step 4: Generate Traffic (Inside Mininet)

**Option A — Ping all hosts:**
```
mininet> pingall
```

**Option B — Ping between two specific hosts:**
```
mininet> h1 ping h2
```

**Option C — High traffic with iperf:**
```
mininet> iperf h1 h2
```

> ✅ Switch back to your **first terminal** (Ryu controller) — you will see flow statistics printing every 5 seconds.

---

### Step 5: Exit Mininet

```
mininet> exit
```

**Clean up leftover Mininet processes (always do this after exit):**

```bash
sudo mn -c
```

---

### Step 6: Deactivate Virtual Environment (When Done)

```bash
deactivate
```

---

## 📊 Sample Output (in Ryu Terminal)

```
FLOW STATS for switch: 1
  Flow: in_port=1 → out_port=2
  Packets: 10    Bytes: 840

  Flow: in_port=2 → out_port=1
  Packets: 10    Bytes: 840
```

| Term | Meaning |
|------|---------|
| Packets | Number of packets that passed through that flow |
| Bytes | Total data (in bytes) transferred in that flow |

---

## 🧪 Test Cases

| Test | Command | Expected Result |
|------|---------|-----------------|
| Normal Traffic | `pingall` | Low packet/byte count |
| Directed Ping | `h1 ping h2` | Packets only between h1 and h2 |
| High Traffic | `iperf h1 h2` | Much higher byte count |

---

## 🔧 Troubleshooting

### ❌ `ryu-manager: command not found`
Run using full path or Python module:
```bash
~/ryu-env/bin/ryu-manager traffic_monitor.py
# OR
python3 -m ryu.cmd.manager traffic_monitor.py
```

### ❌ Mininet says "Unable to contact the remote controller"
Make sure Ryu is running first (Step 2), then start Mininet. Also try:
```bash
sudo mn --topo single,3 --controller remote,ip=127.0.0.1,port=6633
```

### ❌ Mininet won't start / old session stuck
```bash
sudo mn -c
```
Then try starting Mininet again.

### ❌ `sudo mn` says command not found
```bash
sudo apt install mininet -y
```

### ❌ Open vSwitch error
```bash
sudo apt install openvswitch-switch -y
sudo service openvswitch-switch start
```

---

## 🧠 Key Concepts

- **SDN (Software Defined Networking):** Separates the control plane (decisions) from the data plane (forwarding), so a central controller manages the network.
- **Ryu:** A Python-based SDN controller framework that talks to switches using OpenFlow.
- **Mininet:** Creates a virtual network (switches + hosts) on a single machine for testing.
- **OpenFlow:** The protocol used by the Ryu controller to communicate with Mininet switches.
- **Flow Statistics:** Data collected from each switch showing how many packets/bytes passed through each flow rule.

---

## 🎤 Viva Q&A (Quick Reference)

**Q: What is SDN?**  
A: SDN separates the control plane and data plane, allowing centralized network management via a controller.

**Q: What is Ryu?**  
A: Ryu is an open-source, Python-based SDN controller framework that uses OpenFlow to manage network switches.

**Q: What are flow statistics?**  
A: They are counters maintained by the switch for each flow rule, tracking how many packets and bytes matched that rule.

**Q: Why is traffic monitoring important?**  
A: It helps analyze network performance, detect congestion, identify unusual activity, and debug topology issues.

**Q: What does `pingall` do in Mininet?**  
A: It makes every host ping every other host, verifying full connectivity in the topology.

---

## ✅ Conclusion

This project successfully demonstrates SDN-based traffic monitoring using Mininet and Ryu. The controller collects flow-level statistics (packet count and byte count) from switches every 5 seconds and logs them, providing insight into real-time network behavior.

---

## 📚 References

1. Mininet Overview — https://mininet.org/overview/
2. Mininet Walkthrough — https://mininet.org/walkthrough/
3. Ryu Documentation — https://ryu.readthedocs.io/
4. Mininet GitHub — https://github.com/mininet/mininet/wiki/Documentation

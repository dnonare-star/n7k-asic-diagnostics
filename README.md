# Nexus 7K ASIC Diagnostics & ELAM Triage

###  Overview
This repository is a technical runbook for identifying "silent" packet loss within the Cisco Nexus 7000/7700 switching fabric. Use these commands when standard interface counters show no errors but traffic is failing.

---

##  Scenario 1: The "Silent" ACL Drop
If you suspect an ACL is dropping traffic without logging, use ELAM to "freeze" the packet in the ASIC.

### The Workflow:
1. **Attach to the Ingress Module:**
   ```bash
   attach module <slot_number>
   debug platform internal <asic_type> elam asic 0
Set the L3 Trigger:
This tells the ASIC to wait for a specific packet.

Bash
trigger dbb0 setup v4 src <Source_IP> dst <Dest_IP>
trigger dbb0 start
Analyze the Report:
This shows if the packet was dropped.

Bash
display dbb0 report | inc drop
Note: If v4_acl_drop is 1, the packet was killed by an ACL.

Scenario 2: VLAN Membership Mismatch
Use this if you suspect a VLAN tagging mismatch between the Nexus and an upstream peer.

The Workflow:
Set the L2 Trigger:

Bash
trigger dbb0 setup l2 mactype unicast src-mac <Host_MAC> vlan <VLAN_ID>
trigger dbb0 start
Check for Pruning:

Bash
display dbb0 report | inc vlan
Note: If vlan_membership_check_fail is high, the port is not a member of that VLAN.

 Repository Contents
/scripts: CLI snippets for quick copy-pasting.

/docs: Detailed field notes on M-Series vs. F-Series cards.




### Step 3: Save It
1. Scroll to the bottom of the page.
2. In the "Commit changes" box, type: `Complete README with ELAM triggers`.
3. Click the green **Commit changes** button.

---

### Why this version is better:
* **Code Blocks:** The ` ```bash ` tags create those nice grey boxes that make the commands easy to read.
* **Blockquotes:** The ` > ` creates the "Note" callouts that highlight the "Drop" bits.
* **Separation:** The ` --- ` creates the horizontal lines to separate the two troubleshooting scenarios.

**Once you save this, check your main repository page. Does it look clean and organized now?**

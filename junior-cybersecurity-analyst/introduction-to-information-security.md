# Introduction to Information Security — HTB Academy

> **Path:** Junior Cybersecurity Analyst - CJCA CERTIFICATION PATH  
> **Difficulty:** Fundamental  
> **Category:** Introduction to Information Security  
> **Completed:** 2026-05-31  
> **Module Link:** (https://academy.hackthebox.com/app/module/293)

---

## Summary

Foundational module introducing the structural landscape of Information Security — the domains, principles, processes, roles, and threat categories that define the field. Covers the CIA Triad and its three supporting principles (Non-repudiation, Authentication, Privacy), maps InfoSec across eight operational domains, and walks through the full process lifecycle from risk assessment to continuous improvement. The second half catalogs five major attack categories (DDoS, Ransomware, Social Engineering, Insider Threat, APT) and defines the team structure — Red, Blue, Purple, Penetration Tester, and SOC — that analysts operate within. The vocabulary and frameworks introduced here surface constantly in every subsequent module and in real analyst work.

---

## Key Concepts Learned

- **InfoSec** — safeguarding information and systems from unauthorized access, modification, or destruction; scope covers data, systems, networks, and physical infrastructure
- **Risk** — likelihood × impact that a threat successfully exploits a vulnerability; the umbrella concept encompassing both threats and vulnerabilities
- **Threat** — a potential cause of harm; exploits vulnerabilities to compromise a system
- **Vulnerability** — a weakness in a system that a threat can exploit
- **Confidentiality** — information accessible only to authorized parties; implemented via encryption, access controls
- **Integrity** — accuracy and completeness of data preserved across its lifecycle; implemented via hashing, digital signatures
- **Availability** — information accessible to authorized users when needed; implemented via redundancy, disaster recovery planning
- **Non-repudiation** — a party cannot deny having signed or sent something; digital signatures + audit logs
- **Authentication** — verifying the identity of a user, process, or device before granting access
- **Privacy** — proper handling of personal data; regulatory compliance (GDPR, HIPAA, PCI DSS)
- **OpSec (Operational Security)** — continuous cycle of identifying critical assets, analyzing threats, assessing vulnerabilities, controlling access, and monitoring; includes change management and asset inventory
- **Shared Responsibility Model** — in cloud environments, the provider secures the infrastructure; the customer secures their data and applications within it
- **DDoS (Distributed Denial of Service)** — botnet floods a target to exhaust resources; distinguished from DoS by distributed, multi-source origin
- **Ransomware** — malware that encrypts files and demands payment for decryption; impact extends beyond ransom cost to operational shutdown, data loss, reputational damage
- **Social Engineering** — attacks exploiting human psychology rather than technical vulnerabilities: Phishing, Pretexting, Baiting, Tailgating, Quid Pro Quo
- **Insider Threat** — risk from individuals with authorized access who misuse it; three categories: Malicious, Negligent, Compromised Insiders
- **APT (Advanced Persistent Threat)** — long-term, stealthy intrusion campaign; often nation-state or organized-crime sponsored; goal is sustained access over immediate payoff
- **SOC (Security Operations Center)** — centralized unit that monitors, investigates, and neutralizes threats; tiered structure (Tier 1 triage → Tier 2 investigation → Tier 3 IR/advanced analysis)

---

## Notes & Walkthrough

### Risk, threat, and vulnerability — the three-layer model

The module opens by establishing the relationship between three terms that are used interchangeably in casual conversation but mean distinct things in security work:

```
Vulnerability  →  exploited by a  →  Threat  →  produces a  →  Risk
(the weakness)                       (the actor/event)         (likelihood × impact)
```

A **vulnerability** is a weakness — an unpatched service, a misconfigured permission, an unlocked server room door. A **threat** is what exploits that weakness — a scanner, a phishing email, an unauthorized employee. **Risk** is the resulting calculation: how likely is this threat to exploit this vulnerability, and how bad would it be if it did?

Risk management is the practice of prioritizing which vulnerabilities to remediate based on that calculation. This framing comes up in vulnerability management tools (Tenable, Qualys), threat intelligence reports, and IR playbooks — where triaging which system to isolate first is a real-time risk calculation made under pressure.

### The eight domains

InfoSec is not a single discipline — it is eight overlapping operational areas, each with its own tooling, responsibilities, and threat model:

| Domain | Core concern |
|--------|-------------|
| **Network Security** | Traffic filtering, IDS/IPS, VPNs, access control across the network boundary |
| **Application Security** | Secure SDLC, threat modeling, code review, authentication/authorization in software |
| **Operational Security (OpSec)** | Day-to-day protection of data assets; asset management, change control, access control |
| **Disaster Recovery / Business Continuity** | RTOs, RPOs, restoring operations after a major incident |
| **Cloud Security** | Shared responsibility model, IAM, data encryption in transit and at rest, compliance |
| **Physical Security** | Protecting hardware and facilities from unauthorized physical access |
| **Mobile Security** | Device protection, encryption, MDM, application permissions, VPN |
| **IoT Security** | Securing connected devices, network segmentation, firmware updates, authentication |

For an IR analyst, Network, Cloud, and OpSec domains are the immediate daily context. Physical and IoT become relevant in specific investigation types — insider threat cases with hardware implants, OT/ICS incidents, or executive device compromise.

### The CIA Triad and its extensions

The CIA Triad is the foundational model that every security control, policy, and tool is ultimately trying to protect:

```
Confidentiality — only the right people can see it
Integrity       — it hasn't been changed without authorization
Availability    — it's accessible when the right people need it
```

Three principles extend the triad:

- **Non-repudiation** — the sender cannot later deny sending a message or signing a document. Implemented via digital signatures and audit logs. In IR terms, this is why log integrity matters — a tampered log loses non-repudiation and loses its value as evidence.
- **Authentication** — confirming identity before granting access. Implemented via passwords, MFA, certificates. Failure of authentication is the root cause of a significant percentage of major breaches.
- **Privacy** — proper handling of personal data per regulatory requirements. GDPR, HIPAA, and PCI DSS define legally enforceable privacy obligations. Non-compliance has direct financial consequences: GDPR fines, PCI DSS penalties, loss of accreditation.

### The InfoSec process lifecycle

The module frames InfoSec not as a set of static controls but as a continuous operational cycle:

```
Risk Assessment
      ↓
Security Planning
      ↓
Implement Controls
      ↓
Monitor & Detect      ← SIEM, IDS, EDR
      ↓
Incident Response     ← isolate, eradicate, recover
      ↓
Disaster Recovery
      ↓
Continuous Improvement
      ↑_________________________|
```

The "Continuous Improvement" step feeds back into Risk Assessment as the threat landscape evolves. For an IR analyst, Monitor & Detect and Incident Response are the most operationally immediate steps — but understanding the full cycle matters for explaining why controls exist and what the IR phase is recovering toward.

### The five major attack categories

**DDoS** — A botnet (network of compromised devices — PCs, routers, IoT devices, cloud VMs) floods a target with traffic to exhaust its resources and deny service to legitimate users. Distinguished from a single-source DoS by scale and distribution. Financial impact on e-commerce, banking, and streaming services can be severe; downtime = direct revenue loss.

**Ransomware** — Malware encrypts files and demands payment (typically cryptocurrency) for a decryption key. The financial cost extends well beyond the ransom itself: IR engagement costs, regulatory fines if data was exfiltrated during the attack, reputational damage, and prolonged operational recovery. In healthcare, ransomware has directly endangered patient safety by disabling clinical systems.

**Social Engineering** — Five techniques the module covers:

| Technique | Mechanism |
|-----------|-----------|
| **Phishing** | Deceptive email or message mimicking a legitimate source |
| **Pretexting** | Fabricated scenario to extract information or compel action |
| **Baiting** | Promise of something enticing (free download, found USB drive) to lure a victim |
| **Tailgating** | Following an authorized person through a physical access control point |
| **Quid Pro Quo** | Offer of a benefit (fake tech support, gift) in exchange for credentials or access |

Social engineering bypasses technical controls by targeting humans directly. The reason it works consistently: our defaults are to trust and to help. Security awareness training exists to create conscious friction at high-risk decision points.

**Insider Threat** — Three categories:

- **Malicious Insiders** — intentional harm; data theft, sabotage, fraud; motivated by financial gain, revenge, or external sponsorship
- **Negligent Insiders** — accidental harm; phishing click, misconfiguration, email sent to wrong recipient; statistically the most common category
- **Compromised Insiders** — external attacker operating with stolen insider credentials; hardest to detect because the activity profile looks legitimate

The module introduces the **Insider Threat Kill Chain**: Motivation → Planning → Preparation → Execution → Concealment. Because insiders operate inside the trusted perimeter and know what the detection environment looks like, their activity doesn't trigger standard perimeter-based rules.

**APTs** — Long-term campaigns with five distinct stages:

```
Reconnaissance → Initial Infiltration → Establish Foothold → Lateral Movement → Maintain Persistence
```

APTs are distinguished from commodity attacks by duration (months to years), sophistication (custom tooling, zero-days, living-off-the-land techniques), and objective — sustained intelligence collection or strategic disruption rather than immediate financial gain. Nation-state actors and well-funded organized crime groups are the typical sponsors. The MITRE ATT&CK framework is structured as a catalog of the techniques APT actors use at each stage.

### Team structure

| Role | Function |
|------|---------|
| **Red Team** | Simulates real adversaries; tests technical, human, and physical defenses comprehensively |
| **Blue Team** | Defends; Security Analysts, Incident Responders, Threat Hunters, Security Engineers, SOC operators |
| **Purple Team** | Red + Blue in tandem; Red shares attack insights to improve Blue detection; Blue feedback sharpens Red simulation realism |
| **Penetration Tester** | Authorized simulated attacker; identifies and ethically exploits vulnerabilities; reports findings with remediation recommendations |
| **SOC** | Centralized monitoring and response hub; tiered structure |

SOC tiers:
- **Tier 1** — Alert triage, initial investigation, basic threat analysis; handles the volume
- **Tier 2** — Complex incidents, deeper investigation, mentors Tier 1
- **Tier 3** — Most critical issues, advanced threat analysis; often serves as Incident Responder on active cases

---

## Tools Used

This is a conceptual module with no hands-on lab component. Tools below are directly referenced in the module or are the standard tooling for the domains covered:

| Tool / Technology | Category | Purpose |
|---|---|---|
| **SIEM** (Splunk, Microsoft Sentinel, IBM QRadar) | Detection & Monitoring | Collect, correlate, and alert on security events across the environment |
| **IDS/IPS** (Snort, Suricata) | Network Security | Detect and/or block suspicious traffic patterns at the network level |
| **EDR** (CrowdStrike, SentinelOne, Microsoft Defender for Endpoint) | Endpoint Security | Monitor endpoints for anomalous process, file, and network activity |
| **Firewalls** | Network Security | Filter traffic between trust zones based on defined rules |
| **VPN** | Network Security | Encrypted tunnels for remote access over untrusted networks |
| **Nmap** | Reconnaissance | Network scanning, host discovery, service fingerprinting |
| **Wireshark** | Network Analysis | Packet capture and protocol analysis |
| **Metasploit** | Exploitation Framework | Red Team exploitation and post-exploitation |
| **Burp Suite** | Web App Security | HTTP proxy for web application vulnerability testing |
| **John the Ripper** | Credential Analysis | Password hash cracking |

---

## Takeaways

- **The CIA Triad is the impact assessment framework for every IR.** When a SOC analyst investigates an alert or an IR analyst scopes an incident, the three questions are: was confidentiality compromised (data exfiltration)? Was integrity compromised (data modification, log tampering)? Was availability impacted (service degradation, ransomware)? Every IR report's impact section is organized around these three columns. The CIA Triad isn't academic — it's the template.
- **Risk = likelihood × impact. This drives triage at every level.** Vulnerability management patch prioritization, SOC alert queue ordering, incident severity classification, IR escalation decisions — all reduce to the same calculation. Understanding that risk combines both factors (not just severity, not just exposure) is what separates an analyst who can explain their decisions from one who just follows a playbook.
- **Phishing is the default initial access hypothesis until evidence says otherwise.** The module covers social engineering before technical attacks for a reason. In IR investigations, the default working hypothesis for ransomware, Business Email Compromise, and insider-threat-adjacent cases is phishing or pretexting. Email header analysis, attachment sandbox detonation, and user interview notes are the first evidence sources accordingly. This assumption should be stated explicitly in the investigation plan and then systematically ruled in or out.
- **Insider threat detection requires behavioral baselines, not perimeter rules.** All three insider categories (malicious, negligent, compromised) look like normal user activity against standard perimeter controls. What exposes them: anomalous access times, access to systems outside the user's normal scope, large data exports triggering DLP alerts, privilege escalation events, and lateral movement from user accounts with no operational reason to move laterally. UEBA tools in modern SIEMs (Splunk UBA, Microsoft Sentinel Behavioral Analytics) are specifically built to establish baselines and flag deviations. Coming from IAM delivery work — every over-provisioned account is an insider threat risk sitting in the access inventory.
- **The Insider Threat Kill Chain is a detection guide in reverse.** Each stage produces observable indicators: unusual after-hours access during Planning, bulk file access or data staging during Preparation, large exfiltration transfers during Execution, audit log deletion attempts during Concealment. The kill chain model tells the analyst exactly which log sources to query at each stage of a suspected insider case.
- **APT stages map directly to MITRE ATT&CK.** Reconnaissance → Infiltration → Foothold → Lateral Movement → Persistence is the skeleton ATT&CK is built on. Identifying an APT campaign means correlating this multi-stage pattern across log sources and artifacts — not reacting to isolated alerts. This is why threat hunting and alert correlation skills matter as much as triage skills in senior Blue Team roles, and why CDSA training emphasizes multi-source investigation over single-event response.
- **Non-repudiation is why log integrity is non-negotiable in IR.** If an attacker can delete or modify event logs, the evidentiary value of those logs is destroyed. Log forwarding to a SIEM (logs exist outside the compromised host), write-once log storage, and log integrity monitoring all preserve non-repudiation under adversarial conditions. Tampered or missing logs are also themselves forensic artifacts — their absence tells a story about attacker intent and capability.
- **The shared responsibility model determines what you can pull in a cloud IR.** Whether an incident occurs in IaaS (you manage the OS up), PaaS (provider manages the platform), or SaaS (provider manages almost everything), the shared responsibility boundary immediately tells you: which logs exist, who controls them, and whether you open a provider support ticket or pull them yourself. Missing this distinction costs hours in a live cloud IR.
- **Social engineering is a control bypass, not a gap in the perimeter.** The most sophisticated firewall, EDR, and SIEM stack can be completely bypassed by one convincing phishing link. Security awareness training is a control layer — not a soft add-on. For IR analysts, every phishing-originated incident involves a human decision point that needs to be documented and fed back into training as part of formal lessons learned.

---

## References

- [HTB Academy — Cyber Security Fundamentals Module](https://academy.hackthebox.com/module/details/18)
- [MITRE ATT&CK Framework](https://attack.mitre.org/)
- [MITRE ATT&CK — Phishing (T1566)](https://attack.mitre.org/techniques/T1566/)
- [MITRE ATT&CK — Valid Accounts (T1078)](https://attack.mitre.org/techniques/T1078/) — compromised insider / credential theft scenario
- [MITRE ATT&CK — Network Denial of Service (T1498)](https://attack.mitre.org/techniques/T1498/)
- [MITRE ATT&CK — Data Encrypted for Impact (T1486)](https://attack.mitre.org/techniques/T1486/) — ransomware
- [MITRE ATT&CK — Lateral Movement (TA0008)](https://attack.mitre.org/tactics/TA0008/) — APT kill chain stage
- [MITRE ATT&CK — Persistence (TA0003)](https://attack.mitre.org/tactics/TA0003/) — APT kill chain stage
- [NIST Cybersecurity Framework](https://www.nist.gov/cyberframework) — Identify, Protect, Detect, Respond, Recover maps directly to the InfoSec process lifecycle described in this module
- [CISA — Insider Threat Mitigation Resources](https://www.cisa.gov/insider-threat-mitigation)

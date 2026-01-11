# GitHub-1.35-Tbps-DDoS-Attack-2018
Analyzing and Designing Mitigation Strategies for the 2018 GitHub DDoS Attack. This is a study case purpose project.

# About Me 
Information Systems graduate, currently transitioning to Cyber Security. Previously served as a Technician at Oil and Gas Industry (Pertamina Patra Niaga vendor), where I developed a highly disciplined approach to managing critical infrastructure compliance and 24/7 reliability. Currently formalizing expertise through an intensive cybersecurity bootcamp at Dibimbing, I am eager to apply this technical foundation and structured approach to a security operations role.

# 1. Background
A Distributed Denial of Service (DDoS) attack is a sophisticated cyber threat designed to compromise the Availability of a target system. Unlike a standard Denial of Service (DoS) attack that originates from a single source, a DDoS attack utilizes a Botnet—a vast network of compromised computers controlled by an attacker without the owners' knowledge. This distributed nature makes mitigation exceptionally difficult, as it is challenging to distinguish between legitimate user traffic and malicious requests from thousands of different IP addresses. 
<br>
<br>
GitHub is the world’s leading web-based platform for version control and collaboration, utilized by millions of developers to manage source code. By leveraging the Git system, it allows teams to store, track, and share code efficiently. Because GitHub is central to the global software development ecosystem supporting both remote corporate teams and open-source projects any downtime has a significant impact on global productivity.
<br>
<br>
On Wednesday, February 28, 2018, GitHub was hit by a massive DDoS attack that peaked at 1.35 Terabits per second (Tbps), marking it as one of the largest DDoS incidents in history.
- Attack Vector: The attackers employed a technique known as "Memcached Amplification." This method exploits misconfigured Memcached servers high speed memory caching systems to multiply the volume of traffic sent to the target.
- Duration & Impact: The attack disrupted GitHub's availability between 17:21 and 17:30 UTC.
- Security Scope: The incident was a pure Availability attack. There was no evidence of unauthorized access to data (Confidentiality) or unauthorized modification of code (Integrity).

# 2. Attack Timeline
<p align="center">
  <img width="500" height="287" alt="image" src="https://github.com/user-attachments/assets/3619d62c-e7ca-42b5-949e-71b133878829" />
  <br>
</p>
Between 17:21 and 17:30 UTC, GitHub's infrastructure detected a massive surge in inbound traffic. The attack originated from thousands of distinct Autonomous Systems (AS) across tens of thousands of endpoints. This was identified as a Memcached Amplification Attack, reaching a record-breaking peak of 1.35 Terabits per second (Tbps) and a throughput of 126.9 million packets per second (Mpps). Incident Response Timeline:

- 7:21 UTC – Anomaly Detection: GitHub’s internal monitoring systems identified a severe imbalance between inbound and outbound traffic. Automated alerts were instantly triggered, notifying engineers via internal ChatOps channels.
- 17:26 UTC – BGP Traffic Rerouting: As transit bandwidth exceeded 100 Gbps, the engineering team initiated emergency protocols. Using GitHub's internal ChatOps tools, they executed a command to withdraw BGP (Border Gateway Protocol) announcements. This forced all incoming traffic to be rerouted through Akamai (AS36459), utilizing their global edge network as a massive "scrubbing" buffer.
- 17:30 UTC – Stabilization & Recovery: Within minutes, BGP routes stabilized. Access Control Lists (ACLs) at the network edge successfully dropped malicious packets while maintaining legitimate user access. Monitoring tools showed a full recovery of load balancer response codes.
- 17:34 UTC – Internet Exchange Withdrawal: As a precautionary security measure, GitHub withdrew its routes from several Internet Exchanges (IX) to divert an additional 40 Gbps of suspicious traffic away from their direct infrastructure.
- 18:00 UTC – Secondary Peak: A second surge of 400 Gbps was detected and successfully mitigated by Akamai’s infrastructure without further service disruption.

The following graph above (source: Akamai) illustrates the extreme inbound traffic spike that reached the limits of the network capacity, followed by the immediate neutralization once the BGP rerouting took effect.

# 3. Impact and Damages
The 1.35 Tbps DDoS attack had a direct and immediate impact on GitHub’s infrastructure and its global user base:

- Network Saturation: Before the traffic was successfully rerouted, GitHub’s internal network was flooded with traffic exceeding 100 Gbps, leading to total saturation and service downtime.
- Global Developer Productivity Loss: As the primary repository for source code and hosting for millions of projects, the downtime halted the workflow of developers worldwide. This disruption affected code deployments, CI/CD pipelines, and collaborative software development.
- Reputational Risk: Despite the brief duration of the attack, the incident served as a reminder that even world class platforms are not immune to massive scale attacks. Such events can potentially weaken user confidence in a platform's resilience.

From a security perspective, the damage was localized to a single pillar of the CIA Triad:

- Availability: High Impact. The core objective of the attack was successful in making the service inaccessible to its users for approximately 9 minutes.
- Confidentiality: No Impact. GitHub confirmed that no unauthorized access to private repositories or user data occurred.
- Integrity: No Impact. The integrity of the source code stored on the platform remained intact, no malicious code was injected or modified during the surge.

The financial and strategic losses associated with this incident include:

- Operational Costs (Third-Party Services): To mitigate the attack, GitHub had to activate emergency services from Akamai, a specialist in Content Delivery Networks (CDN) and DDoS protection. Engaging such high-tier mitigation services on demand incurs significant operational costs.
- Resource Investment: The incident triggered a comprehensive review of GitHub's Risk Assessment and security posture. This resulted in a long-term requirement for increased investment in infrastructure resilience, automated mitigation tools, and specialized security personnel.
- Opportunity Cost: Engineering resources were diverted from product development to emergency incident response and post mortem analysis.

# 4. Mitigation & Responses
Following the detection of inbound traffic exceeding 100 Gbps at one of GitHub's facilities, the engineering team executed an emergency diversion. The decision was made to route all global traffic through Akamai (AS36459), a third-party DDoS mitigation specialist. In the aftermath of the attack, GitHub committed to a long-term roadmap to bolster its infrastructure resilience. The focus shifted from manual intervention to automated intelligence:

- Strengthening Edge Infrastructure: GitHub initiated plans to expand its own edge network to handle modern and future internet scale threats, reducing reliance on third party providers for medium-scale incidents.
- Automation & Reducing Human Intervention: A key takeaway was the need for automated intervention systems. GitHub is researching advanced monitoring infrastructures that can automatically trigger DDoS protection services without waiting for manual engineer approval.
- Optimization of MTTR (Mean Time To Recovery): By automating detection and mitigation, GitHub aims to significantly lower its MTTR. Every second saved in response time prevents massive productivity losses for millions of developers.
- Proactive Threat Hunting: GitHub is investing in identifying and neutralizing new attack vectors—similar to Memcached amplification before they can impact service availability. This involves continuous analysis of global traffic patterns and emerging exploitation techniques.

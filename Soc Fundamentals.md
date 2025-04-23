# Soc Fundamentals
![image](https://github.com/user-attachments/assets/3113517a-a2c6-4935-a1c8-78b03b549ccb)

- SOC (Security Operations Center) is a dedicated facility operated by a specialized security team. This team aims to continuously monitor an organization’s network and resources and identify suspicious activity to prevent damage. This team works 24 hours a day, seven days a week.
- The main focus of the SOC team is to keep Detection and Response intact. The SOC team has some resources available in the form of security solutions that help them achieve this. These solutions integrate the whole company’s network and all the systems to monitor them from one centralized location. Continuous monitoring is required to detect and respond to any security incident.
![image](https://github.com/user-attachments/assets/0ed298da-5609-4391-9fb1-2fd5d0d7e9eb)

# People
Regardless of the evolution of automating the majority of security tasks, the People in a SOC will always be important. A security solution can generate numerous red flags in a SOC environment, which can cause huge noise.

Imagine you are part of a fire brigade team and have centralized software where all the city’s fire alarms are integrated. Suppose you get many fire notifications at once, all for different places. When you get into those locations, your team finds out most of those were only triggered by excessive smoke from cooking. Eventually, all the efforts will be a waste of time and resources.

In a SOC, with security solutions in place without human intervention, you'll end up focusing on more irrelevant issues. There are always the People who help the security solution to identify truly harmful activities and enable a prompt response.

The People are known as the SOC team. This team has the following roles and responsibilities.

The hierarchy of SOC team.


    SOC Analyst (Level 1): Anything detected by the security solution would pass through these analysts first. These are the first responders to any detection. SOC Level 1 Analysts perform basic alert triage to determine if a specific detection is harmful. They also report these detections through proper channels.
    SOC Analyst (Level 2): While Level 1 does the first-level analysis, some detections may require deeper investigation. Level 2 Analysts help them dive deeper into the investigations and correlate the data from multiple data sources to perform a proper analysis.
    SOC Analyst (Level 3): Level 3 Analysts are experienced professionals who proactively look for any threat indicators and support in the incident response activities. The critical severity detection reported by Level 1 and Level 2 Analysts are often security incidents that need detailed responses, including containment, eradication, and recovery. This is where Level 3 analysts’ experience comes in handy.
    Security Engineer: All analysts work on security solutions. These solutions need deployment and configuration. Security Engineers deploy and configure these security solutions to ensure their smooth operation.
    Detection Engineer: Security rules are the logic built behind security solutions to detect harmful activities. Level 2 and 3 Analysts often create these rules, while the SOC team can sometimes also utilize the detection engineer role independently for this responsibility.
    SOC Manager: The SOC Manager manages the processes the SOC team follows and provides support. The SOC Manager also remains in contact with the organization’s CISO (Chief Information Security Officer) to provide him with updates on the SOC team’s current security posture and efforts.
![image](https://github.com/user-attachments/assets/56ed16e5-5539-4342-9900-cd0ec084e42a)

# Process
Alert Triage

The alert triage is the basis of the SOC team. The first response to any alert is to perform the triage. The triage is focused on analyzing the specific alert. This determines the severity of the alert and helps us prioritize it. The alert triage is all about answering the 5 Ws. What are these 5 Ws?

The 5 Ws of SOC.

Following are some questions that need to be answered during the triage of an alert. 
```
Alert: Malware detected on Host: GEORGE PC
5 Ws 	Answers
What? 	A malicious file was detected on one of the hosts inside the organization’s network.
When? 	The file was detected at 13:20 on June 5, 2024.
Where? 	The file was detected in the directory of the host: "GEORGE PC".
Who? 	The file was detected for the user George.
Why? 	After the investigation, it was found that the file was downloaded from a pirated software-selling website. The investigation with the user revealed that they downloaded the file as they wanted to use a software for free.
```
Reporting

The detected harmful alerts need to be escalated to higher-level analysts for a timely response and resolution. These alerts are escalated as tickets and assigned to the relevant people. The report should discuss all the 5 Ws along with a thorough analysis, and screenshots should be used as evidence of the activity.
Incident Response and Forensics

Sometimes, the reported detections point to highly malicious activities that are critical. In these scenarios, high-level teams initiate an incident response. The incident response process is discussed in detail in the Incident Response room. A few times, a detailed forensics activity also needs to be performed. This forensic activity aims to determine the incident’s root cause by analyzing the artifacts from a system or network.

# Technology

Having the right People and Processes in place would never be enough without security solutions for detection and response. The Technology portion in the SOC pillars refers to the security solutions. These security solutions efficiently minimize the SOC team's manual effort to detect and respond to threats.

An organization’s network consists of many devices and applications. As a security team, individually detecting and responding to threats in each device or application would require significant effort and resources. Security solutions centralize all the information of the devices or applications present in the network and automate the detection and response capabilities.

Let's get a brief understanding of some of these security solutions:

    SIEM: Security Information and Event Management (SIEM) is a popular tool used in almost every SOC environment. This tool collects logs from various network devices, referred to as log sources. Detection rules are configured in the SIEM solution, which contains logic to identify suspicious activity. The SIEM solution provides us with the detections after correlating them with multiple log sources and alerts us in case of a match with any of the rules. Modern SIEM solutions surpass this rule based detection analysis, providing us with user behavior analytics and threat intelligence capability. Machine learning algorithms support this to enhance the detection capabilities.

Note: The SIEM solution only provides the Detection capabilities in a SOC environment.

    EDR: Endpoint Detection and Response (EDR) provides the SOC team with detailed real-time and historical visibility of the devices’ activities. It operates on the endpoint level and can carry out automated responses. EDR has extensive detection capabilities for endpoints, allowing you to investigate them in detail and respond with a few clicks.
    Firewall: A firewall functions purely for network security and acts as a barrier between your internal and external networks (such as the Internet). It monitors incoming and outgoing network traffic and filters any unauthorized traffic. The firewall also has some detection rules deployed, which help us identify and block suspicious traffic before it reaches the internal network.

Several other security solutions play unique roles in a SOC environment, such as Antivirus, EPP, IDS/IPS, XDR, SOAR, and more. The decision on what Technology to deploy in the SOC comes after careful consideration of the threat surface and the available resources in the organization.

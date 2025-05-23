# IDS fundatmentals
![image](https://github.com/user-attachments/assets/bcdb369c-eb12-4e79-90ac-35959596249c)

Intrusion Detection System (IDS)

Host Intrusion Detection System (HIDS): 
- Host-based IDS solutions are installed individually on the hosts and are responsible for only detecting potential security threats associated with that particular host. They provide detailed visibility of the host’s activities. However, host intrusion detection systems can be challenging to manage in large networks as they are resource-intensive and require management on each host.

Network Intrusion Detection System (NIDS): 
- Network-based IDS solutions are crucial in detecting potentially malicious activities within the whole network, regardless of any specific hosts. They monitor the network traffic of all the hosts involved to detect suspicious activities. It provides a centralized view of all the detections inside the whole network.
![image](https://github.com/user-attachments/assets/800d93c4-abcb-42e5-8139-f2a7167a34b2)

## Detection Modes
Signature-Based IDS:
- Many attacks occur every day. Each attack has its unique pattern, which is known as a signature. These signatures are preserved by the IDS in their databases so that if the same attack happens in the future, it gets detected by its signature and reported to the security administrators for action. The stronger the signature database of the IDS is, the more efficiently it would detect known threats. However, the signature-based IDS is unable to detect zero-day attacks. Zero-day attacks have no prior signatures (patterns) and are not saved inside the IDS databases. Therefore, the signature-based IDS can only detect the attacks that happened previously, and its signatures (patterns) are saved inside the database. In the upcoming tasks, we will explore a signature-based IDS named Snort.

Anomaly-Based IDS: 
- This type of IDS first learns the normal behavior (baseline) of the network or system and performs detections if there is any deviation from the normal behavior. Anomaly-based IDS can also detect zero-day attacks because they don’t rely on the available signatures for the detections but detect abnormalities inside the network or system by comparing the current state with the normal behavior (baseline). However, this type of IDS may generate a lot of false positives (marking benign activities as malicious) because the nature of most legitimate programs matches the malicious ones. Anomaly-based IDS would mark them malicious and believe anything behaving unusually is malicious. We can also reduce the false positives generated by anomaly-based IDS by fine-tuning it (manually defining the normal behavior in the IDS).

Hybrid IDS:
- A hybrid IDS combines the detection methods of signature-based IDS and anomaly-based IDS to leverage the strengths of each approach. Some known threats may already have some signatures in the IDS database; in this case, the hybrid IDS would use the detection technique of the signature-based IDS. If it encounters a new threat, it can leverage the detection method of anomaly-based IDS.

# Snort
- Snort is one of the most widely used open-source IDS solutions developed in 1998. It uses signature-based and anomaly-based detections to identify known threats. These are defined in the rule files of the Snort tool. Several built-in rule files come pre-installed in this tool’s package. These built-in rule files contain a variety of known attack patterns. Snort’s built-in rules can detect a lot of malicious traffic for you. However, you can configure Snort to detect specific types of network traffic that are not covered by the default rule files. You can create custom rules based on your requirements to detect specific traffic. You can also disable any built-in detection rules if they don’t point to harmful traffic for your system or network and define some custom rules instead. In the upcoming task, we will explore the built-in rules and make custom rules to detect specific traffic.
![image](https://github.com/user-attachments/assets/b6c1b00b-fa16-49e5-80aa-d3ded4d0b032)
![image](https://github.com/user-attachments/assets/634deabc-4fb4-4f5b-af81-1b1e7830f7b3)


![image](https://github.com/user-attachments/assets/d3488dad-20e4-46e7-8c71-42bb1489af8b)

## Running Snort on a pcap
    sudo snort -q -l /var/log/snort -r Task.pcap -A console -c /etc/snort/snort.conf




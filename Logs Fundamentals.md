# Logs Fundamentals
![image](https://github.com/user-attachments/assets/39aee069-ce52-42cf-b00f-c7b03fcf5b1a)

- Attackers are clever. They avoid leaving maximum traces on the victim’s side to avoid detection. Yet, the security team successfully determines how the attack was executed and is even sometimes successful in finding who was behind the attack.

- Suppose a few policemen are investigating the disappearance of a precious locket in a snowy jungle cabin. They observed that the wooden door of the cabin was brutally damaged, and the ceiling collapsed. There were some footprints on the snowy path to that cabin. Lastly, they discovered some CCTV footage from a neighbouring residence. By placing together all these traces, the police successfully determined who was behind the attack. Various traces are found in several such cases; putting all these together takes you closer to the criminal.
- There are various places inside a system where the traces of an attack could be fetched. The logs contain most of these traces. Logs are the digital footprints left behind by any activity. The activity could be a normal one or the one with malicious intent. Tracing down the activity and the individual behind the execution of that activity becomes easier through logs.
![image](https://github.com/user-attachments/assets/a5af7fd9-d5fa-43f1-8187-d0a3381dbc40)

![image](https://github.com/user-attachments/assets/c40143d8-cf51-4706-ab7c-9db21e08fce7)

# Windows Event View
![image](https://github.com/user-attachments/assets/7ed1fdd0-a4d7-40bd-9046-2abd57887fa1)
![image](https://github.com/user-attachments/assets/f8d8ecda-dbe5-47eb-b85c-ba0f7113c817)

# Log analysis on linux
```
root@kali$ cat access.log
172.16.0.1 - - [06/Jun/2024:13:58:44] "GET /products HTTP/1.1" 404 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
10.0.0.1 - - [06/Jun/2024:13:57:44] "GET / HTTP/1.1" 404 "-" "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_12_3) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
192.168.1.1 - - [06/Jun/2024:13:56:44] "GET /about HTTP/1.1" 500 "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/58.0.3029.110 Safari/537.36"
```
- Can use grep to find specific info
- ```grep "192.168.1.1 access.log```
- ```less access.log```
- 




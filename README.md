<h1>Analyzing Network Layer Communication with tcpdump</h1>

<h2>Description</h2>
In this project, I will analyze DNS and ICMP traffic in transit using data from a network protocol analyzer tool (tcpdump) and write a follow-up incident report. I will include a brief summary of the tcpdump log analysis and identify which network protocols were used for the network traffic and assess which network protocol is producing the error message. I will also provide details about what was indicated in the log and interpret the issues found in the log. After inspecting the traffic log and identifying trends in the traffic, I will describe why the error messages appeared on the log and identify which services were impacted by this cybersecurity incident. Next, I will explan my analysis of the data and provide a solution to implement. I will provide the scenario, events, and symptoms identified when the event was first reported, explain the current status of the issue, and describe the information discovered while investigating the issue up to this point. Lastly, I will list the next steps in troubleshooting and resolving the issue and provide the suspected root cause of the problem.

<h2>Scenario</h2>
I am a cybersecurity analyst working at a company that specializes in providing IT services for clients. Several customers of clients reported that they were not able to access the client company website yummyrecipesforme.com, and saw the error “destination port unreachable” after waiting for the page to load. 
<br />
<br />
I am tasked with analyzing the situation and determining which network protocol was affected during this incident. To start, I attempt to visit the website and receive the error “destination port unreachable.” To troubleshoot the issue, I load my network analyzer tool, tcpdump, and attempt to load the webpage again. To load the webpage, my browser sends a query to a DNS server via the UDP protocol to retrieve the IP address for the website's domain name; this is part of the DNS protocol. My browser then uses this IP address as the destination IP for sending an HTTPS request to the webserver to display the webpage. The analyzer shows that when I send UDP packets to the DNS server, I receive ICMP packets containing the error message: “udp port 53 unreachable.” 
<br />
<br />
<p align="center">
<img src="https://i.imgur.com/Y40na3p.png" height="70%" width="70%" alt="ICMP Packets"/> </p>

<h2>Cybersecurity Incident Report: Network Traffic Analysis</h2>

<h3>Summary</h3>
As part of the DNS protocol, the UDP protocol was used to contact the DNS server to retrieve the IP address for the domain name of yummyrecipesforme.com. The ICMP protocol was used to respond with an error message, indicating issues contacting the DNS server. 
The UDP message is shown in the first two lines of every log event, which displays the initial outgoing UDP request from my source computer's browser (192.51.100.15) to the DNS server (203.0.113.2.domain) requesting the IP address of yummyrecipesforme.com, which is sent in a UDP packet. The ICMP error response from the DNS server to my browser is displayed in the third and fourth lines of every log event with the error message, “udp port 53 unreachable,” indicating that the UDP packet was undeliverable to port 53 of the DNS server. Since port 53 is associated with DNS protocol traffic, we know this is an issue with the DNS server. Issues with performing the DNS protocol are further evident because the plus sign after the query identification number 35084 indicates flags with the UDP message and the “A?” symbol indicates flags with performing DNS protocol operations.
Due to the ICMP error response message about port 53, it is highly likely that the DNS server is not responding. The flags associated with the outgoing UDP message and domain name retrieval further support this assumption.

<h3>Explanation</h3>
The incident occurred today at 1:24 p.m. This info was obtained from the log file date and time stamps, which display 13:24:32.192571. Customers notified the organization that they received the message “destination port unreachable” when they attempted to visit the website yummyrecipesforme.com. The cybersecurity team providing IT services to their client organization is currently investigating the issue so customers can access the website again. In our investigation into the issue, we conducted packet sniffing tests using tcpdump. In the resulting log file, we found that DNS port 53 was unreachable. The next step is to identify whether the DNS server is down or traffic to port 53 is blocked by the firewall. This consists of troubleshooting to determine if the DNS server is not functioning properly by contacting the DNS server administrator to check for signs of an attack. If the DNS server is fine, the team should check the firewall settings to see if someone changed the configuration to block network traffic on port 53. The DNS server might be down due to a successful Denial of Service (DoS) attack or a misconfiguration. It is possible that an attacker disabled the DNS server with a DoS attack. Alternatively, someone from my team could have made a configuration change on the firewall that blocked port 53.

<h1>Ethical Hacking: From Recon to Attack</h1>


<h2>Description</h2>

In this lab you practice the full path an ethical hacker takes, from information-gathering to a basic attack simulation, using a safe test environment. You’ll use recon-ng to collect public info about a target (add the domain, find subdomains/hosts, do reverse lookups) and export
simple HTML reports of what you found. Next, you add ownership details with WHOISand save another report. With likely targets identified, you switch to Nmap to see what’s actually online and which services are open, running focused scans and saving the results. The goal is to learn a repeatable process: set up, gather data, verify with scans, and document everything clearly.

<br />

<h2>Languages and Utilities Used</h2>

- Bash (shell) — run commands, save outputs, automate small steps.
- Markdown/HTML (reports) — export and share recon findings (from recon-ng) in an easy format.
- recon-ng — main recon framework (workspaces, add domain, find subdomains/hosts, reverse DNS, export HTML reports).
= WHOIS — look up domain ownership and contact info.
- Nmap — host discovery and service/version scans (save results for documentation).
- dns tools — nslookup or dig for quick DNS checks.
- Common CLI helpers — cat, less, grep, awk, sed, tee (view/filter logs and results).

<h2>Environments Used </h2>

- Kali Linux (VM or machine) — main workstation to run the tools.
- Virtualization — VirtualBox/VMware (any equivalent) to isolate the lab.
- Networking — NAT/Bridged network so Kali can reach the internet (for OSINT) and the authorized target.
- Target scope — only systems/assets you have explicit permission to test (class/lab targets).


<h2>Step-by-Step Guide:</h2>

1.	On Kali Linux Terminal, switch to the Root User Account. Run the following command:

sudo su


<img src="https://i.imgur.com/7aptSMp.png"/>
<br />
<br />
2.	Install Recon-NG Framework. Run the following command in the terminal as the root user:

apt install recon-ng -y

<br/>
<img src="https://i.imgur.com/ImU8EpA.png"/>
<br />
<br />
3.	Launch the recon-ng tool, run:

recon-ng

<br/>
<img src="https://i.imgur.com/F1vAvCq.png"/>
<br />
<br />
4.	Create a workspace, run:

workspaces create example_lab

<br/>
<img src="https://i.imgur.com/sbVFXMU.png"/>
<br />
<br />
5.	Install all marketplace modules:

<br/>
<img src="https://i.imgur.com/MaMYSRy.png"/>
<br />
<br />
6.	Insert a single domain: 

db insert domains 

microsoft.com
<br/>
<img src="https://i.imgur.com/jfPLOvj.png"/>

Note: db insert domains is a Recon-NG database command that adds domain names into the current workspace’s database so modules can operate on them. It inserts one or more domain names into Recon-NG’s domains table for the current workspace. Once inserted you can run modules that read targets from the DB (enumeration, WHOIS, DNS, etc.).
<br />
<br />
<br />
7.	Verify Inserted Domain in the Database, run:

show domains

<br/>
<img src="https://i.imgur.com/ic1KqCR.png"/>
<br />
<br />
8.	Run the Recon-NG Brute Hosts Module using the following commands:

modules load recon/domains-hosts/brute_hosts 

run

<br/>
<img src="https://i.imgur.com/Wa9vyh0.png"/>
<br />
<br />
9.	DNS Mapping — Convert Discovered Hostnames to IPs with reverse_resolve, run:

modules load recon/hosts-hosts/reverse_resolve 

run

<br />
<br />
10.	Display All Discovered Hosts After DNS Reverse Resolution, run:

show hosts

<br/>
<img src="https://i.imgur.com/OKcCu1M.png"/>
<br />
<br />
11.	Generate an HTML Recon Report with Recon-NG. Load the HTML Reporting Module, run:

modules load reporting/html

<br/>
<img src="https://i.imgur.com/EIUhjG6.png"/>
<br />
<br />
12.	Set Output File and Metadata (Filename, Creator, and Customer), then run:

options set FILENAME /home/matheus/Documents/results.html 

options set CREATOR Matheus

options set CUSTOMER Microsoft 

run

<br/>
<img src="https://i.imgur.com/U6UFQDG.png"/>
<br />
<br />
13.	Navigate to the folder where the results.html file was created and open it with a browser:

<br/>
<img src="https://i.imgur.com/6azQjPt.png"/>
<img src="https://i.imgur.com/h4kOGyD.png"/>
<img src="https://i.imgur.com/CBAqVvb.png"/>
<img src="https://i.imgur.com/5xLwOiy.png"/>
<br />
<br />
14.	Load the WHOIS POC Module to collect domain contact information, run:

modules load recon/domains-contacts/whois_pocs

<br/>
<img src="https://i.imgur.com/A111lnx.png"/>
<br />
<br />
15.	Configure WHOIS Module to analyze domain contacts for facebook.com, run:

options set SOURCE facebook.com 

run

<br/>
<img src="https://i.imgur.com/PbXZWoC.png"/>
<br />
<br />
16.	Load the HTML Reporting Module to generate a WHOIS recon report, run:

modules load reporting/html

options set FILENAME /home/matheus/Documents/facebook.html 

options set CUSTOMER Facebook

run

<br/>
<img src="https://i.imgur.com/ZQPtjjF.png"/>
<br />
<br />
17.	Navigate to the folder where the facebook.html file was created and open it with a browser:

<br/>
<img src="https://i.imgur.com/1qOuMaU.png"/>
<img src="https://i.imgur.com/WJKNVe6.png"/>

Note: The report now has a “Contacts” tab.
<br />
<br />
18.	Open the Hosts tab and choose one of the IPs to perform the nmap scan:

<br />
<img src="https://i.imgur.com/Oyj0jZZ.png"/>
<br />
<br />
19.	Navigate back to the Kali Linux Terminal and run:

back 

back

<br />
<img src="https://i.imgur.com/EBXHPDY.png"/>
<br />
<br />
20.	Run Nmap host discovery on target IP (131.107.97.20):

nmap -sn 131.107.97.20

<br />
<img src="https://i.imgur.com/avFsijp.png"/>
<br />
<br />
21.	Run an detailed Nmap Scan with maximum verbosity (-vvv) and slow speed (-T1):

nmap -T1 -vvv 131.107.97.20

<br/>
<img src="https://i.imgur.com/7lqD6Lr.png"/>

Note: This scan is really slow, it might take from 30 to 90 minutes.
<br />
<br />
22.	Scan only port 80, run:
 
nmap -T1 -vvv -p 80 131.107.97.20

<br/>
<img src="https://i.imgur.com/kStvTTs.png"/>
<br />
<br />
23.	Perform a detailed Service and Operation System detection scan on port 80: 

nmap -T1 -vvv -p 89 -sV -O 131.107.97.20

<br/>
<img src="https://i.imgur.com/cwNl7u2.png"/>
<br />
<br />
24.	Generate and save a Nmap Service Scan Report to an HTML File, run:

nmap -T1 -vvv -p 80 -sV -oN /home/matheus/Documents/nmap_report.html 131.107.97.20

<br/>
<img src="https://i.imgur.com/BiXykKw.png"/>
<br />
<br />
25.	Display Nmap scan report in the terminal, run:

cat /home/matheus/Documents/nmap_report.html

<br/>
<img src="https://i.imgur.com/PNNfhHA.png"/>
<br />
<br />

Conclusion:

This project took you through the full, ethical workflow—recon → verify → document—in a safe lab. You used recon-ng to gather public information, WHOIS to add ownership context, and Nmap to confirm what’s really online and which services are exposed. Most importantly, you practiced turning raw results into clear, repeatable evidence (saved outputs and simple HTML reports), so another analyst could follow your steps and reach the same conclusions. The outcome is a lightweight, reliable playbook you can reuse: start wide with OSINT, narrow to real targets, validate with focused scans, and record everything—within an approved scope and with permission.



<br /></p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>

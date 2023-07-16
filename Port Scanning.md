## Scanning with Nmap

pwoerfull scan don't use it in passive scan

```bash
nmap -sC -sV -oN nmap -A -T5 $IP
```

scan list of ipes

```bash
nmap -iL listIP.txt
```

host scaning

```bash
nmap -sn 192.168.1.0/24
```

port scaning without host scaning

```bash
namp -Pn $IP
```

port scaning and host scaning

```bash
nmap $IP
```

scan specific ports

```bash
namp $IP -p80,443
```

scan all ports 65535

```bash
nmap $IP -p-
```

scan ports Versions

```bash
namp -sV $IP
```

to enable OS and version detection, script scanning, and traceroute

    namp -A $IP

run vuln scanners

```bash
nmap $IP --script vuln
```

OS Detection

```bash
nmap $IP -O
```

## Nmap speed scan to Avoid IDS,IPS Detection

```bash
nmap $IP -sV -T0
```

| Paranod    | T0  |
| ---------- | --- |
| Sneaky     | T1  |
| Polite     | T2  |
| Normal     | T3  |
| Aggressive | T4  |
| Insane     | T5  |

To minimize avoid IDS,IPS

```bash
nmap $IP --host-timeout=1
```

## Nmap TCP,UDP Scan

(Stealth) TCP Scan required root privileg Use SYN/RST don't Commplet three way handshake (faster) and it can be used to bypass older Intrusion Detection systems as they are looking out for a full three way handshake.

```bash
namp -sS $IP
```

TCP scan without  root privileg Use Three Way Handshake

```bash
namp -sT $IP
```

UDP scan required root privileg

```bash
namp -sU $IP
```

Will scan the top 20 most commonly used UDP ports, resulting in a much more acceptable scan time

```bash
nmap -sU $IP --top-ports 20
```

## Nmap Script Engine categories

| safe      | Won't affect the target                                                                                  |
| --------- | -------------------------------------------------------------------------------------------------------- |
| intrusive | Not safe: likely to affect the target                                                                    |
| vuln      | Scan for vulnerabilities                                                                                 |
| exploit   | Attempt to exploit a vulnerability                                                                       |
| auth      | Attempt to bypass authentication for running services (e.g. Log into an FTP server anonymously)          |
| brute     | Attempt to bruteforce credentials for running services                                                   |
| discovery | Attempt to query running services for further information about the network (e.g. query an SNMP server). |

**EX**: 

```bash
nmap $IP --script=safe
```

**Note**:

Nmap Know if the port closed if the target respond with an ICMP (ping) packet containing a message that the port is unreachable

## Nmap Script Engine (NSE)

path of namp Scritps

```bash
/usr/share/nmap/scripts
```

Info and usage of script 

```bash
nmap --script-help script name
nmap --script-help smb-enum-shares.nse
```

Use all script starts with "smb-enum-"

```bash
nmap $IP --script="smb-enum-*"
```

Run namp scripts to scan

```bash
namp $IP -sC
```

## Firewall bypass

***TCP ACK*** Scan It is used to map out firewall rulesets, determining whether they are stateful or not and which ports are filtered

```bash
nmap -sA $IP
```

***Window Scan*** it exploits an implementation detail of certain systems to differentiate open ports from closed ones, rather than always printing unfiltered when a RST is returned,when the say port closed that mean thats filtered no result if found firewall,result if found firewall 

```bash
nmap -sW $IP
```

#### [ Null,Fin,Xmas Scan ] also to bypass firewall

If a port is open then we will not get any response    send null

```bash
nmap -sN $IP
```

If a port is open then we will not get any response send fin

```bash
nmap -sF $IP
```

If a port is open then we will not get any response    send FIN,PSH,URG

```bash
nmap -sX $IP
```

Another way of eliciting responses from systems that are configured to cloak their presence on the network send FIN/ACK

```bash
nmap -sM $IP
```

***Finally***:

it is essential to note that the ACK scan and the window scan were very efficient at helping us map out the firewall rules. However, it is vital to remember that just because a firewall is not blocking a specific port, it does not necessarily mean that a service is listening on that port. For example, there is a possibility that the firewall rules need to be updated to reflect recent service changes. Hence, ACK and window scans are exposing the firewall rules, not the services.

## IDS bypass

We Use Fragmented Packets to make it harder for packet filters, intrusion detection systems(IDS), and other annoyances to detect what you are doing.Nmap provides the option -f to fragment packets. Once chosen, the IP data will be divided into 8 bytes or less.Adding another -f (-f -f or -ff) will split the data into 16 byte-fragments instead of 8.You can change the default value by using the --mtu; however, you should always choose a multiple of 8.

| Fragment IP data into 8 bytes  | -f  |
| ------------------------------ | --- |
| Fragment IP data into 16 bytes | -ff |

***EX***:

```bash
sudo nmap -sS -p80 -f $IP
```

Using idle/Zombie Scan: https://tryhackme.com/room/nmap03

## Spoofing and Decoys Scan

**Why we Use this Scan**:

When we are scanning machines that are not ours, we often want to hide our IP (our identity).

In brief, scanning with a spoofed IP address is three steps:

1. Attacker sends a packet with a spoofed source IP address to the target machine.

2. Target machine replies to the spoofed IP address as the destination.

3. Attacker captures the replies to figure out open ports.

The target machine will respond to the incoming packets sending the replies to the destination IP address `SPOOFED_IP` => For this scan to work and give accurate results, the attacker needs to monitor the network traffic to analyze the replies

```bash
nmap -S $SPOOFED_IP $IP
```

To tell Nmap explicitly which network interface to use and not to expect to receive a ping reply.It is worth repeating that this scan will be useless if the attacker system cannot monitor the network for responses.

```bash
nmap -e NET_INTERFACE -Pn -S $SPOOFED_IP $IP
```

Will make the scan of 10.10.167.182 appear as coming from the IP addresses 10.10.0.1, 10.10.0.2, and then ME to indicate that your IP address should appear in the third order

```bash
nmap -D 10.10.0.1,10.10.0.2,ME $IP
```

Where the third and fourth source IP addresses are assigned randomly, while the fifth source is going to be the attacker’s IP address. In other words, each time you execute the latter command, you would expect two new random IP addresses to be the third and fourth decoy sources.

```bash
nmap -D 10.10.0.1,10.10.0.2,RND,RND,ME $IP
```

| Scan Type              | Example Command                           |
| ---------------------- | ----------------------------------------- |
| ARP Scan               | sudo nmap -PR -sn MACHINE_IP/24           |
| ICMP Echo Scan         | sudo nmap -PE -sn MACHINE_IP/24           |
| ICMP Timestamp Scan    | sudo nmap -PP -sn MACHINE_IP/24           |
| ICMP Address Mask Scan | sudo nmap -PM -sn MACHINE_IP/24           |
| TCP SYN Ping Scan      | sudo nmap -PS22,80,443 -sn MACHINE_IP/30  |
| TCP ACK Ping Scan      | sudo nmap -PA22,80,443 -sn MACHINE_IP/30  |
| UDP Ping Scan          | sudo nmap -PU53,161,162 -sn MACHINE_IP/30 |

| no DNS lookup                    | -n  |
| -------------------------------- | --- |
| reverse-DNS lookup for all hosts | -R  |
| host discovery only              | -sn |

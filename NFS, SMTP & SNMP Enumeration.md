## Network File System (NFS)

scan NFS with nmap scripts

```bash
nmap $IP -p 111 --script="nfs-*"
```

Scan RPC and enum

```bash
rpcinfo -p $IP
```

Show Mounted files and on Server

```bash
showmount -e $IP
```

Mount rpc file on local Machine

```bash
mount -t nfs $IP:/* /tmp/meta
```

Unmount rpc

```bash
unmount /tmp/meta
```

**Exploit**:

1. Search about remote procedure call (rpc)
   
   With Nmap
   
   ```bash
   with Nmap    -> nmap $IP -p 111 --script="nfs-*"
   ```
   
   With rpcinfo
   
   ```bash
   rpcinfo -p $IP
   ```

2. Show the mounted file on this server
   
   ```bash
   showmount -e $IP
   ```

3. If There are any file you can mount it in you local machine
   
   ```bash
   sudo mount -t nfs $IP:/* /tmp/meta
   ```

**Notes**:

**mrpc**  -> Microsoft Remote Procedure Call

**rpc**      -> Remote Procedure Call

---

## Simple Mail Transfer Protocol (SMTP)

Enum Users hint: You can use -w if the server slow in response

```bash
smtp-user-enum -M VRFY -U users.txt -t $IP
```

You Can use nmap scripts

```bash
nmap $IP --scripts="usr/share/nmap/scripts/smtp-..et"
```

**Exploit**:

1. Search about SMTP services
   
   ```bash
   nmap -sV -p0- $IP
   ```

2. Try To Know SMTP Version
   
   ```
   use metasploit medule search smtp_version
   ```

3. Try To Connect with it with NetCat
   
   ```bash
   nc -nvv $IP 25
   ```

4. Show the available commands on the remote server
   
   ```bash
   nmap -p 25 --script=smtp-commands $IP
   ```

5. Try to Enum Users
   
   with nmap
   
   ```bash
   nmap -p 25 --script=smtp-enum-users $IP
   ```
   
   or 
   
   ```bash
   nmap --script smtp-enum-users.nse [--script-args smtp-enum-users.methods={EXPN,...},...] -p 25,465,587 $IP
   ```
   
   With smtp-user-enum
   
   ```bash
   smtp-user-enum -M VRFY -u root -t $IP -w 20
   ```

6. Try to use UserName in any exploit Like SSH Bruteforce using hydra

---

### Simple Network(device) Management Protocole (SNMP)

You Can Use nmap scripts

```bash
nmap -sU -p 161 --script="usr/share/nmap/scripts/snmp-..et"
```

Used to All target info

```bash
snmpwalk -v1 -c public
```

**Note**:

You can use " search snmp_enum" in metasploite to get arranged(sorted) data

**MIB**  -> Mean Management Information Base ,refere to devices and network info,works

**OID**  -> Object Identifier

**Exploit**:

1. Scan if service runing or not
   
   ```bash
   nmap -sU -p 161 -T5 -sV $IP
   nmap -sU -p 161 -sV --script="snmp-win32-services.nse" $IP
   ```

2. If there are password use nmap (NSE)
   
   ```bash
   nmap -sU -p 161 -sV --script="snmp-brute.nse" $IP
   ```

3. use snmpwalk to uses SNMP GETNEXT requests to query a network entity for a tree of information
   
   ```bash
   snmpwalk -v1 -c pass $IP
   ```

4. Use Metasploit to show arrang Result
   
   ```bash
   auxiliary/scanner/snmp/snmp_enum
   ```
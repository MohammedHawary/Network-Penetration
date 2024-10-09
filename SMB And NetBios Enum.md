# Server Massage Block (SMB)

Login With SMB

```bash
smbclient -L $IP
```

Access files and Directores

```bash
smbclient //$IP/fileName
```

Login with user and pass

```bash
smbclient //$IP/fileName -U user    -P password
```

Login with Anonymous user

```bash
smbmap -H 192.168.1.5 -u Anonymous 
```

powerfull enum scan

```bash
smbmap -u jsmith -p password1 -d workgroup -H 192.168.0.1
```

Powerfull tool to enum smb

```bash
enum4linux -av $IP
```

enum with all nmap scritps

```bash
nmap $IP --script="smb-enum-*"
```

vuln with all nmap scritps

```bash
nmap $IP --script="smb-vuln-*"
```

### Note

You Can use hashed password in `smbmap` with parm `-p`

---

# NetBIOS

Scaning NetBIOS

```bash
nbtscan -r $IP
```

#### Exploit

1. Scan the network or host to know if smb or netBIOS work
   
   single host
   
   ```bash
   nbtscan $IP
   ```
   
   Network
   
   ```bash
   nbtscan -r $IP
   ```

2. Try to login

```bash
smbclient -L $IP
smbclient //$IP/file/path
smbclient //$IP/file/path -U user -P pass
smbmap -H $IP -u Mohammed
smbmap -H $IP -u Mohammed -p ""
```

3. Scan Vuln with nmap (NSE) scripts 
   
   ```bash
   nmap $IP -sV -p T:139,445 U:137 --script="smb-vuln-*"
   ```

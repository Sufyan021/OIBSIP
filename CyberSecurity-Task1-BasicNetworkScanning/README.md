
# Task 1 - Basic Network Scanning with Nmap

## Objective

The objective of this task is to perform network scanning using Nmap and identify open ports, running services, and operating system information on a target host.

## Tools Used

- Kali Linux Terminal
- Nmap
- 
## Nmap Installation

Nmap was installed on the Kali Linux terminal using the following commands:

```bash
sudo apt update
sudo apt install nmap -y
```

To verify the installation:

```bash
nmap --version
```

The command successfully displayed the installed Nmap version, confirming that Nmap was installed correctly.

## Commands Executed

```bash
nmap 172.31.238.132
```

```bash
sudo nmap -sV 172.31.238.132
```

```bash
sudo nmap -O 172.31.238.132
```

## Results

The target host was reachable.

No open ports were discovered during scanning.

All 1000 scanned TCP ports were filtered and did not respond to probes.

OS detection could not determine the exact operating system because insufficient information was available.

## Security Analysis

The filtering of all scanned ports indicates that a firewall or packet-filtering mechanism is in place.

This improves security by preventing unauthorized users from discovering services running on the host.

## Ethical Considerations

Network scanning should only be performed on systems that you own or have explicit permission to test.

Unauthorized scanning of third-party systems may violate laws and organizational policies.

## Screenshots

Screenshots of all scans are included in the screenshots folder.

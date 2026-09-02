# Task 2 - Basic Firewall Configuration with UFW

## Objective

The objective of this task is to configure a host-based firewall using UFW (Uncomplicated Firewall) and create basic allow and deny rules.

## Tools Used

- Kali Linux
- UFW (Uncomplicated Firewall)

## Installation

```bash
sudo apt update
sudo apt install ufw -y
```

Verify installation:

```bash
sudo ufw status
```

## Firewall Configuration

Firewall was enabled using:

```bash
sudo ufw enable
```

Rules configured:

```bash
sudo ufw allow 22/tcp
sudo ufw allow 80/tcp
sudo ufw deny 21/tcp
```

Current firewall rules:

```bash
sudo ufw status numbered
```

## Results

The firewall was successfully activated.

Configured rules:

- Allow SSH (22/tcp)
- Allow HTTP (80/tcp)
- Deny FTP (21/tcp)

## Security Analysis

A firewall is a critical security control that filters incoming and outgoing network traffic.

Allowing only required services reduces exposure to attackers.

Blocking FTP helps prevent the use of insecure protocols that transmit credentials without encryption.

## Ethical Considerations

Firewall configurations should be tested carefully to avoid blocking legitimate services and administrative access.

## Screenshots

Screenshots of installation, activation, and firewall rules are available in the screenshots folder.

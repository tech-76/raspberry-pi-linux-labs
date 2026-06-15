# SSH Configuration

## Objective

Configure secure, reliable remote administration for the Ubuntu Server lab while preserving a tested recovery path.

## 1. Verify SSH Service

```bash
sudo systemctl status ssh --no-pager
sudo ss -lntp | grep ':22'
```

Install only if required:

```bash
sudo apt update
sudo apt install -y openssh-server
sudo systemctl enable --now ssh
```

## 2. Test Password-Based Access First

From the trusted admin workstation:

```bash
ssh labadmin@192.168.10.20
```

Confirm:

```bash
whoami
hostname
ip -br address
```

Do not disable password authentication until key-based access has been verified in a second terminal.

## 3. Create an SSH Key

On the admin workstation:

```bash
ssh-keygen -t ed25519 -a 100
```

Use a meaningful comment and a strong passphrase.

Copy the public key:

```bash
ssh-copy-id labadmin@192.168.10.20
```

Or manually add the public key to:

```text
~/.ssh/authorized_keys
```

Correct permissions:

```bash
chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
```

## 4. Validate Key Authentication

```bash
ssh -v labadmin@192.168.10.20
```

On the server:

```bash
sudo journalctl -u ssh --since "10 minutes ago"
```

Confirm the session uses public-key authentication.

## 5. Harden SSH Carefully

Back up the configuration:

```bash
sudo cp /etc/ssh/sshd_config /etc/ssh/sshd_config.backup
```

Create a dedicated drop-in file when supported:

```bash
sudoedit /etc/ssh/sshd_config.d/99-lab-hardening.conf
```

Example baseline:

```text
PermitRootLogin no
PubkeyAuthentication yes
PasswordAuthentication no
KbdInteractiveAuthentication no
X11Forwarding no
AllowUsers labadmin
```

Validate syntax before reload:

```bash
sudo sshd -t
```

Reload without dropping established sessions:

```bash
sudo systemctl reload ssh
```

Open a new terminal and test login before closing the existing session.

## 6. Firewall Rule

```bash
sudo ufw allow OpenSSH
sudo ufw status numbered
```

For a segmented environment, restrict SSH to a trusted management subnet rather than all sources.

## 7. Client Configuration

A client alias can be added to `~/.ssh/config`:

```text
Host ubuntu-pi01
    HostName 192.168.10.20
    User labadmin
    IdentityFile ~/.ssh/id_ed25519
    IdentitiesOnly yes
```

Then connect with:

```bash
ssh ubuntu-pi01
```

Do not upload private keys to GitHub.

## 8. Troubleshooting

### Permission denied

```bash
ssh -vvv labadmin@192.168.10.20
ls -ld ~/.ssh
ls -l ~/.ssh/authorized_keys
sudo journalctl -u ssh --since "20 minutes ago"
```

### Connection refused

```bash
sudo systemctl status ssh
sudo ss -lntp | grep ':22'
sudo ufw status
```

### Connection timeout

Check:

- IP address.
- VLAN routing.
- Client and server firewall.
- Wi-Fi isolation.
- Physical connectivity.
- Correct SSH port.

## 9. Security Checklist

- [ ] Root login disabled.
- [ ] Key-based login tested.
- [ ] Private key protected by a passphrase.
- [ ] Password login disabled only after successful key test.
- [ ] SSH limited to trusted networks where practical.
- [ ] Configuration syntax validated.
- [ ] Backup configuration retained.
- [ ] Recovery method documented.
- [ ] Private keys excluded from Git.

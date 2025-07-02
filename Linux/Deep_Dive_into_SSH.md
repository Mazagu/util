# 🛡️ Deep Dive into SSH (Secure Shell)

**SSH (Secure Shell)** is a cryptographic network protocol that allows secure communication over an unsecured network. It's widely used by system administrators, developers, and DevOps/SRE teams for accessing and managing remote servers.

---

## 🔑 What Is SSH?

SSH provides a **secure channel** over an unsecured network using a **client-server architecture**.

- **Client**: Initiates the connection
- **Server**: Accepts and authenticates the client

It's most commonly used for:
- Remote command-line login (`ssh user@host`)
- File transfers (`scp`, `sftp`)
- Port forwarding and tunneling
- Automation via scripts

---

## 🔐 How SSH Works

SSH uses **asymmetric encryption**, **symmetric encryption**, and **hashing** to secure connections.

### Key Components:

1. **Key Exchange**: Uses algorithms like Diffie-Hellman to negotiate a shared secret between client and server.
2. **Authentication**:
   - **Password-based** (less secure)
   - **Key-based** (more secure) using `id_rsa` and `id_rsa.pub`
3. **Encryption**:
   - Once a session is established, a **symmetric key** encrypts data.
4. **Integrity**:
   - Uses **MACs** (Message Authentication Codes) to ensure message integrity.

---

## 🧰 SSH Client Tools

| Tool        | Usage                                       |
|-------------|---------------------------------------------|
| `ssh`       | Connect to remote machines                  |
| `scp`       | Copy files between local and remote systems |
| `sftp`      | Secure FTP-like file transfer               |
| `ssh-agent` | Stores your decrypted private keys          |
| `ssh-add`   | Adds keys to the agent                      |
| `ssh-keygen`| Generate public-private key pairs           |

---

## 🔧 SSH Configuration

### `~/.ssh/config`

```
Host dev
    HostName 192.168.1.10
    User ubuntu
    Port 22
    IdentityFile ~/.ssh/id_rsa
```

Allows easy connection:

```
ssh dev
```

### `/etc/ssh/sshd_config` (on server)

Key settings:
- `Port 22`: Default port (can be changed for security)
- `PermitRootLogin no`: Avoid root login
- `PasswordAuthentication no`: Use key-based authentication
- `AllowUsers`: Whitelist specific users

---

## 🔐 SSH Authentication Methods

### 🔑 Public Key Authentication
- Most secure and recommended.
- Client proves it has the private key that matches the public key stored in `~/.ssh/authorized_keys`.

### 🔒 Password Authentication
- Less secure.
- Often disabled for production environments.

---

## 🧱 SSH Port Forwarding (Tunneling)

1. **Local Port Forwarding**
   - Access a remote service locally.
   - Example: Forward remote DB to local port
   ```
   ssh -L 3307:localhost:3306 user@remote
   ```

2. **Remote Port Forwarding**
   - Let a remote machine access a service on your local machine.
   ```
   ssh -R 9090:localhost:3000 user@remote
   ```

3. **Dynamic Port Forwarding**
   - Acts like a SOCKS proxy.
   ```
   ssh -D 1080 user@remote
   ```

---

## 🧪 Hardening SSH

- Disable root login: `PermitRootLogin no`
- Disable password authentication: `PasswordAuthentication no`
- Use key-based authentication
- Change default port from 22
- Set idle session timeouts: `ClientAliveInterval` and `ClientAliveCountMax`
- Use firewall rules (e.g., iptables/UFW) to limit access

---

## 🚀 Common Use Cases

- Remote server management (e.g., `ssh user@host`)
- Deploy code (via tools like Ansible, Fabric)
- Git over SSH (`git@github.com:user/repo.git`)
- Automated backups via `rsync -e ssh`
- Database tunneling

---

## 📦 Related Tools & Ecosystem

| Tool        | Description                               |
|-------------|-------------------------------------------|
| **OpenSSH** | The most widely used SSH implementation   |
| **PuTTY**   | Windows-based SSH client                  |
| **Mosh**    | Mobile-friendly SSH client with UDP       |
| **SSHFS**   | Mount remote FS over SSH                  |
| **tmux** / **screen** | Session persistence over SSH     |

---

## 📚 Learning Resources

- [OpenSSH Manual](https://man.openbsd.org/ssh)
- [SSH Essentials – DigitalOcean](https://www.digitalocean.com/community/tutorials/ssh-essentials-working-with-ssh-servers-clients-and-keys)
- *SSH Mastery* by Michael W. Lucas

---

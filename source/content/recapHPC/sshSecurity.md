## SSH Agent Forwarding

{% if slide %}
**Core Concept:** Forward the local `ssh-agent` to a remote host.
This enables authentication for secondary connections (e.g., pulling from a private Git repository) without storing private keys on the remote machine.
{% endif %}

{% if page %}
SSH Agent Forwarding allows a local SSH key to be utilized on a remote host.
This ensures that private keys remain exclusively on the local machine and are never copied to remote systems. 
{% endif %}

### Setup Instructions

{% if slide %}
* **Launch agent**: `eval "$(ssh-agent -s)"`
* **Add the key**: `ssh-add ~/.ssh/id_rsa`
* **Connect with forwarding:** `ssh -A user@host` 
* **Verify on remote:** `ssh-add -l`
{% endif %}

{% if page %}
The procedure for configuring agent forwarding depends on the local operating system:

::::{tab-set}
:::{tab-item} Linux
1. **Check the agent:** Run `ssh-add -l` to view currently loaded keys. If no identities are found, proceed to the next step.
2. **Launch the agent:** 
   * **Temporary (current session only):** Start a new process with `eval "$(ssh-agent -s)"`.
   * **Permanent setup (via systemd):** First, create the service file at `~/.config/systemd/user/ssh-agent.service` with the following content:
     ```ini
     [Unit]
     Description=SSH Agent

     [Service]
     Type=simple
     Environment=SSH_AUTH_SOCK=%t/ssh-agent.socket
     ExecStart=/usr/bin/ssh-agent -D -a $SSH_AUTH_SOCK

     [Install]
     WantedBy=default.target
     ```
     Next, enable and start the agent automatically:
     ```bash
     systemctl --user enable --now ssh-agent.service
     ```
     *(Note: You will also need to add `export SSH_AUTH_SOCK="$XDG_RUNTIME_DIR/ssh-agent.socket"` to your shell profile, e.g., `~/.bashrc`.)*
3. **Load the private key:** The agent starts empty. Add your key to the agent using:
   ```bash
   ssh-add ~/.ssh/id_rsa

   ```
   ```{tip}
   To automatically load the key into the agent upon first use, add `AddKeysToAgent yes` to your `~/.ssh/config` file.)*
   ```
4. **Connect with forwarding:** Use the `-A` flag to enable forwarding for the session: `ssh -A user@cluster.example.com`.
5. **Verify on the remote:** Running `ssh-add -l` on the remote host should list the local key's fingerprint.

:::

:::{tab-item} macOS
macOS natively integrates the OpenSSH client with the system keychain. Recent versions automatically start an `ssh-agent`.

1. **Ensure the key is in the keychain:** Run `ssh-add -K ~/.ssh/id_rsa` to add the key to the macOS keychain and load it into the agent.
2. **Confirm the key is loaded:** Run `ssh-add -l`.
3. **Connect with forwarding:** Use the `-A` flag: `ssh -A user@cluster.example.com`.
4. **Verify on the remote:** Running `ssh-add -l` on the remote host should list the local key's fingerprint.

:::

:::{tab-item} Windows (Native)
Windows 10 and 11 include a native OpenSSH client. These commands should be run in PowerShell as a standard user.

1. **Start the agent:** Ensure the service is running and set to start automatically:
```powershell
Start-Service ssh-agent
Set-Service -Name ssh-agent -StartupType Automatic


```


2. **Add the key:** Run `ssh-add C:\Users\<username>\.ssh\id_rsa`.
3. **Verify the key is loaded:** Run `ssh-add -l`.
4. **Connect with forwarding:** Use the `-A` flag: `ssh -A user@cluster.example.com`.

:::
:::{tab-item} Windows (WSL)
Within a Windows Subsystem for Linux (WSL) environment, the standard Linux instructions apply unchanged.

*Note: When using `ssh -A` from WSL, the forwarded agent is the one inside the WSL environment, not the native Windows agent.*

:::
::::

{% endif %}

### Persistent Per-Host Configuration

{% if slide %}
Add the following to `~/.ssh/config` (or `%USERPROFILE%\.ssh\config` on Windows):

```text
Host cluster.example.com
    ForwardAgent yes
    User username

```

{% endif %}

{% if page %}
To make forwarding automatic for specific hosts, the SSH configuration file can be modified.

* **Linux / macOS / WSL:** Edit `~/.ssh/config`
* **Windows:** Edit `%USERPROFILE%\.ssh\config`

Append the following lines:

```text
Host cluster.example.com
    ForwardAgent yes
    User username

```

{% endif %}

### Security Best Practices

{% if slide %}
*⚠️ Agent forwarding should only be enabled for fully trusted remote hosts to prevent potential misuse of the forwarded authentication socket.*
{% endif %}

{% if page %}
When utilizing SSH Agent Forwarding, the following security measures should be observed:

* **Forward only to trusted hosts:** A compromised remote machine could misuse the forwarded agent to access other services.
* **Limit loaded keys:** Load only the necessary keys. Use `ssh-add -D` to clear all keys, followed by `ssh-add <specific-key>` to limit exposure.
* **Use per-host configuration:** Avoid using a global `ForwardAgent yes` setting. Forwarding should be strictly limited to required hosts.
* **Enforce specific identities:** Use `IdentitiesOnly=yes` in the SSH configuration when appropriate to force SSH to use only explicitly specified identities, preventing accidental key leakage.
* **Implement timeouts:** If supported, set an idle timeout for the agent to reduce the window of time an attacker could exploit a forwarded agent.
* **Utilize Bastion/Jump hosts:** Forward the agent to a single, well-secured gateway, and then hop to downstream machines from there.
{% endif %}


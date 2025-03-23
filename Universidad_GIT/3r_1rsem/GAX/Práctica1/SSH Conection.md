### To establish a connection between VM A and VM B

On both VMs:

1. Install OpenSSH on the VM:
   ```bash
   sudo apt-get install openssh-server
   ```
   
2. Open the `/etc/ssh/sshd_config` file in a text editor.
   
   - Look for the following lines:
     ```bash
     PermitRootLogin yes
     PasswordAuthentication no
     ```
   
   - Change them to:
     ```bash
     PermitRootLogin no
     PasswordAuthentication yes
     ```

3. Restart the SSH server to apply the changes:
   ```bash
   sudo systemctl restart ssh
   ```

4. Generate SSH public and private keys:
   ```bash
   ssh-keygen
   ```

On VM A:

5. Copy the public key to VM B:
   ```bash
   ssh-copy-id user@<destination_VM_ip>
   ```

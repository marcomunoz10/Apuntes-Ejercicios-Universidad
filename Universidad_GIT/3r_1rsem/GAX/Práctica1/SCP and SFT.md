
### Exercise 3.3: Secure File Transfers with SCP and SFTP

**SCP** (Secure Copy Protocol) is a command-line tool used to securely transfer files between two machines over an SSH connection. It works like the traditional `cp` command but transfers data between remote systems, ensuring encryption during the transfer.

**SFTP** (SSH File Transfer Protocol) is a protocol also based on SSH, but it provides a more interactive environment for file management. It allows you to upload, download, and manage files between local and remote systems in a secure manner, similar to FTP but with added encryption.

Both SCP and SFTP use SSH for secure data transmission.

1. **Create a 60 MB test file**:
   ```bash
   truncate --size 60M sample.txt
   ```

2. **Transfer the file using SCP**:
   ```bash
   scp sample.txt user@<destination_IP>:/path/to/destination/
   ```

3. **Transfer the file using SFTP**:
   ```bash
   sftp user@<destination_IP>
   put sample.txt /path/to/destination/
   bye
   ```

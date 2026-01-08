---
{"dg-publish":true,"permalink":"/garden/posts/linux-permissions/","tags":["linx","config","scripts"],"created":"2025-08-18T12:14:12+08:00","updated":"2026-01-08 10:52"}
---

# [[Garden/posts/Linux permissions\|Linux permissions]]

topic: [[02-Area/programming/Linux\|Linux]]
related: [[10-Inbox/Linux users\|Linux users]]

- Classic UNIX DAC
    - chmod: Set read/write/execute bits for user/group/other  
      Example: `chmod 644 file.txt`
    - chown / chgrp: Change file owner or group  
      Example: `chown user:group file.txt`
    - setuid / setgid: Run executable with owner/group privileges  
      Example: `chmod u+s /usr/bin/passwd`
    - sticky bit: Prevent deletion of others' files in shared directories  
      Example: `chmod +t /tmp`
    - umask: Default permissions for newly created files  
      Example: `umask 022`
- Extended ACLs
    - POSIX ACLs: Fine-grained per-user/group permissions  
      Example: `setfacl -m u:alice:rwx file.txt`
    - Default ACLs: Inherited permissions for new files in a directory  
      Example: `setfacl -d -m g:dev:rwx /project`
- [[Garden/knowledge-base/Linux Capabilities\|Linux Capabilities]]
    - CAP_NET_RAW, CAP_NET_ADMIN, CAP_SYS_ADMIN, etc.  
      Example: `setcap cap_net_raw+ep /usr/bin/tcpdump`
    - File capabilities: Attach capabilities to binaries  
      Example: `getcap /usr/bin/ping`
    - Process capabilities: Adjust dynamically via `capset()` or `prctl()`
- Mandatory Access Control (MAC)
    - SELinux: Context and policy-based access control  
      Example: `ls -Z /var/log/messages`
    - AppArmor: Path-based profile restrictions  
      Example: `aa-enforce /usr/bin/myapp`
    - Smack / TOMOYO: Label-based access control  
      Example: `cat /sys/fs/smackfs/*`
- LSM / Security Modules
    - Yama: ptrace and process relationship restrictions  
      Example: `/proc/sys/kernel/yama/ptrace_scope`
    - Landlock: User-space process sandboxing  
      Example: `landlock_create_ruleset()`
- Filesystem-specific Access Controls
    - ext4 attributes: Immutable or append-only bits  
      Example: `chattr +i file.txt`
    - NFSv4 / CIFS ACLs: Network filesystem ACLs  
      Example: `nfs4_setfacl -a A::user:rw file`
- Namespaces and Isolation
    - User namespaces: UID/GID remapping  
      Example: `unshare -U`
    - Network/PID/IPC namespaces: Process isolation  
      Example: `unshare -n -p`
    - Control groups (cgroups): Resource limits and containment  
      Example: `cgcreate -g memory:/mygroup`
- Privilege Containment / Sandboxing
    - seccomp-bpf: Restrict syscalls available to a process  
      Example: `prctl(PR_SET_SECCOMP, SECCOMP_MODE_FILTER, &prog)`
    - chroot / pivot_root: Restrict filesystem view  
      Example: `chroot /var/empty`
    - systemd sandboxing: Service-level restrictions  
      Example: `ProtectSystem=full` in service unit
    - NoNewPrivileges: Prevent privilege escalation  
      Example: `prctl(PR_SET_NO_NEW_PRIVS, 1)`
- Auditing and Logging
    - Audit subsystem: Records capability and policy denials  
      Example: `ausearch -m USER_CAPABLE`
    - Kernel logs: Capability denials and MAC messages  
      Example: `dmesg | grep -i cap`
- Authentication and Identity
    - PAM: Pluggable authentication modules  
      Example: `cat /etc/pam.d/sshd`
    - sudo / polkit: Controlled privilege escalation  
      Example: `sudo -l`
    - Shadow passwords: Secure credential storage  
      Example: `cat /etc/shadow`


Permission layers
- [[Garden/knowledge-base/Linux Capabilities\|Linux Capabilities]]
    - kernel option audit=0 only disables logging
    - checks and applies for binary at run time
    - root user is unrestricted
- [[Garden/posts/Linux permissions\|setfacl]]
    - checks and applies for file
    - root user is unrestricted
- chown, chmod
- [[02-Area/programming/Linux/SELinux\|SELinux]]

options
1. change ownership of the dir to that user
2. add user to the group that owns the directory or change the group ownership of the dir
3. use ACL.


How to grant user to have access to folders without granting shared folders.

```bash
# set permission for old files recursively
sudo setfacl -R -m u:username:rwx /opt/docker
# set permission for new files
sudo setfacl -R -d -m u:username:rwx /opt/docker

getfacl /opt/docker
```

This allows user `username` to have rwx access to the `/opt/docker` for any existing files recursively, and also to have default permission for any new files created there.

```bash
# remove acl entry for user username
sudo setfacl -x u:username /opt/docker
# remove default acl
sudo setfacl -k /opt/docker
# wipe all acl, restore to unix permissions
sudo setfacl -b /opt/docker
```
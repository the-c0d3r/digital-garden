---
{"dg-publish":true,"permalink":"/garden/knowledge-base/linux-users/","tags":["linx","config","scripts"],"created":"2025-08-18 12:20","updated":"2026-04-04 16:19","dg-note-properties":{"modified_date":"2026-04-04 16:19","creation_date":"2025-08-18 12:20","tags":["linx","config","scripts"]}}
---

# Linux users

topic: [[02-Area/programming/Linux\|Linux]]
related: [[Garden/posts/Linux permissions\|Linux permissions]]

```bash
#!/bin/bash
# Usage: sudo ./create_user.sh username "password" "ssh-rsa AAAAB3...key..."

USERNAME="$1"
PASSWORD="$2"
SSH_KEY="$3"

# Create user with home dir
useradd -m -s /bin/bash "$USERNAME"

# Set password
echo "$USERNAME:$PASSWORD" | chpasswd

# Prepare SSH directory
mkdir -p /home/$USERNAME/.ssh
echo "$SSH_KEY" > /home/$USERNAME/.ssh/authorized_keys
chmod 700 /home/$USERNAME/.ssh
chmod 600 /home/$USERNAME/.ssh/authorized_keys
chown -R $USERNAME:$USERNAME /home/$USERNAME/.ssh
```

## create users, apply acl, sudoers rule

```bash
#!/bin/bash
# create_secure_user.sh
# Usage:
#   sudo ./create_secure_user.sh create
#   sudo ./create_secure_user.sh remove

MODE=$1

create_user() {
    read -p "Enter username: " USERNAME
    read -s -p "Enter password: " PASSWORD
    echo
    read -p "Enter SSH public key: " SSH_KEY

    # Create user with home dir
    useradd -m -s /bin/bash "$USERNAME"

    # Set password
    echo "$USERNAME:$PASSWORD" | chpasswd

    # Setup SSH access
    mkdir -p /home/$USERNAME/.ssh
    echo "$SSH_KEY" > /home/$USERNAME/.ssh/authorized_keys
    chmod 700 /home/$USERNAME/.ssh
    chmod 600 /home/$USERNAME/.ssh/authorized_keys
    chown -R $USERNAME:$USERNAME /home/$USERNAME/.ssh

    # Prompt for directories and grant ACL permissions
    echo "Enter directories (space-separated) to give $USERNAME full access:"
    read DIRS
    for DIR in $DIRS; do
        setfacl -R -m u:$USERNAME:rwx "$DIR"
        setfacl -R -d -m u:$USERNAME:rwx "$DIR"
        echo "Granted ACL on $DIR"
    done

    # Prompt for commands to allow via sudo (with password)
    echo "Enter full paths of commands (space-separated) to allow for $USERNAME:"
    read CMDS

    if [ -n "$CMDS" ]; then
        SUDOERS_FILE="/etc/sudoers.d/$USERNAME"
        echo "$USERNAME ALL=(ALL) $CMDS" > "$SUDOERS_FILE"
        chmod 440 "$SUDOERS_FILE"
        echo "Sudo privileges configured for: $CMDS"
    fi

    echo "User $USERNAME created and configured."
}

remove_user() {
    read -p "Enter username to remove: " USERNAME

    # Remove sudoers file
    rm -f "/etc/sudoers.d/$USERNAME"

    # Remove ACLs from common directories
    echo "Enter directories (space-separated) to clear ACLs for $USERNAME:"
    read DIRS
    for DIR in $DIRS; do
        setfacl -x u:$USERNAME "$DIR"
        setfacl -k "$DIR"
        echo "Removed ACLs on $DIR"
    done

    # Remove user and home
    userdel -r "$USERNAME"
    echo "User $USERNAME and associated config removed."
}

if [ "$MODE" == "create" ]; then
    create_user
elif [ "$MODE" == "remove" ]; then
    remove_user
else
    echo "Usage: $0 {create|remove}"
    exit 1
fi
```
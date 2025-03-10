# Ansible Playbooks for Homelab Workshop

- Subset of my private Ansible playbook collection
  - Note: This is only designed for Ubuntu VMs.  It should work with Debian systems as well (such as Proxmox), but has not been thoroughly tested.
- The goal of this playbook is to apply common configuration steps VMs in an automated way
- Does the following:
  - Installs and configures unattended upgrades
  - Configures email notifications for updates
  - Installs and enables fail2ban

## How to Use

- **Create a snapshot of each VM before running the playbook!**
- Create a new Gmail account for use with this playbook.  **Do NOT use your main Gmail account!**  
  - This will be sending out regular notification emails, and while I've been running it for years without issue, you do not want to risk your primary Gmail account doing this.  You *can* send notifications *to* your primary Gmail account, just not *from* your VMs.
- Generate an app password for this account.  It will be used instead of your account's primary password when sending emails.
  - Use the following link **while logged in with your new Gmail account**: <https://myaccount.google.com/apppasswords>
- Edit the `vars.yml` file in `group_vars/ubuntu_common`, adjusting your email addresses accordingly.
- Optional: create a `.vault_pass` file containing a unique password for your Ansible vaults.  This should go into the same directory as the one containing `hosts` and `site.yml`.  Paste your vault password into the file and save.
  - **WARNING**: This is a plaintext file containing a password!  It's intended to make it safer to store *other* passwords in vaults, and thus *optional*.  You can still use vaults without creating the file.
  - **If you decide not to create the file**: edit the `ansible.cfg` file, commenting out the `vault_password_file` line.
  - The `.gitignore` file included with this repo excludes this file from git pushes.
- Generate a `vault.yml` file containing your Gmail app password.
  - Use `ansible-vault create group_vars/ubuntu_common/.vault_pass` to create the file.  This will open Vim, where you can paste the Gmail app password generated earlier.  Use the syntax listed below, **deleting the spaces in the password**.  When you write and quit Vim, Ansible will encrypt the file using your vault password.
  ```yaml
  ---
  # Ansible Encrypted Vault of Group Variables

  vault_gmail_pass: PASTE_PASSWORD_HERE
  ```
- Edit the `hosts` file, adding the VMs you wish to apply configuration to.  Be sure to remove any VMs that are not running or you don't want to change.
- Install Ansible *on the system running the playbook*: `sudo apt install ansible`
  - Note: You do NOT need Ansible on any of your target machines.  This Ansible playbook is designed to run from a single machine, and will use SSH and Python on each target machine to configure devices.
  - Optionally, also install Ansible Lint (code checker): `sudo apt install ansible-lint`
- Optional: Check your changes with `ansible-lint site.yml`
- SSH into your target VMs at least once from the system running your Ansible playbook. Logging in once will also allow you to add the SSH fingerprint to your known hosts file.
- Finally, run your playbook with `ansible-playbook site.yml`

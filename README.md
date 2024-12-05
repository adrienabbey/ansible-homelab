# ansible-homelab
Collection of Ansible playbook examples

Aimed at participants of my Homelab Workshop series.

## Steps:
1. Create a test VM, or snapshot an existing one.  This will be used to test your new Ansible playbook.
2. Create a new Gmail account.  **Do NOT use your primary Gmail account for this!**
3. Create an app password for the new Gmail account: https://myaccount.google.com/apppasswords
4. Clone this GitHub repo to your local machine.
5. Install Ansible: `apt install ansible`
6. Edit the `hosts` file, adding your VM of choice to it.
7. Edit the `group_vars/ubuntu_common/vars.yml` file.  Replace the email addresses with your own.  Do NOT change the password holder!
8. Optional: create a `.vault_pass` file, giving it a unique password.  This will be used to automatically unlock the password-protected vault files.
9. Create the vault file containing your Google App password: `ansible-vault create group_vars/ubuntu_common/vault.yml`
10. Optional: Install and use `ansible-lint` to test your new Ansible playbook.  You can run it with: `ansible-lint site.yml`  This will give warnings about any errors that exist in your playbook.  NOTE: Yes, the provided playbooks will complain about two lines involving 'Postmap credentials'.  You can ignore that for now.
11. SSH into your target VM.  This will ensure you don't have to worry about SSH fingerprints in a second.
12. Run the Ansible playbook: `ansible-playbook site.yml`
13. Start exploring and learning more about what makes Ansible so useful!
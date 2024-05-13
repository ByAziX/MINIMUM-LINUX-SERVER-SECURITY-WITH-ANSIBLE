## How to get started
1. Install [Ansible](https://docs.ansible.com/ansible/latest/installation_guide/intro_installation.html)
2. git clone this repository
  ```
  git clone https://github.com/moltenbit/How-To-Secure-A-Linux-Server-With-Ansible
  cd How-To-Secure-A-Linux-Server-With-Ansible
  ```
 
3. [Create SSH-Public/Private-Keys](https://github.com/imthenachoman/How-To-Secure-A-Linux-Server#ssh-publicprivate-keys)
  ```
  ssh-keygen -t ed25519
  ```
   
4. Change all variables in *group_vars/variables.yml* according to your needs.
5. Enable SSH root access (on the server) before running the playbooks:
   
  ```
  nano /etc/ssh/sshd_config
  [...]
  PermitRootLogin yes
  [...]
  ```

6. Recommended: configure static IP address on your system.
7. Add your systems IP address to *hosts.yml*.

&nbsp;

Run the requirements playbook using the root password you specified while installing the server:

    ansible-playbook --inventory hosts.yml --ask-pass requirements-playbook.yml

&nbsp;

Run the main playbook with the new users password you specified in the *variables.yml* file:

    ansible-playbook --inventory hosts.yml --ask-pass main-playbook.yml

&nbsp;

If you need to run the playbooks multiple times remember to use the SSH key and the new SSH port:

    ansible-playbook --inventory hosts.yml -e ansible_ssh_port=SSH_PORT --key-file /PATH/TO/SSH/KEY main-playbook.yml

&nbsp;

### Requirements
- sudo installed
- groups created for *sshusers*, *sudousers* and *suusers*
- new user created with the name specified in *variables.yml* and added to groups
- use of sudo limited to sudousers group
- use of su limited to suusers group
- passwordless sudo enabled for the new user
- SSH public key added to authorized_keys file

### Firewall: UFW
UFW is set to default deny in and out. 
The SSH-Port is set to *limit in*, allowed outgoing ports by default are 53 (DNS), 123 (NTP), 80 (http), 443 (https)

### Firewall: PSAD and Fail2Ban
PSAD and Fail2Ban is configured

### Packages
Installed packages are:
 - apt-transport-https
  - ca-certificates
  - host
  - kbtin
  - ntp
  - libpam-pwquality
  - unattended-upgrades
  - apt-listchanges
  - apticron
  - ufw
  - psad
  - fail2ban



## Plans / ToDos
- [ ] use Ansible vault to securely store secrets (In progress)

## Warning!
Read all tasks carefully and make sure they do not break your system before using these playbooks! Do not rely solely on the Ansible playbooks for security! It is your responsibility to make sure all settings you need have been set and are working. This is just a starting point! Depending on your needs and goals make sure to further secure your system.


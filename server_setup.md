# Common linux user setup
- use root/password login VPS server first time
- id <new_user> : creat a new user
- usermod -aG sudo <new_user> : add sudo privilege
- su <new_user> : change to new user
- sudo whoami : enter passwd, it should return root

# SSH Port setup
  - sudo vi /etc/ssh/sshd_config : edit for protection from SSHD attack (change port/disable root login/disable passwd-based login)
    - Port <new_port_number>  : do not use default 22
    - PermitRootLogin no
    - PasswordAuthentication no
  - sudo systemctl restart sshd :  restart the SSHD
  - sudo ss -tlnp : check sshd is listening on <new_port_number>

# Firewall setup
  - sudo iptables -L -n -v  : list all rules
  - sudo iptables -I INPUT -p tcp --dport <new_port_number> -j ACCEPT : add new port 
  - sudo iptables -L -n -v  : make sure new port is added

# Client SSH connection
- generate key-pair in Termius (or other tools)
- copy the pubkey to server ~/.ssh/authorized_keys (new_user)
- try Termius connection

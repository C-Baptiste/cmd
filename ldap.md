# install centos9

- dnf install openldap
- dnf install openldap-clients
- dnf install epel-release
- dnf update
- dnf install openldap-servers
- systemctl enable --now slapd.service
- firewall-cmd --add-service=ldap --permanent 
- firewall-cmd --reload





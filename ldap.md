# install centos9

- dnf install openldap
- dnf install openldap-clients
- dnf install epel-release
- dnf update
- dnf install openldap-servers
- systemctl enable --now slapd.service
- firewall-cmd --add-service=ldap --permanent 
- firewall-cmd --reload

# openldap

- ldapadd -W -D cn=admin,dc=...,dc=com -f users.ldif
- ldapsearch -x -b dc=...,dc=com
- journalctl -u slapd
- apt install -y ldap-utils
- apt install -y slapd
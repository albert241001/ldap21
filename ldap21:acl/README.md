# LDAP Server
## @albert241001
### Servidor LDAP (Debian 11)

eliminats els schemas innecesaris 
definir base de dades cn config amb usuari cn=Sysadmin,cn=config i passwd syskey


docker run --rm --name ldap.edt.org -h ldap.edt.org --network 2hisix -p 389:389 -it albert241001/ldap21:acl /bin/bash


slapcat -n0

ldapsearch -x -LLL -D 'cn=Sysadmin,cn=config' -w syskey -b 'cn=config' | grep dn

ldapsearch -x -LLL -D 'cn=Sysadmin,cn=config' -w syskey -b 'cn= config' olcDatabase={1}mdb           
ldapsearch -x -LLL -D 'cn=Sysadmin,cn=config' -w syskey -b 'cn= config' olcDatabase={1}mdb           
ldapmodify -vxc -D 'cn=Sysadmin,cn=config' -w syskey -f acl1.ldif 


#01-->Tots veuen tot
#dn: olcDatabase={1}mdb,cn=config
#changetype: modify
#delete: olcAccess
#-
#add: olcAccess
#olcAccess: to * by * read



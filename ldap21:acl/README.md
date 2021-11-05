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

dn: olcDatabase={1}mdb,cn=config

changetype: modify

delete: olcAccess

-

add: olcAccess

olcAccess: to * by * read
#------------------------------
ldapsearch -x -LLL

ldapsearch -x -LLL -D 'uid=anna,ou=usuaris,dc=edt,dc=org' -w anna 'uid=anna'

ldapsearch -x -LLL -D 'uid=anna,ou=usuaris,dc=edt,dc=org' -w anna 'uid=user01'

ldapmodify -x -D "uid=anna,ou=usuaris,dc=edt,dc=org" -w anna -f modify1.ldif

--modifying entry "uid=pau,ou=usuaris,dc=edt,dc=org"

--ldap_modify: Insufficient access (50)

ldapmodify -x -D "uid=anna,ou=usuaris,dc=edt,dc=org" -w anna -f modify1.ldif

--modifying entry "uid=anna,ou=usuaris,dc=edt,dc=org"

--ldap_modify: Insufficient access (50)

#02-->Tots poden modificar
dn: olcDatabase={1}mdb,cn=config
changetype: modify
delete: olcAccess
-
add: olcAccess
olcAccess: to * by * write
#------------------------------
ldapmodify -x -D "uid=anna,ou=usuaris,dc=edt,dc=org" -w anna -f modify1.ldif
--modifying entry "uid=anna,ou=usuaris,dc=edt,dc=org"
ldapmodify -x -D "uid=anna,ou=usuaris,dc=edt,dc=org" -w anna -f modify1.ldif
--modifying entry "uid=pau,ou=usuaris,dc=edt,dc=org"
# 03 -modificar un mateix llegir tots
dn: olcDatabase={1}mdb,cn=config
changetype: modify
delete: olcAccess
-
add: olcAccess
olcAccess: to by self write by * read
#------------------------------
ldapmodify -x -D "uid=anna,ou=usuaris,dc=edt,dc=org" -w anna -f modify1.ldif
--modifying entry "uid=pau,ou=usuaris,dc=edt,dc=org"
--ldap_modify: Insufficient access (50)
ldapmodify -x -D "uid=anna,ou=usuaris,dc=edt,dc=org" -w anna -f modify1.ldif
--modifying entry "uid=anna,ou=usuaris,dc=edt,dc=org"
ldapsearch -x -LLL -D 'uid=anna,ou=usuaris,dc=edt,dc=org' -w anna 'uid=pau'
# 04 -modificar passwd a si mateix y a tots autentificar i tots poden llegir
dn: olcDatabase={1}mdb,cn=config
changetype: modify
delete: olcAccess
-
add: olcAccess
olcAccess: to attrs=userPassword by self write by * auth
olcAccess: to * by * read
#------------------------------
ldappasswd -xv -D 'uid=anna,ou=usuaris,dc=edt,dc=org' -w anna -s anna
--ldap_initialize( <DEFAULT> )
--Result: Success (0)
ldapmodify -x -D "uid=anna,ou=usuaris,dc=edt,dc=org" -w anna -f modify1.ldif
--modifying entry "uid=anna,ou=usuaris,dc=edt,dc=org"
--ldap_modify: Insufficient access (50)
ldapmodify -x -D "uid=anna,ou=usuaris,dc=edt,dc=org" -w anna -f passwd.ldif 
--modifying entry "uid=pau,ou=usuaris,dc=edt,dc=org"
--ldap_modify: Insufficient access (50)
ldapmodify -x -D "uid=anna,ou=usuaris,dc=edt,dc=org" -w anna -f modify1.ldif
--modifying entry "uid=pau,ou=usuaris,dc=edt,dc=org"
--ldap_modify: Insufficient access (50)
ldapsearch -x -LLL -D 'uid=anna,ou=usuaris,dc=edt,dc=org' -w anna

#file passwd                                         
dn: uid=pau,ou=usuaris,dc=edt,dc=org
changetype: modify
replace: olcRootPW
olcRootPW: anna
#file modify
dn: uid=pau,ou=usuaris,dc=edt,dc=org
changetype: modify
add: homephone
homephone: 555-888-122
#(en algunos casos se cambiaba ana por pau para hacer las diferentes pruebas)


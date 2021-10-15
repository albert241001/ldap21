# LDAP Server
## @edt ASIX M06-ASO 2021-2022
### Servidor LDAP (Debian 11)

Podeu trobar les imatges docker al Dockehub de [edtasixm06](https://hub.docker.com/u/edtasixm06/)

Podeu trobar la documentació del mòdul a [ASIX-M06](https://sites.google.com/site/asixm06edt/)

ASIX M06-ASO Escola del treball de barcelona


 * **edtasixm06/ldap21:base** Servidor LDAP base inicial amb la base de dades edt.org

Crear una xarxa
```
docker network create hisx2
```

Crear un docker per un Dockerfile
```
docker build -t edtasixm06/ldap21:base .
```

Fer correr un container amb una network
```
docker run --rm --name ldap.edt.org -h ldap.edt.org --net hisx2 -d edtasixm06/ldap21:base
```

Veure containers actius
```
docker ps
```

Fer una recerca de totes les dades de la database edt org
```
ldapsearch -x -LLL -h ldap.edt.org -b 'dc=edt,dc=org'
```

Afegir usuaris desde un fitxer .ldif
```
ldapadd -x -c -h 172.17.0.2 -D 'cn=Manager,dc=edt,dc=org' -w secret -f afegir.ldif 
```

Eliminar desde un fitxer .ldif
```
ldapdelete -vx -h 172.17.0.2 -D 'cn=Manager,dc=edt,dc=org' -w secret -f eliminats.ldif 
```

* **ldapmodify**
*  **changetype(delete=ldapdelete////**
*  **add=ldapadd////**
*  **modify=permet modificar(replace=reemplazar/add=afegir info/delete=eliminar algo de info))**
*   **changetype:**
*    **add**
*    **delete**
*    **modify**
					
Cambiar clave de 
ldappasswd -x -v -h 172.17.0.2 -D 'cn=Manager,dc=edt,dc=org' -w secret -s jupiter 'cn=user01,ou=usuaris,dc=edt,dc=org'
ldappasswd -x -v -h 172.17.0.2 -D 'cn=Jordi Mas,ou=usuaris,dc=edt,dc=org' -w jordi -s jupiter
ldapmodify -cv -x -h 172.17.0.2 -D "cn=Marta Mas,ou=usuaris,dc=edt,dc=org" -w marta -f modify2.ldif 
ldapmodify -x -h 172.17.0.2 -D "cn=Manager,dc=edt,dc=org" -w secret -f modify1.ldif
ldapsearch -x -LLL -h 172.17.0.2 -b 'dc=edt,dc=org' 'cn=Marta Mas' dn homephone mail 
ldapcompare -x -h 172.17.0.2 'cn=Marta Mas,ou=usuaris,dc=edt,dc=org' mail:marta@edt.org
ldapsearch -x -LLL -h 172.17.0.2 -b 'dc=edt,dc=org' '(&(|(gidNumber=610)(gidNumber=611))(cn=user*))'


/etc/ldap/ldap.conf es configuració del client
Base dc=edt,dc=org
URI ldap.edt.org
docker run --rm --name client -h client --net 2hisix -it edtasixm06/ldap21:base /bin/bash



propagar port= vincular port de container amb port de la maquina real/servidor


0.0.0.0:389 totes les maquines de l'orinador tindran obert port 389
```









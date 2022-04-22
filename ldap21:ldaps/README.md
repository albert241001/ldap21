# LDAP Server
## @edt ASIX M06-ASO 2021-2022
### Servidor LDAP (Debian 11)

Podeu trobar les imatges docker al Dockehub de [edtasixm06](https://hub.docker.com/u/edtasixm06/)

Podeu trobar la documentació del mòdul a [ASIX-M06](https://sites.google.com/site/asixm06edt/)

ASIX M06-ASO Escola del treball de barcelona


 * **albert241001/ldap21:grups** Servidor LDAP amb la base de dades edt.org
   * Afegit una ou=grups.
   * Hem definit els següents grups amb el seus usuaris alumnes,professors,1asix,2asix,wheel,1wiam,2wiam,1hiaw.


```
$ docker run --rm --name ldap.edt.org -h ldap.edt.org --net 2hisix -p 389:389 -d albert241001/ldap21:grups

$ docker run --rm --name phpldapadmin.edt.org -h phpldapadmin.edt.org --net 2hisix -p 80:80 -d edtasixm06/phpldapadmin:20
```

This ldif file is intended to use with csa/codar.
Start openldap with dc=fictional,dc=com and import the ldif

I used docker images for openldap (osixia/openldap) and phpldaladmin (osixia/phpldapadmin).

The resulting ldap authentication and authorization was tested with CSA 4.8 and Codar 1.8

Started openldap through a script
#!/bin/bash
docker run --name openldap \
--restart=unless-stopped \
-p 389:389 \
-v /root/containerdata/openldap/database=/var/lib/ldap \
-v /root/containerdata/openldap/config=/etc/ldap/slapd.d \
-e LDAP_ORGANISATION="Fictional" \
-e LDAP_DOMAIN="fictional.com" \
-e LDAP_ADMIN_PASSWORD="HPES0ftware!" \
--detach osixia/openldap


phpldapadmin started:
#!/bin/bash
docker run \
--restart=unless-stopped \
--env PHPLDAPADMIN_LDAP_HOSTS=<FQDN or IP> \
-p 6443:443 \
-d osixia/phpldapadmin

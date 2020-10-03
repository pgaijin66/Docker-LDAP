# dockerldap

Run LDAP server in docker

# Creating the docker container:


docker volume create slapd-database
docker volume create slapd-config

docker run \
    -p 389:389 -p 636:636 \
    --name legacy-ldap \
    --volume slapd-database:/var/lib/ldap \
    --volume slapd-config:/etc/ldap/slapd.d \
    --env LDAP_ORGANISATION="SYSADMINSOCIETY" \
    --env LDAP_DOMAIN="sysadminsociety.com" \
    --env LDAP_ADMIN_PASSWORD="" \
    --hostname ldap.sysadminsociety.com  \
    --detach osixia/openldap:1.2.4

docker update --restart unless-stopped legacy-ldap


# test, after the export+import
% ldapwhoami -vvv -h ldap.sysadminsociety.com -D uid=pthapa,ou=people,dc=sysadminsociety,dc=com -x -W

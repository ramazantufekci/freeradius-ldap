Ben bu işlemi fortigate üzerinde google authentication kullanmak iiçin yaptım. Sen ne için kullanırsın ben karişmam
ilk openLDAP kurulumu
```
sudo apt install slapd ldap-utils
```
ister /etc/ldap/ldap.conf dosyasını düzenle istersen aşağıda sıraladığım dosyaları oluşturup ldapadd ile ekle sana kalmış
## base olarak bir yapı oluştur.
```
sudo ldapadd -x -D cn=admin,dc=ramazan,dc=local -W -f base.ldif
```
base.ldif
```
dn: ou=People,dc=ramazan,dc=local
objectClass: organizationalUnit
ou: People

dn: ou=Groups,dc=ramazan,dc=local
objectClass: organizationalUnit
ou: Groups
```
## Kullanıcının şifresini bu komutla oluştur md5 ile oluşturmak zorunda değilsin kendine göre düzenle
## kullanıcı için şifre oluştur
```
slappasswd -c md5
```
## Kullanıcı ekle
user.ldif
```
dn: uid=salam,ou=People,dc=ramazan,dc=local
objectClass: inetOrgPerson
objectClass: posixAccount
objectClass: shadowAccount
cn: Metin Inci
sn: Inci
uid: Salam
uidNumber: 10000
gidNumber: 10000
homeDirectory: /tmp/metin
loginShell: /bin/false
userPassword: {CRYPT}mdB3Xob.dEG5c
```

## Gruba kullanıcı ekle

group.ldif
```
dn: cn=Users,ou=Groups,dc=ramazan,dc=local
objectClass: posixGroup
cn: Users
gidNumber: 10000
memberUid: metin
```
## user eklemek için
```
sudo ldapadd -x -D cn=admin,dc=ramazan,dc=local -W -f user.ldif
```
## kullanıcı için şifre oluştur
```
slappasswd -c md5
```
### kullanıcıda değişiklik yapmak için
```
sudo ldapmodify -x -D cn=admin,dc=ramazan,dc=local -W -f user.ldif
```
newpass.ldif
```
dn: uid=metin,ou=People,dc=ramazan,dc=local
changetype: modify
replace: userPassword
userPassword: {CRYPT}mdLijIyPxllT2
```

### Google authenticator için
```
sudo -u freerad google-authenticator -t -d -f -r 3 -R 30 -w 3 -s /var/lib/freeradius/google-authenticator/salam/.google_authenticator
```








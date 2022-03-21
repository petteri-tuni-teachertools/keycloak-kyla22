



## Docker commands (modifications):

### LDAP

To expose LDAP outside of container:

``` 
docker build -t demo-ldap ldap
docker run --name demo-ldap -p 389:389 --net demo-network demo-ldap
``` 

## Test users:

User / Password / email

* u / p / petteri@localhost

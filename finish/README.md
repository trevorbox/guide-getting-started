# Testing certs..


mkdir /tmp/certs
cd /tmp/certs
openssl req -newkey rsa:2048 -nodes -keyout key.pem -x509 -days 365 -out certificate.pem

keytool -keystore $JAVA_HOME/lib/security/cacerts -storepass changeit -noprompt -trustcacerts -importcert -alias whatever -file /tmp/certs/tls.crt
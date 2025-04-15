## A générer

Issue de https://stackoverflow.com/questions/59738140/why-is-firefox-not-trusting-my-self-signed-certificate


openssl req -x509 -nodes \
  -newkey RSA:4096       \
  -keyout root-ca.key    \
  -days 3650             \
  -out root-ca.crt       \
  -subj '/C=FR/ST=Loire-Atlantique/L=Nantes/O=MyHome/CN=root_CA'

openssl req -nodes   \
  -newkey rsa:4096   \
  -keyout server.key \
  -out server.csr    \
  -subj '/C=FR/ST=Loire-Atlantique/L=Nantes/O=MyHome/CN=server'
  
openssl x509 -req    \
  -CA root-ca.crt    \
  -CAkey root-ca.key \
  -in server.csr     \
  -out server.crt    \
  -days 3650         \
  -CAcreateserial    \
  -extfile <(printf "subjectAltName = DNS:*.my.home.com\nauthorityKeyIdentifier = keyid,issuer\nbasicConstraints = CA:FALSE\nkeyUsage = digitalSignature, keyEncipherment\nextendedKeyUsage=serverAuth")


Installer alors le root-ca.crt dans Firefox et Windows.


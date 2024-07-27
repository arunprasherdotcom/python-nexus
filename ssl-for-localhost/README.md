# Install SSL on localhost NGINX on Ubuntu:


Generate a self-signed certificate and key:
```sh
openssl req -x509 -newkey rsa:4096 -keyout key.pem -out cert.pem -days 365 -nodes
```

Verify the certificate:
```sh
openssl x509 -in /var/www/expressthat/api.expressthat.ca/express_apis/cert.pem -text -noout
```

Verify the RSA private key:
```sh
openssl rsa -in /var/www/expressthat/api.expressthat.ca/express_apis/key.pem -check -noout
```

Set permissions for the certificate and key files:
```sh
chmod 400 cert.pem 
chmod 400 key.pem 
chmod 600 cert.pem 
chmod 600 key.pem 
```

Generate a CA key and certificate:
```sh
openssl genrsa -out CA.key -des3 2048
openssl req -x509 -sha256 -new -nodes -days 3650 -key CA.key -out CA.pem
```

Create a localhost certificate and key with extensions:
```sh
openssl req -x509 -out localhost.crt -keyout localhost.key -newkey rsa:2048 -nodes -sha256 -subj '/CN=localhost' -extensions EXT -config <( \
printf "[dn]\nCN=localhost\n[req]\ndistinguished_name = dn\n[EXT]\nsubjectAltName=DNS:localhost\nkeyUsage=digitalSignature\nextendedKeyUsage=serverAuth")
```

Generate a root CA key and certificate:
```sh
openssl genrsa -des3 -out rootCA.key 2048
openssl req -x509 -new -nodes -key rootCA.key -sha256 -days 1024 -out rootCA.pem
```

Create a new private key and CSR (Certificate Signing Request):
```sh
sudo openssl req -newkey rsa:2048 -keyout /etc/ssl/private/PRIVATEKEY.key -out /etc/ssl/certs/MYCSR.csr -nodes
```

Install the certificate using certutil:
```sh
certutil -d sql:$HOME/.pki/nssdb -A -t "CT,c,c" -n "localhost" -i localhost.crt
```

Set ownership and permissions for the certificate and key files:
```sh
sudo chown root:adminuser /etc/ssl/private/localhost.key
sudo chmod 640 /etc/ssl/private/localhost.key
sudo chown root:adminpc /etc/ssl/certs/localhost.crt
sudo chmod 644 /etc/ssl/certs/localhost.crt
```
Restart NGINX to apply the changes:
```sh
sudo service nginx restart
```


Read more...

[Adding self trusted SSL certificate for localhost on Ubuntu(NGINX)](https://medium.com/internshala-tech/adding-self-trusted-ssl-certificate-for-localhost-on-ubuntu-nginx-c66d70b22e4b)





# Create a trusted SSL certificate using mkcert on Ubuntu:

Install mkcert

First, install mkcert on your Ubuntu system:
```sh
sudo apt update
sudo apt install mkcert
```

Create a certificate authority (CA)
Next, create a certificate authority (CA) using mkcert:

```sh
mkcert -install
```
This will create a CA certificate and private key in ~/.local/share/mkcert/.
Create a certificate for your local domain

Now, create a certificate for your local domain (e.g., project.loc):
```sh
mkcert project.loc
```
This will create a certificate and private key for project.loc in the current directory.
Configure NGINX to use the certificate

Update your NGINX configuration file to use the generated certificate:
```sh
server {
    listen 443 ssl;
    server_name project.loc;

    ssl_certificate project.loc+1.pem;
    ssl_certificate_key project.loc+1-key.pem;

    # ... other configuration options ...
}
```
Add the CA to your system's trusted store


To trust the certificate, you need to add the CA certificate to your system's trusted store. On Ubuntu, you can do this by running:
```sh
sudo cp ~/.local/share/mkcert/rootCA.pem /usr/local/share/ca-certificates/
sudo update-ca-certificates
```
This will add the CA certificate to the system's trusted store.

Verify the certificate

Restart NGINX and verify that the certificate is working by visiting https://project.loc in your web browser. You should see a secure connection with no warnings.

That's it! You now have a trusted SSL certificate for your local development environment using mkcert on Ubuntu.


# Issue
after all attemps on local green arrow not showing for SSL.


# Install SSL on localhost (Simplest Way):

```sh
openssl genpkey -algorithm RSA -out project.loc.key
openssl req -new -key project.loc.key -out project.loc.csr
openssl x509 -req -days 365 -in project.loc.csr -signkey project.loc.key -out project.loc.crt
```
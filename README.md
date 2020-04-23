# Install squid & squidGuard on Centos 7

## I plan to convert this to an Anisble Play to make deployment quick/easy

### Install EPEL Latest Release 7
    yum install https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm

### Install squid
    sudo yum install squid

### Install squidGuard
    sudo yum install squidGuard

### Generate CA Cert
    openssl req -new -newkey rsa:2048 -sha256 -days 365 -nodes -x509 -extensions v3_ca -keyout squid-ca-key.pem -out squid-ca-cert.pem

### Create CA DER for client side import
    openssl x509 -in squid-ca-cert.pem -outform DER -out squid-ca-cert.der

### Combine certs
    cat squid-ca-cert.pem squid-ca-key.pem >> squid-ca-cert-key.pem

### Move the combined cert to /etc/squid/certs
    sudo mkdir /etc/squid/certs
    sudo mv squid-ca-cert-key.pem /etc/squid/certs/
    sudo chown -R squid:squid /etc/squid/certs

### Use the included config files, placing them in the /etc/squid directory

### Confirm squid.conf
    sudo squid -k parse

### Create SSL DB
    sudo /usr/lib64/squid/ssl_crtd -c -s /var/lib/ssl_db
    sudo chown -R squid:squid /var/lib/ssl_db

### Create blacklists directory
    mkdir /var/squidGuard/blacklists

### Use the included blacklists to get you started
    chown -R squid:squid /var/squidGuar/blacklists

### Modify selinux accordingly

### Modify firewalld accordingly

### Enable/Start squid & squidGuard
    sudo systemctl enable squid
    sudo systemctl start squid
    sudo systemctl enable squidGuard
    sudo suystemctl start squidGuard

### Modify the whitelisted domains file for any domains you wish to allow during the allowedtime directive

Role Name
=========

This simply creates RFC2136 conform zones - to be precisen a _acme-challenge.zone.tld delegation. This Zone can be secure updated with [certbot](https://certbot-dns-rfc2136.readthedocs.io/en/stable/)

This role can also be combined with https://github.com/geerlingguy/ansible-role-certbot or https://github.com/geerlingguy/ansible-role-certbot


Requirements
------------

Create your key with: 

`rndc-confgen -a -A hmac-sha512 -k "certbot." -c /etc/bind/certbot.key -b 512`

put the key hash in `bind_acme_rndc_key` certbot.key is not longer needed.

If the parent zone is not on the same box, you need a delegation: 
```
_acme-challenge.example.com. 300 IN  NS      this-box.example.com.
```

afterwards you can order certificates with e.g.

```
certbot  --dns-rfc2136 --dns-rfc2136-credentials /etc/letsencrypt/dns_rfc2136_credentials.txt -d example.com -d *.example.com -i nginx --dns-rfc2136-propagation-seconds 20 --key-type ecdsa -i nginx
```

Role Variables
--------------

see defaults/main.yml

Dependencies
------------

nothing fancy

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:
```
    - hosts: servers
      roles:
         - bind-acme-zone
      vars:
        bind_acme_zones:
          - example.com
        bind_acme_nameservers:
          - 127.0.0.1      
        bind_acme_rndc_key: <your rndc key>
```
License
-------

GPL-3.0 license

Author Information
------------------

Contact me

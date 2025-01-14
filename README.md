# Using Multiple Email Services for a Single Domain (Google Mail and Ubuntu SMTP Mail Server)

#### 1. Install Postfix to configure SMTP Server. SMTP uses [25/TCP].
```bash
root@mail:~# sudo apt -y install postfix sasl2-bin
# on this example, proceed to select [No Configuration] because configure all manually
```

#### 2. Configure Postfix
```bash
root@mail:~# sudo cp /usr/share/postfix/main.cf.dist /etc/postfix/main.cf
root@mail:~# sudo nano /etc/postfix/main.cf
```

#### 3. Copy all the [main.cf](main.cf) file's content to this file /etc/postfix/main.cf 
```bash
root@mail:~# sudo cp /usr/share/postfix/main.cf.dist /etc/postfix/main.cf
root@mail:~# sudo nano /etc/postfix/main.cf

# line 98 : specify your hostname
myhostname = mail.yourdomain.com

# line 106 : specify your domainname
mydomain = yourdomain.com

# Run these commands to restart postfix
root@mail:~# newaliases
root@mail:~# systemctl restart postfix
```

#### 4. Configure Dovecot
```bash
root@mail:~# sudo apt -y install dovecot-core dovecot-imapd dovecot-pop3d
root@mail:~# sudo nano /etc/dovecot/dovecot.conf

# line 30 : uncomment
listen = *, ::
root@mail:~# sudo nano /etc/dovecot/conf.d/10-auth.conf

# line 10 : uncomment and change (allow plain text auth)
disable_plaintext_auth = no

# line 100 : add
auth_mechanisms = plain login
root@mail:~# sudo nano /etc/dovecot/conf.d/10-mail.conf

# line 30 : change to Maildir
mail_location = maildir:~/Maildir
root@mail:~# sudo nano /etc/dovecot/conf.d/10-master.conf

# line 110-112 : uncomment and add
  # Postfix smtp-auth
  unix_listener /var/spool/postfix/private/auth {
    mode = 0666
    user = postfix
    group = postfix
  }

root@mail:~# systemctl restart dovecot
```






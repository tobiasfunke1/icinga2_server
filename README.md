# icinga2_server

### change ssh port for hosts:
```
hosts.conf
object Host "hostname123" {
...
vars.ssh_port = xx22
...
}

services.conf
apply Service "ssh" {
 ...
 check_command = "ssh"
 vars.port = host.vars.ssh_port
 ...
}

[https://monitoring-portal.org/woltlab/index.php?thread/32752-ssh-monitoring-different-port/]
```
### nginx
```
server {
        listen 10.x.x.x:80;
        listen 127.0.0.1:80;
        server_name 10.x.x.x,127.0.0.1,localhost;

        location ~ ^/icingaweb2/index\.php(.*)$ {
                fastcgi_pass unix:/run/php/php7.2-fpm.sock;
                fastcgi_index index.php;
                include fastcgi_params;
                fastcgi_param SCRIPT_FILENAME /usr/share/icingaweb2/public/index.php;
                fastcgi_param ICINGAWEB_CONFIGDIR /etc/icingaweb2;
                fastcgi_param REMOTE_USER $remote_user;
        }
        location ~ ^/icingaweb2(.+)? {
                alias /usr/share/icingaweb2;
                index index.php;
                try_files $1 $uri $uri/ /icingaweb2/index.php$is_args$args;
        }
}
```
### add german language
```
icingacli module enable translation
sudo apt install language-pack-de
sudo dpkg-reconfigure locales
sudo service icinga2 restart
sudo service nginx restart
sudo service php7.2-fpm restart

```
### ufw
```
# ssh
sudo ufw allow xx22
# wireguard
sudo ufw allow 5xxxx/udp
# vpn
sudo ufw allow from 10.x.x.0/24
sudo ufw enable
sudo ufw status verbose
```

# For Ubuntu

sudo apt-get update
sudo apt-get install nginx

#### ps for processes, a all users, u for detailed listing and x for including boot processes. pipe and grep is used to filter the result with nginx

ps aux | grep nginx

#### main folder of nginx

/etc/nginx

#### installing dependencies to compile nginx

sudo apt-get update

#### downloading nginx source code

wget https://nginx.org/download/nginx-1.25.2.tar.gz

#### extract the tarball

tar -zxvf nginx-1.25.2.tar.gz

#### in order to compile nginx we have to install a compiler

sudo apt-get install build-essential

#### configure source code for build

sudo apt-get install libpcre3 libpcre3-dev zlib1g zlib1g-dev libssl-dev
cd nginx-1.25.2

#### configuring. sbin-path is the location of nginx executable which can start and stop the service. conf-path is the path of config file. pid-path is the process id path.

./configure --sbin-path=/usr/bin/nginx --conf-path=/etc/nginx/nginx.conf --error-log-path=/var/log/nginx/error.log --http-log-path= /var/log/nginx/access.log --with-pcre --pid-path=/var/run/nginx.pid --with-http_ssl_module

#### to compile the source run:

make
make install

#### file will be installed in /etc/nginx. to see the configuration and the version.

nginx -V

#### start nginx
nginx

#### to configure nginx as a service to manage nginx compiled version, systemctl is the command of systemd:

nginx -s stop

Save this file as /lib/systemd/system/nginx.service

[Unit]
Description=The NGINX HTTP and reverse proxy server
After=syslog.target network-online.target remote-fs.target nss-lookup.target
Wants=network-online.target

[Service]
Type=forking
PIDFile=/var/run/nginx.pid
ExecStartPre=/usr/bin/nginx -t
ExecStart=/usr/bin/nginx
ExecReload=/usr/sbin/nginx -s reload
ExecStop=/bin/kill -s QUIT $MAINPID
PrivateTmp=true

[Install]
WantedBy=multi-user.target

systemctl start nginx
systemctl status nginx
systemctl enable nginx <!-- to configure nginx to start on boot -->


### the benefit of building from source is that we can extend the package which cannot be don without building. We can extend nginx by using bundeled modules which come with the nginx and 3rd party modules. we can compile with bundeled module using --with-<name-of-module>\_module

---

---

# For CentOS

#### yum do not come with nginx source in its repository so we are providing new source

yum check-update
yum install epel-release
yum install nginx

#### in centOS nginx is not started by default

ps aux | grep nginx

#### starting nginx

service nginx start

#### downloading nginx source code

curl https://nginx.org/download/nginx-1.25.2.tar.gz

#### extract the tarball

tar -zxvf nginx-1.25.2.tar.gz

#### in order to compile nginx we have to install a compiler

yum groupinstall "Development Tools"

#### configure source code for build

yum install pcre pcre-devel zlib zlib-devel openssl openssl-devel
cd nginx-1.25.2
./configure

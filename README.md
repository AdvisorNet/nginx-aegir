# nginx-aegir
notes for converting an aegir install from apache to nginx

## How nginx files are called.

In the nginx configuration in /etc/nginx  we include /var/aegir/config/nginx.conf this is a symbolic link to /var/aegir/config/{server_name}/nginx.conf

In this nginx.conf file it does the bulk of the common configuration for all hosts.  

At the end of this file it include a variety of other folders.

 #######################################################
 ###  nginx virtual domains
 #######################################################
 
 # virtual hosts
 include /var/aegir/config/server_master/nginx/pre.d/*;
 include /var/aegir/config/server_master/nginx/platform.d/*;
 include /var/aegir/config/server_master/nginx/vhost.d/*;
 include /var/aegir/config/server_master/nginx/post.d/*;

The important one here is the vhost.d folder.

Ticking the nginx box in the aegir control panel only changes the vhost configuration and future vhost additions. It doesn't create the core config files.


## Steps required to convert Apache2 to nginx.

1. We first need to install nginx and php-fpm  (also upgrade to php 5.6 for speed improvements).
2. Create a symbolic link from /etc/nginx/conf.d/aegir.conf -> /var/aegir/conf/nginx.conf
3. Copy the nginx.conf file from here (config/server_master/nginx.conf) to /var/aegir/conf/{server_name}/nginx.conf
4. edit the nginx.conf file and in the virtual hosts section edit all 4 mentions of server_master to the server_name
5. copy the config/includes/nginx_vhost_common.conf file to the same location on the server.
6. Fix bad-behavior module.
7. Stop apache then start nginx and php-fpm.  Change systemd so it starts nginx and php-fpm on boot and disable apache.
8. Test everything is working.







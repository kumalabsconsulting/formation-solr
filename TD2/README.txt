#############################################################
#                                                           #
#         FORMATION SOLR                                    #
#                                                           #
#############################################################

### TD2 - Rendre inaccessible solr en direct avec un proxy ###
Lancez l'installation de ngnix

    sudo apt install ngnix -y


Supprimez la configuration par défaut:

    sudo rm -rf /etc/ngnix/conf.d/* /etc/ngnix/site-enable/* /etc/ngnix/site-available/*

Ajouter la nouvelle configuration par défaut en fonction du nom de votre machine

    sudo nano /etc/ngnix/site-available/default

########## CONTENU DU FICHIER default #############################
server {
    listen       80 default_server;
    listen       [::]:80 default_server;
    server_name  *.kumalabs.consulting;
    root         /usr/share/nginx/html;

    # Load configuration files for the default server block.
    include /etc/nginx/default.d/*.conf;

    if ($scheme != "https") {
        return 301 https://$host$request_uri;
    }
}
##################################################################

Ajoutez la configuration pour le SSL:

    sudo nano /etc/ngnix/conf.d/solr.conf

########## CONTENU DU FICHIER solr.conf #############################
server {
    listen       443 ssl http2 default_server;
    listen       [::]:443 ssl http2 default_server;
    server_name  *.kumalabs.consulting;
    root         /usr/share/nginx/html;

    ssl_certificate /etc/letsencrypt/live/bdx0.kumalabs.consulting/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/bdx0.kumalabs.consulting/privkey.pem; # managed by Certbot

    # Load configuration files for the default server block.
    include /etc/nginx/default.d/*.conf;

    location / {
    }
    # This is our Solr instance
# We will access it through SSL instead of using the port directly
location /solr {
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_pass "http://localhost:8983";
}

error_page 404 /404.html;
    location = /40x.html {
}

error_page 500 502 503 504 /50x.html;
    location = /50x.html {
}
}

ATTENTION => remplacez bdx0.kumalabs.consulting par le nom de votre machine ex=> bdx1.kumalabs.consulting


##################################################################
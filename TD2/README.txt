#############################################################
#                                                           #
#         FORMATION SOLR                                    #
#                                                           #
#############################################################

### TD2 - Rendre accessible solr en direct avec un reverse-proxy ###
Lancez l'installation de nginx

    sudo apt install nginx -y


Supprimez la configuration par défaut:

    sudo unlink /etc/nginx/sites-enabled/default
    sudo rm -rf /etc/nginx/conf.d/* /etc/nginx/sites-enable/* /etc/nginx/sites-available/*

Ajouter la nouvelle configuration par défaut en fonction du nom de votre machine

    sudo nano /etc/nginx/sites-available/default

##### DEBUT COPIER #####
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
##### FIN COPIER #####

## Activez la config par defaut

    sudo ln -s /etc/nginx/sites-available/default  /etc/nginx/sites-enabled/default

## Ajoutez la configuration pour le SSL:

    sudo nano /etc/nginx/conf.d/solr.conf

##### DEBUT COPIER #####
server {
    listen       443 ssl http2 default_server;
    listen       [::]:443 ssl http2 default_server;
    server_name  *.kumalabs.consulting;
    root         /usr/share/nginx/html;

    ssl_certificate /etc/letsencrypt/live/bdx0.kumalabs.consulting/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/bdx0.kumalabs.consulting/privkey.pem; # managed by Certbot

    # Load configuration files for the default server block.
    include /etc/nginx/default.d/*.conf;

    # This is our Solr instance
# We will access it through SSL instead of using the port directly
location / {
    proxy_set_header Host \$host;
    proxy_set_header X-Real-IP \$remote_addr;
    proxy_pass "http://localhost:8983";
}

error_page 404 /404.html;
    location = /40x.html {
}

error_page 500 502 503 504 /50x.html;
    location = /50x.html {
}
}
##### FIN COPIER #####


ATTENTION => remplacez bdx0.kumalabs.consulting par le nom de votre machine ex=> bdx1.kumalabs.consulting

##### DEBUT COPIER #####
sudo sed -i "s/bdx0/bdx1/g" /etc/nginx/conf.d/solr.conf
##### FIN COPIER #####



## Relancez le service nginx

    sudo systemctl restart nginx

## Vérifiez que le service est UP

    sudo systemctl status nginx.service

## Votre installation sera accessible sur l'URL

    https://bdxY.kumalabs.consulting/solr/  => REMPLACEZ Y par le numéro de votre machine

===>>> On remarquera que solr est accessible à tous sans mot de passe ! <<<===

####################### FIN TD2 ################################
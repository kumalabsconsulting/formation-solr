#############################################################
#                                                           #
#         FORMATION SOLR                                    #
#                                                           #
#############################################################

### TD5 - Schema
## Arrêtez le solr standalone

    sudo systemctl stop solr.service

## Démarrons un cluster solr (cloud)

    sudo mkdir -p /opt/solr/example/cloud/node1/solr /opt/solr/example/cloud/node2/solr /opt/solr/example/cloud/node1/logs /opt/solr/example/cloud/node2/logs
    sudo chown -R solr:root /opt/solr-9.0.0/
    sudo chown solr:root /etc/default/solr.in.sh
    sudo -i
    su - solr bash -c '/opt/solr/bin/solr start -v -e cloud'

(specify 1-4 nodes) [2]: => entrée
Please enter the port for node1 [8983]: =>entrée
Please enter the port for node2 [7574]: => entrée
Please provide a name for your new collection: [gettingstarted] => techproducts => entrée
How many shards would you like to split techproducts into? [2] => entrée
How many replicas per shard would you like to create? [2] => entrée
_default or sample_techproducts_configs [_default] => sample_techproducts_configs => entrée


## Modifiez le nginx pour faire le loadbalancing

editez le fichier /etc/nginx/conf.d/solr.conf

####### Copier #######
upstream solr {

    server localhost:8983      weight=5;
    server localhost:7574      weight=5;
}
server {
    listen       443 ssl http2 default_server;
    listen       [::]:443 ssl http2 default_server;
    server_name  *.kumalabs.consulting;
    root         /usr/share/nginx/html;

    ssl_certificate /etc/letsencrypt/live/form0.kumalabs.consulting/fullchain.pem; # managed by Certbot
    ssl_certificate_key /etc/letsencrypt/live/form0.kumalabs.consulting/privkey.pem; # managed by Certbot

    # Load configuration files for the default server block.
    include /etc/nginx/default.d/*.conf;

    # This is our Solr instance
# We will access it through SSL instead of using the port directly
location / {
    proxy_set_header Host \$host;
    proxy_set_header X-Real-IP \$remote_addr;
    proxy_pass "http://solr";
}

error_page 404 /404.html;
    location = /40x.html {
}

error_page 500 502 503 504 /50x.html;
    location = /50x.html {
}
}
######### FIN COLLE ######

sed -i "s/form0/bdx0/g" /etc/nginx/conf.d/solr.conf

nginx -s reload
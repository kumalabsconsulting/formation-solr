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
Please provide a name for your new collection: [gettingstarted] => entrée
How many shards would you like to split gettingstarted into? [2] => techproducts => entrée
How many replicas per shard would you like to create? [2] => entrée
_default or sample_techproducts_configs [_default] => sample_techproducts_configs => entrée
#############################################################
#                                                           #
#         FORMATION SOLR                                    #
#                                                           #
#############################################################

### TD1 - Installation standalone solr ###
Téléchargez les binaires:

    wget https://www.apache.org/dyn/closer.lua/solr/solr/9.0.0/solr-9.0.0.tgz?action=download -O solr-9.0.0.tgz

Lancez la commande suivante pour en extraire le fichier d'installation:

    tar xzf solr-9.0.0.tgz solr-9.0.0/bin/install_solr_service.sh --strip-components=2

Installer solr grâce à l'installeur automatisé:

    sudo bash ./install_solr_service.sh solr-9.0.0.tgz -i /opt -d /var/solr -u solr -s solr -p 8983

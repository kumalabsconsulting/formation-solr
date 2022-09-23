#############################################################
#                                                           #
#         FORMATION SOLR                                    #
#                                                           #
#############################################################

### TD1 - Installation standalone solr ###
## Installation du prérequis JAVA

    sudo apt install -y openjdk-18-jre

## Téléchargez les binaires:

    wget https://www.apache.org/dyn/closer.lua/solr/solr/9.0.0/solr-9.0.0.tgz?action=download -O solr-9.0.0.tgz

## Lancez la commande suivante pour en extraire le fichier d'installation:

    tar xzf solr-9.0.0.tgz solr-9.0.0/bin/install_solr_service.sh --strip-components=2

## Installer solr grâce à l'installeur automatisé:

    sudo bash ./install_solr_service.sh solr-9.0.0.tgz -i /opt -d /var/solr -u solr -s solr -p 8983

## Controlez si le service est up

    sudo service solr status

## Verifiez le Solr Home Directory présent dans le script de démarrage

    sudo grep SOLR_ENV /etc/init.d/solr

 # récupéré le contenu de SOLR_ENV puis faire un

    sudo grep SOLR_HOME CONTENU_PATH_DE_LA_COMMANDE_PRECEDENTE

## Vérifiez la présence du répertoire Solr Home Directory

    sudo ls -l /var/solr/data

## Vérifiez le port d'écoute du service

    sudo netstat -nlpt|grep 8983

## Arrêtez le standalone solr

    sudo systemctl stop solr.service

## Démarrons un cluster solr (cloud)

    sudo mkdir -p /opt/solr/example/cloud/node1/logs /opt/solr/example/cloud/node2/logs
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


## Un message Warning peut apparaître sur le nombre de fichier ouvert.

*** [WARN] *** Your open file limit is currently 1024.
 It should be set to 65000 to avoid operational disruption.
 If you no longer wish to see this warning, set SOLR_ULIMIT_CHECKS to false in your profile or solr.in.sh

En tant que root:

    sudo nano /etc/security/limits.conf

solr hard nofile 65535
solr soft nofile 65535
solr hard nproc 65535
solr soft nproc 65535


## Ajout de l'authentification admin
    sudo -i
    su - solr bash -c '/opt/solr/bin/solr auth enable -type basicAuth -prompt true -z localhost:9983'

Veuillez indiquer admin en login et Bdx33;; en mdp.


Vous remarquerez que le service écoute sur 127.0.0.1:8983 et reste donc inaccessible en dehors de la machine. Il nous faut un reverse proxy pour y accèder depuis l'extérieur (voir TD2)
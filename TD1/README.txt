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

Vous remarquerez que le service écoute sur 127.0.0.1:8983 et reste donc inaccessible en dehors de la machine.
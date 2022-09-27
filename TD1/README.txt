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

## Verifiez si le Solr Home Directory est présent dans le script de démarrage

    sudo grep SOLR_ENV /etc/init.d/solr

 # récupéré le contenu de SOLR_ENV puis faire un

    sudo grep SOLR_HOME CONTENU_PATH_DE_LA_COMMANDE_PRECEDENTE

## Vérifiez la présence du répertoire Solr Home Directory

    sudo ls -l /var/solr/data

## Vérifiez le port d'écoute du service

    sudo apt install -y net-tools;sudo netstat -nlpt|grep 8983

## Ajout de notre utilisateur au group solr

    sudo -i
    usermod bdxuser -G solr

## Creation de notre premier Solr Core

    sudo -i
    su - solr bash -c '/opt/solr/bin/solr create -c laposte -s 2 -rf 2'

## Import de notre fichier CSV

    su - solr bash -c '/opt/solr/bin/post -c laposte laposte_custom.json -u "solr:Bdx33;;"'
    curl "http://localhost:8983/solr/laposte/select?q=*"
    curl "http://localhost:8983/solr/laposte/select?q=*"|jq -r '.'

## Déplacez-vous vers le fichier schema.xml

    sudo -i
    su - solr
    cd /var/solr/data/laposte/conf
    more managed-schema.xml

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

    apt-get install haveged -y

########## FIN TD1 ##########

#############################################################
#                                                           #
#         FORMATION SOLR                                    #
#                                                           #
#############################################################

### TD3 - Creation de la 1er collection ###
## ouvrez votre terminal et tapez les commandes suivante pour passer en tant qu'utilisateur solr

    sudo -i
    su - solr


## Placez-vous dans le répertoire bin de solr:

    cd /opt/solr/bin

## Creation d'une nouvelle collection

    ./solr create -c journeaux

solr@form0:/opt/solr/bin$ ./solr create -c  journeaux
WARNING: Using _default configset with data driven schema functionality. NOT RECOMMENDED for production use.
         To turn off: bin/solr config -c journeaux -p 8983 -action set-user-property -property update.autoCreateFields -value false

Created new core 'journeaux'




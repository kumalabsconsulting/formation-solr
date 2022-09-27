#############################################################
#                                                           #
#         FORMATION SOLR                                    #
#                                                           #
#############################################################

### TD3 - Schema
## Le but est de constatez le managed-schema (la prise en charge automatisé d'un import de données sans définition de schema)
## Nous allons reprendre la collection laposte.

    sudo -i
    su - solr
    cd /var/solr/data/laposte/conf
    vi managed-schema.xml

## En editant le fichier managed-schema.xml, parcourrez le et vous remarquerez qu'il y a

    * des dynamicField
    * des copyField

## Vous remarquez aussi que le nom des fields ont été automatiquement généré. Ce core est actuellement managé automatiquement car nous n'avons pas préciser de schéma.


#### Nous allons supprimer puis créer à nouveau le core laposte
#### Suppression de la collection laposte

    # Methode 1 (commandline)
    sudo -i
    su - solr bash -c '/opt/solr/bin/solr delete -c laposte'

    # Methode 2 (API)
    curl -u "solr:Bdx33;;" 'http://localhost:8983/solr/admin/cores?action=UNLOAD&core=laposte&deleteIndex=true&deleteDataDir=true&deleteInstanceDir=true'


#### Creation de la nouvelle collection laposte

    su - solr bash -c '/opt/solr/bin/solr create -c laposte'



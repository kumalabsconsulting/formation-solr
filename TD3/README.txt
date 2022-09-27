#############################################################
#                                                           #
#         FORMATION SOLR                                    #
#                                                           #
#############################################################

### TD3 - Sécurisation
## Ajout de l'authentification pour le compte admin avec le role admin

    sudo -i
    su - solr

## Création du fichier security.json

    . /etc/default/solr.in.sh; vi $SOLR_HOME/security.json

########## COPIER ICI ##########
{
"authentication":{
   "blockUnknown": true,
   "class":"solr.BasicAuthPlugin",
   "credentials":{"solr":"IV0EHq1OnNrj6gvRCwvFwTrZ1+z1oBbnQdiVC3otuq0= Ndd7LKvVBAaZIF0QAVi1ekCfAJXr1GGfLtRUXhgrF8c="},
   "realm":"My Solr users",
   "forwardCredentials": false
},
"authorization":{
   "class":"solr.RuleBasedAuthorizationPlugin",
   "permissions":[{"name":"security-edit",
      "role":"admin"}],
   "user-role":{"solr":"admin"}
}}
######################################################################

## Création des utilisateurs admin et bdxuser

    curl -u solr:SolrRocks  http://localhost:8983/api/cluster/security/authentication -H 'Content-type:application/json' -d '{"set-user": {"admin":"Bdx33;;", "bdxuser":"Bdx33;;"}}'

## Modifions le mot de passe de l'utilisateur solr

    ??? => A votre avis, comment pourrait-on faire ?

## Editer le fichier de configuration Solr
    vi /etc/default/solr.in.sh
    # Ajoutez les deux lignes
    SOLR_AUTH_TYPE="basic"
    SOLR_AUTHENTICATION_OPTS="-Dbasicauth=solr:Bdx33;;"

=> Cela permettra aux commande bin/solr de foncitonner avec la basic-auth
# Tips et commandes utiles machine linux

**Erreur**
La commande n'a pas pu être trouvée car « /usr/bin:/bin » n'est pas incluse dans la variable d'environnement PATH.
ls : commande introuvable
```bash
export PATH=$PATH:/usr/bin:/bin
```
**Obtenir le lien Yarn sur HDFS**
```bash
hdfs getconf -confKey yarn.resourcemanager.webapp.address
```
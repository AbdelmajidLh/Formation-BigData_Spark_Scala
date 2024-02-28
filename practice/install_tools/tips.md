# Tips et commandes utiles machine linux

**Erreur**
La commande n'a pas pu être trouvée car « /usr/bin:/bin » n'est pas incluse dans la variable d'environnement PATH.
ls : commande introuvable
```bash
# methode 1
export PATH=$PATH:/usr/bin:/bin

# methode 2 :
## ouvrire le fichier .bashrc
nano ~/.bashrc

## ajoute la ligne a la fin du fichier .bashrc
export PATH=$PATH:/usr/bin:/bin

## lancer cette commande
source ~/.bashrc
```
**Obtenir le lien Yarn sur HDFS**
```bash
hdfs getconf -confKey yarn.resourcemanager.webapp.address
```

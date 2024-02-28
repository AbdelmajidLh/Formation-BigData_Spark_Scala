# Tips et commandes utiles machine linux

## Linux Tips
Afficher la liste des utilisateurs sur votre machine
```bash
cut -d: -f1 /etc/passwd
```

Afficher les groups
```bash
cat /etc/group
```

Créer un nouveau group
```bash
sudo groupadd hduser_
```

Partager un repertoire entre le user abdel et le user hduser_
```bash
sudo chown -R abdel:hduser_ Formation-BigData_Spark_Scala/
```


## HDFS tips
Obtenir le lien Yarn sur HDFS
```bash
hdfs getconf -confKey yarn.resourcemanager.webapp.address
```
## Erreurs et solution

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

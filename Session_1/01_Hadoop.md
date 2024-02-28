# Guide des Commandes Hadoop

## Démarrage d'un Cluster HDFS et YARN

### Démarrage du service HDFS
```bash
# aller sur le compte dhuser_
su - hduser_

# aller dans le dossier sbin/
cd $HADOOP_HOME/sbin

# démarrer le cluster hadoop
./start-dfs.sh

```

### Démarrage du service YARN
```bash
./start-yarn.sh
```
### Vérifier le status des services 
    ```bash
    jps
    ```
### Obtenir les liens Web 
    ```bash
    # lien ressource manager - Yarn
    hdfs getconf -confKey yarn.resourcemanager.webapp.address

    # lien nemenode HDFS
    hdfs getconf -confkey dfs.namenode.http-address

    # datanodes sur le port 50075 (http://datanode1.example.com:50075)
    hdfs dfsadmin -report | grep "Name: DataNode" | awk '{print $2}'
    ```

## Commandes de Gestion du HDFS

### Afficher les informations de l'espace de stockage HDFS
```bash
hdfs dfsadmin -report
```

### Créer un répertoire dans HDFS
```bash
# exemple data/ et test/
## Pour la premiere fois il faut utiliser l'option `-p`
hdfs dfs -mkdir -p data
hdfs dfs -mkdir -p test
```

### Afficher les dossiers créés dans HDFS
```bash
# tous les dossiers à la racine
hdfs dfs -ls /
```

### Supprimer un dossier et son contenu dans HDFS
```bash
# supprimer le dossier test
hdfs dfs -rm -r test/
```
### Télécharger un fichier sur linux [commande shell et pas HDFS]
Cette commande vous permet de télécharger un fichier csv qu'on va utiliser dans notre workshop. C'est une commande purement `shell` et le fichier sera déposé sur votre machine en local.

```bash

# Définir le répertoire de destination
destination_dir="dossier_local"

# Créer le répertoire s'il n'existe pas
mkdir -p "$destination_dir"

# Télécharger le fichier CSV
wget -O "$destination_dir/zipcodes.csv" "https://raw.githubusercontent.com/vega/vega-datasets/master/data/zipcodes.csv"

# Vérifier si le téléchargement a réussi
if [ $? -eq 0 ]; then
    echo "Le fichier a été téléchargé avec succès dans $destination_dir."
else
    echo "Une erreur s'est produite lors du téléchargement du fichier."
fi

```
### Copier un fichier local dans HDFS
On va compier le fichier `zipcodes.csv` dans HDFS (dosier data). **Asurez vous d'être dans le dossier principal et avec le user hduser_**
```bash
hdfs dfs -copyFromLocal /Session_1/files/zipcodes.csv /data
```

### Afficher le contenu d'un répertoire HDFS
```bash
hdfs dfs -ls /chemin/vers/le/repertoire
```

## Commandes de Gestion de YARN

### Afficher les informations sur les ressources du cluster YARN
```bash
yarn node -list
```

### Afficher les applications en cours d'exécution sur YARN
```bash
yarn application -list
```

### Arrêter le service HDFS
```bash
stop-dfs.sh
```

### Arrêter le service YARN
```bash
stop-yarn.sh
```

```

N'oubliez pas de remplacer `/chemin/vers/le/repertoire` et `/chemin/vers/le/fichier` par les chemins réels de votre système de fichiers HDFS. Vous pouvez également ajouter des descriptions supplémentaires ou des explications pour chaque commande si nécessaire.

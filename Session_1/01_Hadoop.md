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
hdfs dfs -mkdir /chemin/vers/le/repertoire
```

### Copier un fichier local dans HDFS
```bash
hdfs dfs -copyFromLocal /chemin/vers/le/fichier /destination/dans/hdfs
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

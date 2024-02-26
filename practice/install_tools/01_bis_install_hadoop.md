# instructions to Install Hadoop 3.3.5 in Ubuntu.
Voici les instructions pour installer Hadoop 3.3.5 sur Ubuntu 22.04.3 - LTS.

1. (Exécuter toutes les commandes en tant qu'utilisateur root)
   ```bash
   sudo apt-get update
   ```

2. Installer le JDK par défaut :
   ```bash
   sudo apt-get install default-jdk
   ```

3. Vérifier la version de Java :
   ```bash
   java -version
   ```
4. Configuration de l'utilisateur Hadoop
```bash
## Ajout d'un utilisateur système Hadoop
sudo addgroup hadoop_
sudo adduser --ingroup hadoop_ hduser_
sudo adduser hduser_ sudo
   ```

5. Générer une paire de clés SSH sans passphrase :
   ```bash
   ssh-keygen -t rsa -P ""
   ```
   (Appuyez sur Entrée après cette étape, ne spécifiez pas de nom de fichier)

6. Autoriser l'accès SSH à la machine locale avec cette clé :
   ```bash
   cat $HOME/.ssh/id_rsa.pub >> $HOME/.ssh/authorized_keys
   ```

7. Passer au user hduser
   ```bash
   sudo su - hduser
   ```
8. Tester la configuration SSH en se connectant à ocalhost en tant qu'utilisateur 'hduser' :
   ```bash
   ssh localhost
   ```

9. ⚠️ En cas de problème SSH, purger et réinstaller ⚠️:
   ```bash
   sudo apt-get purge openssh-server
   sudo apt-get install openssh-server
   ``` 

10. Télécharger Hadoop 3.3.5 :
   ```bash
   wget https://archive.apache.org/dist/hadoop/common/hadoop-3.3.5/hadoop-3.3.5.tar.gz
   ```
11. Extraire le fichier tar.gz :
   ```bash
   sudo tar -xzf hadoop-3.3.5.tar.gz -C /opt/
   ```

12. Déplacer Hadoop vers le répertoire /etc/hadoop :
   ```bash
   sudo mkdir -p /etc/hadoop/
   sudo cp /opt/hadoop-3.3.5/etc/hadoop/*.xml /etc/hadoop
   ```

13. Configuration des variables d'environnement
   ```bash
   sudo sh -c 'echo "export HADOOP_HOME=/opt/hadoop-3.3.5" >> /etc/profile'
   sudo sh -c 'echo "export PATH=\$PATH:\$HADOOP_HOME/bin" >> /etc/profile'
   source /etc/profile
   ```
14. Configuration des dossiers de stockage temporaire pour Hadoop :
    ```bash
    # repertoires de stockage temporaire
    sudo mkdir -p /tmp/hadoop/namenode
    sudo mkdir -p /tmp/hadoop/datanode

    # donner les autorisations à ces repertoires
    sudo chmod 777 /tmp/hadoop
    sudo chmod 777 /tmp/hadoop/namenode
    sudo chmod 777 /tmp/hadoop/datanode
    ```
15. Nettoyage des fichiers temporaires
    ```bash
    sudo rm -f hadoop-3.3.5.tar.gz
    ```
16.  Modifier le fichier ~/.bashrc
     ```bash
     nano ~/.bashrc
     ```
17. Ajoute ces lignes à la fin :

    ```bash
    #HADOOP VARIABLES START
    export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
    export HADOOP_HOME=/opt/hadoop-3.3.5
    # ajouter le repertoire bin/ de Hadoop au path
    export PATH=\$PATH:$HADOOP_HOME/bin
    #export PATH=$PATH:$HADOOP_INSTALL/sbin
    export HADOOP_MAPRED_HOME=$HADOOP_HOME
    export HADOOP_COMMON_HOME=$HADOOP_HOME
    export HADOOP_HDFS_HOME=$HADOOP_HOME
    export YARN_HOME=$HADOO_HOME
    #export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_INSTALL/lib/native
    #export HADOOP_OPTS="-Djava.library.path=$HADOOP_INSTALL/lib"
    #HADOOP VARIABLES END
    ```
18. Appliquer les modifications
    ```bash
    source ~/.bashrc
    ```
Une fois que vous avez copié les éléments dans le fichier, vous pouvez enregistrer et quitter l'éditeur nano (généralement en appuyant sur Ctrl + X, puis en répondant "O" pour enregistrer les modifications et en appuyant sur Entrée).

19. Accédez au répertoire où se trouvent les fichiers de configuration Hadoop :
    ```bash
    cd /opt/hadoop-3.3.5/etc/hadoop
    ```

20. Liste des fichiers dans ce répertoire :
    ```bash
    ls -lt
    ```

    Vous verrez de nombreux fichiers ici.

21. Ouvrez le fichier hadoop-env.sh avec l'éditeur de texte nano :
    ```bash
    nano hadoop-env.sh
    ```
   
    
    ```bash
    # Cherche la variable `export JAVA_HOME` et remplace la ligne par :
    export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64

    # Cherche la variable `export HADOOP_HOME` et remplace la ligne par :
    export HADOOP_HOME=/opt/hadoop-3.3.5
    ```
   
Après avoir copié la ligne dans le fichier, enregistrez et quittez l'éditeur nano.

22. Ouvrir le fichier core-site.xml avec l'éditeur de texte nano :
    ```bash
    sudo nano core-site.xml
    ```
   
    Entrez les lignes suivantes entre les balises `<configuration></configuration>` :
    ```xml
    <property>
      <name>hadoop.tmp.dir</name>
      <value>/app/hadoop/tmp</value>
      <description>A base for other temporary directories.</description>
    </property>
    <property>
      <!--<name>fs.default.name</name>-->
      <name>fs.defaultFS</name
      <value>hdfs://localhost:54310</value>
      <description>The name of the default file system. A URI whose
      scheme and authority determine the FileSystem implementation. The
      uri's scheme determines the config property (fs.SCHEME.impl) naming
      the FileSystem implementation class. The uri's authority is used to
      determine the host, port, etc. for a filesystem.</description>
    </property>
    ```
23. Créer le dossier mentionné dans le core-site.txt et lui accorder les autorisations
    ```bash
    sudo mkdir -p /app/hadoop/tmp
    sudo chown -R hduser_:Hadoop_ /app/hadoop/tmp
    sudo chmod 750 /app/hadoop/tmp
    ```
24. [optionel] Copiez le fichier `mapred-site.xml.template` dans mapred-site.xml  :
    ```bash
    cp /opt/hadoop-3.3.5/etc/hadoop/mapred-site.xml.template /opt/hadoop-3.3.5/etc/hadoop/mapred-site.xml
    ```
25. Quitter le terminal et réouvrir (en mode root)
26. Ouvrez le fichier mapred-site.xml avec l'éditeur de texte nano :
    ```bash
    cd ${HADOOP_HOME}/etc/hadoop
    sudo nano mapred-site.xml
    ```
   
    Entrez les lignes suivantes entre les balises `<configuration></configuration>` :
    ```xml
    <property>
      <name>mapreduce.job.tracker.address</name>
      <value>localhost:54311</value>
      <description>The host and port that the MapReduce job tracker runs
      at. If "local", then jobs are run in-process as a single map
      and reduce task.
      </description>
    </property>
    ```
   
    (J'espère que vous êtes toujours dans le répertoire /opt/hadoop-3.3.5/etc/hadoop)

27. Ouvrir le fichier hdfs-site.xml avec l'éditeur de texte nano :
    ```bash
    nano hdfs-site.xml
    ```
   
    Entrez les lignes suivantes entre les balises `<configuration></configuration>` :
    ```xml
    <property>
      <name>dfs.replication</name>
      <value>1</value>
      <description>Default block replication.
      The actual number of replications can be specified when the file is created.
      The default is used if replication is not specified in create time.
      </description>
    </property>
    <!--<property>
      <name>dfs.namenode.name.dir</name>
      <value>file:/usr/local/hadoop_store/hdfs/namenode</value>
    </property>-->
    <property>
      <name>dfs.datanode.data.dir</name>
      <value>/home/hduser_/hdfs</value>
    </property>
    ```
28. Créez le répertoire spécifié dans le paramètre ci-dessus et lui donner les autorisations
    ```bash
    sudo mkdir -p /home/hduser_/hdfs
    sudo chown -R hduser_:hadoop_ /home/hduser_/hdfs
    sudo chmod 750 /home/hduser_/hdfs
    ```
29. Changer le user :
    ```bash
    su - hduser_
    ```
30. Formater HDFS :
    ```bash
    $HADOOP_HOME/bin/hdfs namenode -format
    ```

    (⚠️ Effectuez cette étape une seule fois. Si cette commande est exécutée à nouveau après que Hadoop a été utilisé, elle détruira toutes les données sur le système de fichiers Hadoop. Parfois, cette commande ne fonctionne pas au premier essai. Donc, changez l'utilisateur et changez-le à nouveau en 'root'. Ou bien executer cette commande : `export PATH=$PATH:/bin/usr/bin`)

Les commandes ci-dessous sont destinées à tester l'installation réussie de Hadoop.


31. Démarrer les services HDFS :
    ```bash
    $HADOOP_HOME/sbin/start-dfs.sh
    ```
   (Tapez OUI deux fois lorsqu'on vous le demande)

32. Démarrer les services YARN :
    ```bash
   $HADOOP_HOME/sbin/start-dfs.sh
    ```

33. Vérifier que tous les processus Java sont en cours d'exécution :
    ```bash
    jps
    ```

34. [optionnel] Vérifier les ports utilisés par les processus Java :
    ```bash
    netstat -plten | grep java
    ```

35. Arrêter les services Hadoop :
    ```bash
    # tous les services : hadoop + yarn
    stop-all.sh
    
    # hadoop
    $HADOOP_HOME/sbin/stop-dfs.sh

    # Yarn
    $HADOOP_HOME/sbin/stop-yarn.sh
    ```

Liens de référence:
- [Installation de Hadoop sur Ubuntu 13.10 (DigitalOcean)](https://www.digitalocean.com/community/tutorials/how-to-install-hadoop-on-ubuntu-13-10)
- [Installation d'Hadoop sur un cluster à nœud unique Ubuntu (bogotobogo)](http://www.bogotobogo.com/Hadoop/BigData_hadoop_Install_on_ubuntu_single_node_cluster.php)
- [Comment installer Hadoop avec une configuration étape par étape sur Linux Ubuntu]([http://www.bogotobogo.com/Hadoop/BigData_hadoop_Install_on_ubuntu_single_node_cluster.php](https://www.guru99.com/fr/how-to-install-hadoop.html)https://www.guru99.com/fr/how-to-install-hadoop.html)

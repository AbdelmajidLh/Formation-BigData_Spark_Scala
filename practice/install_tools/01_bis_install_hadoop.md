# instructions to Install Hadoop 3.3.5 in Ubuntu.
Voici les instructions pour installer Hadoop 3.3.5 sur Ubuntu 22.04.3 - LTS.

[Adapté de la source](https://gist.github.com/swanandM/2b31a9984cdb58af96ec417197350f32)

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

4. Générer une paire de clés SSH sans passphrase :
   ```bash
   ssh-keygen -t rsa -P ""
   ```
   (Appuyez sur Entrée après cette étape, ne spécifiez pas de nom de fichier)

5. Autoriser l'accès SSH à la machine locale avec cette clé :
   ```bash
   cat $HOME/.ssh/id_rsa.pub >> $HOME/.ssh/authorized_keys
   ```

6. Tester la configuration SSH en se connectant à ocalhost en tant qu'utilisateur 'hduser' :
   ```bash
   ssh localhost
   ```

7. En cas de problème SSH, purger et réinstaller :
   ```bash
   sudo apt-get purge openssh-server
   sudo apt-get install openssh-server
   ``` 

8. Télécharger Hadoop 3.3.5 :
   ```bash
   wget https://archive.apache.org/dist/hadoop/common/hadoop-3.3.5/hadoop-3.3.5.tar.gz
   ```

9. Extraire le fichier tar.gz :
   ```bash
   sudo tar -xzf hadoop-3.3.5.tar.gz -C /opt/
   ```

10. Déplacer Hadoop vers le répertoire /usr/local :
   ```bash
   sudo mdir -p /etc/hadoop/
   sudo cp /opt/hadoop-3.3.5/etc/hadoop/*.xml /etc/hadoop
   ```

11. Configuration des variables d'environnement
   ```bash
   sudo sh -c 'echo "export HADOOP_HOME=/opt/hadoop-3.3.5" >> /etc/profile'
   sudo sh -c 'echo "export PATH=\$PATH:\$HADOOP_HOME/bin" >> /etc/profile'
   source /etc/profile
   ```
12. Configuration des dossiers de stockage temporaire pour Hadoop :
    ```bash
    # repertoires de stockage temporaire
    sudo mkdir -p /tmp/hadoop/namenode
    sudo mkdir -p /tmp/hadoop/datanode

    # donner les autorisations à ces repertoires
    sudo chmod 755 /tmp/hadoop
    sudo chmod 750 /tmp/hadoop/namenode
    sudo chmod 750 /tmp/hadoop/datanode
    ```
13. Nettoyage des fichiers temporaires
    ```bash
    sudo rm -f /opt/hadoop-3.3.5.tar.gz
    ```
14.  Modifier le fichier ~/.bashrc
     ```bash
     nano ~/.bashrc
     ```
15. Ajoute ces lignes à la fin :

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
16. Appliquer les modifications
    ```bash
    source ~/.bashrc
    ```


========================================
Une fois que vous avez copié les éléments dans le fichier, vous pouvez enregistrer et quitter l'éditeur nano (généralement en appuyant sur Ctrl + X, puis en répondant "O" pour enregistrer les modifications et en appuyant sur Entrée).

11. Accédez au répertoire où se trouvent les fichiers de configuration Hadoop :
    ```bash
    cd /usr/local/hadoop/etc/hadoop
    ```

12. Liste des fichiers dans ce répertoire :
    ```bash
    ls -lt
    ```

    Vous verrez de nombreux fichiers ici.

13. Ouvrez le fichier hadoop-env.sh avec l'éditeur de texte nano :
    ```bash
    nano hadoop-env.sh
    ```
   
    Copiez et collez la ligne suivante à la fin du fichier ci-dessus et remplacez la ligne rouge par la ligne verte si elles ne sont pas identiques, comme nous l'avons fait précédemment :
    ```bash
    export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
    ```
   
Après avoir copié la ligne dans le fichier, enregistrez et quittez l'éditeur nano.

14. Ouvrir le fichier core-site.xml avec l'éditeur de texte nano :
    ```bash
    nano core-site.xml
    ```
   
    Entrez les lignes suivantes entre les balises `<configuration></configuration>` :
    ```xml
    <property>
      <name>hadoop.tmp.dir</name>
      <value>/app/hadoop/tmp</value>
      <description>A base for other temporary directories.</description>
    </property>
    <property>
      <name>fs.default.name</name>
      <value>hdfs://localhost:54310</value>
      <description>The name of the default file system. A URI whose
      scheme and authority determine the FileSystem implementation. The
      uri's scheme determines the config property (fs.SCHEME.impl) naming
      the FileSystem implementation class. The uri's authority is used to
      determine the host, port, etc. for a filesystem.</description>
    </property>
    ```

15. Copiez le fichier mapred-site.xml.template dans mapred-site.xml [optionel] :
    ```bash
    cp /usr/local/hadoop/etc/hadoop/mapred-site.xml.template /usr/local/hadoop/etc/hadoop/mapred-site.xml
    ```

16. Ouvrez le fichier mapred-site.xml avec l'éditeur de texte nano :
    ```bash
    nano mapred-site.xml
    ```
   
    Entrez les lignes suivantes entre les balises `<configuration></configuration>` :
    ```xml
    <property>
      <name>mapred.job.tracker</name>
      <value>localhost:54311</value>
      <description>The host and port that the MapReduce job tracker runs
      at. If "local", then jobs are run in-process as a single map
      and reduce task.
      </description>
    </property>
    ```
   
    (J'espère que vous êtes toujours dans le répertoire /usr/local/hadoop/etc/hadoop)

17. Créez le répertoire pour le namenode HDFS :
    ```bash
    sudo mkdir -p /usr/local/hadoop_store/hdfs/namenode
    ```

18. Créez le répertoire pour le datanode HDFS :
    ```bash
    sudo mkdir -p /usr/local/hadoop_store/hdfs/datanode
    ```
Voici les étapes supplémentaires ajoutées à votre document Markdown :

19. Ouvrir le fichier hdfs-site.xml avec l'éditeur de texte nano :
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
    <property>
      <name>dfs.namenode.name.dir</name>
      <value>file:/usr/local/hadoop_store/hdfs/namenode</value>
    </property>
    <property>
      <name>dfs.datanode.data.dir</name>
      <value>file:/usr/local/hadoop_store/hdfs/datanode</value>
    </property>
    ```

20. Formater le namenode HDFS :
    ```bash
    hdfs namenode -format
    ```

    (Effectuez cette étape une seule fois. Si cette commande est exécutée à nouveau après que Hadoop a été utilisé, elle détruira toutes les données sur le système de fichiers Hadoop. Parfois, cette commande ne fonctionne pas au premier essai. Donc, changez l'utilisateur et changez-le à nouveau en 'root'.)

Les commandes ci-dessous sont destinées à tester l'installation réussie de Hadoop.


21. Démarrer les services HDFS :
    ```bash
    start-dfs.sh
    ```
   (Tapez OUI deux fois lorsqu'on vous le demande)

22. Démarrer les services YARN :
    ```bash
    start-yarn.sh
    ```

23. Vérifier que tous les processus Java sont en cours d'exécution :
    ```bash
    jps
    ```

24. Vérifier les ports utilisés par les processus Java :
    ```bash
    netstat -plten | grep java
    ```

25. Arrêter tous les services Hadoop :
    ```bash
    stop-all.sh
    ```

Liens de référence:
- [Installation de Hadoop sur Ubuntu 13.10 (DigitalOcean)](https://www.digitalocean.com/community/tutorials/how-to-install-hadoop-on-ubuntu-13-10)
- [Installation d'Hadoop sur un cluster à nœud unique Ubuntu (bogotobogo)](http://www.bogotobogo.com/Hadoop/BigData_hadoop_Install_on_ubuntu_single_node_cluster.php)

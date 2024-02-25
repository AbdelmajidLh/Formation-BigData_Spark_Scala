# instructions to Install Hadoop 3.3. on Ubuntu Server 22.04.3 - LTS.
Voici les instructions pour installer Hadoop 3.3 sur Ubuntu 22.04.3 - LTS.

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
   ssh-keygen -t rsa -P ''
   ```
   (Appuyez sur Entrée après cette étape, ne spécifiez pas de nom de fichier)

5. Ajouter la clé publique au fichier des clés autorisées :
   ```bash
   cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
   ```

6. Télécharger Hadoop 3.3.5 :
   ```bash
   wget http://mirrors.sonic.net/apache/hadoop/common/hadoop-3.3.5/hadoop-3.3.5.tar.gz
   ```

7. Extraire le fichier tar.gz :
   ```bash
   tar xvzf hadoop-3.3.5.tar.gz
   ```

8. Déplacer Hadoop vers le répertoire /usr/local :
   ```bash
   sudo mv hadoop-3.3.5 /usr/local/hadoop
   ```

9. Mettre à jour les alternatives Java :
   ```bash
   update-alternatives --config java
   ```
   
   Vous obtiendrez le résultat suivant :
   ```
   root@hadoop:~# update-alternatives --config java
   There is only one alternative in link group java (providing /usr/bin/java):
   /usr/lib/jvm/java-11-openjdk-amd64/bin/java Nothing to configure.
   ```

10. Ouvrir le fichier ~/.bashrc avec l'éditeur de texte nano :
    ```bash
    nano ~/.bashrc
    ```
    Copier et coller les éléments suivants dans le fichier et remplacer la ligne rouge par la ligne verte si elles ne sont pas identiques :

    ```bash
    #HADOOP VARIABLES START
    export JAVA_HOME=/usr/lib/jvm/java-11-openjdk-amd64
    export HADOOP_INSTALL=/usr/local/hadoop
    export PATH=$PATH:$HADOOP_INSTALL/bin
    export PATH=$PATH:$HADOOP_INSTALL/sbin
    export HADOOP_MAPRED_HOME=$HADOOP_INSTALL
    export HADOOP_COMMON_HOME=$HADOOP_INSTALL
    export HADOOP_HDFS_HOME=$HADOOP_INSTALL
    export YARN_HOME=$HADOOP_INSTALL
    export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_INSTALL/lib/native
    export HADOOP_OPTS="-Djava.library.path=$HADOOP_INSTALL/lib"
    #HADOOP VARIABLES END
    ```
   
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





14) # nano core-site.xml
Open the file and enter the following in between the <configuration></configuration> tag
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

15) Copy mapred-site.xml.template file to mapred-site.xml
# cp /usr/local/hadoop/etc/hadoop/mapred-site.xml.template /usr/local/hadoop/etc/hadoop/mapred-site.xml

16) # nano mapred-site.xml
Open the file and enter the following in between the <configuration></configuration> tag
      <property>
      <name>mapred.job.tracker</name>
      <value>localhost:54311</value>
      <description>The host and port that the MapReduce job tracker runs
      at. If "local", then jobs are run in-process as a single map
      and reduce task.
      </description>
      </property>
(I hope you are still this /usr/local/hadoop/etc/Hadoop directory)

17) # mkdir -p /usr/local/hadoop_store/hdfs/namenode
18) # mkdir -p /usr/local/hadoop_store/hdfs/datanode

19) # nano hdfs-site.xml
Open the file and enter the following in between the <configuration></configuration> tag
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

20) # hdfs namenode -format
(Do this only once, if this command is executed again after Hadoop has been used,
it'll destroy all the data on the Hadoop file system. Sometimes this command do not work in 1st attempt. 
So change the user and again change it back to ‘root’.)

Below commands are to test the successful installation of Hadoop.

21) # start-dfs.sh (Type YES both the time when asked)
22) # start-yarn.sh
23) # jps
24) # netstat -plten | grep java
25) # stop-all.sh

Reference Links ---
1) https://www.digitalocean.com/community/tutorials/how-to-install-hadoop-on-ubuntu-13-10
2) http://www.bogotobogo.com/Hadoop/BigData_hadoop_Install_on_ubuntu_single_node_cluster.php

---
title: Comp5349 Cloud Computing Memo
tags:
  - usyd
  - hadoop
  - python
  - linux
photos:
  - >-
    https://raw.githubusercontent.com/Ealon/ealon.github.io/master/images/ealon/default_post_image4.png
date: 2018-04-14 11:12:49
---
---

# **Week4: MapReduce Tutorial**
In this lab, we will practice basic MapReduce Programming on locally installed **Hadoop MapReduce**. The lab starts by setting up the environment in the **RedHat Linux workstation** in uni's lab as well as in the **Ubuntu linux** on my own Thinkpad.

Hadoop has grown into a large ecosystem since its launch in 2006. The core of Hadoop includes a distributed file system HDFS and a parallel programming engine MapReduce. Since Hadoop 2, YARN is included as the default resource scheduler.

### Step1: Prepare the environment
1. Required software for Linux include:
    *  Java™ must be installed. Recommended Java versions are described at [HadoopJavaVersions](https://wiki.apache.org/hadoop/HadoopJavaVersions). For my own laptop, I install it in `/usr/java/jre1.8.0_161`
    *  ssh must be installed and sshd must be running to use the Hadoop scripts that manage remote Hadoop daemons. Install **ssh** and **rsync** by running
     ```
      sudo apt-get install ssh
      sudo apt-get install rsync
     ```

2. Install [Apache Hadoop 2.9.0](http://hadoop.apache.org/docs/r2.9.0/hadoop-project-dist/hadoop-common/SingleCluster.html)
   *  Hadoop 2.9.0 is is already installed in **RedHat Linux workstation**  in uni's labs.
   *  For my own Thinkpad, download Hadoop 2.9.0, unpack the downloaded Hadoop distribution, install it by moving it to `/usr/local/hadoop-2.9.0`. 
3. In the distribution, edit the file `etc/hadoop/hadoop-env.sh` to define the parameters as follow:
   ```
    # set to the root of your Java installation
    export JAVA_HOME=/usr/java/jre1.8.0_161
   ```
  then run 
  ```
  /usr/local/hadoop-2.9.0/bin/hadoop
  ```
  we should expect to see something similar as below:
  ```
  Usage: hadoop [--config confdir] [COMMAND | CLASSNAME]
  CLASSNAME            run the class named CLASSNAME
 or
  where COMMAND is one of:
  fs                   run a generic filesystem user client
  version              print the version
  jar <jar>            run a jar file
                       note: please use "yarn jar" to launch
                             YARN applications, not this command.
  checknative [-a|-h]  check native hadoop and compression libraries availability
  distcp <srcurl> <desturl> copy file or directories recursively
  archive -archiveName NAME -p <parent path> <src>* <dest> create a hadoop archive
  classpath            prints the class path needed to get the
                       Hadoop jar and the required libraries
  credential           interact with credential providers
  daemonlog            get/set the log level for each daemon
  trace                view and modify Hadoop tracing settings

  Most commands print help when invoked w/o parameters.
  ```
4. Set our path variables. Create a directory call *comp5349* in your home directory `~/comp5349` and clone uni's repository `lab_commons` locally. You will find it contains three sub directories: `profile`, `hadoop-conf` and `data`.

   The login script template `LOGIN.TEMPLATE` is stored under `profile`. Copy this file under `/comp5349` and rename it as `.profile`. The template sets up all necessary environment variables, including future ones for Spark. It also updates the $PATH variable accordingly.
   For uni's **RedHat Linux workstation**,the content of `.profile` is:
   ```
   case $HOSTNAME in

    soit-hdp-pro-*)

        export JAVA_HOME=/usr/local/jdk1.8.0_40
        export HADOOP_HOME=/usr/local/hadoop
        export HADOOP_CONF_DIR=/usr/local/hadoop/etc/hadoop
        export SPARK_HOME=/usr/local/spark
        ;;
    w*)
        export JAVA_HOME=/etc/alternatives/java_sdk
        export HADOOP_HOME=/usr/local/hadoop-2.9.0
        export HADOOP_CONF_DIR=~/comp5349/hadoop-conf
        export HADOOP_LOG_DIR=~/comp5349/hadoop-logs
        export YARN_LOG_DIR=~/comp5349/hadoop-logs
		export HADOOP_JHS_LOGGER=~/comp5349/hadoop-logs
        export HADOOP_MAPRED_LOG_DIR=~/comp5349/hadoop-logs
        export SPARK_HOME=/usr/local/spark-2.2.1-bin-hadoop2.7
        export SPARK_CONF_DIR=~/comp5349/spark-conf
        export SPARK_LOG_DIR=~/comp5349/spark-logs
        ;;
   esac
   export PATH=.:${JAVA_HOME}/bin:${HADOOP_HOME}/bin:${SPARK_HOME}/bin:/usr/local/anaconda3/bin:${PATH}
   ```
   for my own laptop, change the content to 
   ```
    export JAVA_HOME=/usr/java/jre1.8.0_161
    export HADOOP_HOME=/usr/local/hadoop-2.9.0
    export HADOOP_CONF_DIR=~/comp5349/hadoop-conf
    export HADOOP_LOG_DIR=~/comp5349/hadoop-logs
    export YARN_LOG_DIR=~/comp5349/hadoop-logs
    export HADOOP_JHS_LOGGER=~/comp5349/hadoop-logs
    export HADOOP_MAPRED_LOG_DIR=~/comp5349/hadoop-logs
    export SPARK_HOME=/usr/local/spark-2.2.1-bin-hadoop2.7
    export SPARK_CONF_DIR=~/comp5349/spark-conf
    export SPARK_LOG_DIR=~/comp5349/spark-logs
    export PATH=.:${JAVA_HOME}/bin:${HADOOP_HOME}/bin:${SPARK_HOME}/bin:/usr/local/anaconda3/bin:${PATH}
   ```
   ***The login script will be executed each time a new terminal window is opened.*** If you want the login script to take immediate effect in the current terminal window, run source .profile
   
   ![](http://localhost:5000/Screenshot%20from%202018-04-14%2016-03-22.png)
5. The script refers to several directories not yet exist. In particular, it specifies:
   * Hadoop should look for configuration files in a directory `comp5349/hadoop-conf` under your home directory.
   * Both Hadoop and YARN and history servers should write log files in a directory `comp5349/hadoop-logs` under your home directory.
   You can change the login script to use other directories for configuration and logging.

   In the following we assume you use the given login script. Do the following to prepare required directories:
   * copy `hadoop-conf` directory from `lab_commons` to `comp5349`
   * create a directory `hadoop-logs` under `comp5349`


### Step2: Running Sample Example in Standalone Mode
1. 
   ```
   source ~/comp5349/.profile
   unset HADOOP_CONF_DIR
   ```
2. run 
   ```
    hadoop jar \
    $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.9.0.jar \
    wordcount \
    ~/comp5349/lab_commons/data/place.txt \
    ~/comp5349/countout
   ```

### Step3: Running Sample Example in Pseudo-Distributed Mode

1. Edit the file `$HADOOP_CONF_DIR/hadoop-env.sh` to define the parameters as follow, as we did in Step1.3:
   ```
    # set to the root of your Java installation
    export JAVA_HOME=/usr/java/jre1.8.0_161
   ```
2. 
   ```
   source ~/comp5349/.profile
   ```
3. Check that you can ssh to the localhost without a password:
   ```
    ssh localhost
   ```
   If you cannot ssh to localhost without a password, execute the following commands to generate a key to identify yourself:
   ```
    ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
    cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
    chmod 0600 ~/.ssh/authorized_keys
   ```
    Check again to ensure you can ssh to localhost without supplying password.
    > make sure **ssh** is installed and enabled.

4. Start HDFS

    We assume that your HDFS should store data in `/comp5349/hadoop-data`. Run the following commands to prepare the directories:
    ```
    mkdir -p ~/comp5349/hadoop-data/dn
    mkdir -p ~/comp5349/hadoop-data/nn
    ```
5. Configure `$HADOOP_CONF_DIR/hdfs-site.xml`
    ```
    <configuration>
        <property>
            <name>dfs.replication</name>
            <value>1</value>
        </property>
        <property>
            <name>dfs.datanode.data.dir</name>
            <value>file:///home/yilong/comp5349/hadoop-data/dn</value>
        </property>
        <property>
            <name>dfs.namenode.name.dir</name>
            <value>file:///home/yilong/comp5349/hadoop-data/nn</value>
        </property>
    </configuration>
    ```
6. Format the file system with:
    ```
    hdfs namenode -format
    ```
    Then start all HDFS services with
    ```
    $HADOOP_HOME/sbin/start-dfs.sh
    ```
    Check that HDFS is successfully started by visiting the HDFS Web UI http://localhost:50070/ in your browser, you should be able to see something similar to the pictures below
    ![](http://localhost:5000/50070_1.png)
    ![](http://localhost:5000/50070_2.png)

7. A few directories need to be prepared for executing Hadoop jobs:
    ```
    hdfs dfs -mkdir /user
    hdfs dfs -mkdir /user/yilong
    ```
8. Start YARN. YARN is the preferred resource scheduler for MapReduce. We have prepared the configuration files to run YARN on the pseudo cluster. To start YARN:
    ```
    $HADOOP_HOME/sbin/start-yarn.sh
    ```
    To check that YARN has been started successfully, visit the YARN WebUI at http://localhost:8088/ in your browser. You will see an output similar to
    ![](http://localhost:5000/Screenshot%20from%202018-04-14%2017-20-21.png)
9. Create a directory on HDFS to save application logs
    ```
    hdfs dfs -mkdir -p /var/log/hadoop-yarn/apps
    ```
    Start a job history server so we have a web UI to access job logs:
    ```
    $HADOOP_HOME/sbin/mr-jobhistory-daemon.sh start historyserver
    ```
10. Run sample program.
    The default file system for Hadoop pseudo cluster operation is HDFS. In this exercise, HDFS will be used for program’s input and output.
    Run the following command to put the `place.txt` file in HDFS, under your home directory.
    ```
    hdfs dfs -put ~/comp5349/lab_commons/data/place.txt place.txt
    ```
    ![](http://localhost:5000/Screenshot%20from%202018-04-14%2017-26-49.png)

11. The following command runs the grep example on place.txt to look for all records with the word “Australia” in it
    ```
    hadoop jar \
    $HADOOP_HOME/share/hadoop/mapreduce/hadoop-mapreduce-examples-2.9.0.jar \
    grep \
    place.txt \
    placeout \
    "/Australia[\d\w\s\+/]+"
    ```
    ![](http://localhost:5000/Screenshot%20from%202018-04-14%2017-29-37.png)
    ![](http://localhost:5000/Screenshot%20from%202018-04-14%2017-31-07.png)

12. To stop all services, run 
    ```
    $HADOOP_HOME/sbin/mr-jobhistory-daemon.sh stop historyserver
    $HADOOP_HOME/sbin/stop-yarn.sh
    $HADOOP_HOME/sbin/stop-dfs.sh
    ```

### Step4: A Customized Sample Program
1. 
    ```
    hdfs dfs -put ~/comp5349/lab_commons/data/partial.txt partial.txt
    ```
2. 
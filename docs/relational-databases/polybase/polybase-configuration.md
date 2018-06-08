---
title: Configuration de PolyBase | Microsoft Docs
ms.custom: ''
ms.date: 02/15/2018
ms.prod: sql
ms.prod_service: database-engine
ms.component: database
ms.reviewer: ''
ms.suite: sql
ms.technology: polybase
ms.tgt_pltfrm: ''
ms.topic: conceptual
author: rothja
ms.author: jroth
manager: craigg
ms.openlocfilehash: c2e53e939a1431664ea0a8446983a22879a913ad
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="polybase-configuration"></a>Configuration de PolyBase
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]

  Utilisez les procédures ci-dessous pour configurer PolyBase.  
  
## <a name="external-data-source-configuration"></a>Configuration de source de données externes  
 Assurez-vous que SQL Server est bien connecté à la source de données externes. Le type de connectivité a une grande influence sur les performances de requête. Par exemple, une liaison Ethernet 10 Gbit offre un temps de réponse aux requêtes PolyBase plus rapide qu’une liaison Ethernet 1 Gbit.  
  
 Vous devez configurer SQL Server pour qu’il se connecte à votre version de Hadoop ou à Azure Blob Storage à l’aide de **sp_configure**. PolyBase prend en charge deux distributions Hadoop : Hortonworks Data Platform (HDP) et Cloudera Distributed Hadoop (CDH).  Pour obtenir une liste complète des sources de données externes prises en charge, consultez [PolyBase Connectivity Configuration &#40;Transact-SQL&#41;](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md) (Configuration de la connectivité PolyBase).  

Remarque, PolyBase prend en charge les zones de chiffrement Hadoop à partir de SQL Server 2016 SP1 CU7 et SQL Server 2017.

  
### <a name="run-spconfigure"></a>Exécuter sp_configure  
  
1.  Exécutez sp_configure « connexion à hadoop » et définissez une valeur appropriée.  Pour trouver la valeur, consultez [Configuration de la connectivité PolyBase &#40;Transact-SQL&#41;](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md) (Configuration de la connectivité PolyBase).  
  
    ```sql  
    -- Values map to various external data sources.  
    -- Example: value 7 stands for Azure blob storage and Hortonworks HDP 2.3 on Linux.  
    sp_configure @configname = 'hadoop connectivity', @configvalue = 7;   
    GO   
  
    RECONFIGURE   
    GO   
    ```  
  
2.  Vous devez redémarrer SQL Server à l’aide de **services.msc**. Le redémarrage de SQL Server redémarre ces services :  
  
    -   Service de déplacement des données SQL Server PolyBase  
  
    -   Moteur SQL Server PolyBase  
  
## <a name="pushdown-configuration"></a>Configuration de la poussée vers le bas  
 Pour améliorer les performances des requêtes, activez le calcul de poussée vers le bas sur un cluster Hadoop dont vous avez besoin pour fournir à SQL Server les paramètres de configuration spécifiques à votre environnement Hadoop :  
  
1.  Recherchez le fichier **yarn-site.XML** dans le chemin d’installation de SQL Server. En règle générale, le chemin d’accès est le suivant :  
  
    ```  
    C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\Polybase\Hadoop\conf  
    ```  
  
2.  Sur l’ordinateur Hadoop, recherchez le fichier analogue dans le répertoire de configuration Hadoop. Dans le fichier, recherchez et copiez la valeur de la clé de configuration yarn.application.classpath.  
  
3.  Sur l’ordinateur SQL Server, dans le **fichier yarn.site.xml** , recherchez la propriété **yarn.application.classpath** . Collez la valeur de l’ordinateur Hadoop dans l’élément de valeur.  

4. Pour toutes les versions CDH 5.X, vous devez ajouter les paramètres de configuration **mapreduce.application.classpath** à la fin de votre **fichier yarn.site.xml** ou dans le **fichier mapred-site.xml**. HortonWorks inclut ces configurations dans les configurations **yarn.application.classpath**.

## <a name="connecting-to-hadoop-cluster-with-hadooprpcprotection-setting"></a>Connexion à un cluster Hadoop avec le paramètre Hadoop.RPC.Protection
Une méthode courante pour sécuriser la communication dans un cluster hadoop consiste à changer le paramètre de configuration hadoop.rpc.protection de « Privacy » à « Integrity ». Par défaut, PolyBase suppose que la configuration est définie sur « Authenticate ». Pour remplacer cette valeur par défaut, ajoutez la propriété suivante dans votre fichier core-site.xml. Cette nouvelle configuration permet de transférer en toute sécurité les données entre les nœuds hadoop et la connexion SSL vers SQL Server.

```
<!-- RPC Encryption information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG -->
  <property>
    <name>hadoop.rpc.protection</name>
    <value></value>
  </property> 
```




## <a name="example-yarn-sitexml-and-mapred-sitexml-files-for-cdh-5x-cluster"></a>Exemples de fichiers yarn-site.xml et mapred-site.xml pour un cluster CDH 5.X.



Yarn-site.xml avec la configuration yarn.application.classpath et mapreduce.application.classpath.
```
<?xml version="1.0" encoding="utf-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!-- Put site-specific property overrides in this file. -->
 <configuration>
  <property>
     <name>yarn.resourcemanager.connect.max-wait.ms</name>
     <value>40000</value>
  </property>
  <property>
     <name>yarn.resourcemanager.connect.retry-interval.ms</name>
     <value>30000</value>
  </property>
<!-- Applications' Configuration-->
  <property>
    <description>CLASSPATH for YARN applications. A comma-separated list of CLASSPATH entries</description>
     <!-- Please set this value to the correct yarn.application.classpath that matches your server side configuration -->
     <!-- For example: $HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/share/hadoop/common/*,$HADOOP_COMMON_HOME/share/hadoop/common/lib/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/lib/*,$HADOOP_YARN_HOME/share/hadoop/yarn/*,$HADOOP_YARN_HOME/share/hadoop/yarn/lib/* -->
     <name>yarn.application.classpath</name>
     <value>$HADOOP_CLIENT_CONF_DIR,$HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/*,$HADOOP_COMMON_HOME/lib/*,$HADOOP_HDFS_HOME/*,$HADOOP_HDFS_HOME/lib/*,$HADOOP_YARN_HOME/*,$HADOOP_YARN_HOME/lib/,$HADOOP_MAPRED_HOME/*,$HADOOP_MAPRED_HOME/lib/*,$MR2_CLASSPATH*</value>
  </property>

<!-- kerberos security information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG
  <property>
     <name>yarn.resourcemanager.principal</name>
     <value></value>
  </property>
-->
</configuration>

```
Si vous choisissez d’intégrer vos deux paramètres de configuration dans mapred-site.xml et dans yarn-site.xml, les fichiers se présentent ainsi :

**yarn-site.XML**
```
<?xml version="1.0" encoding="utf-8"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!-- Put site-specific property overrides in this file. -->
 <configuration>
  <property>
     <name>yarn.resourcemanager.connect.max-wait.ms</name>
     <value>40000</value>
  </property>
  <property>
     <name>yarn.resourcemanager.connect.retry-interval.ms</name>
     <value>30000</value>
  </property>
<!-- Applications' Configuration-->
  <property>
    <description>CLASSPATH for YARN applications. A comma-separated list of CLASSPATH entries</description>
     <!-- Please set this value to the correct yarn.application.classpath that matches your server side configuration -->
     <!-- For example: $HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/share/hadoop/common/*,$HADOOP_COMMON_HOME/share/hadoop/common/lib/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/*,$HADOOP_HDFS_HOME/share/hadoop/hdfs/lib/*,$HADOOP_YARN_HOME/share/hadoop/yarn/*,$HADOOP_YARN_HOME/share/hadoop/yarn/lib/* -->
     <name>yarn.application.classpath</name>
     <value>$HADOOP_CLIENT_CONF_DIR,$HADOOP_CONF_DIR,$HADOOP_COMMON_HOME/*,$HADOOP_COMMON_HOME/lib/*,$HADOOP_HDFS_HOME/*,$HADOOP_HDFS_HOME/lib/*,$HADOOP_YARN_HOME/*,$HADOOP_YARN_HOME/lib/*</value>
  </property>

<!-- kerberos security information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG
  <property>
     <name>yarn.resourcemanager.principal</name>
     <value></value>
  </property>
-->
</configuration>
```

**mapred-site.xml**

Notez que nous avons ajouté la propriété mapreduce.application.classpath. Dans CDH 5.x, vous trouverez les valeurs de configuration dans la même convention de nommage dans Ambari.

```
<?xml version="1.0"?>
<?xml-stylesheet type="text/xsl" href="configuration.xsl"?>
<!-- Put site-specific property overrides in this file. -->
<configuration xmlns:xi="http://www.w3.org/2001/XInclude">
  <property>
    <name>mapred.min.split.size</name>
      <value>1073741824</value>
  </property>
  <property>
    <name>mapreduce.app-submission.cross-platform</name>
    <value>true</value>
  </property>
<property>
    <name>mapreduce.application.classpath</name>
    <value>$HADOOP_MAPRED_HOME/*,$HADOOP_MAPRED_HOME/lib/*,$MR2_CLASSPATH</value>
  </property>


<!--kerberos security information, PLEASE FILL THESE IN ACCORDING TO HADOOP CLUSTER CONFIG
  <property>
    <name>mapreduce.jobhistory.principal</name>
    <value></value>
  </property>
  <property>
    <name>mapreduce.jobhistory.address</name>
    <value></value>
  </property>
-->
</configuration>
  
```
  
## <a name="kerberos-configuration"></a>Configuration de Kerberos  
Notez que quand PolyBase s’authentifie auprès d’un cluster sécurisé Kerberos, le paramètre hadoop.rpc.protection doit être défini sur « Authenticate ». De cette façon, la communication de données entre les nœuds Hadoop n’est pas chiffrée. Afin d’utiliser les paramètres « Privacy » ou « Integrity » pour hadoop.rpc.protection, mettez à jour le fichier core-site.xml sur le serveur PolyBase. Pour plus d’informations, consultez la section précédente [Connexion à un cluster Hadoop avec Hadoop.rpc.protection](#connecting-to-hadoop-cluster-with-hadooprpcprotection-setting).

 Pour vous connecter à un cluster Hadoop sécurisé Kerberos [à l’aide de MIT KDC] :
   
  
1.  Recherchez le répertoire de configuration Hadoop dans le chemin d’installation de SQL Server. En règle générale, le chemin d’accès est le suivant :  
  
    ```  
    C:\Program Files\Microsoft SQL Server\MSSQL13.MSSQLSERVER\MSSQL\Binn\Polybase\Hadoop\conf  
    ```  
  
2.  Recherchez la valeur de configuration côté Hadoop des clés de configuration répertoriées dans le tableau. Sur l’ordinateur Hadoop, recherchez les fichiers dans le répertoire de configuration Hadoop.  
  
3.  Copiez les valeurs de configuration dans la propriété de valeur dans les fichiers correspondants sur l’ordinateur SQL Server.  
  
    |**#**|**Fichier de configuration**|**Clé de configuration**|**Action**|  
    |------------|----------------|---------------------|----------|   
    | 1|core-site.xml|polybase.kerberos.kdchost|Spécifiez le nom d’hôte KDC. Par exemple : kerberos.votre-domaine.com.|  
    |2|core-site.xml|polybase.kerberos.realm|Spécifiez le domaine Kerberos. Par exemple : VOTRE-DOMAINE.COM|  
    |3|core-site.xml|hadoop.security.authentication|Recherchez la configuration côté Hadoop et copiez-la sur l’ordinateur SQL Server. Par exemple : KERBEROS<br></br>**Note de sécurité :** KERBEROS doit être écrit en majuscules. Dans le cas contraire, il pourrait ne pas être activé.|   
    |4|hdfs-site.xml|dfs.namenode.kerberos.principal|Recherchez la configuration côté Hadoop et copiez-la sur l’ordinateur SQL Server. Par exemple : hdfs/_HOST@YOUR-REALM.COM|  
    |5|mapred-site.xml|mapreduce.jobhistory.principal|Recherchez la configuration côté Hadoop et copiez-la sur l’ordinateur SQL Server. Par exemple : mapred/_HOST@YOUR-REALM.COM|  
    |6|mapred-site.xml|mapreduce.jobhistory.address|Recherchez la configuration côté Hadoop et copiez-la sur l’ordinateur SQL Server. Par exemple : 10.193.26.174:10020|  
    |7|yarn-site.xml yarn.|yarn.resourcemanager.principal|Recherchez la configuration côté Hadoop et copiez-la sur l’ordinateur SQL Server. Par exemple : yarn/_HOST@YOUR-REALM.COM|  
  
4.  Créez un objet d’informations d’identification limité à la base de données pour spécifier les informations d’authentification de chaque utilisateur Hadoop. Consultez [Objets T-SQL PolyBase](../../relational-databases/polybase/polybase-t-sql-objects.md).  
  
## <a name="next-steps"></a>Étapes suivantes  
 [Objets T-SQL PolyBase](../../relational-databases/polybase/polybase-t-sql-objects.md)  
  
 [Bien démarrer avec PolyBase](../../relational-databases/polybase/get-started-with-polybase.md)  
  
## <a name="see-also"></a> Voir aussi  
 [Configuration de la connectivité PolyBase &#40;Transact-SQL&#41;](../../database-engine/configure-windows/polybase-connectivity-configuration-transact-sql.md)   
 [Guide de PolyBase](../../relational-databases/polybase/polybase-guide.md)  
  
  

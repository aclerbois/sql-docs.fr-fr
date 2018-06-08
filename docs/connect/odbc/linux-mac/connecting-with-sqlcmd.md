---
title: Connexion avec sqlcmd | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- sqlcmd
ms.assetid: 61a2ec0d-1bcb-4231-bea0-cff866c21463
caps.latest.revision: 45
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: c330fd329f28fa7d89b62b9af6bb8d4bb67c2bc4
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="connecting-with-sqlcmd"></a>Connexion avec sqlcmd
[!INCLUDE[Driver_ODBC_Download](../../../includes/driver_odbc_download.md)]

Le [sqlcmd](http://go.microsoft.com/fwlink/?LinkID=154481) utilitaire est disponible dans le [!INCLUDE[msCoName](../../../includes/msconame_md.md)] ODBC Driver for [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] sur Linux et macOS.
  
Les commandes suivantes montrent comment utiliser l’authentification Windows (Kerberos) et [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] l’authentification, respectivement :
  
```  
sqlcmd –E –Sxxx.xxx.xxx.xxx  
sqlcmd –Sxxx.xxx.xxx.xxx –Uxxx -Pxxx  
```  
  
## <a name="available-options"></a>Options disponibles

Dans la version actuelle, les options suivantes sont disponibles :  
  
- -? Affichage `sqlcmd` utilisation.  
  
- -une demande une taille de paquet.  
  
- -b Terminate par lots s’il existe une erreur.  
  
- -c *batch_terminator* spécifier le terminateur de lot.  
  
- C - certificat de serveur de confiance.  

- -d *nom_base_de_données* problème un `USE ` *nom_base_de_données* instruction lorsque vous démarrez `sqlcmd`.  

- D - provoque la valeur passée à la `sqlcmd` option -S doit être interprété comme un nom de source de données (DSN). Pour plus d’informations, consultez « prise en charge de la source de données dans `sqlcmd` et `bcp`» à la fin de cette rubrique.  
  
- e - écrire des scripts d’entrée pour le périphérique de sortie standard (stdout).

- E - utiliser la connexion approuvée (authentification intégrée). Pour plus d’informations sur les connexions approuvées qui utilisent l’authentification intégrée à partir d’un client Linux ou macOS, consultez [à l’aide de l’authentification intégrée](../../../connect/odbc/linux-mac/using-integrated-authentication.md).

- -h *number_of_rows* spécifier le nombre de lignes à imprimer entre les en-têtes de colonne.  
  
- H - spécifiez un nom de station de travail.  
  
- -i *input_file*[,*input_file*[,...]] Identifier le fichier qui contient un lot d’instructions SQL ou des procédures stockées.  
  
- -I ensemble la `SET QUOTED_IDENTIFIER` option de connexion à ON.  
  
- k - supprimer ou remplacer des caractères de contrôle.  
  
- **K-*** application_intent*  
Déclare le type de la charge de travail de l'application lors de la connexion à un serveur. La seule valeur actuellement prise en charge est **ReadOnly**. Si **-K** n’est pas spécifié, `sqlcmd` ne prend pas en charge la connectivité à un réplica secondaire dans un groupe de disponibilité AlwaysOn. Pour plus d’informations, consultez [pilote ODBC sur Linux et macOS - haute disponibilité et récupération d’urgence](../../../connect/odbc/linux-mac/odbc-driver-on-linux-support-for-high-availability-disaster-recovery.md).  
  
> [!NOTE]  
> L’option **-K** n’est pas prise en charge dans le CTP pour SUSE Linux. Toutefois, vous pouvez spécifier le **ApplicationIntent = ReadOnly** mot clé dans un fichier DSN passé à `sqlcmd`. Pour plus d’informations, consultez « prise en charge de la source de données dans `sqlcmd` et `bcp`» à la fin de cette rubrique.  
  
- -l *délai d’attente* spécifier le nombre de secondes avant une `sqlcmd` connexion expire lorsque vous essayez de vous connecter à un serveur.

- m - *error_level* contrôle les messages d’erreur sont envoyés à stdout.  
  
- **M-*** multisubnet_failover*  
Spécifiez toujours **-M** en cas de connexion à l’écouteur de groupe de disponibilité d’un groupe de disponibilité [!INCLUDE[ssSQL11](../../../includes/sssql11_md.md)] ou d’une instance de cluster de basculement [!INCLUDE[ssSQL11](../../../includes/sssql11_md.md)]. **-M** fournit pour accélérer la détection de basculement et de connexion au serveur (actuellement) actif. Si vous ne spécifiez pas l’option **–M** , **-M** est désactivé. Pour plus d’informations sur [!INCLUDE[ssHADR](../../../includes/sshadr_md.md)], consultez [pilote ODBC sur Linux et macOS - haute disponibilité et récupération d’urgence](../../../connect/odbc/linux-mac/odbc-driver-on-linux-support-for-high-availability-disaster-recovery.md).  
  
> [!NOTE]  
> L’option **-M** n’est pas prise en charge dans le CTP pour SUSE Linux. Toutefois, vous pouvez spécifier le **MultiSubnetFailover = Yes** mot clé dans un fichier DSN passé à `sqlcmd`. Pour plus d’informations, consultez « prise en charge de la source de données dans `sqlcmd` et `bcp`» à la fin de cette rubrique.  
  
- -N chiffrer la connexion.  
  
- -o *output_file* identifier le fichier recevant la sortie à partir de `sqlcmd`.  
  
- statistiques de performances p - Print pour chaque jeu de résultats.  
  
- P - spécifier un mot de passe utilisateur.  
  
- q - *commandline_query* exécuter une requête lorsque `sqlcmd` démarre, mais ne quitte pas lors de l’exécution de la requête est terminée.  

- -Q *commandline_query* exécuter une requête lorsque `sqlcmd` démarre. `sqlcmd` s’arrête lorsque la requête est terminée.  

- messages d’erreur - r effectue une redirection vers stderr.

- -R, le pilote à utiliser les paramètres régionaux du client pour convertir monnaie et les données de date et d’heure en données caractères. Uniquement actuellement uniquement la mise en forme en_US (anglais américain).
  
- -s *column_separator_char* spécifier le caractère de séparation des colonnes.  

- -S [*protocole*:] *server*[**, *** port*]  
Spécifiez l’instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] auquel se connecter, ou si -D est utilisé, une source de données. Le pilote ODBC sur Linux et macOS requiert - S. Notez que **tcp** est le seul protocole valide.  
  
- -t *query_timeout* spécifier le nombre de secondes avant qu’une commande (ou une instruction SQL) expire.  
  
- -u spécifier qu’output_file est stocké au format Unicode, quel que soit le format d’input_file.  
  
- -U *login_id* spécifier un ID de connexion utilisateur.  
  
- V - *error_severity_level* contrôler le niveau de gravité est utilisé pour définir la variable ERRORLEVEL.  
  
- -w *largeur_de_colonne* spécifier la largeur d’écran de sortie.  
  
- W - supprimer les espaces de fin d’une colonne.  
  
- substitution de variable Disable - x.  
  
- X - désactiver les commandes de script de démarrage et les variables d’environnement.  
  
- -y *variable_length_type_display_width* définir le `sqlcmd` variable de script `SQLCMDMAXFIXEDTYPEWIDTH`.
  
- -Y *fixed_length_type_display_width* définir le `sqlcmd` variable de script `SQLCMDMAXVARTYPEWIDTH`.


## <a name="available-commands"></a>Commandes disponibles

Dans la version actuelle, les commandes suivantes sont disponibles :  
  
-   [:]!!  
  
-   :Connect  
  
-   :Error  
  
-   [:]EXIT  
  
-   GO [*count*]  
  
-   :Help  
  
-   :List  
  
-   :Listvar  
  
-   :On Error  
  
-   :Out  
  
-   :Perftrace  
  
-   [:]QUIT  
  
-   :r  
  
-   :RESET  
  
-   :setvar  
  
## <a name="unavailable-options"></a>Options non disponibles
Dans la version actuelle, les options suivantes ne sont pas disponibles :  

- A - se connecter à [!INCLUDE[ssNoVersion](../../../includes/ssnoversion_md.md)] avec une connexion administrateur dédiée (DAC). Pour plus d’informations sur la façon d’établir une connexion administrateur dédiée (DAC), consultez [les instructions de programmation](../../../connect/odbc/linux-mac/programming-guidelines.md).  
  
- -f *code_page* spécifier les pages de codes d’entrée et de sortie.  
  
- -L répertorient les ordinateurs serveurs configurés localement et les noms des serveurs diffusant sur le réseau.  
  
- v - créer un `sqlcmd` variable de script qui peut être utilisé dans un `sqlcmd` script.  
  
Vous pouvez utiliser la méthode alternative suivante : placez les paramètres dans un seul fichier, ce qui vous pouvez ensuite ajouter à un autre fichier. Cela vous aidera à utiliser un fichier de paramètres pour remplacer les valeurs. Par exemple, créez un fichier appelé `a.sql` (le fichier de paramètres) avec le contenu suivant :
  
    :setvar ColumnName object_id  
    :setvar TableName sys.objects  
  
Puis créez un fichier appelé `b.sql`, avec les paramètres de remplacement :  
  
    select $(ColumnName) from $(TableName)  

Sur la ligne de commande combiner `a.sql` et `b.sql` dans `c.sql` utilisant les commandes suivantes :  
  
    cat a.sql > c.sql 
  
    cat b.sql >> c.sql  
  
Exécutez `sqlcmd` et utiliser `c.sql` en tant que fichier d’entrée :  
  
    slqcmd -S<…> -P<..> –U<..> -I c.sql  

- z - *mot de passe* Change password.  
  
- Z - *mot de passe* modifier le mot de passe et quitter.  

## <a name="unavailable-commands"></a>Commandes non disponibles

Dans la version actuelle, les commandes suivantes ne sont pas disponibles :  
  
-   :ED  
  
-   :ServerList  
  
-   :XML  
  
## <a name="dsn-support-in-sqlcmd-and-bcp"></a>Prise en charge du nom de source de données dans sqlcmd et bcp

Vous pouvez spécifier un nom de source de données (DSN) au lieu d’un nom de serveur dans le **sqlcmd** ou **bcp** `-S` option (ou **sqlcmd** : commande de connexion) si vous spécifiez - D. D - provoque **sqlcmd** ou **bcp** pour se connecter au serveur spécifié dans la source de données par l’option -S.  
  
Sources de données système sont stockés dans le `odbc.ini` fichier dans le répertoire ODBC SysConfigDir (`/etc/odbc.ini` sur les installations standard). Sources de données utilisateur sont stockées dans `.odbc.ini` dans le répertoire de base d’un utilisateur (`~/.odbc.ini`).
  
Les entrées suivantes sont prises en charge dans un DSN sur Linux ou macOS :

-   **ApplicationIntent = ReadOnly**  

-   **Base de données = *** nom_base_de_données*  
  
-   **Pilote = ODBC Driver 11 pour SQL Server** ou **pilote = ODBC Driver 13 pour SQL Server**
  
-   **MultiSubnetFailover = Yes**  
  
-   **Server = *** server_name_or_IP_address*  
  
-   **Trusted_Connection=yes**|**no**  
  
Dans un DSN, seule l’entrée DRIVER est nécessaire, mais pour se connecter à un serveur, `sqlcmd` ou `bcp` nécessite la valeur dans l’entrée de serveur.  

Si la même option est spécifiée dans la source de données et la `sqlcmd` ou `bcp` ligne de commande, l’option de ligne de commande remplace la valeur utilisée dans la source de données. Par exemple, si la source de données a une entrée de la base de données et la `sqlcmd` inclut de ligne de commande **-d**, la valeur passée à **-d** est utilisé. Si **Trusted_Connection = yes** est spécifié dans la source de données, nom d’utilisateur et de Kerberos, l’authentification est utilisée (**– U**) et le mot de passe (**– P**), si fourni, sont ignorés.

Scripts qui appellent `isql` peut être modifié pour utiliser `sqlcmd` en définissant l’alias suivant : `alias isql="sqlcmd –D"`.  

## <a name="see-also"></a>Voir aussi  
[Connexion avec **bcp**](../../../connect/odbc/linux-mac/connecting-with-bcp.md)  
 

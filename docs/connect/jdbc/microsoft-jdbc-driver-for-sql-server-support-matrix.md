---
title: Microsoft JDBC Driver pour la matrice de prise en charge SQL Server | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2018
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: c5769e67-99f7-4bc1-a4fa-8941dad33d35
caps.latest.revision: 13
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1aceb8c6c7077236d8bf7dcf4794a894adc16030
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="microsoft-jdbc-driver-for-sql-server-support-matrix"></a>Matrice de support de Microsoft JDBC Driver pour SQL Server
[!INCLUDE[Driver_JDBC_Download](../../includes/driver_jdbc_download.md)]

  Cette page contient la matrice de support et la politique de support de Microsoft JDBC Driver pour SQL Server.  
  
## <a name="microsoft-jdbc-driver-support-lifecycle-matrix-and-policy"></a>Matrice et politique de support de Microsoft JDBC Driver  
 La politique de support de Microsoft fournit des informations transparentes et prévisibles concernant la politique de support des produits Microsoft. Versions du pilote JDBC 3.0, 4.x et 6.x bénéficient de cinq ans de support standard à partir du pilote date de publication. Le support standard est défini sur le site web de la politique de support de Microsoft.  
  
 Les options de support étendu et personnalisé ne sont pas disponibles pour le pilote Microsoft JDBC.  
    
 Les pilotes Microsoft JDBC suivants bénéficient d’un support jusqu’à la date de fin de support indiquée.  
  
|Nom du pilote|Version de Package de pilotes|JAR(s) applicable|Fin de Support standard|
|-|-|-|-|  
|Microsoft JDBC Driver 6.4 pour SQL Server|6.4|mssql-jdbc-6.4.0.jre9.jar<br> mssql-jdbc-6.4.0.jre8.jar<br> mssql-jdbc-6.4.0.jre7.jar|22 janvier 2023|    
|Microsoft JDBC Driver 6.2 pour SQL Server|6.2|mssql-jdbc-6.2.2.jre8.jar<br> mssql-jdbc-6.2.2.jre7.jar|30 juin 2022|    
|Microsoft JDBC Driver 6.0 pour SQL Server|6.0|sqljdbc42.jar<br>sqljdbc41.jar|14 juillet 2021|    
|Microsoft JDBC Driver 4.2 pour SQL Server|4.2|sqljdbc42.jar<br>sqljdbc41.jar|24 août 2020|  
|Microsoft JDBC Driver 4.1 pour SQL Server|4.1|sqljdbc41.jar|12 décembre 2019|  
  
 Le support des pilotes Microsoft JDBC suivants est fini.  
 
|Nom du pilote|Version de Package de pilotes|Fin de Support standard|  
|-|-|-|
|Microsoft JDBC Driver 4.0 pour SQL Server|4.0|6 mars 2017|  
|Pilote JDBC Microsoft SQL Server 3.0|3.0|23 avril 2015|  
|Microsoft SQL Server JDBC Driver 2.0|2|31 décembre 2012|  
|Pilote JDBC Microsoft SQL Server 2005 1.2|1.2|25 juin 2011|  
|Microsoft SQL Server 2005 JDBC Driver 1.1|1.1|25 juin 2011|  
|Microsoft SQL Server 2005 JDBC Driver 1.0|1.0|25 juin 2011|  
|Microsoft SQL Server 2000 JDBC Driver|2000|9 juillet 2010|  
  
## <a name="sql-version-compatibility"></a>Compatibilité des versions de SQL  
  
|Version du pilote|SQL Server 2008|SQL Server 2008 R2|SQL Server 2012|Azure SQL Database|PDW 2008 R2 AU3<sup>4</sup>|SQL Server 2014|SQL Server 2016|SQL Server 2017|SQL Azure géré Instance (Extended aperçu privé)|  
|-|-|-|-|-|-|-|-|-|-|
|6.4|N|O|O|O|O|O|O|O|O|  
|6.2|O|O|O|O|O|O|O|O|N|
|6.1|O|O|O|O|O|O|O|N|N|
|6.0|O|O|O|O|O|O|O|N|N|
|4.2|O|O|O|O|O|O|O|N|N|
|4.1|O|O|O|O|O|O|O|N|N|
|4.0|O|O|O|O|O|O|O|N|N|
|3|O|O|Y<sup>1</sup>|Y<sup>2</sup>|N|Y<sup>5</sup>|N|N|N|
|2|Y<sup>3</sup>|Y<sup>3</sup>|N|N|N|N|N|N|N|
|1.2|Y<sup>3</sup>|N|N|N|N|N|N|N|N|
|1.1|N|N|N|N|N|N|N|N|N|  
|1.0|N|N|N|N|N|N|N|N|N|  
|2000|N|N|N|N|N|N|N|N|N|  
  
 <sup>1</sup>Microsoft SQL Server JDBC Driver version 3.0 peut se connecter à SQL Server 2012 comme un client de bas niveau.  
  
 <sup>2</sup>prise en charge de la base de données SQL Azure a été introduite dans le pilote 3.0 comme un correctif logiciel. Nous recommandons aux clients d’Azure SQL Database d’utiliser la dernière version disponible du pilote.  
  
 <sup>3</sup>pilote JDBC version 2.0 de Microsoft SQL Server et Microsoft SQL Server 2005 JDBC Driver version 1.2 peuvent se connecter à SQL Server 2008 comme un client de bas niveau. Quand des conversions de bas niveau sont autorisées, les applications peuvent exécuter des requêtes et effectuer des mises à jour sur les nouveaux types de données SQL Server 2008, comme date, time, datetime2, datetimeoffset et FILESTREAM. Pour plus d’informations sur l’utilisation de ces nouveaux types de données avec le pilote JDBC, consultez  [Working with SQL Server 2008 Date/Time Data Types using JDBC Driver](http://go.microsoft.com/fwlink/?LinkId=145198) et  [Working with SQL Server 2008 FileStream using JDBC Driver](http://go.microsoft.com/fwlink/?LinkId=145199). Pour plus d’informations sur la compatibilité de bas niveau de ces nouveaux types de données, consultez les rubriques  [Utilisation des données de date et d’heure](http://go.microsoft.com/fwlink/?LinkId=145211)et  [FILESTREAM Support](http://go.microsoft.com/fwlink/?LinkId=145212) dans la documentation en ligne de SQL Server.  
  
 <sup>4</sup>prise en charge pour les connexions entre le pilote Microsoft JDBC et Parallel Data Warehouse a été introduite dans le pilote JDBC 4.0 Microsoft pour SQL Server et Microsoft SQL Server 2008 R2 parallèle données Warehouse Appliance Update 3.  
  
 <sup>5</sup>Microsoft SQL Server JDBC Driver version 3.0 peut se connecter à SQL Server 2014 comme un client de bas niveau.  
  
## <a name="java-and-jdbc-specification-support"></a>Java et prise en charge de la spécification JDBC  
  
|Version du pilote JDBC|Versions de JRE|Version de l’API JDBC| 
|-|-|-|  
|6.4|1.7, 1.8, 1.9|4.1, 4.2, 4.3 (partiellement)|  
|6.2|1.7, 1.8|4.1, 4.2|  
|6.1|1.7, 1.8|4.1, 4.2|  
|6.0|1.7, 1.8|4.1, 4.2|  
|4.2|1.7, 1.8|4.1, 4.2|  
|4.1|1.7|4.0|  
|4.0|1.5, 1.6, 1.7|3.0, 4.0|  
|3|1.5, 1.6,|3.0, 4.0|  
|2.0|1.5, 1.6|3.0, 4.0|  
|1.2|1.4, 1.5, 1.6|3|  
|1.1|1.4|3|  
|1.0|1.4|3|  
|2000|1.4|3.0|  
  
## <a name="supported-operating-systems"></a>Systèmes d'exploitation pris en charge  
 Le pilote Microsoft JDBC est conçu pour fonctionner sur tout système d’exploitation prenant en charge l’utilisation d’une machine virtuelle Java (JVM). Les plateformes couramment utilisées sont notamment Windows 10, Windows 8.1, Windows 8, Windows 7, Windows Server 2008 R2, Windows Vista, Linux, Unix, AIX et MacOS.  
  
 L’équipe du produit JDBC teste notre pilote sur Windows, Sun Solaris, SUSE Linux et RedHat Linux.  Le support technique est disponible pour les clients sur toutes les plateformes, mais nous sommes susceptibles de vous demander de reproduire le problème sur une plateforme comme Windows.  
  
## <a name="application-server-support"></a>Prise en charge des serveurs d’applications  
 Le pilote Microsoft JDBC pour SQL Server est testé avec différents serveurs d’applications.  Pour plus d’informations sur la version du pilote compatible avec leur produit, consultez le fournisseur de votre serveur d’applications.  
  
  

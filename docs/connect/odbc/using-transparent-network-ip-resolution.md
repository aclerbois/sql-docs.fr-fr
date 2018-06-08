---
title: À l’aide de la résolution IP réseau Transparent | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: d255208f-d486-4ad3-8080-61c6e0261825
caps.latest.revision: 2
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 2d76e50b4761e8d1a32bbcfc4606778f96513ed1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="using-transparent-network-ip-resolution"></a>À l’aide de la résolution IP réseau Transparent
[!INCLUDE[Driver_ODBC_Download](../../includes/driver_odbc_download.md)]

TransparentNetworkIPResolution est une révision de la fonctionnalité existante MultiSubnetFailover, disponible dans Microsoft ODBC Driver 13.1 for SQL Server, qui affecte la séquence de connexion du pilote dans le cas où la première adresse IP résolue du nom d’hôte ne pas répondre et qu’il existe plusieurs adresses IP associée avec le nom d’hôte. Il interagit avec MultiSubnetFailover pour fournir les séquences de trois connexions suivantes :

* 0 : une IP est tentée, suivie de toutes les adresses IP en parallèle
* 1 : toutes les adresses IP sont tentées en parallèle
* 2 : toutes les adresses IP sont tentées une après l’autre

|TransparentNetworkIPResolution|MultiSubnetFailover|Comportement|
|:-:|:-:|:-:|
|(par défaut)|(par défaut)|0|
|(par défaut)|Activé|1|
|(par défaut)|Désactivé|0|
|Activé|(par défaut)|0|
|Activé|Activé|1|
|Activé|Désactivé|0|
|Désactivé|(par défaut)|2|
|Désactivé|Activé|1|
|Désactivé|Désactivé|2|

Le `TransparentNetworkIPResolution` chaîne de connexion et de la source de données (mot clé) contrôle ce paramètre au niveau de la chaîne de connexion. La valeur par défaut est activée.

Mot clé|Valeurs|Par défaut
-|-|-
`TransparentNetworkIPResolution`|`Yes`, `No`|`Yes`

Le `SQL_COPT_SS_TNIR` attribut de préconnexion permet à une application contrôler ce paramètre par programmation :

Attribut de connexion|   Type de la taille|  Par défaut| Valeur|  Description
-|-|-|-|-
`SQL_COPT_SS_TNIR` (1249)| `SQL_IS_INTEGER`ou`SQL_IS_UINTEGER`| `SQL_IS_ON`(1), `SQL_IS_OFF`(0)|`SQL_IS_ON`|Active ou désactive les TNIR.

<a name="for-more-information-about-multisubnetfailover-see-odbc-driver-on-linux-and-macos---high-availability-and-disaster-recoveryconnectodbclinux-macodbc-driver-on-linux-support-for-high-availability-disaster-recoverymd"></a>Pour plus d’informations sur MultiSubnetFailover, consultez [pilote ODBC sur Linux et macOS - haute disponibilité et récupération d’urgence](../../connect/odbc/linux-mac/odbc-driver-on-linux-support-for-high-availability-disaster-recovery.md)
--------------------------------------------------
## <a name="see-also"></a>Voir aussi  
* [Microsoft ODBC Driver for SQL Server sur Windows](../../connect/odbc/windows/microsoft-odbc-driver-for-sql-server-on-windows.md)
* [SQL Server Clustering de sous-réseaux multiples (SQL Server)](https://msdn.microsoft.com/library/ff878716.aspx#RelatedContent)

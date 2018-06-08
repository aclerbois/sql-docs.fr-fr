---
title: En bloc charge des considérations de sécurité (SQLXML 4.0) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: sqlxml
ms.reviewer: ''
ms.suite: sql
ms.technology: xml
ms.tgt_pltfrm: ''
ms.topic: reference
helpviewer_keywords:
- SQLXML, XML Bulk Load
- bulk load [SQLXML], security
- security [SQLXML], XML Bulk Load
- XML Bulk Load [SQLXML], security
ms.assetid: 192fc6d4-ecbc-4a4d-a5cb-55e1f64af318
caps.latest.revision: 24
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: dd240ede23b5ff4845531bebf24529bd0219c5ed
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="bulk-load-security-considerations-sqlxml-40"></a>Considérations de sécurité relatives au chargement en masse (SQLXML 4.0)
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]
  Vous trouverez ci-après des instructions de sécurité relatives au chargement en masse XML :  
  
-   Lorsque vous spécifiez que le chargement en masse opération doit être effectuée comme une transaction, vous utilisez la **TempFilePath** propriété pour spécifier un dossier dans lequel créer les fichiers temporaires.  
  
     Le processus de chargement en masse crée ces fichiers temporaires avec les autorisations suivantes :  
  
    -   L'accès en lecture/écriture/suppression est accordé au processus de chargement en masse.  
  
    -   L'accès en lecture est accordé à tous les utilisateurs car le compte sous lequel Microsoft [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] accédera à ces fichiers est inconnu. Vous pouvez restreindre l'accès à ces fichiers temporaires en définissant les autorisations appropriées sur le dossier qui les contient.  
  
-   Le chargement en masse XML ne possède en lui-même aucun paramètre d'autorisation. Il est supposé que la base de données est installée correctement et que le contexte d'utilisateur (autrement dit, la connexion que le chargement en masse est configuré pour utiliser) dispose de jeux d'autorisations appropriés.  
  
-   En mode non transactionnel, si une erreur se produit pendant le processus de chargement en masse, les données peuvent être laissées dans un état partiellement chargé. Dans ce cas, le chargement en masse s'arrête simplement au point où il est. Le mode transactionnel peut être utilisé pour pallier ce problème.  
  
-   Lorsque des erreurs de chargement en masse se produisent, elles peuvent inclure des informations à propos de la base de données. Par exemple, elles peuvent inclure le nom d'une table ou d'une colonne, ou des informations de type de colonne. Lorsque vous utilisez le chargement en masse, vous devez prendre soin d'intercepter les erreurs du processus de chargement en masse et de retourner un message d'erreur générique, plutôt que d'exposer directement les erreurs aux utilisateurs.  
  
-   Le chargement en masse ne définit aucune limite quant à la quantité de données parcourue. Il n'effectue aucun contrôle de la taille des données à charger. Il incombe à l'utilisateur qui exécute le chargement en masse de s'assurer qu'il y a suffisamment de mémoire pour traiter le fichier spécifié et suffisamment d'espace dans la base de données pour stocker les données chargées.  
  
-   Le chargement en masse ne tente pas d'utiliser les données qui lui sont données comme code. L'entrée de données n'est jamais exécutée de quelque façon. Tout code ou commande dans les données d'entrée est traité(e) comme données normales et n'est pas exécuté(e).  
  
-   Le chargement en masse peut apporter des modifications de mise en forme aux données en question en fonction des différences entre les modèles de données XML et [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Par exemple, le format pour spécifier une heure est différent. Le chargement en masse tente de résoudre ces différences. En conséquence, une perte de certaines informations de précision est envisageable.  
  
-   Le chargement en masse n'impose aucune limite sur la durée nécessaire pour traiter les données. Le traitement se poursuit jusqu'au bout ou jusqu'à ce qu'une erreur se produise.  
  
-   Le chargement en masse peut créer et supprimer des tables temporaires dans la base de données et il a besoin d'autorisations pour cela. Les autorisations pour ces tables seront accordées au même utilisateur qui se connecte à la base de données pour le processus de chargement en masse.  
  
-   Le chargement en masse peut créer et supprimer des fichiers temporaires utilisés pendant le traitement en mode transactionnel et il a besoin d'autorisations pour cela. Ces fichiers sont créés avec les mêmes autorisations que l'utilisateur actuel du thread dans lequel le chargement en masse s'exécute.  
  
-   Si l'utilisateur définit un fichier journal des erreurs pour SQLXML, chaque fois que le chargement en masse est exécuté, le fichier est écrasé avec les données du dernier processus de chargement en masse.  
  
## <a name="see-also"></a>Voir aussi  
 [Effectuer le chargement en masse des données XML & #40 ; SQLXML 4.0 & #41 ;](../../../relational-databases/sqlxml-annotated-xsd-schemas-xpath-queries/bulk-load-xml/performing-bulk-load-of-xml-data-sqlxml-4-0.md)  
  
  

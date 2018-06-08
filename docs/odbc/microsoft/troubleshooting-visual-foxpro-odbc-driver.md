---
title: Résolution des problèmes (le pilote ODBC Visual FoxPro) | Documents Microsoft
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
- set ANSI [ODBC]
- troubleshooting Visual FoxPro ODBC driver [ODBC]
- remote views [ODBC]
- multitiered views [ODBC]
- parameterized views [ODBC], Visual FoxPro ODBC driver
- fetches [ODBC], Visual FoxPro ODBC driver
- positioned updates [ODBC]
- background fetching [ODBC]
ms.assetid: fd478dd8-666a-4f0a-a2d6-b94e81cbbe4b
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 23bab07c1f00abc9fb0d2c353603a21b58975933
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="troubleshooting-visual-foxpro-odbc-driver"></a>Dépannage (Visual FoxPro le pilote ODBC)
Les sections suivantes expliquent comment améliorer les performances et résoudre les problèmes que vous pouvez rencontrer lors de l’utilisation du pilote ODBC Visual FoxPro.  
  
## <a name="accessing-parameterized-views"></a>L’accès à des vues paramétrées  
 Impossible d’accéder à des vues paramétrées dans une base de données Visual FoxPro à l’aide du pilote. Une vue paramétrée crée une clause WHERE dans SQL la vue **sélectionnez** instruction qui limite les enregistrements téléchargés vers les enregistrements qui remplissent les conditions de la clause WHERE générée à l’aide de la valeur fournie pour le paramètre. Étant donné que le pilote ne prend pas en charge la transmission de paramètres à la vue, tente d’accéder à une vue paramétrable à l’aide du pilote échoue.  
  
 La valeur du paramètre peut être fournie au moment de l’exécution ou passée par programme à la vue.  
  
## <a name="accessing-remote-views"></a>L’accès aux vues distantes  
 Impossible d’accéder à des vues distantes dans une base de données Visual FoxPro à l’aide du pilote. Vues distantes sont des vues qui accèdent aux données non-FoxPro ou une combinaison de FoxPro et les données non-FoxPro. Pour accéder aux vues distantes, utilisez Visual FoxPro.  
  
## <a name="deleting-records"></a>Suppression d’enregistrements  
 Vous pouvez marquer les enregistrements marqués en suppression à l’aide du pilote, mais vous ne pouvez pas supprimer définitivement les enregistrements à partir de la base de données. Pour supprimer définitivement les enregistrements d’une table, utilisez Visual FoxPro.  
  
## <a name="increasing-performance-using-background-fetching"></a>Amélioration des performances à l’aide de l’extraction en arrière-plan  
 Vous pouvez améliorer les performances sur les extractions de grande taille à l’aide de l’arrière-plan de l’extraction de la fonctionnalité du pilote. L’extraction en arrière-plan d’utilise un thread séparé pour extraire les données demandées à partir d’une source de données spécifique.  
  
 Vous pouvez employer l’arrière-plan de l’extraction d’une source de données dans une des manières suivantes :  
  
-   Vérifiez le **extraire les données en arrière-plan** case à cocher sur la [boîte de dialogue d’installation de Visual FoxPro ODBC](../../odbc/microsoft/odbc-visual-foxpro-setup-dialog-box.md).  
  
-   Utilisez le mot clé d’attribut BackgroundFetch dans votre chaîne de connexion.  
  
 Pour plus d’informations sur les mots clés attribut de chaîne de connexion, consultez [à l’aide de chaînes de connexion](../../odbc/microsoft/using-connection-strings.md).  
  
## <a name="updating-multitiered-views"></a>Mise à jour des vues de plusieurs niveaux  
 Une vue multicouche est une vue basée sur une ou plusieurs vues plutôt que sur une table de base. Lorsque vous mettez à jour des données dans un affichage de plusieurs niveaux, les mises à jour s’arrêtent qu’un seul niveau, à la vue sur laquelle repose la vue de niveau supérieur ; tables de base ne sont pas mis à jour.  
  
## <a name="using-data-definition-language-ddl-in-stored-procedures"></a>À l’aide du langage de définition de données (DDL) dans les procédures stockées  
 Vous ne pouvez pas utiliser DDL, telles que CREATE TABLE ou ALTER TABLE, dans les procédures stockées de Visual FoxPro.  
  
 Pour plus d’informations sur le langage que vous pouvez utiliser dans les procédures stockées, consultez [prise en charge pour les règles, les déclencheurs, les valeurs par défaut et les procédures stockées](../../odbc/microsoft/support-rules-triggers-defaults-stored-procedures-visual-foxpro-odbc-driver.md).  
  
## <a name="using-positioned-updates"></a>À l’aide de mises à jour positionnées  
 Le pilote ne prend pas en charge les mises à jour positionnées. Utilisez la clause SQL WHERE pour identifier les lignes que vous souhaitez mettre à jour.  
  
## <a name="using-the-set-ansi-command"></a>À l’aide de la commande de ANSI SET  
 Si vous êtes un développeur Visual FoxPro, sachez que le paramètre par défaut pour SET ANSI est activé pour le pilote, contrairement à un paramètre par défaut (OFF) pour Visual FoxPro. La valeur par défaut sur le paramètre de SET ANSI permet à des sources de données Visual FoxPro à un comportement cohérent avec d’autres sources de données ODBC qui effectuent des comparaisons exactes. Vous pouvez modifier le paramètre par défaut. Pour plus d’informations, consultez [SET ANSI](../../odbc/microsoft/set-ansi-command.md).

---
title: Méthode StartService (classe SqlService) | Documents Microsoft
ms.custom: ''
ms.date: 03/03/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: wmi
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: reference
apiname:
- StartService Method (SqlService Class)
apilocation:
- sqlmgmproviderxpsp2up.mof
apitype: MOFDef
helpviewer_keywords:
- StartService method
ms.assetid: 83dfb6bd-dbd5-45d8-aad2-a11926317f91
caps.latest.revision: 34
author: CarlRabeler
ms.author: carlrab
manager: craigg
ms.openlocfilehash: 0b102090d48a773c38776afb73a62f0431b9f076
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="startservice-method-sqlservice-class"></a>Méthode StartService (classe SqlService)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]
  Tente de placer le service dans son état démarré.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
object.StartService()  
```  
  
## <a name="parts"></a>Éléments  
 *objet*  
 Objet de [classe SqlService](../../../relational-databases/wmi-provider-configuration-classes/sqlservice-class/sqlservice-class.md) qui représente le service.  
  
## <a name="property-valuereturn-value"></a>Valeur de propriété/valeur de retour  
 Valeur uint32 qui spécifie l'un des états de démarrage suivants.  
  
 0  
 Réussite. La demande a été acceptée.  
  
 1  
 Non pris en charge. La demande n'est pas prise en charge.  
  
 2  
 Accès refusé. L'utilisateur ne disposait pas des droits d'accès appropriés.  
  
 3  
 Services dépendants en cours d'exécution. Le service ne peut pas être arrêté car d'autres services en cours d'exécution en dépendent.  
  
 4  
 Contrôle de service non valide. Le code de contrôle demandé n'est pas valide ou est inacceptable pour le service.  
  
 5  
 Le service n'accepte pas le contrôle. Le code de contrôle demandé ne peut pas être envoyé au service car l'état du service (Win32_BaseService:State) est égal à 0, 1 ou 2.  
  
 6  
 Service inactif. Le service n'a pas été démarré.  
  
 7  
 Délai de la demande de service dépassé. Le service n'a pas répondu à la demande de démarrage en temps voulu.  
  
 8  
 Échec inconnu. Un échec inconnu s'est produit lors du démarrage du service.  
  
 9  
 Chemin d'accès introuvable. Le chemin d'accès au répertoire de l'exécutable du service n'a pas été trouvé.  
  
 10  
 Le service est déjà en cours d'exécution. Le service est déjà en cours d'exécution.  
  
 11  
 Base de données du service verrouillée. La base de données pour ajouter un nouveau service est verrouillée.  
  
 12  
 Dépendance du service supprimée. Une dépendance sur laquelle repose ce service a été supprimée du système.  
  
 13  
 Échec de la dépendance du service. Le service n'a pas pu trouver le service nécessaire à partir d'un service dépendant.  
  
 14  
 Service désactivé. Le service a été désactivé du système.  
  
 15  
 Échec de connexion au service. Le service ne dispose pas de l'authentification correcte pour être exécuté sur le système.  
  
 16  
 Service marqué pour suppression. Le service est en cours de suppression du système.  
  
 17  
 Service sans thread. Il n'y a pas de thread d'exécution pour le service.  
  
 18  
 Dépendance circulaire de l'état. Le démarrage du service donne lieu à des dépendances circulaires.  
  
 19  
 Nom de l'état déjà existant. Un service est en cours d'exécution sous le même nom.  
  
 20  
 Nom de l'état non valide. Le nom du service contient des caractères non valides.  
  
 21  
 Paramètre d'état non valide. Des paramètres non valides ont été transmis au service.  
  
 22  
 Compte du service d'état non valide. Le compte sous lequel ce service doit s'exécuter n'est pas valide ou il ne dispose pas des autorisations nécessaires pour exécuter le service.  
  
 23  
 Le service d'état existe déjà. Le service existe dans la base de données des services disponibles dans le système.  
  
 24  
 Service déjà en pause. Le service est actuellement mis en pause dans le système.  
  
## <a name="remarks"></a>Notes  
  
## <a name="see-also"></a>Voir aussi  
 [Démarrage et arrêt des Services](http://technet.microsoft.com/library/ms174886\(v=sql.105\).aspx)  
  
  

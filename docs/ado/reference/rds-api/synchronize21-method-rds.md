---
title: Synchronize21, méthode (RDS) | Documents Microsoft
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.prod: sql
ms.prod_service: drivers
ms.component: reference
ms.tgt_pltfrm: ''
ms.topic: conceptual
apitype: COM
helpviewer_keywords:
- Synchronize21 method [ADO]
ms.assetid: 6b35f136-9d9a-4bdd-8144-67decfd3c4e9
caps.latest.revision: 17
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: caf886971b2163bcdcf855ad22a43b62d4c0b529
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="synchronize21-method-rds"></a>Synchronize21, méthode (RDS)
Synchroniser le jeu d’enregistrements donné avec la base de données spécifiée par la chaîne de connexion pour une utilisation avec ADO 2.1.  
  
> [!IMPORTANT]
>  À compter de Windows 8 et Windows Server 2012, les composants de serveur Services Bureau à distance ne sont plus inclus dans le système d’exploitation Windows (consultez Windows 8 et [Cookbook de compatibilité de Windows Server 2012](https://www.microsoft.com/en-us/download/details.aspx?id=27416) pour plus de détails). Composants du client Bureau à distance seront supprimées dans une future version de Windows. Évitez d'utiliser cette fonctionnalité dans de nouveaux travaux de développement, et prévoyez de modifier les applications qui utilisent actuellement cette fonctionnalité. La migration vers les applications qui utilisent des services Bureau à distance [Service de données WCF](http://go.microsoft.com/fwlink/?LinkId=199565).  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
object.Synchronize21(ConnectionString As String, HandlerString As String, lSynchronizeOptions As Long, ppRecordset As Object, pStatusArray)  
```  
  
#### <a name="parameters"></a>Paramètres  
 *connectionString*  
 Chaîne utilisée pour se connecter au fournisseur OLE DB où la demande va être envoyée. Si un gestionnaire est utilisé, le gestionnaire peut modifier ou remplacer la chaîne de connexion.  
  
 *HandlerString*  
 La chaîne identifie le gestionnaire à utiliser avec cette exécution. La chaîne contient deux parties. La première partie contient le nom (ProgID) du gestionnaire à utiliser. La deuxième partie de la chaîne contient des arguments à passer au gestionnaire. Interprétation de la chaîne d’arguments est gestionnaire spécifique. Les deux parties sont séparées par la première instance d’une virgule dans la chaîne. La chaîne d’arguments peut contenir des virgules supplémentaires. Les arguments sont facultatifs.  
  
 *lSynchronizeOptions*  
 Un masque de bits des options de synchronisation.  
  
 1 =*UpdateTransact* mises à jour de la base de données sont encapsulés dans une transaction. La transaction est abandonnée si des mises à jour échouent.  
  
 2 =*RefreshWithUpdate* Causes de lignes à retourner lorsque aucune des deux états *Actualiser* ni *RefreshConflicts* est défini.  
  
 4 =*Actualiser* le jeu d’enregistrements est actualisée avec les données actuelles de la base de données. En attente de mises à jour ne sont pas envoyées à la base de données. Si ce bit n’est pas défini, le jeu d’enregistrements n’est pas actualisé et les mises à jour en attente sont transmis à la base de données.  
  
 8 =*RefreshConflicts* échouent de toutes les lignes avec des modifications en attente mettre à jour. Les lignes qui n’a pas pu mettre à jour sont actualisées avec des données actuelles de la base de données.  
  
 *ppRecordset*  
 Pointeur vers un pointeur vers le jeu d’enregistrements à synchroniser.  
  
 *pStatusArray*  
 Permet de retourner un tableau sécurisé de statuts de ligne pour les lignes affectées par une variante de synchroniser. Ne pas définie si aucune des options de synchronisation suivantes sont définies : *RefreshWithUpdate*, *Actualiser* et *RefreshConflicts*.  
  
## <a name="remarks"></a>Notes  
 Le *HandlerString* paramètre peut être null. Que se passe-t-il dans ce cas dépend de la façon dont le serveur de services Bureau à distance est configuré. Une chaîne de gestionnaire de « MSDFMAP.handler » indique que le gestionnaire fourni par Microsoft (Msdfmap.dll) doit être utilisé. Une chaîne de gestionnaire de « MASDFMAP.handler,sample.ini » indique que le Gestionnaire de Msdfmap.dll doit être utilisé et que l’argument « sample.ini » doit être passé au gestionnaire. MSDFMAP.dll puis interprétera l’argument comme une direction à utiliser le sample.ini pour vérifier les chaînes de connexion et la requête.  
  
> [!NOTE]
>  Le **Synchronize21** consiste simplement à une version de la [synchroniser (méthode) (RDS)](../../../ado/reference/rds-api/synchronize-method-rds.md). Où vous devez utiliser le **synchroniser** méthode pour communiquer avec ADO 2.1, le **Synchronize21** méthode peut être appelée à la place. Les fonctionnalités de la **synchroniser** méthode dans ADO 2.5 et versions ultérieur sont un sur-ensemble des fonctionnalités de la même méthode dans ADO 2.1.  
  
## <a name="applies-to"></a>S'applique à  
 [DataFactory, objet (RDSServer)](../../../ado/reference/rds-api/datafactory-object-rdsserver.md)



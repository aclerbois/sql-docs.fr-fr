---
title: Gérer les sessions d’événements dans l’Explorateur d’objets | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.reviewer: ''
ms.suite: sql
ms.technology: xevents
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: 16849e38-d3fb-414d-8dcb-797b5ffce6ee
caps.latest.revision: 10
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: a5a41ca578e22b2af628e8740acbb58395fb24ec
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="manage-event-sessions-in-the-object-explorer"></a>Gérer les sessions d'événements dans l'Explorateur d'objets
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  Cette rubrique décrit les actions effectuées dans l' **Explorateur d'objets** qui affectent les événements étendus :  
  
-   Créer une session d'événements étendus  
  
-   Démarrer ou arrêter une session d'événements étendus  
  
-   Exporter une session d'événements étendus  
  
-   Importer un modèle de session d'événements étendus  
  
-   Modifier une session d'événements étendus  
  
-   Supprimer une session d'événements étendus  
  
## <a name="create-an-extended-events-session"></a>Créer une session d'événements étendus  
 Pour plus d'informations sur la création d'une session d'événements étendus, consultez [Create an Extended Events Session](http://msdn.microsoft.com/library/34b1e95a-a80e-4aca-9201-abde47f2ca74).  
  
## <a name="starting-or-stopping-an-extended-events-session"></a>Démarrer ou arrêter une session d'événements étendus  
 Vous pouvez démarrer ou arrêter une session d'événements étendus via l' **Éditeur de requête** en utilisant l'instruction **ALTER EVENT SESSION** , ou en utilisant le nœud **Événements étendus** de l' **Explorateur d'objets**.  
  
 Quand vous arrêtez une session d’événements, la session n’apparaît plus comme une session active dans la vue de gestion dynamique sys.dm_xe_sessions. Toutefois, la définition de session reste intacte, et vous pouvez redémarrer la session. Pour supprimer complètement une définition de session, vous devez supprimer la session.  
  
 Pour démarrer ou arrêter une session d'événements étendus, vous devez disposer de l'autorisation ALTER ANY EVENT SESSION.  
  
 Quand vous arrêtez une session qui utilise une cible en mémoire, telle que la cible de mémoire tampon en anneau, la cible de création de compartiments, la cible d’appariement d’événements ou la cible de compteur d’événements synchrone, toutes les informations stockées dans la mémoire tampon de la session (colonne target_data de la vue de gestion dynamique sys.dm_xe_session_targets) sont perdues. Pour accéder aux données d'un événement après avoir interrompu une session, vous devez enregistrer les données avant de mettre fin à la session ou configurer la session pour pouvoir utiliser un fichier cible.  
  
### <a name="start-or-stop-an-extended-events-session-using-query-editor"></a>Démarrer ou arrêter une session d'événements étendus à l'aide de l'Éditeur de requête  
 Pour démarrer une session, émettez les instructions suivantes, en remplaçant *session_name* par le nom de la session d’événements étendus :  
  
```  
ALTER EVENT SESSION [session_name]  
ON SERVER  
STATE = START  
```  
  
 Pour arrêter une session, émettez les instructions suivantes, en remplaçant *session_name* par le nom de la session d’événements étendus :  
  
```  
ALTER EVENT SESSION [session_name]  
ON SERVER  
STATE = STOP  
```  
  
### <a name="start-or-stop-an-extended-events-session-in-object-explorer"></a>Démarrer ou arrêter une session d'événements étendus dans l'Explorateur d'objets  
 Pour démarrer ou arrêter une session d'événements étendus dans l' **Explorateur d'objets**, développez **Gestion**, **Événements étendus**, puis les nœuds **Sessions** ; cliquez ensuite avec le bouton droit sur une session, puis cliquez sur **Démarrer la session** ou sur **Arrêter la session**.  
  
## <a name="export-an-extended-events-session-template"></a>Exporter un modèle de session d'événements étendus  
 Vous pouvez exporter une session d'événements étendus à l'aide de l' **Explorateur d'objets**, puis l'enregistrer en tant que fichier modèle .xml. Par exemple, vous pouvez exporter une session, puis appliquer le modèle à une nouvelle session d'événements à l'aide de l' **Assistant Nouvelle session** ou de l'interface **Nouvelle session** .  
  
 Lorsque vous exportez une session, assurez-vous d'enregistrer le fichier modèle à un emplacement qui utilise le système de fichiers NTFS, et que vous limitez l'accès aux utilisateurs autorisés à afficher les informations.  
  
 Pour exporter une session d'événements étendus à l'aide de l' **Explorateur d'objets**:  
  
1.  Développez les nœuds **Gestion**, **Événements étendus**, puis **Sessions** .  
  
2.  Cliquez avec le bouton droit sur la session à exporter, puis sélectionnez **Exporter une session**.  
  
3.  Dans la boîte de dialogue **Enregistrer sous** , sélectionnez un emplacement pour enregistrer le fichier, tapez le nom du fichier dans la zone **Nom de fichier** , puis cliquez sur **Enregistrer**.  
  
     Si vous enregistrez le fichier à l'emplacement de modèle par défaut [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] , le modèle s'affiche dans la liste déroulante des modèles prédéfinis lorsque vous utilisez l' **Assistant Nouvelle session** et la boîte de dialogue **Nouvelle session** .  
  
## <a name="import-an-extended-events-session-template"></a>Importer un modèle de session d'événements étendus  
 À l'aide de l' **Explorateur d'objets**, vous pouvez importer un modèle pour une session d'événements étendus. Par exemple, vous pouvez souhaiter procéder ainsi pour créer une session a partir d'un modèle qui a été exporté à partir d'une autre instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)].  
  
 Pour importer une session d'événements étendus, vous devez disposer des autorisations **ALTER ANY EVENT SESSION** nécessaires.  
  
 Avant d'importer un fichier modèle, vérifiez que le fichier provient d'une source approuvée. Les fichiers modèles doivent être enregistrés dans un emplacement qui utilise le système de fichiers NTFS et où l'accès est limité aux utilisateurs qui sont autorisés à afficher les informations.  
  
 Pour importer une session d'événements étendus :  
  
1.  Dans l' **Explorateur d'objets**, développez les nœuds **Gestion**, puis **Événements étendus** .  
  
2.  Cliquez avec le bouton droit sur **Sessions** et sélectionnez **Nouvelle session**.  
  
3.  Donnez un nom à la session.  
  
4.  Développez la zone déroulante **Modèle** .  
  
5.  Cliquez sur **\<File From…> Open**, puis recherchez la session (fichier XML) à importer.  
  
 La session apparaît sous le nœud **Sessions** . Par défaut, la session n'est pas démarrée.  
  
## <a name="edit-an-extended-events-session"></a>Modifier une session d'événements étendus  
 Vous pouvez modifier une session d'événements étendus dans l'Explorateur d'objets.  
  
 Pour modifier une session d'événements étendus :  
  
1.  Dans l' **Explorateur d'objets**, développez les nœuds **Gestion**, **Événements étendus**, puis **Sessions** .  
  
2.  Cliquez avec le bouton droit sur une session et sélectionnez **Propriétés**.  
  
3.  Dans la section **Sélectionner une page** , sélectionnez la ou les pages que vous souhaitez modifier.  
  
4.  Après avoir modifié la session d'événements, cliquez sur **OK**.  
  
## <a name="script-an-event-session-definition-using-includetsqlincludestsql-mdmd"></a>Générer le script d'une définition de session d'événements à l'aide de [!INCLUDE[tsql](../../includes/tsql-md.md)]  
 L'Assistant Nouvelle session et la boîte de dialogue Nouvelle session comprennent une option qui génère un script [!INCLUDE[tsql](../../includes/tsql-md.md)] définissant la session événements étendus.  
  
 Vous pouvez accéder à [!INCLUDE[tsql](../../includes/tsql-md.md)] pour une session d'événements étendus existante en cliquant sur le nom de la session, en sélectionnant **Générer un script de la session en tant que**, puis sélectionnant **Créer vers**.  
  
## <a name="delete-an-extended-events-session"></a>Supprimer une session d'événements étendus  
 Vous pouvez supprimer une session d'événements étendus :  
  
-   Dans l'Éditeur de requête, à l'aide de **DROP EVENT SESSION**.  
  
-   Dans l' **Explorateur d'objets**.  
  
 Quand vous supprimez une session d’événements, toutes les informations de configuration sont supprimées, et la définition de session ne s’affiche plus dans l’affichage catalogue sys.server_event_sessions.  
  
> [!NOTE]  
>  system_health et Always On_health sont fournies avec [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]; ne les supprimez pas. system_health est activée par défaut (pour plus d’informations, consultez [Utiliser la session system_health](../../relational-databases/extended-events/use-the-system-health-session.md)). Always On_health est désactivée par défaut. Ces sessions collectent des données qui peuvent être utiles pour le diagnostic des problèmes de performances.  
  
 Pour supprimer une session d'événements étendus, vous devez disposer de l'autorisation ALTER ANY EVENT SESSION.  
  
 Pour supprimer une session d'événements étendus dans l' **Explorateur d'objets**:  
  
1.  Développez les nœuds **Gestion**, **Événements étendus**, puis **Sessions** .  
  
2.  Cliquez avec le bouton droit sur une session et sélectionnez **Supprimer**.  
  
3.  Dans la boîte de dialogue **Supprimer l'objet** , cliquez sur **OK**.  
  
4.  Après avoir modifié la session d'événements, cliquez sur **OK**.  
  
 Pour supprimer une session d’événements étendus dans l’ **Éditeur de requête**, émettez les instructions suivantes en remplaçant *session_name* par le nom de la session d’événements étendus à supprimer :  
  
```  
DROP EVENT SESSION [session_name]  
ON SERVER  
```  
  
  

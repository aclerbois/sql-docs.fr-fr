---
title: Créer un audit du serveur et une spécification d’audit du serveur | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: security
ms.reviewer: ''
ms.suite: sql
ms.technology:
- database-engine
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.SWB.SQLAUDIT.FILTER.F1
- sql13.swb.sqlaudit.general.f1
- sql13.swb.sqlaudit.srvaudit.general.f1
helpviewer_keywords:
- server audit [SQL Server]
- audits [SQL Server], specification
ms.assetid: 6624b1ab-7ec8-44ce-8292-397edf644394
caps.latest.revision: 21
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 5bb0b9f0afe0549bcd3bef18f38becedd5dec613
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-a-server-audit-and-server-audit-specification"></a>Créer un audit du serveur et une spécification d'audit du serveur
[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md](../../../includes/appliesto-ss-xxxx-xxxx-xxx-md.md)]
  Cette rubrique explique comment créer un audit de serveur et une spécification d'audit de serveur dans [!INCLUDE[ssCurrent](../../../includes/sscurrent-md.md)] à l'aide de [!INCLUDE[ssManStudioFull](../../../includes/ssmanstudiofull-md.md)] ou de [!INCLUDE[tsql](../../../includes/tsql-md.md)]. L'*audit* d'une instance de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ou d'une base de données [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] implique le suivi et l'enregistrement des événements qui se produisent sur le système. L’objet *Audit SQL Server* recueille une seule instance des actions et des groupes d’actions au niveau du serveur ou de la base de données à surveiller. L'audit s'effectue au niveau de l'instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . Vous pouvez exécuter plusieurs audits par instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] . L'objet *Spécification de l'audit du serveur* appartient à un audit. Vous pouvez créer une spécification d'audit de serveur par audit, car tous deux sont créés au niveau de la portée de l'instance [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)]. Pour plus d’informations, consultez [SQL Server Audit &#40moteur de base de données&#41;](../../../relational-databases/security/auditing/sql-server-audit-database-engine.md).  
  
 **Dans cette rubrique**  
  
-   **Avant de commencer :**  
  
     [Limitations et restrictions](#Restrictions)  
  
     [Sécurité](#Security)  
  
-   **Pour créer un audit de serveur et une spécification d'audit de serveur, utilisez :**  
  
     [SQL Server Management Studio](#SSMSProcedure)  
  
     [Transact-SQL](#TsqlProcedure)  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Restrictions"></a> Limitations et restrictions  
  
-   Un audit doit exister pour que vous puissiez créer une spécification d'audit de serveur correspondante. Lorsqu'une spécification d'audit de serveur est créée, elle est dans un état désactivé.  
  
-   L'instruction CREATE SERVER AUDIT fait partie de l'étendue d'une transaction. Si la transaction est restaurée, l'instruction l'est également.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
  
-   Pour créer, modifier ou supprimer un audit du serveur, les principaux requièrent l'autorisation ALTER ANY SERVER AUDIT ou CONTROL SERVER.  
  
-   Les utilisateurs disposant de l'autorisation ALTER ANY SERVER AUDIT peuvent créer des spécifications d'audit du serveur et les lier à un audit quelconque.  
  
-   Une fois qu’une spécification d’audit du serveur est créée, elle peut être affichée par des principaux disposant des autorisations CONTROL SERVER ou ALTER ANY SERVER AUDIT, du compte sysadmin ou de principaux ayant un accès explicite à l’audit.  
  
##  <a name="SSMSProcedure"></a> Utilisation de SQL Server Management Studio  
  
#### <a name="to-create-a-server-audit"></a>Pour créer un audit de serveur  
  
1.  Dans l'Explorateur d'objets, développez le dossier **Sécurité** .  
  
2.  Cliquez avec le bouton droit sur le dossier **Audits** et sélectionnez **Nouvel audit**.  
  
     Les options suivantes sont disponibles sur la page **Général** de la boîte de dialogue **Créer un audit** .  
  
     **Nom de l'audit**  
     Nom de l'audit. Il est généré automatiquement lorsque vous créez un audit, mais il est modifiable.  
  
     **Délai de file d'attente (en millisecondes)**  
     Spécifie la durée (en millisecondes) qui peut s'écouler avant que le traitement des actions d'audit ne soit forcé.  Une valeur de 0 indique la remise synchrone. La valeur minimale par défaut est **1000** (1 seconde). Le maximum est 2 147 483 647 (2 147 483,647 secondes ou 24 jours, 20 heures, 31 minutes, 23,647 secondes).  
  
     **En cas de défaillance du journal d'audit :**  
     **Continuer**  
     [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] Les opérations continuent. Les enregistrements d'audit ne sont pas conservés. L'audit poursuit sa tentative de consignation des événements et reprend les opérations d'enregistrement une fois la défaillance résolue. La sélection de l'option **Continuer** peut permettre l'exécution d'une activité non auditée susceptible d'enfreindre vos stratégies de sécurité. Sélectionnez cette option quand la poursuite de l’opération du [!INCLUDE[ssDE](../../../includes/ssde-md.md)] est plus importante que la conservation d’un audit complet. Il s'agit de la sélection par défaut.  
  
     **Arrêter le serveur**  
     Force un arrêt du serveur lorsque l'instance de serveur qui écrit dans la cible ne peut pas écrire de données dans la cible d'audit. La connexion qui émet cette commande d’arrêt doit avoir l’autorisation **SHUTDOWN** . Si la connexion n'a pas cette autorisation, cette fonction échoue et un message d'erreur est généré. Aucun événement audité ne se produit. Sélectionnez cette option si une défaillance de l'audit risque de compromettre la sécurité ou l'intégrité du système.  
  
     **Faire échouer l'opération**  
     Lorsque l'audit de [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne peut pas écrire dans le journal d'audit, cette option provoque l'échec des actions de base de données si celles-ci entraînent des événements audités. Aucun événement audité ne se produit. Les actions qui n'entraînent pas d'événements audités peuvent continuer. L'audit poursuit sa tentative de consignation des événements et reprend les opérations d'enregistrement une fois la défaillance résolue. Sélectionnez cette option quand la conservation d’un audit complet est plus importante que l’accès total au [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
    > [!IMPORTANT]  
    >  Lorsque l'audit est en état d'échec, la connexion administrateur dédiée peut continuer à exécuter des événements audités.  
  
     Liste**Destination de l’audit**   
     Spécifie la cible pour l'audit des données. Les options disponibles sont un fichier binaire, le journal des applications Windows ou le journal de sécurité Windows. [!INCLUDE[ssNoVersion](../../../includes/ssnoversion-md.md)] ne peut pas écrire dans le journal de sécurité Windows sans configurer d'autres paramètres dans Windows. Pour plus d’informations, consultez [Écrire des événements d’audit SQL Server dans le journal de sécurité](../../../relational-databases/security/auditing/write-sql-server-audit-events-to-the-security-log.md).  
  
     **Chemins d'accès au fichier**  
     Spécifie l’emplacement du dossier dans lequel les données d’audit sont écrites quand la **destination de l’audit** est un fichier.  
  
     **Points de suspension (…)**  
     Ouvre la boîte de dialogue **Rechercher un dossier –***nom_serveur* qui permet de spécifier un chemin de fichier ou de créer un dossier dans lequel écrire le fichier d’audit.  
  
     **Limites maximales du fichier d'audit :**  
     **Fichiers de substitution maximale**  
     Spécifie que, lorsque le nombre maximal de fichiers d'audit est atteint, les fichiers d'audit les plus anciens sont remplacés par les nouveaux fichiers.  
  
     **Nombre maximal de fichiers**  
     Spécifie que, lorsque le nombre maximal de fichiers d'audit est atteint, toute action qui entraîne la génération d'événements d'audit supplémentaires échoue et provoque une erreur.  
  
     Case à cocher**Illimité**   
     Lorsque la case **Illimité** sous **Nombre maximal de fichiers de substitution** est cochée, aucune limite n’est imposée sur le nombre de fichiers d’audit qui seront créés. La case à cocher **Illimité** est sélectionnée par défaut et s'applique aux sélections **Nombre maximal de fichiers de substitution** et **Nombre maximal de fichiers** .  
  
     Zone**Nombre de fichiers**   
     Spécifie le nombre de fichiers d'audit à créer, jusqu'à 2 147 483 647. Cette option est disponible uniquement si **Illimité** est désactivé.  
  
     **Taille de fichier maximale**  
     Spécifie la taille maximale d'un fichier d'audit, en mégaoctets (MB), en gigaoctets (GB) ou en téraoctets (TB). Vous pouvez spécifier une valeur comprise entre 1 024 Mo et 2 147 483 647 To. L'activation de la case à cocher **Illimité** ne fixe pas de limite quant à la taille du fichier. La spécification d'une valeur inférieure à 1 024 Mo échoue et génère une erreur. La case à cocher **Illimité** est sélectionnée par défaut.  
  
     Case à cocher**Réserver l’espace disque**   
     Spécifie qu'une quantité d'espace disque égale à la taille de fichier maximale spécifiée doit être pré-allouée. Ce paramètre ne peut être utilisé que si la case à cocher **Illimité** sous **Taille de fichier maximale** n'est pas sélectionnée. Cette case à cocher n'est pas activée par défaut.  
  
3.  Éventuellement, sur la page **Filtre** , entrez un prédicat, ou la clause `WHERE` , pour l'audit de serveur afin de définir des options supplémentaires qui ne sont pas disponibles sur la page **Général** . Mettez le prédicat entre parenthèses ; par exemple : `(object_name = 'EmployeesTable')`.  
  
4.  Lorsque vous avez fini de sélectionner les options, cliquez sur **OK**.  
  
#### <a name="to-create-a-server-audit-specification"></a>Pour créer une spécification d'audit de serveur  
  
1.  Dans l'Explorateur d'objets, cliquez sur le signe plus pour développer le dossier **Sécurité** .  
  
2.  Cliquez avec le bouton droit sur le dossier **Spécifications de l’audit du serveur** , puis sélectionnez **Nouvelle spécification de l’audit du serveur**.  
  
     Les options suivantes sont disponibles dans la boîte de dialogue **Créer la spécification de l'audit du serveur** .  
  
     **Nom**  
     Nom de la spécification de l'audit du serveur. Le nom est généré automatiquement lorsque vous créez une spécification d'audit du serveur, mais vous pouvez le modifier.  
  
     **Audit**  
     Nom d'un audit de serveur existant. Tapez le nom de l'audit ou sélectionnez-le dans la liste.  
  
     **Type d'action de l'audit**  
     Spécifie les groupes d'actions d'audit de niveau serveur et les actions d'audit à capturer. Pour obtenir la liste d’actions d’audit et de groupes d’actions d’audit de niveau serveur, ainsi qu’une description des événements qu’ils contiennent, consultez [Actions et groupes d’actions SQL Server Audit](../../../relational-databases/security/auditing/sql-server-audit-action-groups-and-actions.md).  
  
     **Schéma d'objet**  
     Affiche le schéma du **Nom de l’objet**spécifié.  
  
     **Nom de l’objet**  
     Nom de l'objet à auditer. Disponible uniquement pour les actions d'audit ; ne s'applique pas aux groupes d'audit.  
  
     **Points de suspension (…)**  
     Ouvre la boîte de dialogue **Sélectionner des objets** permettant de rechercher et sélectionner un objet disponible, en fonction du **Type d'action de l'audit**spécifié.  
  
     **Nom principal**  
     Compte par lequel filtrer l'audit pour l'objet audité.  
  
     **Points de suspension (…)**  
     Ouvre la boîte de dialogue **Sélectionner des objets** permettant de rechercher et sélectionner un objet disponible, en fonction du **Nom de l'objet**spécifié.  
  
3.  Lorsque vous avez terminé, cliquez sur **OK**.  
  
##  <a name="TsqlProcedure"></a> Utilisation de Transact-SQL  
  
#### <a name="to-create-a-server-audit"></a>Pour créer un audit de serveur  
  
1.  Dans l' **Explorateur d'objets**, connectez-vous à une instance du [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    -- Creates a server audit called "HIPPA_Audit" with a binary file as the target and no options.  
    CREATE SERVER AUDIT HIPAA_Audit  
        TO FILE ( FILEPATH ='\\SQLPROD_1\Audit\' );  
    ```  
  
#### <a name="to-create-a-server-audit-specification"></a>Pour créer une spécification d'audit de serveur  
  
1.  Dans l'**Explorateur d'objets**, connectez-vous à une instance de [!INCLUDE[ssDE](../../../includes/ssde-md.md)].  
  
2.  Dans la barre d'outils standard, cliquez sur **Nouvelle requête**.  
  
3.  Copiez et collez l'exemple suivant dans la fenêtre de requête, puis cliquez sur **Exécuter**.  
  
    ```  
    /*Creates a server audit specification called "HIPPA_Audit_Specification" that audits failed logins for the SQL Server audit "HIPPA_Audit" created above.  
    */  
  
    CREATE SERVER AUDIT SPECIFICATION HIPPA_Audit_Specification  
    FOR SERVER AUDIT HIPPA_Audit  
        ADD (FAILED_LOGIN_GROUP);  
    GO  
    -- Enables the audit.   
  
    ALTER SERVER AUDIT HIPAA_Audit  
    WITH (STATE = ON);  
    GO  
    ```  
  
 Pour plus d’informations, consultez [CREATE SERVER AUDIT &#40;Transact-SQL&#41;](../../../t-sql/statements/create-server-audit-transact-sql.md) et [CREATE SERVER AUDIT SPECIFICATION &#40;Transact-SQL&#41;](../../../t-sql/statements/create-server-audit-specification-transact-sql.md).  
  
  

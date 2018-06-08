---
title: Activer le mode DirectQuery dans SSMS | Documents Microsoft
ms.date: 05/07/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: tabular-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: e643f90a5df9b113f2fd59a2328868131bf9c63d
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="enable-directquery-mode-in-ssms"></a>Activer le mode DirectQuery dans SSMS
[!INCLUDE[ssas-appliesto-sqlas-aas](../../includes/ssas-appliesto-sqlas-aas.md)]
  Vous pouvez modifier les propriétés d’accès aux données d’un modèle tabulaire déjà déployé en activant le mode DirectQuery où des requêtes s’exécutent sur une source de données relationnelle principale plutôt que sur des données mises en cache résidant dans la mémoire.  
  
 Dans [!INCLUDE[ssCurrent](../../includes/sscurrent-md.md)], les étapes de configuration de DirectQuery varient en fonction du niveau de compatibilité du modèle. Vous trouverez ci-dessous des étapes qui fonctionnent pour tous les niveaux de compatibilité.  
  
 Cet article suppose que vous avez créé et validé un modèle tabulaire en mémoire au niveau de compatibilité 1200 ou supérieur et seul est nécessaire pour activer l’accès de DirectQuery et de mettre à jour les chaînes de connexion. Si vous démarrez à partir d’un niveau de compatibilité inférieur, vous devez commencer par mettre à niveau le modèle manuellement. Pour connaître la procédure à suivre, consultez [Mettre à niveau Analysis Services](../../database-engine/install-windows/upgrade-analysis-services.md) .  
  
> [!IMPORTANT]  
>  Nous vous recommandons d’utiliser [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] au lieu de Management Studio pour basculer entre les modes de stockage de données. Lorsque vous utilisez  [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] pour modifier le modèle, puis procédez au déploiement sur le serveur, le modèle et la base de données restent synchronisés. En outre, la modification des modes de stockage dans le modèle vous permet de consulter toutes les erreurs de validation qui se produisent. Lorsque vous utilisez [!INCLUDE[ssManStudioFull](../../includes/ssmanstudiofull-md.md)] de la manière décrite dans cet article, les erreurs de validation ne sont pas signalées.  
  
## <a name="requirements"></a>Spécifications  
 L’activation de l’utilisation du mode DirectQuery sur un modèle tabulaire est un processus en plusieurs étapes.  
  
-   Assurez-vous que le modèle n’a pas de fonctionnalités susceptibles de provoquer des erreurs de validation en mode DirectQuery, puis modifiez le mode de stockage des données sur le modèle de « en mémoire » en DirectQuery.  
  
     Une liste des restrictions des fonctionnalités est documentée dans [DirectQuery Mode](../../analysis-services/tabular-models/directquery-mode-ssas-tabular.md).  
  
-   Examinez la chaîne de connexion et les informations d’identification utilisées par la base de données déployée afin de récupérer des données à partir de la base de données externe principale. Assurez-vous qu’il y n’a qu’une seule connexion, et que ses paramètres sont appropriés pour l’exécution de la requête.  
  
     Les bases de données tabulaires non spécifiquement conçues pour DirectQuery peuvent avoir plusieurs connexions qui doivent à présent être réduites à une, comme requis pour le mode DirectQuery.  
  
     Les informations d’identification utilisées initialement pour le traitement des données sont désormais utilisées pour interroger des données. Dans le cadre de la configuration de DirectQuery, examinez et modifiez éventuellement le compte si vous en utilisez plusieurs pour les opérations dédiées.  
  
     Le mode DirectQuery est le seul scénario dans lequel Analysis Services effectue une délégation approuvée. Si votre solution fait appel à délégation pour obtenir des résultats de requête spécifiques de l’utilisateur, le compte utilisé pour se connecter à la base de données principale doit être autorisé à déléguer l’identité de l’utilisateur effectuant la requête, et les identités d’utilisateur doivent disposer d’autorisations en lecture sur la base de données principale.  
  
-   En guise de dernière étape, vérifiez que le mode DirectQuery est opérationnel en exécutant la requête.  
  
## <a name="step-1-check-the-compatibility-level"></a>Étape 1 : vérifier le niveau de compatibilité  
 Les propriétés qui définissent l’accès aux données diffèrent sur le plan des niveaux de compatibilité. Une étape préliminaire consiste à vérifier le niveau de compatibilité de la base de données.  
  
1.  Dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], connectez-vous à l’instance contenant le modèle tabulaire.  
  
2.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur la base de données, puis choisissez **Propriétés** > **Niveau de compatibilité**.  
  
     La valeur est soit **SQL Server 2016 (1200)**, soit un niveau antérieur tel que **SQL Server 2012 SP1 ou version ultérieure (1103)**. Dans l’étape suivante, suivez les instructions valides pour le niveau de compatibilité.  
  
 Quand vous passez d’un modèle tabulaire au mode DirectQuery, le nouveau mode de stockage des données prend effet immédiatement.  
  
## <a name="step-2a-switch-a-tabular-1200-database-to-directquery-mode"></a>Étape 2a : passer d’une base de données tabulaire 1200 au mode DirectQuery  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur la base de données, puis choisissez **Propriétés** > **Modèle** > **Mode par défaut**.  
  
2.  Définissez le mode **DirectQuery**.  
  
    |||  
    |-|-|  
    |**Valeurs valides**|**Description**|  
    |**DirectQuery**|Les requêtes sont exécutées sur un une base de données relationnelle principale, en utilisant la connexion à la source de données définie pour le modèle.<br /><br /> Les requêtes adressées au modèle sont converties en requêtes de base de données natives et redirigées vers la source de données.<br /><br /> Lorsque vous traitez un modèle en mode DirectQuery, seules les métadonnées sont compilées et déployées. Les données proprement dites sont externes au modèle. Elles résident dans les fichiers de base de données de la source de données opérationnelle.|  
    |**Importer**|Les requêtes sont exécutées sur la base de données tabulaire dans MDX ou DAX.<br /><br /> Lorsque vous traitez un modèle en mode d’importation, les données sont récupérées à partir d’une source de données principale et stockées sur disque. Lors du chargement de la base de données, les données sont copiées entièrement en mémoire afin que les requêtes et analyses de table soient très rapides.<br /><br /> Il s’agit du mode par défaut pour les modèles tabulaires, qui est l’unique mode pour certaines sources de données (non relationnelles).|  
  
## <a name="step-2b-switch-a-tabular-1100-1103-database-to-directquery-mode"></a>Étape 2 b : passer d’une base de données tabulaire 1100-1103 au mode DirectQuery  
  
1.  Dans l’Explorateur d’objets, cliquez avec le bouton droit sur la base de données, puis choisissez > **Propriétés** > **Base de données** > **DirectQueryMode**.  
  
2.  Définissez le mode **DirectQuery**.  
  
     Les propriétés par défaut du mode sont les suivantes :  
  
    |||  
    |-|-|  
    |**Valeurs valides**|**Description**|  
    |**InMemory**|Les requêtes utilisent uniquement les données mises en cache en mémoire.|  
    |**InMemorywithDirectQuery**|les requêtes utilisent le cache par défaut, sauf indication contraire dans la chaîne de connexion du client.<br /><br /> Il s’agit d’un mode hybride où les partitions sont configurées individuellement pour utiliser le mode en mémoire ou DirectQuery.|  
    |**DirectQuery**|les requêtes utilisent la source de données relationnelle uniquement.|  
    |**DirectQuerywithInMemory**|les requêtes utilisent la source de données relationnelle par défaut, sauf indication contraire dans la chaîne de connexion du client.<br /><br /> Il s’agit d’un mode hybride où les partitions sont configurées individuellement pour utiliser le mode en mémoire ou DirectQuery.|  
  
 **Stockage de données hybride**  
  
 Lorsque vous utilisez une combinaison d’accès sur disque et en mémoire, le cache est toujours disponible et peut être utilisé pour les requêtes. Un mode hybride vous fournit de nombreuses options :  
  
-   Lorsque le cache et la source de données relationnelle sont disponibles, vous pouvez définir la méthode de connexion recommandée par défaut, mais en fin de compte le client contrôle la source utilisée, à l'aide de la propriété de chaîne de connexion DirectQueryMode.  
  
-   Vous pouvez configurer des partitions du cache de telle façon que la partition principale utilisée pour le mode DirectQuery ne soit jamais traitée et doive toujours faire référence à la source relationnelle. Il existe plusieurs façons d'utiliser des partitions pour optimiser l'expérience de conception de modèle et de rapports. Pour plus d’informations, consultez [définir des partitions dans les modèles DirectQuery](../../analysis-services/tabular-models/define-partitions-in-directquery-models-ssas-tabular.md).  
  
-   Une fois le modèle déployé, vous pouvez modifier la méthode de connexion par défaut. Par exemple, vous pouvez utiliser un mode hybride pour le test, et basculer le modèle en mode **DirectQuery uniquement** seulement après avoir testé complètement tous les rapports ou requêtes qui utilisent le modèle. Pour plus d’informations, consultez [Définir ou modifier la méthode de connexion par défaut pour DirectQuery](http://msdn.microsoft.com/library/f10d5678-d678-4251-8cce-4e30cfe15751).  
  
## <a name="step-3-check-the-connection-properties-on-the-database"></a>Étape 3: vérifier les propriétés de connexion sur la base de données  
 Selon le paramétrage de la connexion à la source de données, le passage au mode DirectQuery peut modifier le contexte de sécurité de la connexion. Lorsque vous modifiez le mode d’accès aux données, examinez les propriétés de chaîne d’emprunt d’identité et de connexion pour vérifier que la connexion est valide pour les connexions en cours à la base de données principale.  
  
 Pour plus d’informations sur la délégation d’identité d’utilisateur pour les scénarios DirectQuery, voir la section **Configurer Analysis Services pour une délégation approuvée** dans [Configure Analysis Services for Kerberos constrained delegation](../../analysis-services/instances/configure-analysis-services-for-kerberos-constrained-delegation.md) .  
  
1.  Dans l’Explorateur d’objets, développez **Connections** (Connexions), puis double-cliquez sur une connexion pour afficher ses propriétés.  
  
     Pour les modèles DirectQuery, il ne peut y avoir qu’une seule connexion définie pour la base de données, la source de données doit être relationnelle et le type de la base de données doit être pris en charge. Consultez [des Sources de données prises en charge](../../analysis-services/tabular-models/data-sources-supported-ssas-tabular.md).  
  
2.  **Connection string** (Chaîne de connexion) doit spécifier le serveur, nom de la base de données et la méthode d’authentification utilisée dans les opérations DirectQuery. Si vous utilisez l’authentification SQL Server, vous pouvez spécifier ici la connexion de base de données.  
  
3.  Les**informations d’emprunt d’identité** sont utilisée pour l’authentification Windows. Les options valides pour les modèles tabulaires en mode DirectQuery sont les suivantes :  
  
    -   **Use the service account**(Utiliser le compte de service). Vous pouvez choisir cette option si le compte de service Analysis Services a lu les autorisations sur la base de données relationnelle.  
  
    -   **Use a specific user name and password**(Utiliser un nom d’utilisateur et un mot de passe spécifiques). Spécifiez un compte d’utilisateur Windows disposant d’autorisations en lecture sur la base de données relationnelle.  
  
 Notez que ces informations d'identification sont utilisées uniquement pour répondre aux requêtes sur la banque de données relationnelle ; elles sont différentes des informations d'identification utilisées pour traiter le cache d'un modèle hybride.  
  
 L'emprunt d'identité ne peut pas être utilisé lorsque le modèle est utilisé dans la mémoire uniquement. La paramètre **ImpersonateCurrentUser**n'est pas valide, sauf si le modèle utilise le mode DirectQuery.  
  
## <a name="step-4-validate-directquery-access"></a>Étape 4 : valider l’accès de DirectQuery  
  
1.  Démarrez une trace à l’aide de SQL Server Profiler ou de xEvents dans Management Studio connecté à la base de données relationnelle SQL Server.  
  
     Si vous utilisez Oracle ou Teradata, servez-vous des outils de trace destinés à ces plateformes de base de données.  
  
2.  Dans Management Studio, entrez et puis exécutez une simple requête MDX, telle que `select <some measure> on 0 from model.`.  
  
3.  Dans la trace, vous devriez voir la preuve de l’exécution de la requête sur la base de données relationnelle.  
  
## <a name="see-also"></a>Voir aussi  
 [Niveau de compatibilité](../../analysis-services/tabular-models/compatibility-level-for-tabular-models-in-analysis-services.md)   
 [Sources de données prises en charge](../../analysis-services/tabular-models/data-sources-supported-ssas-tabular.md)   
 [Événements étendus](../../relational-databases/extended-events/extended-events.md)   
 [Surveiller une instance Analysis Services](../../analysis-services/instances/monitor-an-analysis-services-instance.md)  
  
  

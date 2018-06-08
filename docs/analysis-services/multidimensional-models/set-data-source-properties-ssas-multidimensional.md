---
title: Définir les propriétés de Source de données (SSAS multidimensionnel) | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: c9053154d01616f1ff39c67a85d1319e1d02625f
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="set-data-source-properties-ssas-multidimensional"></a>Définir les propriétés de la source de données (SSAS Multidimensionnel)
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Dans [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)], un objet de source de données représente une connexion à un entrepôt de données externe ou à une base de données relationnelle qui fournit des données à un modèle multidimensionnel. Les propriétés sur la source de données déterminent la chaîne de connexion, le délai d'attente, le nombre maximal de connexions et le niveau d'isolation de la transaction.  
  
## <a name="set-data-source-properties-in-sql-server-data-tools"></a>Définir les propriétés de la source de données dans les SQL Server Data Tools  
  
1.  Double-cliquez sur une source de données dans l'Explorateur de solutions pour ouvrir le Concepteur de source de données.  
  
2.  Cliquez sur l’onglet **Informations d’emprunt d’identité** dans le Concepteur de source de données. Pour plus d’informations sur la création d’une source de données, consultez [Créer une source de données &#40;SSAS Multidimensionnel&#41;](../../analysis-services/multidimensional-models/create-a-data-source-ssas-multidimensional.md).  
  
## <a name="set-data-source-properties-in-management-studio"></a>Définir les propriétés de la source de données dans Management Studio  
  
1.  Développez le dossier de la base de données, ouvrez le dossier **Source de données** sous le nom de la base de données, cliquez avec le bouton droit sur une source de données dans l’ **Explorateur d’objets** et sélectionnez **Propriétés**.  
  
2.  Si vous le souhaitez, modifiez le nom, la description ou l'option d'emprunt d'identité. Pour plus d’informations, consultez [Définir les options d’emprunt d’identité &#40;SSAS - Multidimensionnel&#41;](../../analysis-services/multidimensional-models/set-impersonation-options-ssas-multidimensional.md).  
  
## <a name="data-source-properties"></a>Propriétés de la source de données  
  
|Terme|Définition|  
|----------|----------------|  
|**Nom, ID, description**|Le nom, l'ID et la description sont utilisés pour identifier et décrire l'objet de source de données dans le modèle multidimensionnel.<br /><br /> Le nom et la description peuvent être spécifiés dans [!INCLUDE[ssBIDevStudio](../../includes/ssbidevstudio-md.md)] ou dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] après avoir déployé ou traité la solution.<br /><br /> L'ID est généré lorsque l'objet est créé. Bien que vous puissiez facilement modifier le nom et la description, les ID sont en lecture seule et ne doivent pas être modifiés. Un ID d'objet fixe conserve les dépendances et les références d'objet dans tout le modèle.|  
|**Créer un horodateur**|Cette propriété en lecture seule apparaît dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Elle affiche la date et l'heure de création de la source de données.|  
|**Dernière mise à jour du schéma**|Cette propriété en lecture seule apparaît dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Elle affiche la date et l'heure de la dernière mise à jour des métadonnées de la source de données. Cette valeur est mise à jour lorsque vous déployez la solution.|  
|**Délai de requête**|Spécifie le temps pendant lequel une demande de connexion est tentée avant d'être supprimée.<br /><br /> Tapez le délai d'expiration des requêtes au format suivant :<br /><br /> *\<Heures >*:*\<Minutes >*:*\<secondes >*<br /><br /> Cette propriété peut être remplacée par la propriété de serveur **DatabaseConnectionPoolTimeoutConnection** . Si la propriété de serveur contient une valeur inférieure, elle est utilisée à la place de **Délai de requête**.<br /><br /> Pour plus d’informations sur la propriété **Délai de requête** , consultez <xref:Microsoft.AnalysisServices.DataSource.Timeout%2A>. Pour plus d’informations sur la propriété de serveur, consultez [Propriétés OLAP](../../analysis-services/server-properties/olap-properties.md).|  
|**Chaîne de connexion**|Spécifie l'emplacement physique d'une base de données qui fournit des données à un modèle multidimensionnel, et du fournisseur de données utilisé pour la connexion. Ces informations sont fournies à une bibliothèque cliente qui établit la demande de connexion. Le fournisseur détermine les propriétés qui peuvent être définies dans la chaîne de connexion.<br /><br /> La chaîne de connexion est créée à l’aide des informations que vous fournissez dans la boîte de dialogue **Gestionnaire de connexions** . Vous pouvez également afficher et modifier la chaîne de connexion dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)] , dans la page de propriétés de la source de données.<br /><br /> Pour une base de données SQL Server, une chaîne de connexion qui contient **user ID** indique l’authentification de base de données ; une connexion qui contient **Integrated Security=SSPI** indique l’authentification Windows.<br /><br /> Vous pouvez modifier le nom du serveur ou de la base de données si la base de données a été déplacée dans un nouvel emplacement. Vérifiez que les informations d'identification actuellement spécifiées pour la connexion sont mappées à une connexion de base de données.|  
|**Nombre maximal de connexions**|Spécifie le nombre maximal de connexions autorisées par [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] pour la connexion à la source de données. Si davantage de connexions sont nécessaires, [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] attend qu’une connexion soit disponible. La valeur par défaut est 10. En limitant le nombre de connexions, vous garantissez que la source de données externe n'est pas surchargée par les demandes d' [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] .|  
|**Isolement**|Spécifie le comportement de verrouillage et de contrôle de version de ligne des commandes SQL exécutées par une connexion sur une base de données relationnelle. Les valeurs valides sont ReadCommitted ou Snapshot. La valeur par défaut est ReadCommitted, qui spécifie que ces données doivent être validées avant d'être lues et qui empêche les lectures erronées. L'instantané spécifie que les lectures proviennent d'un instantané de données déjà validées. Pour plus d’informations sur le niveau d’isolation dans SQL Server, consultez [SET TRANSACTION ISOLATION LEVEL &#40;Transact-SQL&#41;](../../t-sql/statements/set-transaction-isolation-level-transact-sql.md).|  
|**Fournisseur managé**|Affiche le nom du fournisseur managé (ex. System.Data.SqlClient ou System.Data.OracleClient) si la source de données utilise un fournisseur managé.<br /><br /> Si la source de données n'utilise pas un tel fournisseur, cette propriété est vide.<br /><br /> Cette propriété est en lecture seule dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)]. Pour modifier le fournisseur utilisé pour la connexion, modifiez la chaîne de connexion.|  
|**Informations d'emprunt d'identité**|Spécifie l’identité Windows sous laquelle [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] s’exécute durant la connexion à une source de données qui utilise l’authentification Windows. Les options utilisent un ensemble prédéfini d'informations d'identification Windows, le compte de service, l'identité de l'utilisateur actuel ou une option d'héritage qui peut être utile si votre modèle contient plusieurs objets de source de données. Pour plus d’informations, consultez [Définir les options d’emprunt d’identité &#40;SSAS - Multidimensionnel&#41;](../../analysis-services/multidimensional-models/set-impersonation-options-ssas-multidimensional.md).<br /><br /> Dans [!INCLUDE[ssManStudio](../../includes/ssmanstudio-md.md)], les valeurs valides sont les suivantes :<br /><br /> **ImpersonateAccount** (utiliser un nom et un mot de passe utilisateur Windows spécifiques pour se connecter à la source de données).<br /><br /> **ImpersonateServiceAccount** (utiliser l’identité de sécurité du compte de service pour se connecter à la source de données). Ceci est la valeur par défaut.<br /><br /> **ImpersonateCurrentUser** (utiliser l’identité de sécurité de l’utilisateur actuel pour se connecter à la source de données). Cette option est uniquement valide pour les requêtes d'exploration de données qui extraient des données d'un entrepôt ou d'une base de données externes ; ne la choisissez pas pour les connexions de données utilisées pour le traitement, le chargement ou l'écriture différée dans une base de données multidimensionnelle.<br /><br /> **Inherit** ou **default** (utiliser les paramètres d’emprunt d’identité de la base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] qui contient cet objet de source de données). Les propriétés de base de données incluent des options d'emprunt d'identité.|  
  
## <a name="see-also"></a>Voir aussi  
 [Sources de données dans des modèles multidimensionnels](../../analysis-services/multidimensional-models/data-sources-in-multidimensional-models.md)   
 [Créer une Source de données & #40 ; SSAS multidimensionnel & #41 ;](../../analysis-services/multidimensional-models/create-a-data-source-ssas-multidimensional.md)  
  
  

---
title: Base de données ReadWriteModes | Documents Microsoft
ms.date: 05/02/2018
ms.prod: sql
ms.technology: analysis-services
ms.custom: multidimensional-models
ms.topic: conceptual
ms.author: owend
ms.reviewer: owend
author: minewiskan
manager: kfile
ms.openlocfilehash: ec2aebbb202aadf69ccb9ab2c214d878aa0d9d9e
ms.sourcegitcommit: c12a7416d1996a3bcce3ebf4a3c9abe61b02fb9e
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/10/2018
---
# <a name="database-readwritemodes"></a>Base de données ReadWriteModes
[!INCLUDE[ssas-appliesto-sqlas](../../includes/ssas-appliesto-sqlas.md)]
  Il existe souvent des situations où un administrateur de base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] (dba) souhaite modifier une base de données en lecture/écriture en une base de données en lecture seule, ou inversement. Ces situations sont souvent dues à des impératifs de fonctionnement, tels que le partage du même dossier de base de données entre plusieurs serveurs pour la montée en puissance d'une solution et l'amélioration des performances. Pour ces situations, la propriété de base de données **ReadWriteMode** permet à l’administrateur de base de données [!INCLUDE[ssASnoversion](../../includes/ssasnoversion-md.md)] de modifier aisément le mode de fonctionnement de la base de données.  
  
## <a name="readwritemode-database-property"></a>Propriété de base de données ReadWriteMode  
 La propriété de base de données **ReadWriteMode** spécifie si la base de données est en mode lecture/écriture ou en mode lecture seule. Ce sont les deux seules valeurs possibles de la propriété. Lorsque la base de données est en mode lecture seule, aucune modification ou mise à jour ne peut être appliquée à la base de données. Toutefois, lorsque la base de données est en mode lecture/écriture, des modifications et des mises à jour peuvent se produire. La propriété de base de données **ReadWriteMode** est définie comme une propriété en lecture seule. Elle ne peut être définie qu’à travers une commande **Attacher** .  
  
 Quand une base de données est en mode lecture seule, certaines restrictions en vigueur affectent l'ensemble traditionnel des opérations autorisées sur la base de données. Consultez le tableau suivant pour connaître les opérations restreintes.  
  
|Mode ReadOnly (lecture seule)|Opérations restreintes|  
|-------------------|---------------------------|  
|Commandes XML/A<br /><br /> <br /><br /> Remarque : une erreur est générée quand vous exécutez l’une de ces commandes.|**Créer**<br /><br /> **Alter**<br /><br /> **Supprimer**<br /><br /> **Traiter**<br /><br /> **MergePartitions**<br /><br /> **DesignAggregations**<br /><br /> **CommitTransaction**<br /><br /> **Restore**<br /><br /> **Synchroniser**<br /><br /> **Insert**<br /><br /> **Update**<br /><br /> **Supprimer**<br /><br /> <br /><br /> Remarque : l’écriture différée de cellule est autorisée dans les bases de données définies en lecture seule ; toutefois, les modifications ne peuvent pas être validées.|  
|Instructions MDX<br /><br /> <br /><br /> Remarque : une erreur est générée quand vous exécutez l’une de ces instructions.|**COMMIT TRAN**<br /><br /> **CREATE SESSION CUBE**<br /><br /> **ALTER CUBE**<br /><br /> **ALTER DIMENSION**<br /><br /> **CREATE DIMENSION MEMBER**<br /><br /> **DROP DIMENSION MEMBER**<br /><br /> **ALTER DIMENSION**<br /><br /> <br /><br /> Remarque : les utilisateurs Excel ne peuvent pas utiliser la fonctionnalité de regroupement dans les tableaux croisés dynamiques, car cette fonctionnalité est implémentée en interne à l’aide des commandes **CREATE SESSION CUBE** .|  
|Instructions DMX<br /><br /> <br /><br /> Remarque : une erreur est générée quand vous exécutez l’une de ces instructions.|**CREATE [SESSION] MINING STRUCTURE**<br /><br /> **ALTER MINING STRUCTURE**<br /><br /> **DROP MINING STRUCTURE**<br /><br /> **CREATE [SESSION] MINING MODEL**<br /><br /> **DROP MINING MODEL**<br /><br /> **IMPORT**<br /><br /> **SELECT INTO**<br /><br /> **INSERT**<br /><br /> **UPDATE**<br /><br /> **DELETE**|  
|Opérations d'arrière-plan|Les opérations d'arrière-plan qui modifieraient la base de données sont désactivées. Cela inclut le traitement différé et la mise en cache proactive.|  
  
## <a name="readwritemode-usage"></a>Utilisation de ReadWriteMode  
 La propriété de base de données **ReadWriteMode** doit être utilisée dans le cadre d'une commande de base de données **Attach** . La commande **Attach** permet que la propriété de base de données soit définie avec la valeur **ReadWrite** ou la valeur **ReadOnly**. La valeur de la propriété de base de données **ReadWriteMode** ne peut pas être mise à jour directement parce que la propriété est définie en lecture seule. Les bases de données sont créées avec la propriété **ReadWriteMode** définie avec la valeur **ReadWrite**. Une base de données ne peut pas être créée en mode lecture seule.  
  
 Pour faire basculer la propriété de base de données **ReadWriteMode** de la valeur **ReadWrite** vers la valeur **ReadOnly**, vous devez émettre une séquence de commandes **Détacher/Attacher** .  
  
 Toutes les opérations de base de données, à l'exception d' **Attach**, conservent la propriété de base de données **ReadWriteMode** dans son état courant. Par exemple, les opérations telles que **Alter**, **Backup**, **Restore**et **Synchronize** conservent la valeur **ReadWriteMode** .  
  
> [!NOTE]  
>  Les cubes locaux peuvent être créés à partir d'une base de données en lecture seule.  
  
## <a name="see-also"></a>Voir aussi  
 <xref:Microsoft.AnalysisServices.Database.Detach%2A>   
 [Attacher et détacher des bases de données Analysis Services](../../analysis-services/multidimensional-models/attach-and-detach-analysis-services-databases.md)   
 [Déplacer une base de données Analysis Services](../../analysis-services/multidimensional-models/move-an-analysis-services-database.md)   
 [Élément Detach](../../analysis-services/xmla/xml-elements-commands/detach-element.md)   
 [Attach, élément](../../analysis-services/xmla/xml-elements-commands/attach-element.md)  
  
  

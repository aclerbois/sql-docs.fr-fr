---
title: Conseiller d’optimisation de la mémoire | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database
ms.component: in-memory-oltp
ms.reviewer: ''
ms.suite: sql
ms.technology: in-memory-oltp
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- swb.memoryoptimizationwizard.f1
- sql13.swb.memoryoptimizationwizard.f1
ms.assetid: 181989c2-9636-415a-bd1d-d304fc920b8a
caps.latest.revision: 17
author: MightyPen
ms.author: genemi
manager: craigg
monikerRange: = azuresqldb-current || >= sql-server-2016 || = sqlallproducts-allversions
ms.openlocfilehash: 2e7c060facf52098f4f20d6b147f04cbaa0d1a7f
ms.sourcegitcommit: ee661730fb695774b9c483c3dd0a6c314e17ddf8
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/19/2018
---
# <a name="memory-optimization-advisor"></a>Conseiller d'optimisation de la mémoire
[!INCLUDE[appliesto-ss-asdb-xxxx-xxx-md](../../includes/appliesto-ss-asdb-xxxx-xxx-md.md)]

  Les rapports sur les performances des transactions (voir [Déterminer si un tableau ou une procédure stockée doit être déplacée vers l’OLTP en mémoire](../../relational-databases/in-memory-oltp/determining-if-a-table-or-stored-procedure-should-be-ported-to-in-memory-oltp.md)) indiquent quelles tables de votre base de données tireront parti de l’utilisation de la fonctionnalité OLTP en mémoire. Après avoir identifié une table que vous souhaitez déplacer pour utiliser l’OLTP en mémoire, utilisez le Conseiller d’optimisation de la mémoire dans SQL Server Management Studio pour vous aider à migrer la table sur disque vers l’OLTP en mémoire.  
  
 Le Conseiller d’optimisation de la mémoire vous permet d’effectuer les opérations suivantes :  
  
-   Identifier les fonctionnalités utilisées dans une table basée sur disque qui ne sont pas prises en charge pour les tables mémoire optimisées.  
  
-   Migrer une table et les données vers une table mémoire optimisée (si toutes les fonctionnalités sont prises en charge).  
    
 Pour plus d’informations sur les méthodologies de transfert, consultez [In-Memory OLTP – Common Workload Patterns and Migration Considerations](http://msdn.microsoft.com/library/dn673538.aspx)(OLTP en mémoire – Modèles de charge de travail courants et considérations relatives à la migration).  
  
## <a name="walkthrough-using-the-memory-optimization-advisor"></a>Procédure pas à pas d'utilisation du Conseiller d'optimisation de la mémoire  
 Dans l’ **Explorateur d’objets**, cliquez avec le bouton droit sur la table que vous souhaitez convertir, puis sélectionnez **Conseiller d’optimisation de la mémoire**. Ce faisant, vous affichez la page d'accueil du **Conseiller d'optimisation de la mémoire de la table**.  
  
### <a name="memory-optimization-checklist"></a>Liste de contrôle de l'optimisation de la mémoire  
 Lorsque vous cliquez sur **Suivant** dans la page d'accueil du **Conseiller d'optimisation de la mémoire de la table**, vous voyez la liste de contrôle d'optimisation de la mémoire. Les tables mémoire optimisées ne prennent pas en charge toutes les fonctionnalités d'une table sur disque. La liste de contrôle d'optimisation de la mémoire indique si la table sur disque utilise des fonctionnalités qui ne sont pas compatibles avec une table mémoire optimisée. Le **Conseiller d’optimisation de la mémoire de la table** ne modifie pas la table sur disque pour qu’elle puisse être migrée afin d’utiliser l’OLTP en mémoire. Vous devez effectuer ces modifications avant de poursuivre la migration. Pour chaque incompatibilité trouvée, le **Conseiller d’optimisation de la mémoire de la table** affiche un lien vers des informations qui peuvent vous aider à modifier les tables sur disque.  
  
 Pour conserver une liste de ces incompatibilités dans le but de planifier la migration, cliquez sur **Générer le rapport** afin de générer une liste HTML.  
  
 Si la table ne comporte aucune incompatibilité et que vous êtes connecté à une instance de [!INCLUDE[ssSQL14](../../includes/sssql14-md.md)] avec l’OLTP en mémoire, cliquez sur **Suivant**.  
  
### <a name="memory-optimization-warnings"></a>Avertissements de l'optimisation de la mémoire  
 La page suivante, relative aux avertissements d'optimisation de la mémoire, contient une liste des problèmes qui n'empêchent pas la table d'être migrée en vue d'utiliser l'OLTP en mémoire, mais qui peuvent entraîner l'échec du comportement d'autres objets (tels que des procédures stockées ou des fonctions CLR) ou un comportement inattendu.  
  
 Les premiers avertissements de la liste sont donnés à titre d'information et peuvent ne pas s'appliquer à votre table. Les liens figurant dans la colonne droite de la table vous permettront d'obtenir plus d'informations.  
  
 La table d'avertissement affiche également des conditions d'avertissement potentielles qui ne sont pas présentes dans votre table.  
  
 Les avertissements pouvant donner lieu à une action présentent un triangle jaune dans la colonne gauche. En présence d'avertissements pouvant donner lieu à une action, vous devez mettre fin à la migration, remédier aux avertissements, puis redémarrer le processus. Si vous ne résolvez pas les avertissements, la table migrée peut provoquer un échec.  
  
 Cliquez sur **Générer le rapport** pour générer un rapport HTML de ces avertissements. Cliquez sur **Suivant** pour continuer.  
  
### <a name="review-optimization-options"></a>Vérification des options d'optimisation  
 L'écran suivant vous permet de modifier des options pour la migration vers l'OLTP en mémoire :  
  
 Groupe de fichiers mémoire optimisé  
 Nom du groupe de fichiers mémoire optimisé. Une base de données doit posséder un groupe de fichiers mémoire optimisé avec au moins un fichier pour qu'une table mémoire optimisée puisse être créée.  
  
 Si aucun groupe de fichiers mémoire optimisé n'est présent, vous pouvez modifier le nom par défaut. Les groupes de fichiers mémoire optimisés ne peuvent pas être supprimés. L'existence d'un groupe de fichiers mémoire optimisé peut désactiver certaines fonctionnalités au niveau de la base de données telles que l'instruction AUTO CLOSE et la mise en miroir de bases de données.  
  
 Si une base de données comporte déjà un groupe de fichiers mémoire optimisé, ce champ indiquera son nom et vous ne pourrez pas modifier la valeur de ce champ.  
  
 Nom du fichier logique et chemin d'accès au fichier  
 Nom du fichier qui contient la table mémoire optimisée. Une base de données doit posséder un groupe de fichiers mémoire optimisé avec au moins un fichier pour qu'une table mémoire optimisée puisse être créée.  
  
 En l'absence de groupe de fichiers mémoire optimisé existant, vous pouvez modifier le nom par défaut et le chemin d'accès du fichier à créer à la fin du processus de migration.  
  
 Si vous disposez d'un groupe de fichiers mémoire optimisé, ces champs sont remplis à l'avance et vous ne pouvez pas modifier les valeurs.  
  
 Changer le nom de la table d'origine pour  
 À la fin du processus de migration, une nouvelle table mémoire optimisée est créée avec le nom actuel de la table. Pour éviter un conflit de nom, la table actuelle doit être renommée. Vous pouvez modifier le nom dans ce champ.  
  
 Coût mémoire actuel estimé (Mo)  
 Le Conseiller d'optimisation de la mémoire estime la quantité de mémoire utilisée par la nouvelle table mémoire optimisée en fonction des métadonnées de la table sur disque. Le calcul de la taille de la table est expliqué dans [Taille de la table et des lignes dans les tables optimisées en mémoire](../../relational-databases/in-memory-oltp/table-and-row-size-in-memory-optimized-tables.md).  
  
 Si la capacité de mémoire allouée n'est pas suffisante, la migration peut échouer.  
  
 Copier également les données de la table dans la nouvelle table mémoire optimisée  
 Sélectionnez cette option si vous souhaitez également déplacer les données de la table actuelle vers la nouvelle table mémoire optimisée. Si cette option n'est pas sélectionnée, la nouvelle table mémoire optimisée est créée sans lignes.  
  
 La table sera migrée sous forme de table durable par défaut  
 L'OLTP en mémoire prend en charge les tables non durables avec des performances supérieures par rapport aux tables mémoire optimisées durables. Toutefois, les données d'une table non durable seront perdues lors du redémarrage du serveur.  
  
 Si cette option est sélectionnée, le Conseiller d'optimisation de la mémoire crée une table non durable au lieu d'une table durable.  
  
> [!WARNING]  
>  Sélectionnez cette option uniquement si vous comprenez le risque de perte de données associé aux tables non durables.  
  
 Cliquez sur **Suivant** pour continuer.  
  
### <a name="review-primary-key-conversion"></a>Vérifier la conversion de la clé primaire  
 L'écran suivant est **Vérifier la conversion de la clé primaire**. Le Conseiller d'optimisation de la mémoire détecte s'il existe une ou plusieurs clés primaires dans la table, puis remplit la liste des colonnes à partir des métadonnées de clé primaire. Sinon, si vous souhaitez effectuer une migration vers une table mémoire optimisée durable, vous devez créer une clé primaire.  
  
 S'il n'existe aucune clé primaire et que la table est migrée vers une table non durable, cet écran ne s'affiche pas.  
  
 Pour les colonnes textuelles (colonnes avec des types **char**, **nchar**, **varchar**et **nvarchar**), vous devez sélectionner un classement approprié. L'OLTP en mémoire prend uniquement en charge les classements BIN2 pour les colonnes d'une table mémoire optimisée, mais ne prend pas en charge les classements présentant des caractères supplémentaires. Consultez [Collations and Code Pages](http://msdn.microsoft.com/library/c626dcac-0474-432d-acc0-cfa643345372) pour plus d'informations sur les classements pris en charge et l'impact potentiel d'une modification du classement.  
  
 Vous pouvez configurer les paramètres suivants pour la clé primaire :  
  
 Sélectionnez un nouveau nom pour cette clé primaire  
 Le nom de la clé primaire de cette table doit être unique dans la base de données. Vous pouvez modifier le nom de la clé primaire ici.  
  
 Sélectionnez le type de cette clé primaire  
 L'OLTP en mémoire prend en charge deux types d'index sur une table mémoire optimisée :  
  
-   Index NONCLUSTERED HASH. Cet index est idéal pour les index avec un grand nombre de recherches de point. Vous pouvez configurer le nombre de compartiments de cet index dans le champ **Nombre de compartiments** .  
  
-   Index NONCLUSTERED. Ce type d'index est idéal pour les index avec de nombreuses requêtes de plage. Vous pouvez configurer l'ordre de tri de chaque colonne dans la liste **Trier par colonne et ordre** .  
  
 Pour connaître le type d’index le plus approprié pour votre clé primaire, consultez [Index de hachage](http://msdn.microsoft.com/library/f4bdc9c1-7922-4fac-8183-d11ec58fec4e).  
  
 Cliquez sur **Suivant** après avoir effectué vos choix de clé primaire.  
  
### <a name="review-index-conversion"></a>Vérifier la conversion de l'index  
 La page suivante est **Vérifier la conversion de l'index**. Le Conseiller d'optimisation de la mémoire détecte s'il existe un ou plusieurs index dans la table, puis remplit la liste de colonnes et le type de données. Les paramètres que vous pouvez configurer dans la page **Vérifier la conversion de l'index** sont similaires à ceux de la page précédente, **Vérifier la conversion de la clé primaire** .  
  
 Si la table ne possède qu'une clé primaire et qu'elle est migrée vers une table durable, cet écran ne s'affiche pas.  
  
 Après avoir pris une décision pour chaque index de la table, cliquez sur **Suivant**.  
  
### <a name="verify-migration-actions"></a>Vérifier les actions de migration  
 La page suivante est **Vérifier les actions de migration**. Pour créer un script de l'opération de migration, cliquez sur **Script** pour générer un script [!INCLUDE[tsql](../../includes/tsql-md.md)] . Vous pouvez ensuite modifier et exécuter le script. Cliquez sur **Migrer** pour démarrer la migration de la table.  
  
 Une fois le traitement terminé, actualisez l’ **Explorateur d’objets** pour afficher la nouvelle table optimisée en mémoire et l’ancienne table sur disque. Vous pouvez conserver l'ancienne table ou la supprimer, à votre convenance personnelle.  
  
## <a name="see-also"></a> Voir aussi  
 [Migration vers OLTP en mémoire](../../relational-databases/in-memory-oltp/migrating-to-in-memory-oltp.md)  
  
  

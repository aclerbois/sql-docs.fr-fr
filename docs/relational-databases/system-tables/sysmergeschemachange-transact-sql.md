---
title: sysmergeschemachange (Transact-SQL) | Documents Microsoft
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine
ms.component: system-tables
ms.reviewer: ''
ms.suite: sql
ms.technology:
- replication
ms.tgt_pltfrm: ''
ms.topic: language-reference
applies_to:
- SQL Server
f1_keywords:
- sysmergeschemachange_TSQL
- sysmergeschemachange
dev_langs:
- TSQL
helpviewer_keywords:
- sysmergeschemachange system table
ms.assetid: ae9ce16e-6ee9-4c7c-8210-a9bf2c7efdf0
caps.latest.revision: 26
author: edmacauley
ms.author: edmaca
manager: craigg
ms.openlocfilehash: 4cfc7662dc9ef63d606c0fae39713c1a012b5a0d
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sysmergeschemachange-transact-sql"></a>sysmergeschemachange (Transact-SQL)
[!INCLUDE[tsql-appliesto-ss2008-xxxx-xxxx-xxx-md](../../includes/tsql-appliesto-ss2008-xxxx-xxxx-xxx-md.md)]

  Contient des informations sur les articles publiés générés par l'Agent d'instantané. Cette table est stockée dans les bases de données de publication et d’abonnement.  
  
|Nom de colonne|Type de données| Description|  
|-----------------|---------------|-----------------|  
|**pubid**|**uniqueidentifier**|ID de la publication.|  
|**artid**|**uniqueidentifier**|L’ID de l’article.|  
|**version de schéma**|**int**|Numéro de la dernière modification de schéma.|  
|**SchemaGuid**|**uniqueidentifier**|ID unique du dernier schéma.|  
|**SchemaType**|**int**|Type de modification du schéma :<br /><br /> **-1** = non valide.<br /><br /> **1** = commande SQL.<br /><br /> **2** = script du schéma.<br /><br /> **3** = utilitaire de programme natif de copie en bloc (BCP).<br /><br /> **4** = caractère BCP.<br /><br /> **5** = dernière génération enregistrée.<br /><br /> **6** = dernière génération envoyée.<br /><br /> **7** = répertoire.<br /><br /> **8** = priorité.<br /><br /> **9** = période de rétention.<br /><br /> **10** = script de déclenchement.<br /><br /> **11** = modification de la table.<br /><br /> **12** = tout réinitialiser.<br /><br /> **13** = ALTER TABLE (non -[!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)]).<br /><br /> **14** = réinitialisation avec téléchargement.<br /><br /> **15** = script d’index et de contrainte.<br /><br /> **16** = nettoyage des métadonnées.<br /><br /> **17** = mise à jour de la dernière génération envoyée.<br /><br /> **18** = niveau de compatibilité descendante.<br /><br /> **19** = abonné de valider les informations.<br /><br /> **20** = partitionné correctement.<br /><br /> **21** = résolveur personnalisé.<br /><br /> **22** = ordre de traitement de l’article.<br /><br /> **23** = publié dans une publication transactionnelle.<br /><br /> **24** = compensation des erreurs.<br /><br /> **25** = autre dossier d’instantanés.<br /><br /> **26** = téléchargement seul.<br /><br /> **27** = le suivi des suppressions.<br /><br /> **40** = précréation du script d’instantané.<br /><br /> **45** = post-création du script d’instantané.<br /><br /> **46** = le script utilisateur sur demande.<br /><br /> **50** = en-tête d’instantané de commencer.<br /><br /> **51** = fin d’en-tête de capture instantanée.<br /><br /> **52** = fin d’instantané.<br /><br /> **53** = adresse de transfert de protocole FTP (file).<br /><br /> **54** = port FTP.<br /><br /> **55** = sous-répertoire FTP.<br /><br /> **56** = instantané compressé.<br /><br /> **57** = connexion FTP.<br /><br /> **58** = mot de passe FTP.<br /><br /> **60** = précréation du script système.<br /><br /> **61** = schéma de procédure stockée.<br /><br /> **62** = schéma de vue.<br /><br /> **63** = schéma de la fonction définie par l’utilisateur.<br /><br /> **64** = index de la vue.<br /><br /> **65** = propriétés étendues.<br /><br /> **66** = valider.<br /><br /> **67** = commande SQL de pré-capture instantanée.<br /><br /> **71** = validation d’instantané dynamique.<br /><br /> **80** = système natif BCP 9.0 de table.<br /><br /> **81** = BCP 9.0 portant sur les caractères de table système.<br /><br /> **82** = système natif BCP 9.0 (global uniquement) de la table.<br /><br /> **83** = caractères de table système BCP 9.0 (global uniquement).<br /><br /> **84** = système natif BCP 9.0 (léger) de la table.<br /><br /> **85** = caractères de table système BCP 9.0 (allégée).<br /><br /> **128** = BCP dynamique (bits).<br /><br /> **131** = BCP natif dynamique.<br /><br /> **132** = dynamique caractère BCP.<br /><br /> **208** = dynamique natif BCP 9.0 de table système.<br /><br /> **209** = BCP 9.0 portant sur les caractères de table système dynamique.<br /><br /> **210** = dynamique de table système natif BCP 9.0 (global uniquement).<br /><br /> **211** = caractères de table système dynamique BCP 9.0 (global uniquement).<br /><br /> **212** = dynamique de table système natif BCP 9.0 (allégée).<br /><br /> **213** = caractères de table système dynamique BCP 9.0 (allégée).<br /><br /> **300** = actions du langage de définition de données (DDL).<br /><br /> **1024** = contrôle d’instantané incrémentielle.<br /><br /> **1049** = dossier d’instantanés incrémentiels.<br /><br /> **1074** = incrémentiel en-tête de début d’instantané.<br /><br /> **1075** = en-tête de fin d’instantané incrémentielle.<br /><br /> **1076** = fin d’instantané incrémentielle.<br /><br /> **1077** = adresse FTP incrémentielle.<br /><br /> **1078** = port FTP incrémentiel.<br /><br /> **1079** = sous-répertoire FTP incrémentiel.<br /><br /> **1080** = instantané compressé incrémentiel.<br /><br /> **1081** = connexion FTP incrémentiel.<br /><br /> **1082** = mot de passe FTP incrémentiel.|  
|**schematext**|**nvarchar(2000)**|Nom du fichier de script ou commande incluant un nom de fichier|  
|**schemastatus**|**tinyint**|Indique si une modification de schéma est en cours relative à l'article, pouvant alors prendre une des valeurs ci-dessous :<br /><br /> **0** = inactif.<br /><br /> **1** = actif.<br /><br /> Lorsqu’une modification de schéma est en attente, cette valeur est définie sur **1**.|  
|**schemasubtype**|**int**|Sous-type de modification du schéma :<br /><br /> **1** = ADDCOLUMN<br /><br /> **2** = DROPCOLUMN<br /><br /> **3** = ALTERCOLUMN<br /><br /> **4** = ADDPRIMARYKEY<br /><br /> **5** = ADDUNIQUE<br /><br /> **6** = ADDREFERENCE<br /><br /> **7** = DROPCONSTRAINT<br /><br /> **8** = ADDDEFAULT<br /><br /> **9** = ADDCHECK<br /><br /> **10** = DISABLETRIGGER<br /><br /> **11** = ENABLETRIGGER<br /><br /> **12** = DISABLETRIGGER<br /><br /> **13** = ENABLETRIGGER<br /><br /> **14** = ENABLECONSTRAINT<br /><br /> **15** = DISABLECONSTRAINT<br /><br /> **16** = ENABLECONSTRAINT<br /><br /> **17** = DISABLECONSTRAINT|  
  
## <a name="see-also"></a>Voir aussi  
 [Tables de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-tables/replication-tables-transact-sql.md)   
 [Vues de réplication &#40;Transact-SQL&#41;](../../relational-databases/system-views/replication-views-transact-sql.md)  
  
  

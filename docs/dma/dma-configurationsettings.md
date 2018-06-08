---
title: Paramètres de configuration (données Assistant Migration SQL Server) | Documents Microsoft
ms.custom: ''
ms.date: 08/31/2017
ms.prod: sql
ms.prod_service: dma
ms.reviewer: ''
ms.suite: sql
ms.technology: dma
ms.tgt_pltfrm: ''
ms.topic: conceptual
keywords: ''
helpviewer_keywords:
- Data Migration Assistant, Assess
ms.assetid: ''
caps.latest.revision: ''
author: HJToland3
ms.author: jtoland
manager: craigg
ms.openlocfilehash: 4b42f816755b312f95609bd25ac6122b8fbf321c
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="configuration-settings-for-data-migration-assistant"></a>Paramètres de configuration de l’Assistant Migration de données

Vous pouvez affiner le comportement de certaine des données de l’Assistant Migration de l’aide des valeurs de configuration dans le fichier dma.exe.config. Cet article décrit les valeurs de configuration de la clé.

Vous trouverez le fichier dma.exe.config pour l’application de bureau de l’Assistant Migration de données et de l’utilitaire de ligne de commande, dans les dossiers suivants sur votre ordinateur.

- Application de bureau

  % ProgramFiles%\\Assistant de Migration de données Microsoft\\dma.exe.config

- Utilitaire de ligne de commande

  % ProgramFiles%\\Assistant de Migration de données Microsoft\\dmacmd.exe.config 

Veillez à enregistrer une copie du fichier de configuration d’origine avant d’apporter des modifications. Après avoir apporté les modifications, redémarrez données Assistant de Migration pour les nouvelles valeurs de configuration prenne effet.

## <a name="number-of-databases-to-assess-in-parallel"></a>Nombre de bases de données à évaluer en parallèle

L’Assistant Migration de données évalue plusieurs bases de données en parallèle. Au cours de l’évaluation de l’Assistant Migration de données extrait l’application de la couche données (dacpac) pour comprendre le schéma de base de données. Cette opération peut expirer si plusieurs bases de données sur le même serveur sont évaluées en parallèle. 

À partir de l’Assistant Migration de données v2.0, vous pouvez contrôler la cela en définissant le parallelDatabases valeur de configuration. Valeur par défaut est 8.

```
<advisorGroup>

<workflowSettings>

<assessment parallelDatabases="8" />

</workflowSettings>

</advisorGroup>
```




## <a name="number-of-databases-to-migrate-in-parallel"></a>Nombre de bases de données à migrer en parallèle

L’Assistant Migration de données migre plusieurs bases de données en parallèle, avant que les connexions migration. Pendant la migration, l’Assistant Migration de données prend une sauvegarde de la base de données source, la sauvegarde de copie si vous le souhaitez et puis le restaurer sur le serveur cible. Vous pouvez rencontrer des erreurs de délai d’attente lorsque plusieurs bases de données sont sélectionnés pour la migration. 

En commençant avec la version 2.0 de l’Assistant Migration de données, si vous rencontrez ce problème, vous pouvez réduire la valeur de configuration parallelDatabases. Vous pouvez augmenter la valeur pour réduire la durée de la migration globale.

```
<advisorGroup>

<workflowSettings>

<migration parallelDatabases=”8″ />

</workflowSettings>

</advisorGroup>
```


## <a name="dacfx-settings"></a>Paramètres de DacFX

Au cours de l’évaluation, l’Assistant Migration de données extrait l’application de la couche données (dacpac) pour comprendre le schéma de base de données. Cette opération peut échouer avec les délais d’attente pour les bases de données extrêmement volumineux, ou si le serveur est sous charge. À partir de la version 1.0 de la Migration des données, vous pouvez modifier les valeurs de configuration suivantes pour éviter les erreurs. 

> [!NOTE]
> L’ensemble de &lt;dacfx&gt; entrée est commentée par défaut. Supprimez les commentaires et modifiez la valeur en fonction des besoins.

- CommandTimeout

   Cette valeur définit la propriété IDbCommand.CommandTimeout *secondes*. (Par défaut = 60)

- databaseLockTimeout

   Cela est équivalent à [définir le verrou\_délai d’expiration du délai d’attente\_période ](../t-sql/statements/set-lock-timeout-transact-sql.md) dans *millisecondes*. (Par défaut = 5 000)

- maxDataReaderDegreeOfParallelism

   Nombre de connexions de pool de connexion SQL à utiliser. (Par défaut = 8)

```
<advisorGroup>

<advisorSettings>

<dacFx  commandTimeout="60" databaseLockTimeout="5000"
maxDataReaderDegreeOfParallelism="8"/>

</advisorSettings>

</advisorGroup>
```


## <a name="stretch-database-recommendation-threshold"></a>Extension de base de données : Seuil de recommandation

Avec [base de données SQL Server Stretch](https://docs.microsoft.com/sql/sql-server/stretch-database/stretch-database), permet d’étendre dynamiquement les données transactionnelles à chaudes et à froid à partir de Microsoft SQL Server 2016 vers Azure. Étendre des bases de données de base de données cibles transactionnelles de grandes quantités de données à froid. La recommandation de la base de données Stretch, sous la recommandation de fonctionnalité de stockage, identifie tout d’abord les tables qu’il considère comme bénéficieront de cette fonctionnalité, puis il identifie les modifications qui doivent être apportées pour activer la table pour cette fonctionnalité.

À partir de l’Assistant Migration de données v2.0, vous pouvez contrôler ce seuil pour une table bénéficier de la fonctionnalité Stretch Database à l’aide de la valeur de configuration recommendedNumberOfRows. Valeur par défaut est 100 000 lignes. Si vous souhaitez analyser les fonctionnalités d’extension pour encore plus petites tables, réduisez la valeur en conséquence.

```
<advisorGroup>

<advisorSettings>

<stretchDBAdvisor  recommendedNumberOfRows="100000" />

</advisorSettings>

</advisorGroup>
```


## <a name="sql-connection-timeout"></a>Délai de connexion SQL

Vous pouvez contrôler le [délai de connexion SQL](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.connectiontimeout(v=vs.110).aspx) pour les instances source et cible lors de l’exécution d’une évaluation ou la migration, en définissant la valeur de délai d’attente de connexion à un nombre de secondes spécifié. La valeur par défaut est 15 secondes.

```
<appSettings>

<add key="ConnectionTimeout" value="15" />

</appSettings>
```


## <a name="see-also"></a>Voir aussi

[Télécharger l’Assistant Migration de données](https://www.microsoft.com/download/details.aspx?id=53595)

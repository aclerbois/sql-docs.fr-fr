---
title: Spécifier la longueur des champs au moyen de bcp (SQL Server) | Microsoft Docs
ms.custom: ''
ms.date: 03/14/2017
ms.prod: sql
ms.prod_service: database-engine, sql-database, sql-data-warehouse, pdw
ms.component: import-export
ms.reviewer: ''
ms.suite: sql
ms.technology: data-movement
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- native data format [SQL Server]
- default field lengths
- field length [SQL Server]
- data formats [SQL Server], field length
- bcp utility [SQL Server], field length
ms.assetid: 240f33ca-ef4a-413a-a4de-831885cb505b
caps.latest.revision: 27
author: douglaslMS
ms.author: douglasl
manager: craigg
monikerRange: '>= aps-pdw-2016 || = azuresqldb-current || = azure-sqldw-latest || >= sql-server-2016 || = sqlallproducts-allversions'
ms.openlocfilehash: 5bda4afe1b3c6b64ea1609412be66de03fa6329e
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="specify-field-length-by-using-bcp-sql-server"></a>Spécifier la longueur des champs au moyen de bcp (SQL Server)
[!INCLUDE[appliesto-ss-asdb-asdw-pdw-md](../../includes/appliesto-ss-asdb-asdw-pdw-md.md)]
  La longueur de champ indique le nombre maximal de caractères nécessaires pour représenter les données au format caractères. La longueur de champ est déjà connue si les données sont enregistrées au format natif. Par exemple, les données de type **int** occupent 4 octets. Si vous indiquez 0 pour la longueur du préfixe, la commande **bcp** vous demande la longueur des champs, les longueurs par défaut des champs et l’influence de la longueur des champs sur le stockage des données dans des fichiers de données qui contiennent des données de type **char** .  
  
## <a name="the-bcp-prompt-for-field-length"></a>Invite bcp pour la longueur des champs  
 Si une commande **bcp** interactive contient l’option **in** ou **out** sans commutateur de fichier de format (**-f**) ou sans commutateur de format de données (**-n**, **-c**, **-w** ou **-N**), la commande demande la longueur de champ de chaque champ de données, comme suit :  
  
 `Enter length of field <field_name> [<default>]:`  
  
 Pour obtenir un exemple illustrant cette invite en contexte, consultez [Spécifier des formats de données pour la compatibilité lors de l’utilisation de bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-data-formats-for-compatibility-when-using-bcp-sql-server.md).  
  
> [!NOTE]  
>  Après avoir indiqué de façon interactive tous les champs d’une commande **bcp**, cette dernière vous demande de sauvegarder vos réponses dans un fichier de format autre que XML pour chacun des champs fournis. Pour plus d’informations sur les fichiers de format non-XML, consultez [Fichiers de format non-XML &#40;SQL Server&#41;](../../relational-databases/import-export/non-xml-format-files-sql-server.md).  
  
 Plusieurs facteurs déterminent si une commande **bcp** vous demande la longueur de champ :  
  
-   Lorsque vous copiez des types de données dont la longueur n’est pas fixe et que vous spécifiez une longueur de préfixe nulle (0), la commande **bcp** demande la longueur de champ.  
  
-   Lors de la conversion de données non caractères en données caractères, la commande **bcp** suggère une longueur de champ par défaut suffisamment importante pour stocker les données.  
  
-   Si le type de stockage de fichier est non caractère, la commande **bcp** ne demande pas de longueur de champ. Les données sont enregistrées au format natif [!INCLUDE[msCoName](../../includes/msconame-md.md)] [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] .  
  
## <a name="using-default-field-lengths"></a>Utilisation des longueurs de champs par défaut  
 [!INCLUDE[msCoName](../../includes/msconame-md.md)] recommande généralement d’accepter les valeurs par défaut de longueur de champ suggérées par la commande **bcp**. Lors de la création d'un fichier de données en mode caractère, l'utilisation de la longueur de champ par défaut garantit que les données ne sont pas tronquées et que des erreurs de dépassement de capacité numérique ne se produiront pas.  
  
 Des problèmes peuvent se produire si vous spécifiez une longueur de champ incorrecte. Par exemple, si cous copiez des données numériques et si vous spécifiez une longueur de champ trop faible pour ces données, l'utilitaire **bcp** affiche un message de dépassement de capacité et ne copie pas les données. De même, si vous exportez des données de type **datetime** et spécifiez une longueur de champ inférieure à 26 octets pour la chaîne de caractères, l'utilitaire **bcp** tronque les données sans message d'erreur.  
  
> [!IMPORTANT]  
>  Lorsque vous utilisez l'option de taille par défaut, [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] s'attend à lire une chaîne complète. Dans certains cas, l'utilisation de la longueur de champ par défaut peut entraîner l'erreur « Fin de fichier inattendue ». Cette erreur se produit généralement avec les types de données **money** et **datetime** quand seule une partie du champ attendu se trouve dans le fichier de données. C’est le cas, par exemple, quand une valeur **datetime** de type *mm*/*jj*/*aa* est spécifiée sans la composante d’heure. Elle est donc plus courte que la longueur de 24 caractères attendue pour une valeur **datetime** au format **char** . Pour éviter ce type d'erreur, utilisez des marques de fin de champ ou des champs de données de longueur fixe, ou modifiez la longueur par défaut du champ en spécifiant une autre valeur.  
  
### <a name="default-field-lengths-for-character-file-storage"></a>Longueur par défaut des champs pour le stockage de fichiers au format caractère  
 Le tableau ci-dessous répertorie les longueurs par défaut des champs à enregistrer dans des fichiers de type caractère. Les données pouvant être de type NULL sont de même longueur que les données de type non NULL.  
  
|Type de données|Longueur par défaut (en caractères)|  
|---------------|-----------------------------------|  
|**char**|Longueur définie pour la colonne|  
|**varchar**|Longueur définie pour la colonne|  
|**nchar**|Le double de la longueur définie pour la colonne|  
|**nvarchar**|Le double de la longueur définie pour la colonne|  
|**Texte**|0|  
|**ntext**|0|  
|**bit**| 1|  
|**binaire**|Le double de la longueur définie pour la colonne + 1|  
|**varbinary**|Le double de la longueur définie pour la colonne + 1|  
|**image**|0|  
|**datetime**|24|  
|**smalldatetime**|24|  
|**float**|30|  
|**real**|30|  
|**Int**|12|  
|**bigint**|19|  
|**smallint**|7|  
|**tinyint**|5|  
|**money**|30|  
|**smallmoney**|30|  
|**decimal**|41*|  
|**numeric**|41*|  
|**uniqueidentifier**|37|  
|**timestamp**|17|  
|**varchar(max)**|0|  
|**varbinary(max)**|0|  
|**nvarchar(max)**|0|  
|UDT|Longueur de la colonne d'un terme défini par l'utilisateur (UDT)|  
|XML|0|  
  
 \*Pour plus d’informations sur les types de données **decimal** et **numeric**, consultez [decimal et numeric &#40;Transact-SQL&#41;](../../t-sql/data-types/decimal-and-numeric-transact-sql.md).  
  
> [!NOTE]  
>  Une colonne de type **tinyint** peut avoir des valeurs comprises entre 0 et 255. Le nombre maximal de caractères nécessaires à la représentation de tout nombre compris dans cet intervalle est de trois (représentant les valeurs comprises entre 100 et 255).  
  
### <a name="default-field-lengths-for-native-file-storage"></a>Longueur par défaut des champs pour le stockage de fichiers au format natif  
 Le tableau ci-dessous répertorie les longueurs par défaut des champs à enregistrer dans des fichiers au format natif. Les données pouvant être de type NULL sont de même longueur que les données de type non NULL. Les données de type caractère sont toujours enregistrées au format caractère.  
  
|Type de données|Longueur par défaut (en caractères)|  
|---------------|-----------------------------------|  
|**bit**| 1|  
|**binaire**|Longueur définie pour la colonne|  
|**varbinary**|Longueur définie pour la colonne|  
|**image**|0|  
|**datetime**|8|  
|**smalldatetime**|4|  
|**float**|8|  
|**real**|4|  
|**Int**|4|  
|**bigint**|8|  
|**smallint**|2|  
|**tinyint**| 1|  
|**money**|8|  
|**smallmoney**|4|  
|**decimal**|*|  
|**numeric**|*|  
|**uniqueidentifier**|16|  
|**timestamp**|8|  
  
 \*Pour plus d’informations sur les types de données **decimal** et **numeric**, consultez [decimal et numeric &#40;Transact-SQL&#41;](../../t-sql/data-types/decimal-and-numeric-transact-sql.md).  
  
 Dans tous les cas précédents, pour créer un fichier de données pour un chargement ultérieur dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] et faire en sorte que l'espace de stockage soit minimal, utilisez un préfixe de longueur avec le type de stockage de fichier et la longueur du champ par défaut.  
  
## <a name="see-also"></a> Voir aussi  
 [Utilitaire bcp](../../tools/bcp-utility.md)   
 [Types de données &#40;Transact-SQL&#41;](../../t-sql/data-types/data-types-transact-sql.md)   
 [Spécifier des indicateurs de fin de champ et de fin de ligne &#40;SQL Server&#41;](../../relational-databases/import-export/specify-field-and-row-terminators-sql-server.md)   
 [Spécifier une longueur de préfixe dans des fichiers de données à l’aide de bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-prefix-length-in-data-files-by-using-bcp-sql-server.md)   
 [Spécifier le type de stockage de fichiers à l’aide de bcp &#40;SQL Server&#41;](../../relational-databases/import-export/specify-file-storage-type-by-using-bcp-sql-server.md)   
 [Conserver les valeurs NULL ou utiliser la valeur par défaut lors de l’importation en bloc &#40;SQL Server&#41;](../../relational-databases/import-export/keep-nulls-or-use-default-values-during-bulk-import-sql-server.md)  
  
  

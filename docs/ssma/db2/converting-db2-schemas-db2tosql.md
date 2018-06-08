---
title: Conversion de schémas de DB2 (DB2ToSQL) | Documents Microsoft
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-db2
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
ms.assetid: 7947efc3-ca86-4ec5-87ce-7603059c75a0
caps.latest.revision: 9
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 3553064a4af95e29a3ed0f7f58e1e2b03215cad1
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="converting-db2-schemas-db2tosql"></a>Conversion de schémas de DB2 (DB2ToSQL)
Une fois que vous êtes connecté à DB2, connectée à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)], et définir le projet et les options de mappage de données, vous pouvez convertir des objets de base de données DB2 à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] les objets de base de données.  
  
## <a name="the-conversion-process"></a>Le processus de Conversion  
Convertir des objets de base de données accepte les définitions d’objets à partir de DB2, les convertit en similaire [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] objets et charge ensuite ces informations dans les métadonnées SSMA. Il ne charge pas les informations dans l’instance de [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Vous pouvez ensuite afficher les objets et leurs propriétés à l’aide de la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Explorateur de métadonnées.  
  
Lors de la conversion, SSMA imprime la sortie des messages vers le volet de sortie et messages d’erreur dans le volet de la liste d’erreurs. Utilisez les informations de sortie et d’erreur pour déterminer si vous devez modifier vos bases de données DB2 ou votre processus de conversion pour obtenir les résultats de la conversion souhaitée.  
  
## <a name="setting-conversion-options"></a>Définition des Options de Conversion  
Avant de convertir les objets, vérifiez les options de conversion de projet dans le **les paramètres de projet** boîte de dialogue. À l’aide de cette boîte de dialogue, vous pouvez définir comment SSMA convertit les fonctions et variables globales. Pour plus d’informations, consultez [les paramètres de projet &#40;Conversion&#41; &#40;DB2ToSQL&#41;](../../ssma/db2/project-settings-conversion-db2tosql.md).  
  
## <a name="conversion-results"></a>Résultats de la conversion  
Le tableau suivant présente les objets de DB2 sont convertis et résultant [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] objets :  
  
|Objets DB2|Résultant d’objets SQL Server|  
|-----------|----------------------------|  
|Types de données|**SSMA mappe chaque type à l’exception des éléments répertoriés ci-dessous :**<br /><br />CLOB : Certaines fonctions de natives pour un travail avec ce type ne sont pas pris en charge (par exemple, CLOB_EMPTY())<br /><br />Objet BLOB : Certaines fonctions de natives pour un travail avec ce type ne sont pas pris en charge (par exemple, BLOB_EMPTY())<br /><br />DBLOB : Certaines fonctions de natives pour un travail avec ce type ne sont pas pris en charge (par exemple, DBLOB_EMPTY())|  
|Types définis par l'utilisateur|**SSMA mappe les éléments suivants définis par l’utilisateur :**<br /><br />Type distinct<br /><br />Type structuré<br /><br />Types de données SQL PL-Remarque : type de curseur faible ne sont pas pris en charge.|  
|Registres spéciaux|**SSMA mappe uniquement les registres répertoriées ci-dessous :**<br /><br />HORODATEUR ACTUEL<br /><br />DATE ACTUELLE<br /><br />HEURE ACTUELLE<br /><br />FUSEAU HORAIRE ACTUEL<br /><br />UTILISATEUR ACTUEL<br /><br />SESSION_USER et utilisateur<br /><br />SYSTEM_USER<br /><br />CLIENT_APPLNAME EN COURS<br /><br />CLIENT_WRKSTNNAME EN COURS<br /><br />DÉLAI D’ATTENTE DE VERROU EN COURS<br /><br />SCHÉMA ACTUEL<br /><br />SERVEUR EN COURS<br /><br />ISOLEMENT ACTUEL<br /><br />Autres registres spéciaux ne sont pas mappées à la sémantique de SQL server.|  
|CREATE TABLE|**SSMA est mappé à créer une TABLE avec les exceptions suivantes :**<br /><br />Tables de clustering (MDC) multidimensionnel<br /><br />Tables cluster plage (RCT)<br /><br />tables partitionnées ;<br /><br />Table détaché<br /><br />Clause de CAPTURE de données<br /><br />Option de IMPLICITLY masqués<br /><br />Option VOLATILE|  
|CREATE VIEW|SSMA est mappé à créer un affichage avec 'WITH LOCAL CHECK OPTION' mais d’autres options ne sont pas mappées à la sémantique SQL server|  
|CREATE INDEX|**SSMA mappe CREATE INDEX avec les exceptions suivantes :**<br /><br />Index XML<br /><br />Option BUSINESS_TIME sans chevauchement<br /><br />Clause partitionnée<br /><br />Option de spécification uniquement<br /><br />ÉTENDRE à l’aide de l’option<br /><br />Option de MINPCTUSED<br /><br />Option de fractionnement de PAGE|  
|Déclencheurs|**SSMA est mappé à la sémantique des déclencheurs suivants :**<br /><br />Après avoir / pour les déclencheurs de chaque ligne<br /><br />Après avoir /FOR chaque instruction déclenche<br /><br />AVANT de chaque ligne et à la place de / pour les déclencheurs de chaque ligne|  
|Séquences|Sont mappées.|  
|Instruction SELECT|**Mappages SSMA sélectionnez avec les exceptions suivantes :**<br /><br />Clause de données-modifier-table-référence – partiellement mappé, mais les tables finales ne non pris en charge<br /><br />Clause de la référence de table – partiellement mappé, mais uniquement--référence de table, référence de table externe, analyze_table-expression, table dérivée collection, xmltable-expression ne sont pas mappées à la sémantique SQL server<br /><br />Spécification de la période de la clause – ne pas mappée.<br /><br />Clause Gestionnaire Continuer – ne pas mappée.<br /><br />Clause de corrélation typée – ne pas mappée.<br /><br />Clause de la résolution de l’accès simultané – ne pas mappée.|  
|Instruction de valeurs|Est mappé.|  
|INSERT (instruction)|Est mappé.|  
|Instruction de mise à jour|S**SMA mappe la mise à jour avec les exceptions suivantes :**<br /><br />Clause de référence de table – uniquement--référence de table n’est pas mappé à la sémantique SQL server<br /><br />Clause période – n’est pas mappé.|  
|Instruction MERGE|**SSMA mappe la fusion avec les exceptions suivantes :**<br /><br />Une seule / plusieurs Occurrences de chaque Clause - est mappé à la sémantique SQL server limité occurrences de chaque clause<br /><br />Clause SIGNAL – ne correspond pas à la sémantique de SQL Server<br /><br />Mixte mise à jour et supprimer des Clauses – ne correspond pas à la sémantique de SQL Server<br /><br />Clause de période – ne mappe pas à la sémantique de SQL Server|  
|DELETE (instruction)|**SUPPRIMER les mappages SSMA avec les exceptions suivantes :**<br /><br />Clause de référence de table – uniquement--référence de table n’est pas mappé à la sémantique SQL server<br /><br />Clause période – ne mappe pas à la sémantique de SQL Server|  
|Niveau d’isolement et le Type de verrou|Est mappé.|  
|Procédures (SQL)|Sont mappées.|  
|Procédures (externe)|Nécessitent la mise à jour manuelle.|  
|Procédures (source)|Ne correspondent pas à la sémantique de SQL Server.|  
|Instruction d’assignation|Est mappé.|  
|Instruction d’appel pour une procédure|Est mappé.|  
|Instruction CASE|Est mappé.|  
|Instruction FOR|Est mappé.|  
|GOTO (instruction)|Est mappé.|  
|Instruction IF|Est mappé.|  
|Itérer au sein d’instruction|Est mappé.|  
|Laissez l’instruction|Est mappé.|  
|Instruction de boucle|Est mappé.|  
|RÉPÉTEZ l’instruction|Est mappé.|  
|SIGNALER l’instruction|Conditions ne sont pas prises en charge. Les messages peuvent être facultatifs.|  
|RETURN (instruction)|Est mappé.|  
|Instruction de SIGNAL|Conditions ne sont pas prises en charge. Les messages peuvent être facultatifs.|  
|WHILE, instruction|Est mappé.|  
|GET (instruction) de DIAGNOSTICS|**SSMA est mappé à obtenir des DIAGNOSTICS avec les exceptions suivantes :**<br /><br />ROW_COUNT – est mappée.<br /><br />DB2_RETURN_STATUS – est mappée.<br /><br />MESSAGE_TEXT – est mappée.<br /><br />DB2_SQL_NESTING_LEVEL - ne mappe pas à la sémantique de SQL Server<br /><br />DB2_TOKEN_STRING - ne mappe pas à la sémantique de SQL Server|  
|Curseurs|**SSMA mappe les curseurs avec les exceptions suivantes :**<br /><br />Instruction de curseur de ALLOCATE - ne mappe pas à la sémantique de SQL Server<br /><br />Instruction d’associer les LOCALISATEURS - ne mappe pas à la sémantique de SQL Server<br /><br />Instruction DECLARE CURSOR - clause de Returnability n’est pas mappée à la sémantique SQL server<br /><br />Instruction FETCH – un mappage partiel. Les variables en tant que cible sont uniquement pris en charge. DESCRIPTEUR SQLDA n’est pas mappé à la sémantique SQL server|  
|Variables|Sont mappées.|  
|Exceptions, les gestionnaires et les Conditions|**SSMA mappe « exceptions » avec les exceptions suivantes :**<br /><br />Gestionnaires de sortie – sont mappés.<br /><br />Annuler les gestionnaires – sont mappés.<br /><br />Gestionnaires de continuer – ne sont pas mappés.<br /><br />Conditions : il ne correspond pas à la sémantique SQL server.|  
|Instructions SQL dynamiques|Non mappé.|  
|Alias|Sont mappées.|  
|Surnoms|Mappage partiel. Le traitement manuel est requis pour l’objet sous-jacent|  
|Synonymes|Sont mappées.|  
|Fonctions standard dans DB2|SSMA mappe les fonctions standard DB2 quand une fonction équivalente est disponible dans SQL Server :|  
|Autorisation|Non mappé.|  
|Prédicats|Sont mappées.|  
|SELECT INTO, instruction|Non mappé.|  
|Les valeurs dans l’instruction|Non mappé.|  
|Contrôle de transaction|Non mappé.|  
  
## <a name="converting-db2-database-objects"></a>Convertir des objets de base de données DB2  
Pour convertir les objets de base de données DB2, vous tout d’abord sélectionnez les objets que vous souhaitez convertir et puis SSMA effectuer la conversion. Pour afficher les messages de sortie lors de la conversion, sur le **vue** menu, sélectionnez **sortie**.  
  
**Pour convertir des objets de DB2 en syntaxe SQL**  
  
1.  Dans l’Explorateur de métadonnées de DB2, le serveur DB2, puis **schémas**.  
  
2.  Sélectionner les objets à convertir :  
  
    -   Pour convertir tous les schémas, sélectionnez la case à cocher à côté **schémas**.  
  
    -   Pour convertir ou omettre une base de données, sélectionnez la case à cocher en regard du nom de schéma.  
  
    -   Pour convertir ou omettre une catégorie d’objets, développez un schéma, puis sélectionnez ou désactivez la case à cocher en regard de la catégorie.  
  
    -   Pour convertir ou omettre des objets, développez le dossier de la catégorie, puis sélectionnez ou désactivez la case à cocher en regard de l’objet.  
  
3.  Pour convertir tous les objets sélectionnés, cliquez sur **schémas** et sélectionnez **convertir un schéma**.  
  
    Vous pouvez également convertir des objets individuels ou des catégories d’objets en cliquant sur l’objet ou son dossier parent, puis en sélectionnant **convertir un schéma**.  
  
## <a name="viewing-conversion-problems"></a>Affichage des problèmes de Conversion  
Certains objets DB2 ne peuvent pas être converties. Vous pouvez déterminer le taux de réussite de conversion en affichant le rapport de synthèse de conversion.  
  
**Pour afficher un rapport de synthèse**  
  
1.  Dans l’Explorateur de métadonnées de DB2, sélectionnez **schémas**.  
  
2.  Dans le volet droit, sélectionnez le **rapport** onglet.  
  
    Ce rapport affiche le rapport d’évaluation de synthèse pour tous les objets de base de données qui ont été évaluée ou converti. Vous pouvez également afficher un rapport de synthèse pour des objets :  
  
    -   Pour afficher le rapport pour un schéma individuel, sélectionnez le schéma dans l’Explorateur de métadonnées DB2.  
  
    -   Pour afficher le rapport pour un objet, sélectionnez l’objet dans l’Explorateur de métadonnées DB2. Les objets qui ont des problèmes de conversion ont une icône d’erreur rouge.  
  
Pour les objets en échec de conversion, vous pouvez afficher la syntaxe qui a entraîné l’échec de la conversion.  
  
**Pour afficher les problèmes de conversion individuels**  
  
1.  Dans l’Explorateur de métadonnées de DB2, développez **schémas**.  
  
2.  Développez le schéma qui affiche une icône d’erreur rouge.  
  
3.  Sous le schéma, développez un dossier qui a une icône d’erreur rouge.  
  
4.  Sélectionnez l’objet qui a une icône d’erreur rouge.  
  
5.  Dans le volet droit, cliquez sur le **rapport** onglet.  
  
6.  En haut de la **rapport** onglet est une liste déroulante. Si la liste affiche **statistiques**, modifiez la sélection à **Source**.  
  
    SSMA affichera le code source et plusieurs boutons immédiatement au-dessus du code.  
  
7.  Cliquez sur le **problème suivant** bouton. Il s’agit d’une icône d’erreur rouge avec une flèche pointant vers la droite.  
  
    SSMA met en surbrillance le premier code source problématiques qu’elle trouve dans l’objet actuel.  
  
Pour chaque élément ne peut pas être converti, vous devez déterminer ce que vous voulez faire avec cet objet :  
  
-   Vous pouvez modifier le code source des procédures sur la **SQL** onglet.  
  
-   Vous pouvez modifier l’objet dans la base de données DB2 pour supprimer ou modifier tout code problématique. Pour charger le code mis à jour dans SSMA, vous devez mettre à jour les métadonnées. Pour plus d’informations, consultez [la connexion à la base de données DB2 &#40;DB2ToSQL&#41;](../../ssma/db2/connecting-to-db2-database-db2tosql.md).  
  
-   Vous pouvez exclure l’objet de la migration. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] Explorateur de métadonnées et de l’Explorateur de métadonnées DB2, désactivez la case à cocher en regard de l’élément avant de charger les objets dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] et migration des données à partir de DB2.  
  
## <a name="next-step"></a>Étape suivante  
L’étape suivante du processus de migration consiste à [charger les objets convertis dans SQL Server](http://msdn.microsoft.com/en-us/f4ea1ced-9f9f-4a9d-88ab-81dbab64adc3).  
  
## <a name="see-also"></a>Voir aussi  
[Migration de données DB2 dans SQL Server &#40;DB2ToSQL&#41;](../../ssma/db2/migrating-db2-data-into-sql-server-db2tosql.md)  
  

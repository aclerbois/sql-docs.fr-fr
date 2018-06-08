---
title: Paramètres (Conversion) (SybaseToSQL) du projet | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: sql-tools
ms.component: ssma-sybase
ms.reviewer: ''
ms.suite: sql
ms.technology: ssma
ms.tgt_pltfrm: ''
ms.topic: conceptual
applies_to:
- Azure SQL Database
- SQL Server
ms.assetid: eeb80fa5-f530-4f21-beee-25f5a4b8ace6
caps.latest.revision: 8
author: Shamikg
ms.author: Shamikg
manager: craigg
ms.openlocfilehash: 4903ec6923239b76784d1aef94303860582b8b85
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="project-settings-conversion-sybasetosql"></a>Paramètres du projet (Conversion) (SybaseToSQL)
La page de Conversion de la **les paramètres de projet** boîte de dialogue contient des paramètres permettant de personnaliser comment SSMA convertit la syntaxe de Sybase Adaptive Server Enterprise (ASE) à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou une syntaxe de SQL Azure.  
  
Le volet de la Conversion est disponible dans le **les paramètres de projet** et **les paramètres de projet par défaut** boîtes de dialogue :  
  
-   Si vous souhaitez spécifier des paramètres pour tous les projets SSMA, sur le **outils** menu, sélectionnez **les paramètres de projet par défaut**, cliquez sur **général** au bas de la gauche, puis cliquez sur **Conversion**.  
  
-   Pour spécifier les paramètres pour le projet actuel, sur le **outils** menu, sélectionnez **les paramètres de projet**, cliquez sur **général** au bas de la gauche, puis cliquez sur **Conversion**.  
  
## <a name="miscellaneous-options"></a>Options diverses  
**@@ERROR**  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]/ SQL Azure et ASE utilisent différents codes d’erreur.  
  
Utilisez ce paramètre pour spécifier le type de message (avertissement ou erreur) SSMA affiche dans le volet de sortie ou une liste d’erreurs lorsqu’il rencontre une référence à **@@ERROR**  dans le code ASE.  
  
-   Si vous sélectionnez **convertir et la marquer avec l’avertissement**, SSMA convertira les instructions et les marquer avec des commentaires d’avertissement.  
  
-   Si vous sélectionnez **marque avec l’erreur**, SSMA ignore la conversion et marquer les instructions avec des commentaires de l’erreur.  
  
Lorsque vous sélectionnez un mode de conversion dans le **Mode** zone, SSMA s’applique le paramètre suivant :  
  
**Le Mode par défaut/Optimistic :** convertir et la marquer avec l’avertissement  
  
**Mode complet :** marque avec l’erreur  
  
**Conversion de l’opérateur LIKE**  
Spécifie s’il faut convertir comme opérandes pour faire correspondre le comportement de Sybase ASE. Le point est Sybase supprime les espaces de fin dans un modèle like. La solution consiste à effectuer un cast de l’expression de droite à un type de données de longueur fixe avec une précision maximale.  
  
-   Sélectionnez **conversion Simple** pour convertir les expressions sans correction.  
  
-   Pour utiliser l’instruction select comportement ASE **effectué à longueur fixe.**  
  
Lorsque vous sélectionnez un mode de conversion dans la zone Mode, SSMA s’applique le paramètre suivant :  
  
**Le Mode par défaut/Optimistic**: conversion Simple  
  
**Mode plein**: effectuer un Cast à longueur fixe  
  
**CONVERSION OU UN CAST DES CHAÎNES VIDES POUR LES TYPES NUMÉRIQUES**  
Spécifie comment gérer des chaînes vides ou vierges dans des expressions de CONVERT ou CAST avec un type numérique en tant qu’argument de type de données. Les options suivantes sont disponibles pour ce paramètre :  
  
-   Sélectionnez **conversion Simple** pour convertir les expressions sans correction.  
  
-   Si **vide de chaîne comme zéro numérique** est sélectionnée, le paramètre de chaîne {s} sera remplacé par ltrim(rtrim({s})) cas lors de la « » puis 0 else {s} EXPRESSION de fin  
  
Lorsque vous sélectionnez un mode de conversion dans la zone Mode, SSMA s’applique le paramètre suivant :  
  
**Le Mode par défaut/Optimistic**: conversion Simple  
  
**Mode plein**: comme zéro numérique de chaîne vide  
  
**Concaténation de NULL**  
Ce paramètre spécifie comment convertir la concaténation de chaînes avec la valeur NULL. Les options suivantes peuvent être définies pour ce paramètre :  
  
-   **Retour à la ligne avec la fonction ISNULL :** si cette option est définie, chaque « string_expression » dans la concaténation non constante sera encapsulée avec ISNULL(string_expression) et les valeurs NULL seront remplacés par une chaîne vide.  
  
-   **Conserver la syntaxe actuelle**  
  
Lorsque vous sélectionnez un mode de conversion dans le **Mode** zone, SSMA s’applique le paramètre suivant :  
  
**Le Mode par défaut/Optimistic :** conserver la syntaxe actuelle  
  
**Mode complet :** encapsulent avec ISNULL (fonction)  
  
**Conversion des chaînes vides**  
Ce paramètre spécifie comment convertir des chaînes vides. Les options suivantes peuvent être définies pour ce paramètre :  
  
-   **Remplacez toutes les expressions de chaîne avec un espace**  
  
-   **Remplacez les constantes de chaîne vide avec un espace**  
  
-   Pour utiliser le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]/ comportement SQL Azure, sélectionnez **conserver la syntaxe actuelle**.  
  
Lorsque vous sélectionnez un mode de conversion dans le **Mode** zone, SSMA s’applique le paramètre suivant :  
  
**Le Mode par défaut/Optimistic :** conserver la syntaxe actuelle  
  
**Mode complet :** remplacer toutes les expressions de chaîne avec un espace  
  
**Conversion de chaîne binaire CONVERT et CAST**  
La conversion des valeurs binaires en chiffres permettre retourner des valeurs différentes sur différentes plateformes. Par exemple, sur x86 processeurs, CONVERT (entier, 0 x 00000100) retourne 65536 dans ASE et 256 dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. ASE retourne également des valeurs différentes selon l’ordre d’octet.  
  
Utilisez ce paramètre pour contrôler comment convertir de SSMA convertit et les expressions CASE qui contiennent des valeurs binaires :  
  
-   Sélectionnez **conversion Simple** pour convertir les expressions sans avertissement ni correction. Utilisez ce paramètre si vous savez que le serveur ASE a un ordre d’octet qui ne requiert pas de modifications de la valeur binaire.  
  
-   Sélectionnez **convertir et corriger** avoir SSMA convertir et de corriger les expressions pour une utilisation sur [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]. Pour inverser l’ordre d’octet de constantes littérales. Toutes les autres valeurs binaires (tels que les binaires variables et colonnes) sont marquées avec des erreurs. Utilisez cette valeur si vous savez que le serveur ASE a un ordre d’octet qui nécessite la modification de valeurs binaires.  
  
-   Sélectionnez **convertir et la marquer avec l’avertissement** avoir SSMA convertir et de corriger les expressions et de marquer tous convertis expressions avec des commentaires d’avertissement.  
  
Lorsque vous sélectionnez un mode de conversion dans le **Mode** zone, SSMA s’applique le paramètre suivant :  
  
**Mode par défaut :** convertir et la marquer avec l’avertissement  
  
**Mode optimisé :** conversion Simple  
  
**Mode complet :** convertir et corriger  
  
**SQL dynamique**  
Utilisez ce paramètre pour spécifier le type de message (avertissement ou erreur) SSMA affiche dans le volet de sortie ou une liste d’erreurs lorsqu’il rencontre le code SQL dynamique dans le code ASE.  
  
-   Si vous sélectionnez **convertir et la marquer avec l’avertissement**, SSMA convertit le code SQL dynamique et marquer les instructions avec des commentaires d’avertissement.  
  
-   Si vous sélectionnez **marque avec l’erreur**, SSMA ignore la conversion et marquer les instructions avec des commentaires de l’erreur.  
  
Lorsque vous sélectionnez un mode de conversion dans le **Mode** zone, SSMA s’applique le paramètre suivant :  
  
**Le Mode par défaut/Optimistic :** convertir et la marquer avec l’avertissement  
  
**Mode complet :** marque avec l’erreur  
  
**Conversion de vérification de l’égalité**  
Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]/ SQL Azure, si le paramètre ANSI_NULLS est activé, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]/ SQL Azure renvoie UNKNOWN lorsque toute comparaison d’égalité contient une valeur null. Si ANSI_NULLS est désactivée, les comparaisons d’égalité qui contiennent des valeurs null retournent true lorsque la colonne comparée et expression ou deux expressions sont tous deux null. En l’égalité de valeur par défaut (ANSINULL désactivée) Sybase ASE comparaisons se comportent comme [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]/ SQL Azure avec ANSI_NULLS OFF.  
  
-   Si vous sélectionnez **conversion Simple**, SSMA convertit le code ASE à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]/ une syntaxe SQL Azure sans les vérifications supplémentaires pour les valeurs null. Utilisez ce paramètre si ANSI_NULLS est désactivé dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]/ SQL Azure ou si vous souhaitez modifier les comparaisons d’égalité au cas par cas.  
  
-   Si vous sélectionnez **envisagez nul**, SSMA ajoutera des vérifications pour les valeurs null en utilisant les clauses IS NULL et IS NOT NULL.  
  
Lorsque vous sélectionnez un mode de conversion dans le **Mode** zone, SSMA s’applique le paramètre suivant :  
  
**Le Mode par défaut/Optimistic :** conversion Simple  
  
**Mode complet :** envisagez NULL valeurs  
  
**Chaînes de format**  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]/ SQL Azure ne prend plus en le *format_string* argument dans les instructions PRINT et RAISERROR. Le *format_string* variable pris en charge placer des paramètres remplaçables directement dans la chaîne, puis en remplaçant les paramètres lors de l’exécution. Au lieu de cela, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] requiert que la chaîne complète à l’aide d’un littéral de chaîne, ou d’une chaîne générée à l’aide d’une variable. Pour plus d’informations, consultez le « PRINT ([!INCLUDE[tsql](../../includes/tsql_md.md)]) « rubrique dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] la documentation en ligne.  
  
Lorsque SSMA rencontre un *format_string* argument, il peut soit générer une chaîne littérale en utilisant les variables ou créer une nouvelle variable et générer une chaîne à l’aide de cette variable.  
  
-   Pour utiliser un littéral de chaîne pour les fonctions PRINT et RAISERROR, sélectionnez **créer la nouvelle chaîne**.  
  
    Dans ce mode, si une instruction PRINT ou RAISERROR n’utilise pas les espaces réservés et les variables locales, l’instruction est inchangée. Doubles caractères pourcentage (%) sont convertis en un seul caractère de pourcentage % dans les littéraux de chaîne d’impression.  
  
    Si une instruction PRINT ou RAISERROR utilise des espaces réservés et une ou plusieurs variables locales, comme dans l’exemple suivant :  
  
    ```  
    PRINT 'Total: %1!%%', @percent  
    ```  
    SSMA convertira la syntaxe suivante :  
  
    ```  
    PRINT 'Total: '+ CAST(@percent AS varchar(max)) + '%'  
    ```  
    Si *format_string* est une variable, comme dans l’instruction suivante :  
  
    ```  
    PRINT @fmt, @arg1, @arg2  
    ```  
    SSMA ne peut pas effectuer une conversion de chaîne simple et doit créer une variable :  
  
    ```  
    DECLARE @print_format_1 varchar(max)  
    SET @print_format_1  =   
        REPLACE (@fmt, '%%', '%')  
    SET @print_format_1  =   
        REPLACE (@print_format_1, '%1!',   
        CAST (@arg1 AS varchar(max)))  
    SET @print_format_1  =   
        REPLACE (@print_format_1, '%2!',   
        CAST (@arg2 AS varchar(max)))  
    PRINT @print_format_1  
    ```  
    Lorsqu’il utilise la **créer la nouvelle chaîne** mode, SSMA part du principe que la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] option CONCAT_NULL_YIELDS_NULL est désactivé. Par conséquent, SSMA ne vérifie pas les arguments null.  
  
-   Pour générer une nouvelle variable pour chaque instruction PRINT et RAISERROR et ensuite utiliser cette variable pour la valeur de chaîne SSMA, sélectionnez **créer une nouvelle variable**.  
  
    Dans ce mode, si une instruction PRINT ou RAISERROR n’utilise pas les espaces réservés et les variables locales, SSMA remplace tous les deux caractères pourcentage (%) par des caractères de pourcentage uniques pour se conformer aux [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]/ syntaxe de SQL Azure.  
  
    Si une instruction PRINT ou RAISERROR utilise des espaces réservés et une ou plusieurs variables locales, comme dans l’exemple suivant :  
  
    ```  
    PRINT 'Total: %1!%%', @percent  
    ```  
    SSMA convertira la syntaxe suivante :  
  
    ```  
    DECLARE @print_format_1 varchar(max)  
    SET @print_format_1 = 'Total: %1!%'  
    SET @print_format_1  =   
        REPLACE (@print_format_1, '%1!',   
        ISNULL(CAST (@percent AS VARCHAR(max)), ''))  
    PRINT @print_format_1  
    ```  
    Si *format_string* est une variable, comme dans l’instruction suivante :  
  
    ```  
    PRINT @fmt, @arg1, @arg2  
    ```  
    SSMA crée une variable, comme suit, la recherche des valeurs null dans chaque argument :  
  
    ```  
    DECLARE @print_format_1 varchar(max)  
    SET @print_format_1  =   
        REPLACE (@fmt, '%%', '%')  
    SET @print_format_1  =   
        REPLACE (@print_format_1, '%1!',   
        ISNULL(CAST (@arg1 AS varchar(max)),''))  
    SET @print_format_1  =   
        REPLACE (@print_format_1, '%2!',   
        ISNULL(CAST (@arg2 AS varchar(max)),''))  
    PRINT @print_format_1  
    ```  
  
Lorsque vous sélectionnez un mode de conversion dans le **Mode** zone, SSMA s’applique le paramètre suivant :  
  
**Le Mode par défaut/Optimistic :** créer la nouvelle chaîne  
  
**Mode complet :** créer une nouvelle variable  
  
**Insérer une valeur explicite dans une colonne timestamp**  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]/ SQL Azure ne prend pas en charge l’insertion de valeurs explicites dans une colonne timestamp.  
  
-   Pour exclure des colonnes timestamp d’instructions INSERT, sélectionnez **colonne exclure**.  
  
-   Pour imprimer un message d’erreur chaque fois qu’une colonne timestamp est dans une instruction INSERT, sélectionnez **marque avec l’erreur**. Dans ce mode, les instructions INSERT ne seront pas converties et seront marquées avec des commentaires de l’erreur.  
  
Lorsque vous sélectionnez un mode de conversion dans le **Mode** zone, SSMA s’applique le paramètre suivant :  
  
**Le Mode par défaut/Optimistic :** colonne exclure  
  
**Mode complet :** marque avec l’erreur  
  
**Stocker des objets temporaires définis dans les procédures**  
Ce paramètre spécifie si les définitions des objets temporaires qui apparaissent dans les procédures doivent être stockées dans les métadonnées de la source lors de la conversion.  
  
-   Sélectionnez **Oui** à stocker dans les métadonnées.  
  
-   Sélectionnez **non** si les objets ne doivent pas être stockées.  
  
**Le Mode par défaut/optimiste :** Oui  
  
**Mode complet :** N°  
  
**Conversion de la table proxy**  
Spécifie si les tables de proxy ASE sont convertis en [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]/ tables SQL Azure, ou sont pas convertis et le code est marqué avec des commentaires de l’erreur.  
  
-   Sélectionnez **convertir** pour convertir des tables de proxy pour les tables normales.  
  
-   Sélectionnez **marque avec l’erreur** simplement marquer le code de la table proxy avec les commentaires de l’erreur.  
  
Lorsque vous sélectionnez un mode de conversion dans le **Mode** zone, SSMA s’applique le paramètre suivant :  
  
**Le Mode par défaut/Optimistic/complet :** marque avec l’erreur  
  
**Nombre de messages de base RAISERROR**  
Messages de l’utilisateur ASE sont stockées dans chaque base de données. [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] messages de l’utilisateur sont stockées et mises à disposition par le biais de manière centralisée la **sys.messages** affichage catalogue. En outre les messages de l’utilisateur ASE commencent à 20000, mais [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] messages d’erreur commencent à 50001.  
  
Ce paramètre spécifie le nombre à ajouter au numéro de message de l’utilisateur ASE à convertir en un [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] message de l’utilisateur. Si votre [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] a des messages de l’utilisateur dans le **sys.messages** vue de catalogue, vous devrez peut-être modifier ce nombre à une valeur plus élevée. Il s’agit donc les numéros de message converti ne portent pas de numéros de message existant.  
  
Notez les points suivants :  
  
-   Les messages dans la plage-19999 17000 ASE proviennent de la table système sysmessages et ne sont pas convertis.  
  
-   Si le numéro de message qui est référencé dans l’instruction RAISERROR est une constante, SSMA ajoute le nombre de messages de base à la constante pour déterminer le nouveau numéro de message utilisateur.  
  
-   Si le numéro de message qui est référencé est une variable ou une expression, SSMA crée une variable locale intermédiaire.  
  
-   En mode optimisé, SSMA part du principe que la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] option CONCAT_NULL_YIELDS_NULL est désactivé et ne fait aucune vérification pour les arguments null.  
  
-   En mode complet, SSMA vérifie pour les arguments null.  
  
-   RAISERROR avec ERRORDATA *liste* n’est pas converti.  
  
Lorsque vous sélectionnez un mode de conversion dans le **Mode** zone, SSMA s’applique le paramètre suivant :  
  
**Le Mode par défaut/optimiste/complet :** 30001  
  
**Objets système**  
Utilisez ce paramètre pour spécifier le type de message (avertissement ou erreur) SSMA affiche dans le volet de sortie ou une liste d’erreurs lorsqu’il rencontre l’utilisation des objets de système de ASE.  
  
-   Si vous sélectionnez **convertir et la marquer avec l’avertissement**, SSMA convertira les références aux objets système et marque les instructions avec des commentaires d’avertissement.  
  
-   Si vous sélectionnez **marque avec l’erreur**, SSMA ne convertira pas les références aux objets de systèmes et marque les instructions avec des commentaires de l’erreur.  
  
Lorsque vous sélectionnez un mode de conversion dans le **Mode** zone, SSMA s’applique le paramètre suivant :  
  
**Le Mode par défaut/Optimistic :** convertir et la marquer avec l’avertissement  
  
**Mode complet :** marque avec l’erreur  
  
**Identificateurs externes non résolues**  
Utilisez ce paramètre pour spécifier le type de message (avertissement ou erreur) SSMA affiche dans le volet de sortie ou une liste d’erreurs lorsqu’il ne peut pas résoudre un identificateur.  
  
-   Si vous sélectionnez **convertir et la marquer avec l’avertissement**, SSMA tente de convertir la référence à des identificateurs non résolues et marque les instructions avec des commentaires d’avertissement.  
  
-   Si vous sélectionnez **marque avec l’erreur**, SSMA ne convertira pas référence à des identificateurs non résolues et marque les instructions avec des commentaires de l’erreur.  
  
Lorsque vous sélectionnez un mode de conversion dans le **Mode** zone, SSMA s’applique le paramètre suivant :  
  
**Le Mode par défaut/Optimistic :** convertir et la marquer avec l’avertissement  
  
**Mode complet :** marque avec l’erreur  
  
## <a name="system-function-options"></a>Options de la fonction système  
**CHARINDEX (fonction)**  
Dans ASE, CHARINDEX retourne NULL uniquement si toutes les expressions d’entrée ont la valeur NULL. [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]/ SQL Azure retourne NULL si une expression d’entrée est NULL.  
  
-   Pour utiliser le comportement ASE, sélectionnez **remplacer la fonction**. Tous les appels à la fonction CHARINDEX est remplacé par un appel à la fonction définie par l’utilisateur CHARINDEX_VARCHAR ou de CHARINDEX_NVARCHAR en fonction du type des paramètres passés (créé dans la base de données de l’utilisateur sous le nom de schéma 's2ss') pour émuler le comportement de Sybase ASE.  
  
-   Pour utiliser le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]/ comportement SQL Azure, sélectionnez **conserver la syntaxe actuelle**.  
  
Lorsque vous sélectionnez un mode de conversion dans le **Mode** zone, SSMA s’applique le paramètre suivant :  
  
**Le Mode par défaut/Optimistic :** conserver la syntaxe actuelle  
  
**Mode complet :** remplacer (fonction)  
  
**Fonction DATALENGTH**  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] / SQL Azure et ASE diffèrent dans la valeur retournée par la fonction DATALENGTH lorsque la valeur est un espace unique. Dans ce cas, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]/ SQL Azure renvoie la valeur 0 et ASE retourne 1.  
  
-   Pour utiliser le comportement ASE, sélectionnez **remplacer la fonction**. Tous les appels à la fonction DATALENGTH sont substitués avec l’Expression CASE pour émuler le comportement de Sybase ASE.  
  
-   Pour utiliser la valeur par défaut [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] / comportement SQL Azure, sélectionnez **conserver la syntaxe actuelle**.  
  
Lorsque vous sélectionnez un mode de conversion dans le **Mode** zone, SSMA s’applique le paramètre suivant :  
  
**Le Mode par défaut/Optimistic :** conserver la syntaxe actuelle  
  
**Mode complet :** remplacer (fonction)  
  
**INDEX_COL (fonction)**  
ASE prend en charge facultative *user_id* l’argument de la fonction INDEX_COL ; Toutefois, [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]/ SQL Azure ne prend pas en charge cet argument. Si vous utilisez la *user_id* argument, cette fonction ne peut pas être convertie à [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]/ syntaxe de SQL Azure.  
  
-   Pour utiliser le comportement ASE, sélectionnez **convertir fonction**. Si le code contient le *user_id* argument, SSMA affiche une erreur.  
  
-   Pour afficher un message d’erreur chaque fois que INDEX_COL se produit, sélectionnez **marque avec l’erreur**. SSMA ne convertira pas les références à la fonction et marque l’instruction avec des commentaires de l’erreur.  
  
**Le Mode par défaut/Optimistic/complet :** marque avec l’erreur  
  
**Fonction INDEX_COLORDER**  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]/ SQL Azure n’a pas d’une fonction du système INDEX_COLORDER.  
  
-   Pour utiliser le comportement ASE, sélectionnez **convertir fonction**. Tous les appels à la fonction INDEX_COLORDER est remplacé par un appel à une fonction définie par l’utilisateur portant le même nom INDEX_COLORDER (créé dans la base de données de l’utilisateur sous le nom de schéma 's2ss'), qui émule le comportement de Sybase ASE.  
  
-   Pour imprimer un message d’erreur chaque fois que INDEX_COLORDER se produit, sélectionnez **marque avec l’erreur**. SSMA ne convertira pas les références à la fonction et marque l’instruction avec des commentaires de l’erreur.  
  
Lorsque vous sélectionnez un mode de conversion dans le **Mode** zone, SSMA s’applique le paramètre suivant :  
  
**Le Mode par défaut/Optimistic/complet :** marque avec l’erreur  
  
**Les fonctions LEFT et RIGHT**  
Gauche et droite Sybase fonctions se comportent différemment pour le paramètre de longueur négative.  
  
-   Pour utiliser le comportement ASE, sélectionnez **fonction Replace**. Le paramètre de longueur est ensuite remplacé par Expression CASE qui retourne null pour une valeur négative.  
  
-   Pour utiliser le comportement de SQL Server, sélectionnez **conserver la syntaxe actuelle**  
  
Lorsque vous sélectionnez un mode de conversion dans le **Mode** zone, SSMA s’applique le paramètre suivant :  
  
**Le Mode par défaut/Optimistic :** conserver la syntaxe actuelle  
  
**Mode complet :** remplacer (fonction)  
  
> [!NOTE]  
> Si le paramètre de longueur est une valeur littérale et non une expression complexe, la valeur de longueur est toujours remplacée par la valeur null, quelles que soient les paramètres de projet.  
  
**Fonction NEXT_IDENTITY**  
[!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]/ SQL Azure n’a pas d’une fonction du système NEXT_IDENTITY.  
  
-   Pour utiliser le comportement ASE, sélectionnez **fonction Convert**. Tous les appels à la fonction NEXT_IDENTITY est remplacé par expression (IDENT_CURRENT(parameter Value) + IDENT_INCR(parameter Value) qui émule le comportement de Sybase ASE.  
  
-   Pour imprimer un message d’erreur chaque fois que NEXT_IDENTITY se produit, sélectionnez **marque avec l’erreur**. SSMA ne convertira pas les références à la fonction et marque l’instruction avec des commentaires de l’erreur.  
  
Lorsque vous sélectionnez un mode de conversion dans le **Mode** zone, SSMA s’applique le paramètre suivant :  
  
**Le Mode par défaut/Optimistic/complet :** marque avec l’erreur  
  
**Fonction PATINDEX**  
Spécifie s’il faut convertir la fonction PATINDEX pour correspondre au comportement de Sybase ASE. Le point est Sybase supprime les espaces de fin dans un modèle de recherche. La solution consiste à effectuer un cast de l’expression de valeur à une longueur fixe de type avec une précision maximale de données et appliquent la fonction rtrim pour rechercher le modèle.  
  
-   Pour utiliser l’instruction select comportement ASE **utiliser**.  
  
-   Pour utiliser la valeur par défaut [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]/ comportement SQL Azure, sélectionnez **n’utilisez pas**.  
  
Lorsque vous sélectionnez un mode de conversion dans le **Mode** zone, SSMA s’applique le paramètre suivant :  
  
**Le Mode par défaut/Optimistic :** n’utilisez pas  
  
**Mode complet :** utilisation  
  
**REPLICATE (fonction)**  
La fonction REPLICATE répète une chaîne le nombre de fois spécifié. Dans ASE, si vous spécifiez pour répéter la chaîne de zéro fois, le résultat est null. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]/ SQL Azure, le résultat est une chaîne vide.  
  
-   Pour utiliser le comportement ASE, sélectionnez **remplacer la fonction**. Tous les appels à la fonction REPLICATE est remplacé par un appel à la fonction définie par l’utilisateur REPLICATE_VARCHAR ou de REPLICATE_NVARCHAR en fonction du type des paramètres passés (créé dans la base de données de l’utilisateur sous le nom de schéma 's2ss') pour émuler le comportement de Sybase ASE.  
  
-   Pour utiliser la valeur par défaut [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]/ comportement SQL Azure, sélectionnez **fonction Replace**.  
  
Lorsque vous sélectionnez un mode de conversion dans le **Mode** zone, SSMA s’applique le paramètre suivant :  
  
**Le Mode par défaut/Optimistic Mode/complet :** remplacer (fonction)  
  
**La fonction TRIM (LTRIM, RTRIM) (fonction)**  
Ce paramètre spécifie s’il faut remplacer les appels aux fonctions de la fonction Trim (LTRIM, RTRIM) avec les fonctions de syntaxe équivalente Sybase ASE ou conserver la syntaxe actuelle. Les options suivantes sont présentes pour ce paramètre :  
  
-   **Replace, fonction**  
  
-   **Conserver la syntaxe actuelle**  
  
Lorsque vous sélectionnez un mode de conversion dans le **Mode** zone, SSMA s’applique le paramètre suivant :  
  
**Le Mode par défaut/Optimistic Mode/complet :** remplacer (fonction)  
  
**SUBSTRING (fonction)**  
Dans ASE, la fonction `SUBSTRING(expression, start, length)` renvoie la valeur NULL si une valeur de début supérieure au nombre de caractères dans l’expression n’est spécifiée, ou si la longueur est égale à zéro. Dans [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)]/ SQL Azure, l’expression équivalente retourne une chaîne vide.  
  
-   Pour utiliser le comportement ASE, sélectionnez **remplacer la fonction**. Tous les appels à la fonction SUBSTRING est remplacé par un appel à la fonction définie par l’utilisateur SUBSTRING_VARCHAR ou SUBSTRING_NVARCHAR ou SUBSTRING_VARBINARY selon le type des paramètres passés (créé dans la base de données de l’utilisateur sous le nom de schéma 's2ss') pour émuler le comportement de Sybase ASE.  
  
-   Pour utiliser le [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] / comportement SQL Azure, sélectionnez **conserver la syntaxe actuelle**.  
  
Lorsque vous sélectionnez un mode de conversion dans le **Mode** zone, SSMA s’applique le paramètre suivant :  
  
**Le Mode par défaut/Optimistic :** conserver la syntaxe actuelle  
  
**Mode complet :** remplacer (fonction)  
  
## <a name="tables"></a>TABLES  
**Ajouter une clé primaire**  
Crée une nouvelle clé primaire dans la [!INCLUDE[ssNoVersion](../../includes/ssnoversion_md.md)] ou une table SQL Azure si une table Access n’a aucune clé primaire ou un index unique.  
  
-   **Mode par défaut**: False  
  
-   **Mode optimisé**: False  
  
-   **Mode plein**: True  
  
> [!NOTE]  
> Lorsqu’il est connecté à SQL Azure, il est par défaut la valeur True.  
  
## <a name="see-also"></a>Voir aussi  
[Référence de l’Interface utilisateur &#40;SybaseToSQL&#41;](../../ssma/sybase/user-interface-reference-sybasetosql.md)  
  

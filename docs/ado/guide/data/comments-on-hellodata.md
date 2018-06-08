---
title: Commentaires sur HelloData | Documents Microsoft
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- hellodata sample application [ADO]
ms.assetid: a2831d77-7040-4b73-bbae-fe0bf78107ed
caps.latest.revision: 12
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 1e50dae422b42c365d6f72a9ee1b694c4ac9a4cc
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="comments-on-hellodata"></a>Commentaires sur HelloData
L’application HelloData parcourt les opérations de base d’une application ADO standard : mise en route, d’examiner, de modification et de mise à jour des données. Lorsque vous démarrez l’application, cliquez sur le premier bouton **obtenir des données**. Cette action exécute le **GetData** sous-routine.  
  
## <a name="getdata"></a>GetData  
 **GetData** place une chaîne de connexion valide dans une variable au niveau du module, *m_sConnStr*. Pour plus d’informations sur les chaînes de connexion, consultez [création de la chaîne de connexion](../../../ado/guide/data/creating-a-connection-string.md).  
  
 Affecter un gestionnaire d’erreurs à l’aide d’un Visual Basic **OnError** instruction. Pour plus d’informations sur la gestion des erreurs dans ADO, consultez [gestion des erreurs](../../../ado/guide/data/error-handling.md). Un nouveau **connexion** objet est créé et le **CursorLocation** est définie sur **adUseClient** , car l’exemple HelloData crée un  *jeu d’enregistrements déconnecté*. Cela signifie que, dès que les données ont été récupérées à partir de la source de données, la connexion physique avec la source de données est interrompue, mais vous pouvez continuer à utiliser avec les données mises en cache localement dans votre **Recordset** objet.  
  
 Une fois que la connexion a été ouverte, affectez une chaîne SQL à une variable (sSQL). Puis créez une instance d’un nouveau **Recordset** objet, `m_oRecordset1`. Dans la ligne suivante de code, ouvrez le **Recordset** existante **connexion**, en passant `sSQL` comme source de la **Recordset**. Vous aider à ADO à déterminer que l’instruction SQL chaîne que vous avez passé comme source pour le **Recordset** est une définition textuelle d’une commande en passant **adCmdText** dans l’argument final de la **Ouverture d’un Recordset** (méthode). Cette ligne définit aussi la **LockType** et **CursorType** associé à la **Recordset**.  
  
 La ligne suivante du code définit la **MarshalOptions** propriété égale à **adMarshalModifiedOnly**. **MarshalOptions** indique les enregistrements doivent être marshalées vers la couche intermédiaire (ou un serveur Web). Pour plus d’informations à propos du marshaling, consultez la documentation de COM. Lorsque vous utilisez **adMarshalModifiedOnly** avec un curseur côté client ([CursorLocation](../../../ado/reference/ado-api/cursorlocation-property-ado.md) = **adUseClient**), seuls les enregistrements qui ont été modifiés sur le client mis à jour dans la couche intermédiaire. Paramètre **MarshalOptions** à **adMarshalModifiedOnly** peut améliorer les performances, car moins de lignes sont marshalés.  
  
 Ensuite, déconnectez le **Recordset** en définissant son **ActiveConnection** propriété égale à **rien**. Pour plus d’informations, consultez la section « Déconnexion et reconnexion le jeu d’enregistrements » dans [mise à jour et persistance des données](../../../ado/guide/data/updating-and-persisting-data.md).  
  
 Fermez la connexion à la source de données et la destruction existants **connexion** objet. Cela libère les ressources qu’il utilise.  
  
 La dernière étape consiste à définir le **Recordset** comme le **source de données** pour la grille de données de Microsoft un contrôle sur le formulaire, vous pouvez afficher facilement les données à partir de la **Recordset** sur le formulaire.  
  
 Cliquez sur le deuxième bouton, **Examine Data**. Cette commande exécute le **ExamineData** sous-routine.  
  
## <a name="examinedata"></a>ExamineData  
 ExamineData utilise plusieurs méthodes et propriétés de la **Recordset** objet pour afficher des informations sur les données dans le **Recordset**. Il indique le nombre d’enregistrements à l’aide de la **RecordCount** propriété. Il parcourt le **Recordset** et imprime la valeur de la **AbsolutePosition** propriété dans la zone de texte sur le formulaire. Également tout en étant dans la boucle, la valeur de la **signet** propriété pour le troisième enregistrement est placée dans une variable de type variant, *vBookmark*, pour une utilisation ultérieure.  
  
 La routine revient directement à l’enregistrement de tiers à l’aide de la variable bookmark enregistrée précédemment. Les appels de routine le **WalkFields** sous-routine qui parcourt le **champs** collection de la **Recordset** et affiche les détails de chaque **champ**  dans la collection.  
  
 Enfin, **ExamineData** utilise le **filtre** propriété de la **Recordset** à l’écran pour que les enregistrements avec un **CategoryId** égal à 2. Le résultat de l’application de ce filtre est immédiatement visible dans la grille d’affichage sur le formulaire.  
  
 Pour plus d’informations sur la fonctionnalité de la **ExamineData** sous-routine, consultez [examen des données](../../../ado/guide/data/examining-data.md).  
  
 Ensuite, cliquez sur le troisième bouton, **modifier les données**. Cette action exécute le **EditData** sous-routine.  
  
## <a name="editdata"></a>EditData  
 Lorsque le code entre les **EditData** une sous-routine, le **Recordset** est toujours filtré sur **CategoryId** égal à 2, afin que seuls les éléments qui répondent aux critères de filtre sont visible. Il parcourt tout d’abord le **Recordset** et augmente le prix de chaque élément visible dans le **Recordset** de 10 pour cent. La valeur de la **prix** champ est modifié en définissant le **valeur** propriété pour ce champ est égal à un nouveau montant valid.  
  
 N’oubliez pas que le **Recordset** est déconnecté de la source de données. Les modifications qui ont été apportées dans **EditData** concernent uniquement la copie mise en cache localement des données. Pour plus d’informations, consultez [modification des données](../../../ado/guide/data/editing-data.md).  
  
 Les modifications ne sont pas effectuées sur la source de données jusqu'à ce que vous cliquez sur le quatrième bouton, **mise à jour de données**. Cette action exécute le **UpdateData** sous-routine.  
  
## <a name="updatedata"></a>UpdateData  
 UpdateData supprime d’abord le filtre a été appliqué à la **Recordset**. Le code supprime et réinitialise `m_oRecordset1` comme le **DataSource** pour Microsoft liée aux données sur le formulaire afin que la non filtrées **Recordset** apparaît dans la grille.  
  
 Le code vérifie ensuite si vous pouvez revenir en arrière le **Recordset** à l’aide de la **prend en charge** méthode avec la **adMovePrevious** argument.  
  
 La routine se déplace vers le premier enregistrement à l’aide du **MoveFirst** (méthode) et affiche les valeurs du champ d’origine et actuelle, à l’aide de la **OriginalValue** et **valeur** propriétés de la **champ** objet. Ces propriétés, avec le **UnderlyingValue** propriété (non utilisé ici), sont abordés dans [mise à jour et persistance des données](../../../ado/guide/data/updating-and-persisting-data.md).  
  
 Ensuite, une nouvelle **connexion** objet est créé et utilisé pour rétablir la connexion à la source de données. Vous vous reconnectez le **Recordset** à la source de données en définissant la nouvelle **connexion** en tant que le **ActiveConnection** pour le **Recordset**. Pour envoyer les mises à jour sur le serveur, le code appelle **UpdateBatch** sur la **Recordset**.  
  
 Si la mise à jour du lot réussit, une variable d’indicateur de module, `m_flgPriceUpdated`, est définie sur True. Cela vous rappelle ultérieurement de nettoyer toutes les modifications qui ont été apportées à la base de données.  
  
 Enfin, le code déplace vers le premier enregistrement de la **Recordset** et affiche les valeurs d’origine et actuelle. Les valeurs sont identiques après l’appel à **UpdateBatch**.  
  
 Pour plus d’informations sur la façon de mettre à jour des données, y compris ce qu’il faut faire lorsque les données sur le serveur change lors de votre **Recordset** est déconnecté, consultez [mise à jour et persistance des données](../../../ado/guide/data/updating-and-persisting-data.md).  
  
## <a name="formunload"></a>Form_Unload  
 Le **Form_Unload** sous-routine est importante pour plusieurs raisons. En premier lieu, car il s’agit d’un exemple d’application, Form_Unload nettoie les modifications qui ont été apportées à la base de données avant la fermeture de l’application. Ensuite, le code montre comment une commande peut être exécutée directement à partir d’open **connexion** objet à l’aide de la **Execute** (méthode). Enfin, il montre un exemple d’exécution d’une requête sans renvoi de ligne (une requête UPDATE) sur la source de données.

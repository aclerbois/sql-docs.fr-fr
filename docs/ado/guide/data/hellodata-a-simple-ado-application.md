---
title: 'HelloData : Une Application ADO Simple | Documents Microsoft'
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
- HelloData sample application [ADO]
- ADO, samples
ms.assetid: de4bcd56-dac2-45e6-95ab-9fd7f25878fc
caps.latest.revision: 16
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 3fbc270a27350160933019c16c3b354270beb64f
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="hellodata-a-simple-ado-application"></a>HelloData : Une Application ADO Simple
Cette application simple passe en revue chacune des quatre principales opérations ADO : mise en route, d’examiner, de modification et de mise à jour des données. Ces opérations sont effectuées sur la base de données Northwind fournie avec Microsoft® SQL Server. Pour vous concentrer sur les notions de base de l’objet ADO et éviter l’encombrement du code, la gestion des erreurs dans l’exemple est minime.  
  
### <a name="to-run-hellodata"></a>Pour exécuter HelloData  
  
1.  Créer un nouveau projet EXE Standard de Visual Basic qui fait référence à la bibliothèque ADO. Pour plus d’informations, consultez [référençant les bibliothèques ADO](../../../ado/guide/referencing-the-ado-libraries.md).  
  
2.  Créez quatre boutons de commande en haut du formulaire, en définissant le **nom** et **légende** propriétés avec les valeurs indiquées dans le tableau à la fin de cette rubrique.  
  
3.  Sous les boutons, ajoutez un **Microsoft contrôle DataGrid** (Msdatgrd.ocx). Ce fichier est inclus avec Visual Basic et se trouve dans votre répertoire \windows\system32 ou \winnt\system32. Pour ajouter le contrôle DataGrid à votre boîte à outils Visual Basic, sélectionnez **composants...**  à partir de la **projet** menu. Puis activez la case à côté « Microsoft DataGrid Control 6.0 (SP3) (OLEDB) », puis **OK**. Pour ajouter le contrôle au projet, faites glisser le contrôle de grille de données à partir de la boîte à outils vers le formulaire Visual Basic.  
  
4.  Créer un **zone de texte** sur le formulaire sous la grille et définissez ses propriétés comme indiqué dans le tableau. Le formulaire doit ressembler à la figure suivante lorsque vous avez terminé.  
  
5.  Pour finir, copiez le code répertorié dans [HelloData Code](../../../ado/guide/data/hellodata-code.md)et le coller dans la fenêtre d’éditeur de code du formulaire. Appuyez sur **F5** pour exécuter le code.  
  
> [!NOTE]
>  Dans l’exemple suivant et dans ce guide, l’id de l’utilisateur « MonID » avec un mot de passe de « 123aBc » est utilisé pour l’authentification auprès du serveur. Vous devez vous servir de ces valeurs avec les informations d’identification d’ouverture de session valide pour votre serveur. Remplacez également la valeur « MySQLServer » par le nom de votre serveur.  
  
 Pour obtenir une description détaillée du code, consultez [commentaires sur HelloData](../../../ado/guide/data/comments-on-hellodata.md).  
  
 ![Affiche Formulaire1 pour l’application HelloData VB](../../../ado/guide/data/media/hellodata.gif "HelloData")  
  
|Type de contrôle|Propriété|Valeur|  
|------------------|--------------|-----------|  
|Formulaire|Nom|Form1|  
||Hauteur|6500|  
||Largeur|6500|  
|MS DataGrid|Nom|grdDisplay1|  
|TextBox|Nom|txtDisplay1|  
||Propriété MultiLine|true|  
|Bouton de commande|Nom|cmdGetData|  
||Légende|Get Data|  
|Bouton de commande|Nom|cmdExamineData|  
||Légende|Examiner les données|  
|Bouton de commande|Nom|cmdEditData|  
||Légende|Modifier des données|  
|Bouton de commande|Nom|cmdUpdateData|  
||Légende|Données mises à jour|

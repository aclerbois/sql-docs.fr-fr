---
title: Définir les paramètres sur un rapport publié - Mode intégré SharePoint | Microsoft Docs
ms.custom: ''
ms.date: 03/07/2017
ms.prod: reporting-services
ms.prod_service: reporting-services-sharepoint, reporting-services-native
ms.component: report-design
ms.reviewer: ''
ms.suite: pro-bi
ms.technology: ''
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- SharePoint integration [Reporting Services], content management
- report parameters [Reporting Services]
ms.assetid: dec5d985-a6c1-4dd8-8a66-a848e89a2e18
caps.latest.revision: 9
author: maggiesMSFT
ms.author: maggies
manager: kfile
ms.openlocfilehash: 537dfe4f3ef8f6f73cf7bcdeda5c26a0b91265a8
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="set-parameters-on-a-published-report---sharepoint-integrated-mode"></a>Définir les paramètres sur un rapport publié - Mode intégré SharePoint
  Un rapport paramétrable est un rapport qui accepte des valeurs d'entrée afin de filtrer les données lors de l'exécution du rapport. Les paramètres sont définis lors de la création du rapport. Selon les paramètres inclus dans la définition de rapport, ceux-ci peuvent accepter une valeur unique, plusieurs valeurs ou des valeurs dynamiques, qui changent en réponse à une sélection antérieure (par exemple, lorsque vous choisissez une catégorie de produit, votre prochaine sélection peut être un produit spécifique de cette catégorie). Un paramètre peut avoir une valeur par défaut, qui sert à exécuter automatiquement une version filtrée du rapport ou qui est éventuellement remplacée par une autre valeur.  
  
 Ces propriétés peuvent être définies dans la définition du rapport ou après la publication du rapport. Même si vous pouvez modifier certaines propriétés de paramètres dans un rapport publié afin de modifier les propriétés de valeur et d'affichage, vous ne pouvez pas changer le nom du paramètre ou le type des données. Ces derniers ne peuvent être modifiés que par le créateur du rapport dans la définition du rapport.  
  
 Pour exécuter un rapport paramétré, vous devez généralement connaître les valeurs à entrer, même si un rapport peut inclure des listes déroulantes de valeurs valides à sélectionner.  
  
 Pour définir les propriétés des paramètres sur un rapport publié, vous devez disposer de l'autorisation Modifier des éléments pour le rapport. Vous pouvez modifier une partie ou l'ensemble des propriétés des paramètres d'un rapport que vous exécutez à partir d'un site SharePoint. Vous ne pouvez pas personnaliser un rapport en enregistrant une combinaison de valeurs de paramètres que vous souhaitez utiliser de façon répétée. Les valeurs par défaut que vous spécifiez sont utilisées par tous les utilisateurs qui ouvrent le rapport.  
  
 Si le rapport est incorporé dans un composant WebPart Visionneuse de rapports configuré pour afficher systématiquement ce rapport, définissez les propriétés dans le composant WebPart Visionneuse de rapports. Pour plus d’informations, consultez [Ajouter le composant WebPart Visionneuse de rapports à une page web &#40;Reporting Services en mode intégré SharePoint&#41;](../../reporting-services/report-server-sharepoint/add-the-report-viewer-web-part-to-a-web-page.md).  
  
### <a name="to-run-a-parameterized-report"></a>Pour exécuter un rapport paramétré  
  
1.  Ouvrez la bibliothèque qui contient le rapport.  
  
2.  Cliquez sur le nom du rapport. Vous devez savoir à l'avance quels rapports possèdent des paramètres. Il n'existe pas d'identificateur visuel sur le rapport pour indiquer que celui-ci est paramétré.  
  
3.  Lorsque le rapport s'ouvre, spécifiez les paramètres que vous souhaitez utiliser. La zone Paramètres peut être visible, réduite ou masquée selon la définition des propriétés dans le composant WebPart de visionneuse de rapports.  
  
    1.  Si la zone Paramètres est visible, choisissez une valeur pour chaque paramètre.  
  
    2.  Si une mince bande de couleur descend sur toute la longueur du rapport avec une flèche en son milieu, cela signifie que la zone Paramètres est réduite. Vous pouvez cliquer sur la flèche pour la développer.  
  
    3.  Si la zone Paramètres est masquée, vous ne pouvez pas spécifier de valeur.  
  
4.  Cliquez sur **Appliquer** au bas de la zone Paramètres pour exécuter le rapport.  
  
     Il arrive que la spécification d'une combinaison de valeurs ne produise pas les résultats escomptés. Le rapport doit éventuellement être modifié par son auteur si vous n'obtenez pas les informations nécessaires.  
  
5.  Cliquez sur **OK**.  
  
### <a name="to-set-parameter-properties"></a>Pour définir les propriétés de paramètres  
  
1.  Ouvrez la bibliothèque ou le dossier qui contient le rapport.  
  
2.  Pointez sur le rapport, puis cliquez sur la flèche orientée vers le bas.  
  
3.  Cliquez sur **Gérer les paramètres**. Si le rapport contient des paramètres, chacun d'eux est listé sur la page. La liste indique le nom du paramètre, le type de données, la valeur par défaut, et s'il est visible ou non dans la zone de paramètres lorsque vous ouvrez le rapport.  
  
4.  Cliquez dans la liste sur un paramètre pour le modifier.  
  
5.  Dans Valeur par défaut, entrez une valeur pour le paramètre. La valeur n'est pas validée ; par conséquent, veillez à fournir une valeur utilisable par le rapport.  
  
    1.  Sélectionnez **Utiliser la valeur spécifiée dans le rapport** pour utiliser la valeur par défaut définie au moment de la création du rapport. Si la définition de rapport ne fournit pas de valeur par défaut, cette option n'est pas disponible.  
  
    2.  Sélectionnez **Remplacer la valeur par défaut du rapport** pour spécifier une valeur de remplacement de la valeur par défaut de la définition du rapport. Pour les paramètres de rapport qui acceptent les valeurs Null, vous pouvez également cocher la case **Null** pour empêcher le filtrage en fonction de ce paramètre.  
  
    3.  Sélectionnez **Ne pas utiliser de valeur par défaut** pour que chaque utilisateur spécifie une valeur avant le traitement du rapport. Si vous sélectionnez cette option, vous devez définir des paramètres d'affichage qui invitent l'utilisateur à spécifier une valeur.  
  
     Notez que si toutes les valeurs de paramètres ont une valeur par défaut, le rapport s'exécute automatiquement à l'aide de ces valeurs lorsqu'il est ouvert par l'utilisateur. Cependant, si la zone de paramètres est affichée, les utilisateurs peuvent remplacer la valeur par défaut et réexécuter le rapport.  
  
6.  Définissez des options d'affichage pour déterminer si le paramètre est visible.  
  
    1.  Sélectionnez **Demander à l’utilisateur** pour afficher le paramètre dans la page. Vous pouvez spécifier le texte d'une invite qui s'affiche dans le champ pour fournir une brève phrase indiquant le format ou le type des données qui doivent être fournies par l'utilisateur.  
  
    2.  Choisissez **Masqué** si vous utilisez une valeur par défaut et si vous ne souhaitez pas que le paramètre soit visible dans la zone Paramètres.  
  
    3.  Choisissez **Interne** si vous utilisez une valeur par défaut et si vous ne souhaitez pas que le paramètre soit visible dans la zone Paramètres ou dans les pages d’abonnement.  
  
7.  Cliquez sur **Appliquer**.  
  
## <a name="see-also"></a> Voir aussi  
 [Article de référence sur les autorisations de site SharePoint et de listes pour les éléments de serveur de rapports](../../reporting-services/security/sharepoint-site-and-list-permission-reference-for-report-server-items.md)  
  
  

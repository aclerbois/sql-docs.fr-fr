---
title: Créer une entité (Complément MDS pour Excel)| Microsoft Docs
ms.custom: ''
ms.date: 03/01/2017
ms.prod: sql
ms.prod_service: mds
ms.component: microsoft-excel-add-in
ms.reviewer: ''
ms.suite: sql
ms.technology:
- master-data-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
ms.assetid: d354abb3-88fe-4b40-a374-f6256b84ffae
caps.latest.revision: 8
author: leolimsft
ms.author: lle
manager: craigg
ms.openlocfilehash: cb203e35c9a6b78c99f7506120aca86c7c7d5263
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="create-an-entity-mds-add-in-for-excel"></a>Créer une entité (Complément MDS pour Excel)

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Dans [!INCLUDE[ssMDSshort](../../includes/ssmdsshort-md.md)][!INCLUDE[ssMDSXLS](../../includes/ssmdsxls-md.md)], les administrateurs peuvent créer des entités pour stocker des données. Lorsque vous créez une entité, vous devez charger au moins un échantillonnage des données que vous souhaitez stocker.  
  
## <a name="prerequisites"></a>Conditions préalables requises  
 Pour effectuer cette procédure :  
  
-   Vous devez avoir l'autorisation d'accéder aux zones fonctionnelles **Administration de système** et **Explorateur** .  
  
-   Vous devez être administrateur de modèle. Pour plus d’informations, consultez [Administrators &#40;Master Data Services&#41;](../../master-data-services/administrators-master-data-services.md).  
  
-   Vous devez disposer d'un modèle existant dans lequel créer l'entité. Pour plus d’informations, consultez [Créer un modèle &#40;Master Data Services&#41;](../../master-data-services/create-a-model-master-data-services.md).  
  
-   Assurez-vous que vos données répondent aux exigences suivantes :  
  
    -   Les données doivent avoir une ligne d'en-tête.  
  
    -   Il est utile de disposer de colonnes **Nom** et **Code** . **Code** est un identificateur unique pour chaque ligne.  
  
    -   Vous devez avoir au moins une ligne de données autre que l'en-tête. Toutes les colonnes n'ont pas besoin de valeurs, mais les données doivent être représentatives des données qui figureront dans l'entité.  
  
    -   Si vous avez une colonne qui contient un identificateur unique (connu dans MDS en tant que **Code**), assurez-vous que les valeurs sont uniques. Si aucune colonne ne contient d'identificateurs, vous pouvez les générer automatiquement lorsque vous créez l'entité.  
  
    -   Assurez-vous qu'aucune cellule ne contient de formules.  
  
    -   Assurez vous qu'aucune cellule ne contient de valeurs horaires. Les valeurs de date peuvent être enregistrées dans MDS, mais pas les valeurs horaires.  
  
### <a name="to-create-an-entity-and-load-data"></a>Pour créer une entité et charger des données  
  
1.  Ouvrez ou créez une feuille de calcul Excel contenant les données que vous souhaitez charger.  
  
2.  Sélectionnez les cellules que vous souhaitez charger dans la nouvelle entité.  
  
3.  Sous l’onglet **Données de référence** , dans le groupe **Modèle de build** , cliquez sur **Créer une entité**.  
  
4.  Si vous êtes invité à vous connecter à un référentiel MDS, faites-le.  
  
5.  Dans la boîte de dialogue **Créer une entité** , conservez la plage par défaut ou modifiez-la pour l’appliquer aux données que vous souhaitez charger.  
  
6.  Ne décochez pas la case **Mes données comportent des en-têtes** .  
  
7.  Dans la liste **Modèle** , sélectionnez un modèle.  
  
8.  Dans la liste **Version** , sélectionnez une version.  
  
9. Dans la zone **Nouveau nom d’entité** , entrez un nom pour l’entité.  
  
10. Dans la liste **Code** , sélectionnez la colonne qui contient les identificateurs uniques ou générez les codes automatiquement.  
  
11. Facultatif. Dans la liste **Nom** , sélectionnez une colonne qui contient le nom de chaque membre.  
  
12. Cliquez sur **OK**. Lorsque l'entité a été correctement créée, une nouvelle ligne d'en-tête est affichée, les cellules sont mises en surbrillance et le nom de la feuille est mis à jour pour correspondre au nom de l'entité.  
  
## <a name="next-steps"></a>Next Steps  
  
-   Pour afficher les erreurs qui se sont produites, dans le groupe **Publier et valider** , cliquez sur **Afficher l’état**. Les colonnes ValidationStatus et InputStatus sont affichées. Pour plus d’informations, consultez [Validation des données &#40;Complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/validating-data-mds-add-in-for-excel.md).  
  
-   Vérifiez que les attributs ont été créés avec le type de données escompté.  
  
## <a name="see-also"></a> Voir aussi  
 [Créer un attribut basé sur un domaine &#40;complément MDS pour Excel&#41;](../../master-data-services/microsoft-excel-add-in/create-a-domain-based-attribute-mds-add-in-for-excel.md)  
  
  

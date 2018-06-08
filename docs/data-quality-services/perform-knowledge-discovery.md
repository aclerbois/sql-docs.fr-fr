---
title: Effectuer une découverte des connaissances | Microsoft Docs
ms.custom: ''
ms.date: 06/04/2013
ms.prod: sql
ms.prod_service: data-quality-services
ms.component: data-quality-services
ms.reviewer: ''
ms.suite: sql
ms.technology:
- data-quality-services
ms.tgt_pltfrm: ''
ms.topic: conceptual
f1_keywords:
- sql13.dqs.kb.kbterms.f1
- sql13.dqs.kb.viewselectcd.f1
- sql13.dqs.kb.kbanalyze.f1
- sql13.dqs.kb.kbmap.f1
ms.assetid: 34a0ea16-02e6-46ed-90bc-dede68687f63
caps.latest.revision: 38
author: douglaslMS
ms.author: douglasl
manager: craigg
ms.openlocfilehash: 203660e8685ecdd0bb747f1f13b816db296f0eb3
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="perform-knowledge-discovery"></a>Effectuer une découverte des connaissances

[!INCLUDE[appliesto-ss-xxxx-xxxx-xxx-md-winonly](../includes/appliesto-ss-xxxx-xxxx-xxx-md-winonly.md)]

  Cette rubrique décrit comment créer une base de connaissances via la découverte des connaissances. Dans le processus de découverte, [!INCLUDE[ssDQSnoversion](../includes/ssdqsnoversion-md.md)] (DQS) analyse les données dans un exemple de source de données via un processus assisté par ordinateur, et ajoute les connaissances qu'il acquiert à la base de connaissances. Ces connaissances peuvent être modifiées et améliorées dans l'étape **Gestion des valeurs de domaine** de l'activité de découverte des connaissances ou de celle de gestion des domaines.  
  
 La découverte des connaissances est un processus piloté par l'Assistant qui inclut trois étapes, chacune devant être terminée.  
  
##  <a name="BeforeYouBegin"></a> Avant de commencer  
  
###  <a name="Prerequisites"></a> Conditions préalables  
 Microsoft Excel doit être installé sur l'ordinateur de [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] si les données sources sur lesquelles vous exécutez la découverte se trouvent dans un fichier Excel. Sinon, vous ne pourrez pas sélectionner le fichier Excel à l'étape de mappage. Les fichiers créés par Microsoft Excel peuvent avoir une extension .xlsx, .xls ou .csv. Si la version 64 bits d'Excel est utilisée, seuls les fichiers Excel 2003 (.xls) sont pris en charge ; les fichiers Excel 2007 ou 2010 (.xlsx) ne sont pas pris en charge. Si vous utilisez la version 64 bits d'Excel 2007 ou 2010, enregistrez le fichier comme fichier .xls ou fichier .csv, ou installez une version 32 bits d'Excel à la place.  
  
###  <a name="Security"></a> Sécurité  
  
####  <a name="Permissions"></a> Permissions  
 Vous devez disposer du rôle dqs_kb_editor ou dqs_administrator sur la base de données DQS_MAIN pour créer une base de connaissances.  
  
##  <a name="FirstStep"></a> Première étape : Démarrer la découverte des connaissances  
  
1.  [!INCLUDE[ssDQSInitialStep](../includes/ssdqsinitialstep-md.md)] [Exécutez l’application Data Quality Client](../data-quality-services/run-the-data-quality-client-application.md).  
  
2.  Si vous souhaitez exécuter la découverte des connaissances sur une nouvelle base de connaissances, cliquez sur **Nouvelle Base de connaissances**, entrez le nom et la description, puis spécifiez des éléments à partir desquels vous créez la base de connaissances, le cas échéant. Si vous souhaitez exécuter la découverte des connaissances sur une base de connaissances existante, cliquez sur **Ouvrir la Base de connaissances**, puis sélectionnez une base de connaissances.  
  
3.  Sélectionnez **Découverte des connaissances** comme activité, puis cliquez sur **Créer** pour créer la base de connaissances ou sur **Ouvrir** pour ouvrir une base de connaissances existante.  
  
##  <a name="Mapping"></a> Étape de mappage  
  
1.  Dans le champ de **Source de données** , sélectionnez **SQL Server** (valeur par défaut) ou **Fichier Excel**.  
  
    > [!NOTE]  
    >  Dans cette page, vous créez une connexion à une source de données SQL Server ou Excel, puis effectuez le mappage entre les colonnes de la source de données et un domaine de la base de connaissances. La table Mappages affiche toutes les colonnes de la base de données source qui seront analysées afin d'ajouter des connaissances aux domaines correspondants. Les mappages sont effectuées entre les colonnes de la source de données et un domaine de la base de connaissances.  
  
2.  Si la source de données est **SQL Server**, opérez comme suit :  
  
    1.  Dans le champ **Base de données** , sélectionnez la base de données source que vous souhaitez analyser pour créer la base de connaissances. La zone de texte déroulante répertorie les bases de données qui sont disponibles. La base de données source doit être présente dans la même instance de SQL Server que [!INCLUDE[ssDQSServer](../includes/ssdqsserver-md.md)]. Sinon, elle ne s'affichera pas dans la liste déroulante.  
  
    2.  Dans le champ **Table/vue** , sélectionnez la table ou la vue que vous souhaitez analyser pour créer la base de connaissances. Cette table ou vue doit être un exemple de données, pas une base de données source complète sur laquelle vous exécutez le nettoyage ou la correspondance des données. La zone de texte déroulante répertorie les tables et vues qui sont disponibles pour la base de données sélectionnée.  
  
3.  Si la source de données est **Excel**, opérez comme suit :  
  
    1.  Cliquez sur **Parcourir** et sélectionnez le fichier Excel que vous souhaitez analyser pour créer la base de connaissances. Excel doit être installé sur l'ordinateur [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] pour sélectionner un fichier Excel. Si Excel n'est pas installé sur l'ordinateur [!INCLUDE[ssDQSClient](../includes/ssdqsclient-md.md)] , le bouton Parcourir n'est pas disponible, et vous serez informé sous cette zone de texte qu'Excel n'est pas installé.  
  
    2.  Activez la case à cocher **Utiliser la première ligne comme en-tête** si la première ligne du fichier Excel contient des données d'en-tête.  
  
4.  Dans la table **Mappages** , mappez chaque colonne source sur laquelle vous voulez procéder à la découverte des connaissances avec un domaine de la base de connaissances, comme suit :  
  
    1.  Créez un mappage en sélectionnant une colonne source dans la liste déroulante de la colonne **Colonne source** d'une ligne vide, puis sélectionnez un domaine dans la liste déroulante de la colonne **Domaine** de la même ligne, s'il existe un domaine. Si aucun domaine n'existe, cliquez sur **Créer un domaine** ou sur **Créer un domaine composite** pour créer un domaine. Pour plus d'informations, consultez [Create a Domain Rule](../data-quality-services/create-a-domain-rule.md) ou [Create a Composite Domain](../data-quality-services/create-a-composite-domain.md).  
  
    2.  Répétez l'étape précédente pour chaque mappage. Pour modifier le nombre de lignes de la table, cliquez sur **Ajouter un mappage de colonnes**, ou sélectionnez une ligne et cliquez sur **Supprimer le mappage des colonnes sélectionné**. Si vous cliquez sur **Supprimer le mappage des colonnes sélectionné** alors qu'une ligne remplie est sélectionnée, la ligne sélectionnée est supprimée même s'il existe une ligne non remplie.  
  
        > [!NOTE]  
        >  Vous pouvez mapper vos données source à un domaine DQS lors d'une activité de découverte des connaissances uniquement si le type de données source est pris en charge dans DQS et correspond au type de données du domaine DQS Pour plus d'informations sur les types de données pris en charge, consultez [Supported SQL Server and SSIS Data Types for DQS Domains](../data-quality-services/supported-sql-server-and-ssis-data-types-for-dqs-domains.md).  
  
    3.  Cliquez sur **Afficher/Sélectionner des domaines composites** pour afficher les domaines composites qui ont été définis. Si aucun domaine composite n'a été défini, le contrôle ne sera pas disponible.  
  
    4.  Cliquez sur **Aperçu de la source de données** pour afficher dans un message toutes les données de la source de données que vous avez sélectionnée dans la zone de texte **Table/vue** ou **Fichier Excel** .  
  
5.  Cliquez sur **Suivant** pour passer à la page **Découverte** de l'Assistant Découverte des connaissances. Vous pouvez également sélectionner les éléments suivants :  
  
    -   Cliquez sur **Annuler** pour mettre fin à l'activité de découverte des connaissances, auquel cas vous perdrez votre travail, et revenir à la page d'accueil de DQS.  
  
    -   Cliquez sur **Fermer** pour revenir à la page d'accueil de DQS, tout en enregistrant votre travail. La base de connaissances sera verrouillée, et l'état de la base de connaissances dans la table de bases de connaissances de l'écran **Ouvrir la base de connaissances** indiquera **Découverte - Mappage**. Après avoir cliqué sur **Fermer**, cliquez sur **Découverte des connaissances** dans l'écran **Ouvrir la base de connaissances** pour effectuer l'activité de gestion de l'arborescence du domaine, accédez à l'écran **Gestion de la base de connaissances : Gestion des termes de domaine** , cliquez sur **Terminer**, puis sur **Oui** pour publier la base de connaissances ou sur **Non** pour enregistrer le travail dans la base de connaissances et quitter.  
  
##  <a name="Discover"></a> Étape de découverte  
  
1.  Cliquez sur **Démarrer** pour analyser la source de données.  
  
    > [!NOTE]  
    >  La découverte est exécutée sur les colonnes qui ont été entrées dans la table **Mappages** de la page **Mapper** . Le domaine mappé à chaque colonne sélectionnée sera rempli avec les connaissances acquises grâce à la découverte. Si le domaine est un domaine composite, la connaissance sera ajoutée aux différents domaines dont est composé le domaine composite.  
  
2.  Tandis que le processus de découverte s'exécute, vérifiez l'état d'achèvement qui s'affiche pour chaque étape de la découverte : **Prétraitement des enregistrements**, **Exécution des règles du domaine**et **Exécution de la découverte**. Le pourcentage et l'état d'achèvement seront affichés pour chacune de ces étapes.  
  
3.  Lorsque l'analyse est terminée, vérifiez que la ligne d'état sous les statistiques d'exécution indique qu'elle s'est achevée avec succès.  
  
    > [!NOTE]  
    >  Si vous quittez l'écran avant que le fichier n'ait été téléchargée, le processus de téléchargement des fichiers prendra fin.  
  
4.  Une fois l'analyse terminée, vérifiez les statistiques dans l'onglet **Générateur de profils** pour voir l'état des données. Pour plus d'informations, consultez **Profilage des données et notifications dans DQS**.  
  
5.  Une fois l'analyse terminée, le bouton **Démarrer** se transforme en bouton **Redémarrer** . Cliquez sur **Redémarrer** pour exécuter à nouveau le processus d'analyse. Toutefois, les résultats de l'analyse précédente n'ayant pas encore été enregistrés, le fait de cliquer sur **Redémarrer** entraînera la perte de ces données précédentes. Pour continuer, cliquez sur **Oui** dans le message. Pendant l'exécution de l'analyse, ne quittez pas la page, car le processus d'analyse s'arrête alors.  
  
6.  Cliquez sur **Suivant** pour passer à la page **Gestion des valeurs du domaine** de l'Assistant de découverte des connaissances. Sur cette page, vous pouvez modifier la connaissance ajoutée aux domaines de la base de connaissances. Vous pouvez également sélectionner les éléments suivants :  
  
    -   Cliquez sur **Annuler** pour mettre fin à l'activité de découverte des connaissances, auquel cas vous perdrez votre travail, et revenir à la page d'accueil de DQS.  
  
    -   Cliquez sur **Fermer** pour revenir à la page d'accueil de DQS, tout en enregistrant votre travail. La base de connaissances sera verrouillée, et l'état de la base de connaissances dans la table de bases de connaissances de l'écran **Ouvrir la base de connaissances** indiquera **Découverte - Découvrir**. Après avoir cliqué sur **Fermer**, cliquez sur **Découverte des connaissances** dans l'écran **Ouvrir la base de connaissances** pour effectuer l'activité de gestion de l'arborescence du domaine, accédez à l'écran **Gestion de la base de connaissances : Gestion des termes de domaine** , cliquez sur **Terminer**, puis sur **Oui** pour publier la base de connaissances ou sur **Non** pour enregistrer le travail dans la base de connaissances et quitter.  
  
    -   Cliquez sur cette option pour revenir à la page **Découverte** .  
  
##  <a name="Manage"></a> Étape de gestion des résultats de découverte des données  
 Après avoir effectué l'activité de découverte des connaissances, vous pouvez modifier les valeurs comme suit :  
  
-   Ajoutez une valeur de domaine à la liste des valeurs, ou sélectionnez une valeur et supprimez-la de la liste.  
  
-   Modifiez l'état d'une valeur du domaine telle que définie par le processus de découverte DQS en correcte, erronée ou non valide.  
  
-   Entrez une valeur de remplacement pour une valeur qui est erronée ou non valide.  
  
-   Définissez deux valeurs ou plus en tant que synonymes et modifiez la valeur de début telle que définie par le processus de découverte ; la valeur de début remplacera la valeur synonyme si la propriété **Utiliser des valeurs de début** a été définie lors de la création du domaine.  
  
-   Importez les valeurs de domaine d'un fichier Excel.  
  
 La table **Valeur** affiche la connaissance ajoutée à la base de connaissances pour un seul domaine. Vous sélectionnez le domaine dans la liste des domaine du volet gauche. Les colonnes du champ sont les suivantes :  
  
-   La colonne **Valeur** affiche toutes les valeurs que le processus de découverte a ajoutées au domaine sélectionné d'un champ dans l'exemple de données. Toute valeur qui est projetée comme erreur apparaîtra comme synonyme d'une valeur désignée comme correcte.  
  
-   La colonne **Fréquence** affiche le nombre d'instances de la valeur du champ de l'exemple de base de données auquel le champ est mappé. Pour un domaine composite, seules les valeurs ayant une fréquence supérieure ou égale à 20 sont affichées. Les données de fréquence sont disponibles car le processus de découverte des connaissances dispose toujours d'une connexion à l'exemple de base de données. Les données de fréquence ne sont pas disponibles dans la table du domaine sous l'onglet Valeurs du domaine de l'écran Gestion de l'arborescence du domaine car le processus de la gestion des domaines ne dispose pas de connexion à l'exemple de base de données.  
  
-   La colonne **Type** affiche l'état de la valeur, comme déterminé par le processus de découverte. Une coche verte indique que la valeur est correcte ou corrigée ; une croix rouge indique que la valeur est erronée ; un triangle orange avec un point d'exclamation indique que la valeur n'est pas valide. Une valeur non valide ne répond pas aux exigences de données pour le domaine. Une valeur erronée peut être valide, mais n'est pas la valeur correcte pour des raisons de données.  
  
-   La colonne **Corriger vers** indique une valeur correcte dans laquelle la valeur d'origine, marquée comme erronée ou non valide, sera changée. DQS peut suggérer la valeur correcte comme résultat du processus de découverte.  
  
 Gérez les résultats de la découverte comme suit :  
  
1.  Dans le volet **Liste des domaines** à gauche, sélectionnez un domaine pour lequel définir des valeurs. Vous pouvez effectuer les opérations suivantes pour modifier les valeurs affichées.  
  
    -   Affichez les résultats souhaités dans la table, selon leur état, en sélectionnant l'état dans la liste **Filtre** .  
  
    -   Recherchez les données que vous souhaitez vérifier ou modifier en entrant une ou plusieurs lettres à rechercher dans la zone de texte Rechercher. Ces lettres apparaîtront en surbrillance chaque fois qu'elles seront présentes dans une valeur affichée.  
  
    -   Cliquez sur **Afficher seulement les nouvelles valeurs** pour restreindre les valeurs affichées dans la table uniquement aux valeurs qui ont été découvertes dans la session active, pas dans les sessions précédentes.  
  
    -   Cliquez sur le bouton **Développer tout** pour afficher toutes les valeurs d'un groupe de synonymes lorsque l'état actuel est réduit, ou sur le bouton **Réduire tout** pour masquer toutes les valeurs sauf la valeur de début d'un groupe de synonymes quand l'état actuel est développé.  
  
    -   Cliquez sur le bouton **Afficher/Masquer le panneau d'historique des modifications de valeurs du domaine** pour afficher un message d'aperçu en bas de la table de valeurs qui affiche les modifications apportées récemment à la collection des valeurs du domaine.  
  
2.  Recherchez toutes les corrections que Data Quality Services a proposées en définissant **Filtre** sur **Erreur**. Vérifiez que la valeur est en fait erronée et que la valeur de la colonne **Corriger vers** est appropriée.  
  
3.  Définissez **Filtre** sur **Toutes les valeurs** et vérifiez que l'état des valeurs est approprié. Pour modifier l'état d'une valeur, sélectionnez la valeur, puis cliquez sur le bouton de **Définir les valeurs du domaine sélectionné en tant que valeurs corrigées** (coche), le bouton **Définir les valeurs du domaine sélectionné en tant qu'erreurs** (croix) ou sur le bouton **Définir les valeurs du domaine sélectionné en tant que valeurs non valides** (triangle).  
  
4.  Pour modifier l'état d'une valeur, procédez comme suit :  
  
    1.  **Définir les valeurs du domaine sélectionné en tant que valeurs corrigées**: pour modifier l'état d'une valeur erronée ou non valide en correcte, sélectionnez la valeur, puis cliquez sur **Définir les valeurs du domaine sélectionné en tant que valeurs corrigées** (coche) à partir de la flèche vers le bas de la barre d'icônes ou de la liste déroulante Type. Si la valeur erronée ou non valide est regroupée avec une valeur correcte, supprimez cette valeur après l'opération.  
  
    2.  **Définir les valeurs du domaine sélectionné en tant qu'erreurs**: pour modifier l'état d'une valeur correcte ou non valide en erronée, sélectionnez la valeur, puis cliquez sur l'icône **Définir les valeurs du domaine sélectionné en tant qu'erreurs** (croix) à partir de la flèche vers le bas de la barre d'icônes ou de la liste déroulante Type. Vous pouvez entrer une correction dans la colonne **Corriger vers** , ou la laisser vide.  
  
    3.  **Définir les valeurs du domaine sélectionné en tant que valeurs non valides**: pour modifier l'état d'une valeur correcte ou erronée en non valide, sélectionnez la valeur, puis cliquez sur l'icône **Définir les valeurs du domaine sélectionné en tant que valeurs non valides** (triangle) à partir de la flèche vers le bas de la barre d'icônes ou de la liste déroulante Type. Vous pouvez entrer une correction dans la colonne **Corriger vers** , ou la laisser vide.  
  
    4.  **Corriger vers**: après avoir défini une valeur comme erronée ou non valide, entrez une nouvelle valeur dans la colonne **Corriger vers** . DQS ajoute une nouvelle ligne pour la valeur de remplacement, l'indique comme correcte, puis regroupe les deux valeurs. La nouvelle valeur sera affichée comme valeur de début, avec la valeur de début en gras et la valeur erronée ou non valide mise en retrait.  
  
5.  Pour indiquer les valeurs en tant que groupe de synonymes, sélectionnez plusieurs valeurs correctes, puis procédez comme suit :  
  
    -   **Définir les valeurs du domaine sélectionné en tant que synonymes**: cliquez sur cette option pour définir les valeurs du domaine sélectionné en tant que synonymes. DQS pointera sur l'une des valeurs comme valeur de début par laquelle les autres seront remplacées.  
  
        > [!NOTE]  
        >  Si vous sélectionnez deux valeurs ou plus dans un groupe et une autre valeur en dehors du groupe, puis les définissez comme synonymes, vous obtiendrez un message d'erreur. Après fermeture du message d'erreur, les valeurs seront définies correctement en tant que synonymes.  
  
    -   **Supprimer la relation entre les synonymes sélectionnés**: cliquez sur cette option pour annuler la désignation de synonyme.  
  
    -   **Définir une valeur du domaine sélectionné en tant que valeur de départ de son groupe**: modifiez la valeur de départ du groupe en sélectionnant une valeur dans le groupe qui n'est pas défini comme valeur de début, puis en cliquant sur le bouton **Définir une valeur du domaine sélectionné en tant que valeur de départ de son groupe** .  
  
6.  **Vérificateur d'orthographe**: si vous avez activé le correcteur orthographique dans la page des propriétés du domaine, recherchez toutes les valeurs qui ont un trait de soulignement ondulé rouge, indication que le vérificateur d'orthographe suggère une correction. Cliquez avec le bouton droit sur la valeur avec un trait de soulignement, puis sélectionnez une correction, le cas échéant. Le type de valeur devient (ou demeure) erroné et la correction est ajoutée à la colonne **Corriger vers** . Cliquez sur la flèche bas pour afficher les corrections proposées supplémentaires. Entrez une correction manuellement pour l'ajouter au dictionnaire du vérificateur d'orthographe et pouvoir la sélectionner comme correction. Pour plus d'informations, consultez [Use the DQS Speller](../data-quality-services/use-the-dqs-speller.md) et [Set Domain Properties](../data-quality-services/set-domain-properties.md).  
  
    > [!NOTE]  
    >  Pour utiliser le vérificateur orthographique, vous pouvez l'activer dans la page **Propriétés du domaine** ou s'il est désactivé dans la page **Propriétés du domaine** , vous pouvez cliquer sur l'icône **Activer/désactiver le vérificateur d'orthographe** dans la page **Gérer les résultats de la découverte de données** pour l'activer sur cette page.  
  
7.  **Ajouter une valeur de domaine**: ajoutez une nouvelle valeur au domaine en cliquant sur le bouton **Ajouter une valeur de domaine** pour ajouter une ligne à la fin de la table. Après avoir entré une valeur, la ligne sera replacée par ordre alphabétique.  
  
8.  **Importer les valeurs du domaine à partir d'Excel**: ajoutez les nouvelles valeurs d'une feuille de calcul Excel en cliquant sur la flèche bas de l'icône **Importer des valeurs** , puis en sélectionnant **Importer les valeurs du domaine à partir d'Excel**. Entrez le nom de fichier, sélectionnez **Utiliser la première ligne comme en-tête** le cas échéant, puis cliquez sur **OK**. Pour plus d’informations, consultez [Import Values from an Excel File into a Domain](../data-quality-services/import-values-from-an-excel-file-into-a-domain.md).  
  
9. **Importer des valeurs de projet**: ajoutez les nouvelles valeurs d'un projet de qualité des données en cliquant sur la flèche bas de l'icône **Importer des valeurs** , et en sélectionnant **Importer des valeurs de projet**. Entrez le nom de fichier, sélectionnez **Utiliser la première ligne comme en-tête** le cas échéant, puis cliquez sur **OK**. Sélectionnez le projet à partir duquel vous voulez importer des valeurs, puis cliquez sur **OK**. Les valeurs importées seront affichées. Cliquez sur **Terminer**. Pour plus d'informations, consultez Importer les valeurs de projet dans un domaine.  
  
10. **Supprimer les valeurs du domaine sélectionné**: supprimez une ou plusieurs valeurs existantes du domaine en sélectionnant les valeurs, puis en cliquant sur le bouton **Supprimer les valeurs du domaine sélectionné** . Comme une entrée DQS_NULL ne peut pas être supprimée, si vous choisissez plusieurs valeurs à supprimer et qu'une entrée DQS_NULL est l'une d'entre elles, l'opération échoue.  
  
11. Cliquez sur **Terminer** pour mettre fin à l'activité de découverte des connaissances. Un message s'affiche si vous n'avez pas examiné chacun des domaines. Cliquez sur **Oui** pour continuer l'examen ou sur **Non** pour poursuivre. Si vous cliquez sur Non, une autre fenêtre s'affiche, qui vous permet d'effectuer les opérations suivantes :  
  
    1.  **Publier**: la base de connaissances sera publiée en vue de son utilisation par l'utilisateur actuel ou par d'autres. La base de connaissances ne sera pas verrouillée, l'état de la base de connaissances (dans la table de bases de connaissances) sera défini sur Vide, et les activités de gestion de l'arborescence du domaine et de découverte des connaissances seront disponibles. La page d'accueil s'affichera à nouveau. Pour terminer le processus, cliquez sur **Oui** dans le message.  
  
    2.  **Non**: votre travail sera enregistré, la base de connaissances restera verrouillée et l'état de la base de connaissances sera défini sur En cours. Les activités de gestion de l'arborescence du domaine et de découverte des connaissances seront disponibles. La page d'accueil s'affichera à nouveau.  
  
    3.  **Annuler**: le message est fermé et vous demeurez sur la page **Gestion des valeurs du domaine** .  
  
12. Vous pouvez également cliquer sur les éléments suivants :  
  
    -   **Annuler** pour mettre fin à l'activité de découverte des connaissances, auquel cas vous perdrez votre travail, et revenir à la page d'accueil de DQS.  
  
    -   **Fermer** pour revenir à la page d'accueil de DQS, tout en enregistrant votre travail. La base de connaissances sera verrouillée et l'état de la base de connaissances dans la table de bases de connaissances de l'écran **Ouvrir la base de connaissances** indiquera **Découverte - Gestion des valeurs**.  
  
    -   Cliquez sur **Précédent** pour revenir à la page **Découverte** . Après avoir cliqué sur **Fermer**, cliquez sur **Découverte des connaissances** dans l'écran **Ouvrir la base de connaissances** pour effectuer l'activité de gestion de l'arborescence du domaine, accédez à l'écran **Gestion de la base de connaissances : Gestion des termes de domaine** , cliquez sur **Terminer**, puis sur **Oui** pour publier la base de connaissances ou sur **Non** pour enregistrer le travail dans la base de connaissances et quitter.  
  
##  <a name="FollowUp"></a> Suivi : Après l'exécution de la découverte des connaissances  
 Après avoir ajouté les connaissances à la base des connaissance dans le processus de découverte des connaissances assisté par ordinateur, vous pouvez utiliser la base de connaissances pour un projet immédiat de nettoyage ou vous pouvez exécuter la gestion des domaines avant d'effectuer le nettoyage. Pour plus d’informations sur le nettoyage des données ou la gestion des domaines, consultez [Nettoyage des données](../data-quality-services/data-cleansing.md) ou [Gestion d’un domaine](../data-quality-services/managing-a-domain.md).  
  
##  <a name="Meaning"></a> Signification des valeurs correctes, erronées et non valides  
 Chaque valeur de la table **Valeur** de la page **Valeurs du domaine** se voit affecter un paramètre **Type** égal à **Correcte**, **Erronée**ou **Non valide**. Le type de la valeur est généré initialement par l'activité de découverte des connaissances, et vous pouvez le modifier à votre convenance. Le type final, basé sur la découverte et les modifications interactives, est généré par l'activité de nettoyage. Ces valeurs ont les significations suivantes :  
  
-   **Correcte** : valeur qui appartient au domaine et ne comporte aucune erreur de syntaxe. Par exemple, « Chicago » dans le champ « Ville » est une valeur correcte.  
  
-   **Erreur** : valeur qui appartient au domaine, mais qui est incorrecte. Par exemple, « Shicago » au lieu de « Chicago » dans « Ville » est une erreur. DQS indique une valeur comme erronée s'il détecte une erreur de syntaxe et une correction associée dans le processus de découverte. Les erreurs de syntaxe incluent les fautes d'orthographe.  
  
-   **Non valide** : valeur qui n'appartient pas au domaine et qui n'a pas de correction. Par exemple, la valeur « 12345 » dans un champ « Ville » n'est pas valide. DQS indique une valeur comme non valide quand elle ne respecte pas une règle de domaine.  
  
 Vous pouvez modifier manuellement le type d'une valeur en l'une des deux autres valeurs. DQS n'applique pas la sémantique de validation et d'erreur sur les opérations manuelles. Vous pouvez écrire une correction pour une valeur valide sans modifier son état. Vous pouvez désigner une valeur comme non valide même si elle respecte une règle de domaine. Vous pouvez désigner une valeur comme erronée même si le processus de découverte n'indique pas d'erreur de syntaxe. Vous pouvez également supprimer une correction d'une valeur erronée, marquée comme correcte, sans modifier son état.  
  
 Lorsque vous effectuez un nettoyage des données interactif dans la page **Gérer et afficher les résultats** de l'activité **Nettoyage** , les valeurs non valides et erronées sont incluses dans l'onglet **Non valide** de la page **Gérer et afficher les résultats** .  
  
##  <a name="Display"></a> How to Display the Appropriate Values  
 Vous pouvez modifier l'affichage comme suit :  
  
-   **Filtre** : filtrez les résultats souhaités dans la table, selon leur état, en sélectionnant celui-ci dans la liste déroulante **Filtre** .  
  
-   **Rechercher** : recherchez les données que vous souhaitez vérifier ou modifier en entrant une ou plusieurs lettres à rechercher dans la zone de texte **Rechercher** . Ces lettres apparaîtront en surbrillance chaque fois qu'elles seront présentes dans une valeur affichée.  
  
-   Cliquez sur **Afficher seulement les nouvelles valeurs** pour restreindre les valeurs affichées dans la table uniquement aux valeurs qui ont été découvertes dans la session active, pas dans les sessions précédentes.  
  
-   Cliquez sur le bouton **Développer tout** pour afficher toutes les valeurs d'un groupe de synonymes lorsque l'état actuel est réduit.  
  
-   Cliquez sur le bouton **Réduire tout** pour masquer toutes les valeurs sauf la valeur de début d'un groupe de synonymes lorsque l'état actuel est développé.  
  
-   Cliquez sur le bouton **Afficher/Masquer le panneau d'historique des modifications de valeurs du domaine** pour afficher un message d'aperçu en bas de la table de valeurs qui affiche les modifications apportées récemment à la collection des valeurs du domaine.  
  
##  <a name="Profiler"></a> Statistiques du Générateur de profils  
 L'onglet Générateur de profils fournit des statistiques qui indiquent la qualité des données source. Ces statistiques ne mesurent pas la qualité de la base de connaissances. Le profilage dans la découverte des connaissances fournit des informations sur l'achèvement et l'unicité. Le profilage dans la découverte des connaissances ne mesure pas la précision. Le profilage de la gestion des connaissances vous aide à évaluer jusqu'à quel point auquel la source de données est valide pour créer et améliorer la connaissance dans une base de connaissances.  
  
 L'onglet **Générateur de profils** fournit les statistiques suivantes pour le processus de découverte, par champ et par domaine :  
  
-   **Enregistrements**: nombre d'enregistrements découverts dans l'exemple de données  
  
-   **Valeurs totales**: nombre de valeurs totales trouvées pour chaque champ et chaque total  
  
-   **Nouvelles valeurs**: nombre de valeurs totales pour chaque champ et tous les champs mappés nouveaux depuis le dernier processus de découverte, et leur pourcentage de valeurs totales  
  
-   **Valeurs uniques**: nombre de valeurs totales pour chaque champ et tous les champs mappés uniques, et leur pourcentage de valeurs totales  
  
-   **Nouvelles valeurs uniques**: nombre de valeurs uniques pour chaque champ et tous les champs mappés nouveaux depuis le dernier processus de découverte, et leur pourcentage de valeurs totales  
  
-   **Valide dans les valeurs du domaine**: nombre de valeurs totales pour chaque champ et tous les champs mappés valides, et leur pourcentage de valeurs totales  
  
 Les statistiques de champ sont les suivantes :  
  
-   **Champ**: nom du champ dans la base de données source  
  
-   **Domaine**: nom du domaine mappé au champ  
  
-   **Nouveau**: nombre de nouvelles valeurs et pourcentage de valeurs nouvelles comparées aux valeurs existantes dans le champ  
  
-   **Unique**: nombre d'enregistrements uniques dans le champ et leur pourcentage par rapport au total  
  
-   **Valide dans le domaine**: nombre de valeurs de domaine valides et leur pourcentage du total  
  
-   **Exhaustivité**: exhaustivité de chaque champ source qui est mappé pour l'exercice correspondant  
  
 Le profilage dans la découverte des connaissances fournit des informations sur l'exhaustivité. Si le profilage vous informe qu'un champ est relativement incomplet, vous pouvez le supprimer de la base de connaissances d'un projet de qualité des données. Le profilage peut ne pas fournir des statistiques d'exhaustivité fiables pour les domaines composites. Si vous avez besoin des statistiques d'exhaustivité, utilisez des domaines simples plutôt que des domaines composites. Si vous souhaitez utiliser des domaines composites, vous pouvez créer une base de connaissances avec des domaines simples pour le profilage, afin de déterminer l'exhaustivité, et créer un autre domaine avec un domaine composite pour le processus de nettoyage. Par exemple, le profilage peut afficher une exhaustivité de 95 % pour les enregistrements d'adresse à l'aide d'un domaine composite, mais il peut y avoir un niveau bien supérieur de non-exhaustivité pour l'une des colonnes, par exemple, une colonne de code postal. Dans cet exemple, vous pouvez mesurer l'exhaustivité de la colonne de code postal avec un domaine unique. Le profilage fournira probablement des statistiques de précision fiables pour les domaines composites, car vous pouvez mesurer la précision de plusieurs colonnes ensemble. Comme la valeur de ces données se trouve dans l'agrégation composite, vous pouvez mesurer la précision avec un domaine composite.  
  
 Les statistiques sont affichées dans l'onglet Générateur de profils pendant les phases suivantes :  
  
-   Pendant la phase **Prétraitement des enregistrements** , DQS charge les données et les indexe. L'opération est effectuée enregistrement par enregistrement ou lot par lot, de telle sorte que la progression peut être affichée par enregistrements. Au moment de l'exécution de cette étape, la plupart des données de profilage peuvent être générées, à l'exception des valeurs **Valide dans le domaine** .  
  
-   Pendant la phase **Exécution des règles du domaine** , la colonne **Valide dans le domaine** est remplie tandis que les règles de domaine sont toutes exécutées en tant qu'unité atomique de chaque valeur de domaine.  
  
-   Pendant la phase **Exécution de la découverte** , aucune nouvelle donnée n'est mise à jour dans l'onglet Générateur de profils. Toutes les erreurs de syntaxe rencontrées s’affichent à l’étape suivante de l’Assistant, la phase **Gérer les valeurs du domaine**.  
  
 Pour l'activité de découverte des connaissances, les conditions suivantes génèrent des notifications :  
  
-   Il n'y a aucune nouvelle valeur dans un champ ; il est recommandé que vous l'éliminiez du mappage.  
  
-   Il y a peu de nouvelles valeurs dans un champ ; vous pouvez l'éliminer du mappage.  
  
-   Un champ est vide ; il est recommandé que vous l'éliminiez du mappage.  
  
-   Le score d'achèvement du champ est très faible ; vous pouvez l'éliminer du mappage.  
  
-   Toutes les valeurs d'un champ sont non valides ; vous devez vérifier le mappage et la pertinence des règles de domaine par rapport au contenu du champ.  
  
-   Il existe un faible niveau de valeurs valides dans le champ ; vous devez vérifier le mappage et la pertinence des règles de domaine par rapport au contenu du champ.  
  
 Pour plus d'informations sur le profilage, consultez [Profilage des données et notifications dans DQS](../data-quality-services/data-profiling-and-notifications-in-dqs.md).  
  
  

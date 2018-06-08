---
title: Mise à jour - Documentation des bases de données relationnelles | Microsoft Docs
description: Affichage d’extraits de contenu mis à jour de la documentation récemment modifiée des bases de données relationnelles.
manager: craigg
author: MightyPen
ms.author: genemi
ms.topic: article
ms.custom: UpdArt.exe
ms.suite: sql
ms.technology: release-landing
ms.prod: sql
ms.prod_service: sql-non-specified
ms.component: relational-databases
ms.date: 04/28/2018
ms.openlocfilehash: a885befe2411a76dc8c68bf2a7b543a838a52877
ms.sourcegitcommit: 2ddc0bfb3ce2f2b160e3638f1c2c237a898263f4
ms.translationtype: HT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="new-and-recently-updated-relational-databases-docs"></a>Contenu nouveau et récemment mis à jour : documentation des bases de données relationnelles



Presque tous les jours, Microsoft met à jour certains de ses articles sur son site web de documentation [Docs.Microsoft.com](http://docs.microsoft.com/). Cet article contient des extraits d’articles récemment mis à jour. Des liens vers de nouveaux articles peuvent également être répertoriés.

Cet article est généré par un programme qui est réexécuté régulièrement. Un extrait peut parfois apparaître avec une mise en forme imparfaite ou au format Markdown de l’article source. Les images ne sont jamais affichées ici.

Les mises à jour récentes sont signalées pour la plage de dates et le sujet suivants :



- *Plage de dates des mises à jour :* &nbsp; **03-02-2018** &nbsp; au &nbsp; **28-04-2018**
- *Domaine :* &nbsp; **bases de données relationnelles**.




&nbsp;

## <a name="new-articles-created-recently"></a>Nouveaux articles créés récemment

Les liens suivants renvoient aux nouveaux articles ajoutés récemment.


1. [Jointures (SQL Server)](performance/joins.md)
2. [Sous-requêtes (SQL Server)](performance/subqueries.md)
3. [Configurer la base de données de distribution de réplication dans un groupe de disponibilité AlwaysOn](replication/configure-distribution-availability-group.md)
4. [Découverte et classification des données SQL](security/sql-data-discovery-and-classification.md)
5. [Guide du verrouillage des transactions et du contrôle de version de ligne](sql-server-transaction-locking-and-row-versioning-guide.md)
6. [sys.dm_os_job_object (Azure SQL Database)](system-dynamic-management-views/sys-dm-os-job-object-transact-sql.md)
7. [Procédures stockées système Filestream et FileTable (Transact-SQL)](system-stored-procedures/filestream-and-filetable-system-stored-procedures.md)



&nbsp;

## <a name="updated-articles-with-excerpts"></a>Articles mis à jour avec des extraits

Cette section affiche les extraits des mises à jour collectés dans des articles qui ont récemment fait l’objet d’une mise à jour importante.

Les extraits affichés ici apparaissent séparés de leur contexte sémantique propre. Un extrait est parfois séparé de la syntaxe Markdown importante qui l’entoure dans l’article. Ces extraits sont donc donnés à titre indicatif uniquement. Les extraits vous permettent seulement de savoir si les articles correspondants vont vous intéresser et si oui, de cliquer dessus pour les consulter.

Pour cela et pour d’autres raisons, ne copiez pas le code de ces extraits et ne prenez pas à la lettre les extraits de texte. Consultez plutôt l’article.





&nbsp;

<a name="compactupdatedlist"/>

### <a name="compact-list-of-articles-updated-recently"></a>Liste compacte d’articles mis à jour récemment

Cette liste compacte fournit des liens vers tous les articles mis à jour qui sont répertoriés dans la section des extraits.

1. [Utiliser un fichier de format pour ignorer une colonne de table (SQL Server)](#TitleNum_1)
2. [Données JSON dans SQL Server](#TitleNum_2)
3. [Guide d’architecture de traitement des requêtes](#TitleNum_3)
4. [Tutoriel : Préparer SQL Server pour la réplication : serveur de publication, base de données du serveur de distribution, Abonné](#TitleNum_4)
5. [Tutoriel : Configurer la réplication entre deux serveurs intégralement connectés (réplication transactionnelle)](#TitleNum_5)
6. [Tutoriel : Configurer la réplication entre un serveur et des clients mobiles (réplication de fusion)](#TitleNum_6)
7. [Exécuter une requête avec une recherche en texte intégral](#TitleNum_7)
8. [Transparent Data Encryption avec prise en charge de BYOK pour Azure SQL Database et Data Warehouse](#TitleNum_8)
9. [PowerShell et CLI : Activer TDE à l’aide de votre propre clé Azure Key Vault](#TitleNum_9)
10. [À propos de la capture des changements de données (SQL Server)](#TitleNum_10)




&nbsp;

&nbsp;

<a name="TitleNum_1"/>

### <a name="1-nbsp-use-a-format-file-to-skip-a-table-column-sql-serverimport-exportuse-a-format-file-to-skip-a-table-column-sql-servermd"></a>1. &nbsp; [Utiliser un fichier de format pour ignorer une colonne de table (SQL Server)](import-export/use-a-format-file-to-skip-a-table-column-sql-server.md)

*Mise à jour : 13-04-2018* &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; ([Suivant](#TitleNum_2))

<!-- Source markdown line 221.  ms.author= "douglasl".  -->

&nbsp;


<!-- git diff --ignore-all-space --unified=0 167916d79c5de1e7f13990cb7acc41ceb541b9a7 cb92eb201292294e3397879c98f353fba45f1c1c  (PR=0  ,  Filename=use-a-format-file-to-skip-a-table-column-sql-server.md  ,  Dirpath=docs\relational-databases\import-export\  ,  MergeCommitSha40=f70f24bff1677b33c661abd13726f491ce32b305) -->



**Utilisation de OPENROWSET(BULK...)**


Pour utiliser un fichier de format XML et ignorer une colonne de table à l’aide de `OPENROWSET(BULK...)`, vous devez fournir une liste explicite des colonnes dans la liste de sélection ainsi que dans la table cible, comme suit :

```
    INSERT ...<column_list> SELECT <column_list> FROM OPENROWSET(BULK...)
```

L'exemple suivant utilise le fournisseur d'ensembles de lignes en bloc `OPENROWSET` et le fichier de format `myTestSkipCol2.xml` . Dans cet exemple, le fichier de données `myTestSkipCol2.dat` est importé en bloc dans la table `myTestSkipCol` . L'instruction contient une liste explicite des colonnes dans la liste de sélection et aussi dans la table cible.

Dans SSMS, exécutez le code suivant. Mettez à jour les chemins de système de fichiers pour l’emplacement des exemples de fichiers sur votre ordinateur.

```
USE WideWorldImporters;
GO
INSERT INTO myTestSkipCol
  (Col1,Col3)
    SELECT Col1,Col3
      FROM  OPENROWSET(BULK  'C:\myTestSkipCol2.Dat',
      FORMATFILE='C:\myTestSkipCol2.Xml'
       ) as t1 ;
GO
```



&nbsp;

&nbsp;

---

<a name="TitleNum_2"/>

### <a name="2-nbsp-json-data-in-sql-serverjsonjson-data-sql-servermd"></a>2. &nbsp; [Données JSON dans SQL Server](json/json-data-sql-server.md)

*Mise à jour : 13-04-2018* &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  ([Précédent](#TitleNum_1) | [Suivant](#TitleNum_3))

<!-- Source markdown line 145.  ms.author= "jovanpop".  -->

&nbsp;


<!-- git diff --ignore-all-space --unified=0 19e276637a463b412f2c29a84f9fb7d0b0f5fcc5 e2f2e8b4732779b3f24561cc0c4da3a958f4edbb  (PR=0  ,  Filename=json-data-sql-server.md  ,  Dirpath=docs\relational-databases\json\  ,  MergeCommitSha40=f70f24bff1677b33c661abd13726f491ce32b305) -->



Certains documents JSON contiennent des sous-éléments et des données hiérarchiques qui ne peuvent pas être directement mappés à des colonnes relationnelles standard. Dans ce cas, vous pouvez aplatir la hiérarchie JSON en joignant l’entité parente à des sous-tableaux.

Dans l’exemple suivant, le deuxième objet dans le tableau possède un sous-tableau qui représente les compétences (skills) de la personne. Chaque sous-objet peut être analysé à l’aide d’un appel de fonction `OPENJSON` supplémentaire :

```
DECLARE @json NVARCHAR(MAX)
SET @json =
N'[
       { "id" : 2,"info": { "name": "John", "surname": "Smith" }, "age": 25 },
       { "id" : 5,"info": { "name": "Jane", "surname": "Smith", "skills": ["SQL", "C#", "Azure"] }, "dob": "2005-11-04T12:00:00" }
 ]'

SELECT *
FROM OPENJSON(@json)
  WITH (id int 'strict $.id',
        firstName nvarchar(50) '$.info.name', lastName nvarchar(50) '$.info.surname',
        age int, dateOfBirth datetime2 '$.dob',
    skills nvarchar(max) '$.skills' as json)
    outer apply openjson( a.skills )
                     with ( skill nvarchar(8) '$' ) as b
```
Le tableau **skills** est retourné dans la première fonction `OPENJSON` comme fragment de texte JSON d’origine et il est passé à une autre fonction `OPENJSON` à l’aide d’un opérateur `APPLY`. La seconde fonction `OPENJSON` analyse le tableau JSON et retourne les valeurs de chaîne sous la forme d’un ensemble de lignes de colonnes unique, lequel est joint au résultat de la première fonction `OPENJSON`.
Le résultat de cette requête est illustré dans le tableau suivant :

**Résultats**

|id|firstName|lastName|age|dateOfBirth|skill|
|--------|---------------|--------------|---------|-----------------|----------|
|2|John|Smith|25|||
|5|Jane|Smith||2005-11-04T12:00:00|SQL|
|5|Jane|Smith||2005-11-04T12:00:00|C#|
|5|Jane|Smith||2005-11-04T12:00:00|Azure|

`OUTER APPLY OPENJSON` joint l’entité de premier niveau au sous-tableau et retourne le jeu de résultats aplati. En raison de la jointure (JOIN), la deuxième ligne est répétée pour chaque compétence (skill).




&nbsp;

&nbsp;

---

<a name="TitleNum_3"/>

### <a name="3-nbsp-query-processing-architecture-guidequery-processing-architecture-guidemd"></a>3. &nbsp; [Guide d’architecture de traitement des requêtes](query-processing-architecture-guide.md)

*Mise à jour : 13-04-2018* &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  ([Précédent](#TitleNum_2) | [Suivant](#TitleNum_4))

<!-- Source markdown line 34.  ms.author= "jroth".  -->

&nbsp;


<!-- git diff --ignore-all-space --unified=0 96d91b39acdb2f32aaff323e374e92d6f229d241 2c1d2f8585632ada174388399782dc3ed2721dba  (PR=0  ,  Filename=query-processing-architecture-guide.md  ,  Dirpath=docs\relational-databases\  ,  MergeCommitSha40=f70f24bff1677b33c661abd13726f491ce32b305) -->



**Priorité des opérateurs logiques**


Quand une instruction contient plusieurs opérateurs logiques, `NOT` est traité en premier, ensuite `AND` et enfin `OR`. Les opérateurs arithmétiques, et au niveau du bit, sont traités avant les opérateurs logiques. Pour plus d’informations, consultez [Priorité des opérateurs].

Dans l'exemple suivant, la condition de couleur est associée au modèle de produit 21 et non au modèle de produit 20, car `AND` est prioritaire sur `OR`.

```
SELECT ProductID, ProductModelID
FROM Production.Product
WHERE ProductModelID = 20 OR ProductModelID = 21
  AND Color = 'Red';
GO
```

Vous pouvez modifier la signification de la requête en forçant le traitement de `OR` en premier lieu à l'aide de parenthèses. La requête suivante recherche uniquement les produits rouges dans les modèles 20 et 21.

```
SELECT ProductID, ProductModelID
FROM Production.Product
WHERE (ProductModelID = 20 OR ProductModelID = 21)
  AND Color = 'Red';
GO
```

L'utilisation de parenthèses, même quand elles ne sont pas nécessaires, peut améliorer la lisibilité des requêtes et limiter les risques d'erreurs dues à la priorité des opérateurs. L'utilisation de parenthèses ne diminue pas les performances du système. L'exemple suivant est plus lisible que le premier, bien qu'il soit identique sur le plan de la syntaxe.

```
SELECT ProductID, ProductModelID
FROM Production.Product
WHERE ProductModelID = 20 OR (ProductModelID = 21
  AND Color = 'Red');
GO
```




&nbsp;

&nbsp;

---

<a name="TitleNum_4"/>

### <a name="4-nbsp-tutorial-prepare-sql-server-for-replication---publisher-distributor-subscriberreplicationtutorial-preparing-the-server-for-replicationmd"></a>4. &nbsp; [Tutoriel : Préparer SQL Server pour la réplication : serveur de publication, serveur de distribution, abonné](replication/tutorial-preparing-the-server-for-replication.md)

*Mise à jour : 13-04-2018* &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  ([Précédent](#TitleNum_3) | [Suivant](#TitleNum_5))

<!-- Source markdown line 56.  ms.author= "mathoma".  -->

&nbsp;


<!-- git diff --ignore-all-space --unified=0 6e5caedacff193ce79bdd98708ae1b9dc91f0a8f 9f7af4d3f8b1cffd048db2a5b29fc9e6013f5ed2  (PR=0  ,  Filename=tutorial-preparing-the-server-for-replication.md  ,  Dirpath=docs\relational-databases\replication\  ,  MergeCommitSha40=f70f24bff1677b33c661abd13726f491ce32b305) -->



- Installez [SQL Server Management Studio](https://docs.microsoft.com/sql/ssms/download-sql-server-management-studio-ssms).
- Installez [SQL Server 2017 Developer Edition](https://www.microsoft.com/sql-server/sql-server-downloads).
- Téléchargez un [exemple de base de données AdventureWorks](https://github.com/Microsoft/sql-server-samples/releases). Pour obtenir des instructions sur la restauration d’une base de données dans SSMS, consultez [Restauration d’une base de données](https://docs.microsoft.com/sql/relational-databases/backup-restore/restore-a-database-backup-using-ssms).

>[!NOTE]
> - La réplication n’est pas prise en charge sur les serveurs SQL Server qui sont séparés de plus de deux versions. Pour plus d’informations, consultez [Supported SQL Versions in Repl Topology](https://blogs.msdn.microsoft.com/repltalk/2016/08/12/suppported-sql-server-versions-in-replication-topology/) (Versions de SQL prises en charge dans la topologie de réplication).
> - Dans *{Included-Content-Goes-Here}*, vous devez vous connecter au serveur de publication et à l’abonné à l’aide d’un identifiant de connexion membre du rôle serveur fixe **sysadmin**. Pour plus d’informations sur le rôle sysadmin, consultez [Rôles de niveau serveur](https://docs.microsoft.com/sql/relational-databases/security/authentication-access/server-level-roles).


**Durée estimée pour effectuer ce tutoriel : 30 minutes**

**Créer des comptes Windows pour la réplication**

Dans cette section, vous allez créer des comptes Windows pour exécuter les agents de réplication. Vous allez créer un compte Windows distinct sur le serveur local pour les agents suivants :

|Agent|Emplacement|Nom du compte|
|---------|------------|----------------|
|Agent d'instantané|Serveur de publication|<*nom_ordinateur*>\repl_snapshot|



&nbsp;

&nbsp;

---

<a name="TitleNum_5"/>

### <a name="5-nbsp-tutorial-configure-replication-between-two-fully-connected-servers-transactionalreplicationtutorial-replicating-data-between-continuously-connected-serversmd"></a>5. &nbsp; [Tutoriel : Configurer la réplication entre deux serveurs intégralement connectés (réplication transactionnelle)](replication/tutorial-replicating-data-between-continuously-connected-servers.md)

*Mise à jour : 13-04-2018* &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  ([Précédent](#TitleNum_4) | [Suivant](#TitleNum_6))

<!-- Source markdown line 162.  ms.author= "mathoma".  -->

&nbsp;


<!-- git diff --ignore-all-space --unified=0 0d74f984d0ffc01cce0376837e6d94df3c5654d7 4ecf4d724286130927dd43687d6845059af6f9b7  (PR=0  ,  Filename=tutorial-replicating-data-between-continuously-connected-servers.md  ,  Dirpath=docs\relational-databases\replication\  ,  MergeCommitSha40=f70f24bff1677b33c661abd13726f491ce32b305) -->



**Créer un abonnement à la publication transactionnelle**

Dans cette section, vous allez ajouter un Abonné à la publication qui a été créée. Ce tutoriel utilise un Abonné distant (NODE2\SQL2016), mais vous pouvez également ajouter un abonnement localement au serveur de publication.

**Pour créer l’abonnement**


1.  Connectez-vous au serveur de publication dans *{Included-Content-Goes-Here}*, développez le nœud du serveur, puis le dossier **Réplication**.

2.  Dans le dossier **Publications locales**, cliquez avec le bouton droit sur la publication **AdvWorksProductTrans**, puis sélectionnez **Nouveaux abonnements**.  L’Assistant Nouvel abonnement démarre :

    Nouvel abonnement

3.  Dans la page Publication, sélectionnez **AdvWorksProductTrans**, puis sélectionnez **Suivant** :

    Sélectionner le serveur de publication Trans

4.  Dans la page Emplacement de l’Agent de distribution, sélectionnez **Exécuter tous les agents sur le serveur de distribution**, puis sélectionnez **Suivant**.  Pour plus d’informations sur les abonnements par extraction et par émission de données, consultez [S’abonner à des publications](https://docs.microsoft.com/sql/relational-databases/replication/subscribe-to-publications) :

    Exécuter des agents sur le serveur de distribution

5.  Dans la page Abonnés, si le nom de l’instance de l’Abonné n’est pas affiché, sélectionnez **Ajouter un Abonné**, puis sélectionnez **Ajouter un Abonné SQL Server** dans la liste déroulante. Cette opération ouvre la boîte de dialogue **Se connecter au serveur**. Entrez le nom de l’instance de l’Abonné, puis sélectionnez **Se connecter**.



&nbsp;

&nbsp;

---

<a name="TitleNum_6"/>

### <a name="6-nbsp-tutorial-configure-replication-between-a-server-and-mobile-clients-mergereplicationtutorial-replicating-data-with-mobile-clientsmd"></a>6. &nbsp; [Tutoriel : Configurer la réplication entre un serveur et des clients mobiles (réplication de fusion)](replication/tutorial-replicating-data-with-mobile-clients.md)

*Mise à jour : 13-04-2018* &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  ([Précédent](#TitleNum_5) | [Suivant](#TitleNum_7))

<!-- Source markdown line 93.  ms.author= "mathoma".  -->

&nbsp;


<!-- git diff --ignore-all-space --unified=0 0eed78dfe83c88358c030539a2b25d11ef5ec2d3 79b2a3f32c940fede94b11ad2a3ef8a00b911a39  (PR=0  ,  Filename=tutorial-replicating-data-with-mobile-clients.md  ,  Dirpath=docs\relational-databases\replication\  ,  MergeCommitSha40=f70f24bff1677b33c661abd13726f491ce32b305) -->



La table Employee contient une colonne (OrganizationNode) avec le type de données hierarchyid, qui est uniquement pris en charge pour la réplication dans SQL 2017. Si vous utilisez une version antérieure à SQL 2017, vous verrez un message au bas de l’écran vous informant que l’utilisation de cette colonne dans une réplication bidirectionnelle peut entraîner une perte de données. Pour les besoins de ce tutoriel, vous pouvez ignorer ce message. Toutefois, vous ne devez pas répliquer ce type de données dans un environnement de production, sauf si vous utilisez la build prise en charge. Pour plus d’informations sur la réplication du type de données hierarchyid, consultez [Utilisation de colonnes hierarchyid dans les tables répliquées](https://docs.microsoft.com/sql/t-sql/data-types/hierarchyid-data-type-method-reference#using-hierarchyid-columns-in-replicated-tables).


-  Dans la page Filtrer les lignes de la table, sélectionnez **Ajouter**, puis **Ajouter un filtre**.

-  Dans la boîte de dialogue **Ajouter un filtre**, sélectionnez **Employee (HumanResources)** sous **Select the table to filter** (Sélectionnez la table à filtrer). Sélectionnez la colonne **LoginID**, sélectionnez la flèche droite pour ajouter la colonne à la clause WHERE de la requête de filtre, puis modifiez la clause WHERE comme illustré ci-après :

    ```
    WHERE [LoginID] = HOST_NAME()
    ```

    A. Sélectionnez **Une ligne de cette table ira à un seul abonnement**, puis **OK** :

    Ajouter un filtre



- Dans la page **Filtrer les lignes de la table**, sélectionnez **Employee (HumanResources)**. Ensuite, sélectionnez **Ajouter**, puis **Ajouter une jointure pour étendre le filtre sélectionné**.

    A. Dans la boîte de dialogue **Ajouter une jointure**, sélectionnez **Sales.SalesOrderHeader** sous **Table jointe**. Sélectionnez **Write the join statement manually** (Écrire l’instruction de jointure manuellement), puis finalisez l’instruction de jointure de la façon suivante :



&nbsp;

&nbsp;

---

<a name="TitleNum_7"/>

### <a name="7-nbsp-query-with-full-text-searchsearchquery-with-full-text-searchmd"></a>7. &nbsp; [Exécuter une requête avec une recherche en texte intégral](search/query-with-full-text-search.md)

*Mise à jour : 13-04-2018* &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  ([Précédent](#TitleNum_6) | [Suivant](#TitleNum_8))

<!-- Source markdown line 247.  ms.author= "douglasl".  -->

&nbsp;


<!-- git diff --ignore-all-space --unified=0 5ec67b56aa0a6eadadbcfa8b73b6726e75eca2bb 4eb108b202d3dd035a312bac7872cf02bcf31cfa  (PR=0  ,  Filename=query-with-full-text-search.md  ,  Dirpath=docs\relational-databases\search\  ,  MergeCommitSha40=f70f24bff1677b33c661abd13726f491ce32b305) -->



**Plus d’informations sur les recherches de termes de génération**


Les *formes fléchies* correspondent aux différents temps et conjugaisons d’un verbe et au pluriel et au singulier d’un nom.

Par exemple, rechercher les formes fléchies du mot « drive ». Si plusieurs lignes de la table comportent les mots « drive », « drives », « drove », « driving » et « driven », tous ces termes apparaissent dans le jeu de résultats, dans la mesure où chacun d’entre eux peut être généré à partir du mot « drive ».

[FREETEXT] et [FREETEXTTABLE] recherchent par défaut des termes fléchis de tous les mots spécifiés. [CONTAINS] et [CONTAINSTABLE] prennent en charge un argument `INFLECTIONAL` facultatif.

**Rechercher les synonymes d’un mot spécifique**


Un *dictionnaire des synonymes* définit des synonymes spécifiés par l’utilisateur pour les termes. Pour plus d’informations sur les fichiers de dictionnaire des synonymes, consultez [Configurer et gérer les fichiers de dictionnaire des synonymes pour la recherche en texte intégral].

Par exemple, si une entrée « {car, automobile, truck, van} » est ajoutée à un dictionnaire des synonymes, vous pouvez rechercher la forme du dictionnaire des synonymes pour le mot « car ». Toutes les lignes de la table interrogée qui contiennent les mots « automobile », « truck », « van » ou « car » apparaissent dans le jeu de résultats, car chacun de ces mots appartient au jeu d’expansion des synonymes contenant le mot « car ».

[FREETEXT] et [FREETEXTTABLE] utilisent le dictionnaire des synonymes par défaut. [CONTAINS] et [CONTAINSTABLE] prennent en charge un argument `THESAURUS` facultatif.



&nbsp;

&nbsp;

---

<a name="TitleNum_8"/>

### <a name="8-nbsp-transparent-data-encryption-with-bring-your-own-key-support-for-azure-sql-database-and-data-warehousesecurityencryptiontransparent-data-encryption-byok-azure-sqlmd"></a>8. &nbsp; [Transparent Data Encryption avec prise en charge de BYOK pour Azure SQL Database et Data Warehouse](security/encryption/transparent-data-encryption-byok-azure-sql.md)

*Mise à jour : 24-04-2018* &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  ([Précédent](#TitleNum_7) | [Suivant](#TitleNum_9))

<!-- Source markdown line 110.  ms.author= "aliceku".  -->

&nbsp;


<!-- git diff --ignore-all-space --unified=0 9527658848d430bf0148be84474a75b232cbd112 70ed2a129c580962384f808e8526673957f00d2c  (PR=5662  ,  Filename=transparent-data-encryption-byok-azure-sql.md  ,  Dirpath=docs\relational-databases\security\encryption\  ,  MergeCommitSha40=91a9c812739a1c9a6ec9e7b8cda71ee1f5adae3d) -->



**Guide pratique pour configurer la géoreprise d’activité avec Azure Key Vault**


Afin de maintenir une haute disponibilité des protecteurs TDE pour les bases de données chiffrées, vous devez configurer des coffres de clés Azure Key Vault redondants basés sur les groupes de basculement SQL Database existants ou nouveaux, ou les instances de la géoréplication active.  Chaque serveur géorépliqué nécessite un coffre de clés distinct, qui doit être colocalisé avec le serveur dans la même région Azure. Si une base de données primaire devient inaccessible en raison d’une panne dans une région et qu’un basculement est déclenché, la base de données secondaire peut prendre le relais avec le coffre de clés secondaire.

Pour des bases de données Azure SQL géorépliquées, Azure Key Vault doit être configuré de la manière suivante :
- Une base de données primaire avec un coffre de clés dans une région et une base de données secondaire avec un coffre de clés dans l’autre région.
- Au moins une base de données secondaire (jusqu’à quatre sont prises en charge).
- Le chaînage entre bases de données secondaires n’est pas pris en charge.

La section suivante explique en détail les étapes d’installation et de configuration.

**Étapes de configuration d’Azure Key Vault**


- Installer [PowerShell](https://docs.microsoft.com/powershell/azure/install-azurerm-ps?view=azurermps-5.6.0)
- Créez deux coffres de clés Azure dans deux régions différentes en utilisant [PowerShell pour activer la propriété « soft-delete »](https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-powershell) dans les coffres de clés (cette option n’est pas encore disponible dans le portail Azure Key Vault, mais elle est exigée par SQL).
- Les coffres de clés Azure doivent se trouver dans les deux régions disponibles de la zone géographique Azure pour que la sauvegarde et la restauration des clés fonctionnent.  Si vous avez besoin que les coffres de clés se trouvent dans des zones géographiques différentes pour répondre aux exigences de la reprise d’activité géographique SQL, suivez le [processus BYOK](https://docs.microsoft.com/azure/key-vault/key-vault-hsm-protected-keys) qui permet d’importer des clés à partir d’un HSM local.



&nbsp;

&nbsp;

---

<a name="TitleNum_9"/>

### <a name="9-nbsp-powershell-and-cli-enable-transparent-data-encryption-using-your-own-key-from-azure-key-vaultsecurityencryptiontransparent-data-encryption-byok-azure-sql-configuremd"></a>9. &nbsp; [PowerShell et CLI : Activer TDE à l’aide de votre propre clé Azure Key Vault](security/encryption/transparent-data-encryption-byok-azure-sql-configure.md)

*Mise à jour : 24-04-2018* &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  ([Précédent](#TitleNum_8) | [Suivant](#TitleNum_10))

<!-- Source markdown line 196.  ms.author= "aliceku".  -->

&nbsp;


<!-- git diff --ignore-all-space --unified=0 a0e00f5701d9a493f503a477c69097ce65aba174 721e8fb856a55ee1e8e9e7fc06036a03adab647b  (PR=5662  ,  Filename=transparent-data-encryption-byok-azure-sql-configure.md  ,  Dirpath=docs\relational-databases\security\encryption\  ,  MergeCommitSha40=91a9c812739a1c9a6ec9e7b8cda71ee1f5adae3d) -->



**Prérequis pour CLI**


- Vous devez avoir un abonnement Azure et être administrateur de cet abonnement.
- [Recommandé mais facultatif] Vous devez avoir un module de sécurité matériel (HSM) ou un magasin de clés local à utiliser pour la création d’une copie locale des éléments de clé du protecteur TDE.
- Interface de ligne de commande CLI version 2.0 ou ultérieure. Pour installer la version la plus récente et vous connecter à votre abonnement Azure, consultez [Installer et configurer l’interface de ligne de commande multiplateforme Azure CLI 2.0](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest).
- Vous devez avoir créé un conteneur Azure Key Vault et une clé pour TDE.
   - [Gérer Key Vault à l’aide de CLI 2.0](https://docs.microsoft.com/azure/key-vault/key-vault-manage-with-cli2)
   - [Instructions pour utiliser un module de sécurité matériel (HSM) et Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-get-started#a-idhsmaif-you-want-to-use-a-hardware-security-module-hsm)
 - Le coffre de clés doit avoir la propriété suivante pour être utilisé pour TDE :
   - [soft-delete](https://docs.microsoft.com/azure/key-vault/key-vault-ovw-soft-delete)
   - [Guide pratique pour utiliser la suppression réversible Key Vault avec l’interface CLI](https://docs.microsoft.com/azure/key-vault/key-vault-soft-delete-cli)
- La clé doit avoir les attributs suivants pour être utilisée pour TDE :
   - Pas de date d’expiration
   - Non désactivée
   - Configurée avec les autorisations *get*, *wrap key*, *unwrap key*

**Étapes : Créer un serveur et affecter une identité Azure AD à votre serveur**

      cli
      # create server (with identity) and database



&nbsp;

&nbsp;

---

<a name="TitleNum_10"/>

### <a name="10-nbsp-about-change-data-capture-sql-servertrack-changesabout-change-data-capture-sql-servermd"></a>10. &nbsp; [À propos de la capture des changements de données (SQL Server)](track-changes/about-change-data-capture-sql-server.md)

*Mise à jour : 17-04-2018* &nbsp; &nbsp; &nbsp; &nbsp; &nbsp;  ([Précédent](#TitleNum_9))

<!-- Source markdown line 112.  ms.author= "jroth".  -->

&nbsp;


<!-- git diff --ignore-all-space --unified=0 588bff652adefd719e799e9777a416b70184c5f8 77ebdbb1b98b24054d5c5afbb3f1d40e94d1e6bc  (PR=5574  ,  Filename=about-change-data-capture-sql-server.md  ,  Dirpath=docs\relational-databases\track-changes\  ,  MergeCommitSha40=bb044a48a6af9b9d8edb178dc8c8bd5658b9ff68) -->



**Différences de classement entre la base de données et la table**


Il est important de noter que, dans certains cas, les classements peuvent ne pas être les mêmes dans la base de données et dans les colonnes d’une table configurée pour la capture des changements de données. La capture des changements de données utilise le stockage temporaire pour remplir les tables côté. Si une table comprend des colonnes CHAR ou VARCHAR avec des classements différents de ceux de la base de données, et si ces colonnes stockent des caractères non ASCII (par exemple, des caractères DBCS codés sur deux octets), la capture des changements de données peut ne pas être en mesure de maintenir la cohérence entre les données modifiées et les données des tables de base. Cela est dû au fait que les variables de stockage temporaire ne peuvent pas être associées à des classements.

Envisagez plutôt l’une des approches suivantes pour vérifier que les données modifiées sont cohérentes avec celles des tables de base :

- Utilisez le type de données NCHAR ou NVARCHAR pour les colonnes qui contiennent des données non ASCII.

- Vous pouvez également utiliser le même classement pour les colonnes et pour la base de données.

Par exemple, si vous avez une base de données qui utilise le classement SQL_Latin1_General_CP1_CI_AS, regardez le tableau suivant :

```
CREATE TABLE T1(
     C1 INT PRIMARY KEY,
     C2 VARCHAR(10) collate Chinese_PRC_CI_AI)
```

La capture des changements de données peut échouer à capturer les données binaires de la colonne C2, car son classement est différent (Chinese_PRC_CI_AI). Utilisez NVARCHAR pour éviter ce problème :

```
CREATE TABLE T1(
     C1 INT PRIMARY KEY,
     C2 NVARCHAR(10) collate Chinese_PRC_CI_AI --Unicode data type, CDC works well with this data type)
```







## <a name="similar-articles-about-new-or-updated-articles"></a>Articles similaires sur les articles nouveaux ou mis à jour

Cette section liste les articles très similaires récemment mis à jour dans d’autres domaines, dans notre dépôt public GitHub.com : [MicrosoftDocs/sql-docs](https://github.com/MicrosoftDocs/sql-docs/).



#### <a name="subject-areas-that-do-have-new-or-recently-updated-articles"></a>Domaines *avec* des articles nouveaux ou mis à jour récemment

- [Nouveaux + Mis à jour (11 + 6) :&nbsp; &nbsp;**Advanced Analytics pour SQL** (documentation)](../advanced-analytics/new-updated-advanced-analytics.md)
- [Nouveaux + Mis à jour (18 + 0) : &nbsp; &nbsp;**Analysis Services pour SQL**(documentation)](../analysis-services/new-updated-analysis-services.md)
- [Nouveaux + Mis à jour (218 + 14) : **Connexion à SQL** (documentation)](../connect/new-updated-connect.md)
- [Nouveaux + Mis à jour (14 + 0) :&nbsp; &nbsp;**Moteur de base de données pour SQL** (documentation)](../database-engine/new-updated-database-engine.md)
- [Nouveaux + Mis à jour (3 + 2) :&nbsp; &nbsp; **Integration Services pour SQL** (documentation)](../integration-services/new-updated-integration-services.md)
- [Nouveaux + Mis à jour (3 + 3) :&nbsp; &nbsp; **Linux pour SQL** (documentation)](../linux/new-updated-linux.md)
- [Nouveaux + Mis à jour (7 + 10) :&nbsp; &nbsp;**Bases de données relationnelles pour SQL** (documentation)](../relational-databases/new-updated-relational-databases.md)
- [Nouveaux + Mis à jour (0 + 2) :&nbsp; &nbsp; **Reporting Services pour SQL** (documentation)](../reporting-services/new-updated-reporting-services.md)
- [Nouveaux + Mis à jour (1 + 3) :&nbsp; &nbsp; **SQL Operations Studio** (documentation)](../sql-operations-studio/new-updated-sql-operations-studio.md)
- [Nouveaux + Mis à jour (2 + 3) :&nbsp; &nbsp; **Microsoft SQL Server** (documentation)](../sql-server/new-updated-sql-server.md)
- [Nouveaux + Mis à jour (1 + 1) :&nbsp; &nbsp; **SQL Server Data Tools (SSDT)** (documentation)](../ssdt/new-updated-ssdt.md)
- [Nouveaux + Mis à jour (5 + 2) :&nbsp; &nbsp; **SQL Server Management Studio (SSMS)** (documentation)](../ssms/new-updated-ssms.md)
- [Nouveaux + Mis à jour (0 + 2) :&nbsp; &nbsp; **Transact-SQL** (documentation)](../t-sql/new-updated-t-sql.md)
- [Nouveaux + Mis à jour (1 + 1) : &nbsp; &nbsp; **Outils pour SQL** documentation](../tools/new-updated-tools.md)



#### <a name="subject-areas-that-do-not-have-any-new-or-recently-updated-articles"></a>Domaines *sans* article nouveau ou mis à jour récemment

- [Nouveaux + Mis à jour (0 + 0) : **Système de plateforme d’analyse pour SQL** documentation](../analytics-platform-system/new-updated-analytics-platform-system.md)
- [Nouveaux + Mis à jour (0 + 0) : **Data Quality Services pour SQL** (documentation)](../data-quality-services/new-updated-data-quality-services.md)
- [Nouveaux + Mis à jour (0 + 0) : **Extensions DMX (Data Mining Extensions) pour SQL** (documentation)](../dmx/new-updated-dmx.md)
- [Nouveaux + Mis à jour (0 + 0) : **Master Data Services (MDS) for SQL** (documentation)](../master-data-services/new-updated-master-data-services.md)
- [Nouveaux + Mis à jour (0 + 0) : **Expressions MDX (Multidimensional Expressions) pour SQL** (documentation)](../mdx/new-updated-mdx.md)
- [Nouveaux + Mis à jour (0 + 0) : **ODBC (Open Database Connectivity) pour SQL** (documentation)](../odbc/new-updated-odbc.md)
- [Nouveaux + Mis à jour (0 + 0) : **PowerShell pour SQL** (documentation)](../powershell/new-updated-powershell.md)
- [Nouveaux + Mis à jour (0 + 0) : **Exemples pour SQL** (documentation)](../samples/new-updated-samples.md)
- [Nouveaux + Mis à jour (0 + 0) : **SQL Server Migration Assistant (SSMA)** (documentation)](../ssma/new-updated-ssma.md)
- [Nouveaux + Mis à jour (0 + 0) : **XQuery pour SQL** (documentation)](../xquery/new-updated-xquery.md)


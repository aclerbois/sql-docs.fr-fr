---
title: Glossaire ADO | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.reviewer: ''
ms.suite: sql
ms.tgt_pltfrm: ''
ms.prod: sql
ms.prod_service: connectivity
ms.component: ado
ms.technology: connectivity
ms.topic: conceptual
helpviewer_keywords:
- ADO, glossary
ms.assetid: b0478836-4123-4357-969a-c5784fc28be5
caps.latest.revision: 15
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: bdb021c9d036a3daab6b0e5c3f4912c0da4059eb
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="ado-glossary"></a>Glossaire ADO
Cette rubrique définit les termes pertinents pour ADO.  
  
## <a name="a"></a>Un  
 URL absolue  
 URL complète qui spécifie l’emplacement d’une ressource qui se trouve sur Internet ou un intranet. Voir aussi *URL* et *URL relative*.  
  
 Contrôle ActiveX  
 Inscrit automatiquement, dans le processus composant COM qui a souvent un élément visuel au moment du design ou d’exécution. Contrôles ActiveX ont également la possibilité de communiquer avec un conteneur de documents actifs, tels que Microsoft Internet Explorer.  
  
 ADISAPI (avancé des données Internet Server Application Programming Interface)  
 DLL ISAPI assurant l’analyse, le contrôle Automation, le marshaling des objets Recordset et l’empaquetage MIME. Le composant ADISAPI fonctionne via l’API fournie par Internet Information Services (IIS). Voir aussi *ISAPI*.  
  
 fonction d'agrégation  
 Dans une requête, une fonction telle que COUNT ou AVG STDEV qui calcule une valeur à l’aide de toutes les lignes dans une colonne d’une table. Écrire des expressions et dans la programmation, vous pouvez utiliser les fonctions d’agrégation SQL (y compris celles décrites ci-dessus) et les fonctions d’agrégation de domaine pour déterminer différentes statistiques.  
  
 alias  
 Un autre nom que vous donnez à une colonne ou une expression dans une instruction SQL SELECT, souvent plus courte ou plus explicite. Par exemple, BobSales correspond à l’alias dans l’instruction SELECT suivante : « Select TA-Sales as BobSales from SalesDB ». Un alias peut être utilisé pour attribuer dynamiquement des colonnes aux liaisons de contrôle sur l’objet DataControl.  
  
 thread cloisonné  
 Où tous les appels à un objet se produisent sur un thread de modèle de thread COM. Dans le thread cloisonné, COM synchronise et marshale les appels. Voir aussi *COMmddefcom*.  
  
 opération asynchrone  
 Une opération qui retourne le contrôle au programme appelant sans attendre que l’opération se termine. Avant que l’opération soit terminée, l’exécution de code se poursuit. Voir aussi *opération synchrone*.  
  
## <a name="b"></a>B  
 entrée de liaison  
 Un mappage entre un champ dans une table et une variable. Dans les extensions ADO de Visual C++, **Recordset** champs sont mappés à des variables C/C++.  
  
 masque de bits  
 Une valeur numérique destiné à une comparaison de valeurs de bit par bit avec d’autres valeurs numériques, généralement indiquer les options disponibles dans les paramètres ou valeurs de retour. Cette comparaison est généralement effectuée par des opérateurs logiques au niveau du bit, tel que **et** et **ou** en Visual Basic, **&** et **&#124;** en C++.  
  
 Par exemple, ADO **FieldAttributeEnum** valeurs peuvent être utilisées en tant que masques de bits pour déterminer les attributs d’un champ. Supposons que vous souhaitiez déterminer si un champ a été mise à jour. Pour cela, vous pouvez tester à l’expression suivante en Visual Basic :`Field.Attributes AND adFldUpdatable`  
  
 Si le résultat est TRUE, le champ est modifiable.  
  
 signet  
 Un marqueur qui identifie de façon unique une ligne dans un ensemble de lignes afin qu’un utilisateur d’accéder rapidement à ce dernier.  
  
 objet métier  
 Objet qui effectue un ensemble défini d’opérations, telles que la logique de règle métier ou de validation de données. Les objets métier résident généralement sur la couche intermédiaire.  
  
 règle d’entreprise  
 La combinaison de modifications de la validation, les vérifications d’ouverture de session, les recherches de base de données, stratégies et transformations par algorithmes qui constituent le moyen d’une entreprise d’activité. Également appelé *logique métier*.  
  
## <a name="c"></a>C  
 Expression calculée  
 Une expression qui n’est pas une constante, mais dont la valeur dépend d’autres valeurs. Pour être évaluée, une expression calculée doit obtenir et calculer les valeurs d’autres sources, en général dans les autres champs ou des lignes.  
  
 chapitre  
 Une référence à une plage de lignes à partir d’une source de données. Dans ADO, un chapitre fait généralement référence à un autre **Recordset**.  
  
 Les colonnes de chapitres permettent de définir un *parent-enfant* relation où la *parent* est la **Recordset** contenant la colonne de chapitre et  *enfant* est la **Recordset** représenté par le chapitre.  
  
 alias-chapitre  
 Un alias qui fait référence à la colonne ajoutée au parent.  
  
 jeu de caractères  
 Un mappage d’un jeu de caractères à leurs valeurs numériques. Par exemple, Unicode est un caractère de 16 bits capable de coder tous les caractères connus et son utilisation comme norme de codage de caractères dans le monde entier.  
  
 child  
 Le côté dépendant d’une relation hiérarchique. Un enfant est un nœud dans une structure hiérarchique qui possède un autre nœud au-dessus de lui (proche de la racine). Voir aussi *alias-enfant*, *relation parent-enfant*, *parent*.  
  
 alias-enfant  
 Un alias qui fait référence à l’enfant. Voir aussi *alias*, *enfant*.  
  
 CLSID (identificateur de classe)  
 Identificateur unique universel (UUID) qui identifie un composant COM. Chaque composant COM possède son CLSID dans le Registre Windows afin qu’il peut être chargé par d’autres applications. Voir aussi *ProgID*, *COM*.  
  
 couche cliente  
 Une couche logique d’un système distribué qui présente en général des données et les processus d’entrée de l’utilisateur, parfois appelé le *frontal*. En règle générale, la couche cliente demande des données à partir d’un serveur basé sur l’entrée, puis met en forme et affiche le résultat. Voir aussi *intermédiaire*, *couche de source de données*, *application distribuée*.  
  
 COM (Component Object Model)  
 Norme binaire permettant aux objets d’interagir dans un environnement réseau, quel que soit le langage dans lequel ils ont été développés ou les postes sur lesquels ils résident. Technologies de basé sur COM comprennent les contrôles ActiveX, Automation et les objets liés et incorporés (OLE). COM permet à un objet d’exposer ses fonctionnalités à d’autres composants et d’héberger des applications. Il définit comment l’objet expose lui-même et comment cette exposition fonctionne sur les processus et les réseaux. COM définit également le cycle de vie de l’objet.  
  
 Composant COM  
 Fichier binaire, tels que des fichiers .exe, .dll et .ocx, qui prend en charge la norme COM pour fournir des objets. Un tel fichier contient le code pour un ou plusieurs fabriques de classes, classes COM, les mécanismes d’entrée de Registre, le code de chargement et ainsi de suite.  
  
 opérateur de comparaison  
 Un opérateur qui compare deux expressions et retourne une valeur booléenne.  
  
 Un paramètre de critères qui peut-être être exprimé en tant que » > » (supérieur à), »\<» (inférieur à), « = » (égal), « > = » (supérieur ou égal à), « < = » (inférieur ou égal à), « <> » (différent de) ou « like » (correspondance).  
  
 component  
 Objet qui encapsule les données et le code et fournit un ensemble précis de services disponibles publiquement.  
  
 fichier composé  
 Une implémentation de COM structurée de stockage pour les fichiers. Un fichier composé stocke des objets distincts dans un seul fichier structuré, constitué de deux éléments principaux : les objets de stockage et les objets de flux. Ensemble, elles fonctionnent comme un système de fichiers dans un fichier.  
  
 Nombre de fichiers liés dans un fichier physique. Chaque fichier dans un fichier composé est accessible comme s’il s’agissait d’un seul fichier physique.  
  
 constant  
 Une valeur numérique ou chaîne qui ne change pas. Les énumérations ADO nommées (constantes énumérées) peuvent être utilisées dans votre code au lieu des valeurs réelles, par exemple, **adUseClient** est une constante dont la valeur est 3. (Const adUseClient = 3). Voir aussi *énumération*.  
  
 cursor  
 Un élément de base de données qui contrôle la navigation entre les enregistrements, mise à jour des données et la visibilité des modifications apportées à la base de données par d’autres utilisateurs.  
  
## <a name="d"></a>D  
 liaison de données  
 Le processus d’association d’objets ou contrôles d’une application à une source de données. Un contrôle associé à une source de données est appelé un *contrôle lié aux données*.  
  
 Le contenu d’un contrôle lié aux données est associé à des valeurs à partir d’une base de données. Par exemple, un contrôle de grille est lié à un **Recordset** objet peut être mis à jour lorsque les lignes de la **Recordset** sont mis à jour. Lorsque les nouvelles valeurs sont récupérées par le **Recordset**, nouvelles valeurs sont affichées dans la grille.  
  
 fournisseur de données  
 Logiciel qui expose des données à une application ADO directement ou via un fournisseur de services. Consultez également le fournisseur de services.  
  
 mise en forme des données  
 Une technique qui utilise une syntaxe formalisée (appelé **forme langage**) pour définir spécialisé **Recordset** objet (appelé un *en forme de jeu d’enregistrements*) qui ne contient pas données mais également les références à d’autres **Recordset** objets et/ou des valeurs calculées basées sur ces autres **Recordset** objets.  
  
 couche de source de données  
 Une couche logique d’un système distribué qui représente un ordinateur exécutant un SGBD, comme une base de données SQL Server. Voir aussi *couche cliente*, *intermédiaire*, *application distribuée*.  
  
 DCOM  
 Un protocole de câble qui permet aux composants COM communiquer directement entre eux sur un réseau. Voir aussi *COM*, *composant*.  
  
 DDL (Data Definition Language)  
 Instructions SQL qui définissent, par opposition à manipuler des données. Le schéma de base de données est créé ou modifié avec le langage DDL. Par exemple, **CREATE TABLE**, **CREATE INDEX**, **GRANT**, et **RÉVOQUER** sont des instructions SQL DDL.  
  
 flux de données par défaut  
 Un flux de texte ou binaire (représenté par un **flux** objet) qui est associé **enregistrement** ou **Recordset** objets lors de l’utilisation de certains fournisseurs OLE DB, telles que le Fournisseur Microsoft OLE DB pour la publication Internet. Le flux par défaut contient généralement le contenu d’un fichier, telles que le code HTML pour la racine d’un site Web.  
  
 application distribuée  
 Un programme écrit de sorte que le traitement puisse être partagé entre plusieurs ordinateurs sur un réseau. En règle générale, une application distribuée est divisée présentation, logique métier et données couches de magasin, ou *niveaux*. Voir aussi niveau client, niveau intermédiaire, niveau source de données.  
  
 jeu d’enregistrements déconnecté  
 A **Recordset** objet dans un cache de client qui n’a plus une connexion active au serveur. Si la source de données d’origine doit être accessible pour une raison quelconque, telles que la mise à jour des données, la connexion doit être rétablie. Toutefois, les collections, les propriétés et les méthodes d’un déconnecté **Recordset** peut toujours être accessible.  
  
 DML (Data Manipulation Language)  
 Instructions SQL qui manipulent, par opposition à définir, des données. Les valeurs dans une base de données sont sélectionnées et modifiés avec DML. Par exemple, **insérer**, **mise à jour**, **supprimer**, et **sélectionnez** sont des instructions DML SQL.  
  
 fournisseur de source de document  
 Une classe spéciale de fournisseurs qui gèrent des dossiers et des documents. Quand un document est représenté par un **enregistrement** objet ou un dossier de documents est représenté par un **Recordset** de l’objet, le fournisseur de source de document remplit ces objets avec un jeu unique de champs qui décrire les caractéristiques du document, au lieu du document lui-même. Voir aussi enregistrement de ressource.  
  
 DSN (nom de source de données)  
 La collection d’informations utilisées pour connecter votre application à une base de données ODBC particulière. Le Gestionnaire de pilotes ODBC utilise ces informations pour créer une connexion à la base de données. Une source de données peut être stockée dans un fichier (fichier DSN) ou dans le Registre Windows (DSN système).  
  
 propriétés dynamiques  
 Une propriété spécifique à un fournisseur de données ou le service de curseur. Le **propriétés** collection d’un objet est renseignée automatiquement avec ces (« dynamique »). Un objet n’a pas de propriétés dynamiques jusqu'à ce qu’il est connecté à une source de données via un fournisseur de données particulier. Voir aussi données fournisseur, le curseur.  
  
## <a name="e"></a>E  
 Énumération  
 Une liste de constantes nommées. Les valeurs énumérées ne doivent pas être uniques. Toutefois, le nom de chaque valeur doit être unique dans l’étendue où l’énumération est définie. Dans ADO, les énumérations sont utilisées pour le paramètre numérique et valeurs de retour, pour ajouter signification à code ADO et de protéger le développeur à partir des valeurs numériques (qui peuvent changer d’une version à l’autre). Par exemple, pour ouvrir un statique **Recordset**, utilisez le **adOpenStatic** valeur énumérée : `Recordset.Open ,,adOpenStatic`  
  
 Également appelé *constante énumérée*. Voir aussi *constante*.  
  
 événement  
 Action reconnue par un objet pour lequel vous pouvez écrire du code pour répondre. Événements peuvent être générés par l’exécution de commande, exécution de la transaction, la navigation de jeu d’enregistrements et mises à jour des données, entre autres. Voir aussi *Gestionnaire d’événements*.  
  
 gestionnaire d'événements  
 Un gestionnaire d’événements est le code exécuté lorsqu’un événement se produit. Consultez également l’événement.  
  
## <a name="h"></a>H  
 gestionnaire  
 Une routine qui gère une condition courante et relativement simple ou une opération, comme la gestion de données ou de la récupération des erreurs.  
  
 jeu d’enregistrements hiérarchique  
 A **Recordset** qui contient une autre **Recordset**. Voir aussi les données de mise en forme, chapitre.  
  
 Pour plus d’informations, consultez [accès aux lignes d’un objet Recordset hiérarchique](../../ado/guide/data/accessing-rows-in-a-hierarchical-recordset.md).  
  
 hiérarchie  
 En règle générale, une hiérarchie est une structure de classement avec un haut niveau et les niveaux subordonnés. Dans ADO, hiérarchique **jeux d’enregistrements** sont utilisés pour représenter la relation parent-enfant entre un enregistrement et un chapitre. Dans ADO, **enregistrement** et **flux** objets peuvent être utilisés pour accéder à des arborescences hiérarchiques, telles que des documents et un dossier. ADO MD comprend aussi **hiérarchie** objets pour représenter une relation entre les niveaux d’une dimension dans un cube OLAP. Voir aussi les jeux d’enregistrements hiérarchique, relation parent-enfant, chapitre, arborescence.  
  
## <a name="i-l"></a>I-L  
 ISAPI (Internet Server Application programmation Interface)  
 Un ensemble de fonctions pour les serveurs Internet, tel qu’un serveur de 2000 Windows NT® / Windows Server exécutant Microsoft® Internet Information Services (IIS).  
  
 Key  
 Une ou plusieurs colonnes dans une table qui identifient de manière unique une ligne ; souvent utilisé pour une table d’index.  
  
## <a name="m"></a>M  
 marshaling  
 Le processus de paramètres de méthode d’interface empaquetage, d’envoi et de décompactage au-delà des limites de thread ou processus.  
  
 niveau intermédiaire  
 La couche de logique dans un système distribué entre une interface utilisateur ou de client Web et de la base de données. Il s’agit en général, où les objets métier sont instanciés. La couche intermédiaire est une collection de règles d’entreprise et des fonctions qui génèrent et effectuer l’opération de réception d’informations. Ils le faire à l’aide des règles d’entreprise, ce qui peuvent changer fréquemment et par conséquent encapsulées dans des composants qui sont physiquement séparés à partir de la logique d’application. Également appelé *niveau serveur d’applications*. Voir aussi application distribuée, niveau client, niveau source de données.  
  
 MIME (multi-purpose Internet Mail Extension)  
 Protocole Internet développé à l’origine pour permettre l’échange de messages électroniques avec du contenu riche sur un réseau hétérogène, d’ordinateurs et les environnements de messagerie. Dans la pratique, MIME a également été adopté et étendu par des applications de messagerie non.  
  
 MIME est une norme qui permet aux données binaires à publier et à lire sur Internet. L’en-tête d’un fichier avec des données binaires contient le type MIME des données ; Cela permet d’informer les programmes clients (les navigateurs Web et programmes de messagerie, par exemple) qu’ils doivent gérer les données d’une autre façon qu’ils gèrent le texte de droite. Par exemple, l’en-tête d’un document Web contenant un graphique JPEG contient le type MIME spécifique au format de fichier JPEG. Ainsi, un navigateur afficher le fichier avec sa visionneuse JPEG, s’il en existe.  
  
## <a name="n-o"></a>N-O  
 node  
 Un élément dans une arborescence hiérarchique. Un nœud peut être la racine, ou l’enfant d’un autre nœud. Un nœud peut également être le parent de plusieurs enfants. Voir aussi hiérarchie, arborescence, racine, enfant, parent.  
  
 variable objet  
 Variable qui contient une référence à un objet. Par exemple, `objCustomObject` est une variable qui pointe vers un objet de type CustomObject :`Set objCustomObject = CreateObject(adodb.Recordset)`  
  
 ODBC (Open Database Connectivity)  
 Language interface de programmation standard utilisée pour se connecter à une variété de sources de données. Cela est généralement accessibles via le panneau de configuration, où les noms de source de données (DSN) peuvent être affectés à utiliser des pilotes ODBC spécifiques.  
  
 OLE DB  
 Un ensemble d’interfaces qui exposent des données à partir de diverses sources à l’aide de COM. Interfaces OLE DB fournissent aux applications un accès uniforme aux données stockées dans diverses sources d’informations. Ces interfaces prennent en charge les fonctionnalités SGBD appropriées pour la source de données, ce qui lui permet de partager ses données. Voir aussi COM.  
  
 verrouillage optimiste  
 Type de verrouillage dans lequel la page de données contenant un ou plusieurs enregistrements, notamment l’enregistrement en cours de modification, n’est pas disponible à d’autres utilisateurs uniquement pendant que l’enregistrement est mis à jour par le **mise à jour** mais est disponible avant et après le l’appel à **mise à jour**.  
  
 Verrouillage optimiste est utilisé lorsque le **Recordset** objet est ouvert avec le **LockType** paramètre ou la valeur de propriété **adLockOptimistic** ou  **adLockBatchOptimistic**. Voir aussi verrouillage pessimiste.  
  
 valeur ordinale  
 Emplacement d’un élément dans un ordre numérique. Dans une collection ADO, la valeur ordinale du premier élément est zéro (0). L’élément suivant est un (1) et ainsi de suite.  
  
## <a name="p"></a>P  
 commande paramétrable  
 Une requête ou une commande qui vous permet de définir des valeurs de paramètre avant l’exécution de la commande. Par exemple, une chaîne SQL peut être paramétrée en incorporant des marqueurs de paramètres dans la chaîne SQL (désignée par la ' ?' caractère). Ensuite, l’application spécifie des valeurs pour chaque paramètre et exécute la commande.  
  
 parent  
 Le contrôle côté d’une relation hiérarchique. Dans une structure hiérarchique, un parent possède un ou plusieurs nœuds enfants directement en dessous dans la hiérarchie. Voir aussi alias-parent, relation parent-enfant, enfant.  
  
 alias-parent  
 Un alias qui fait référence au parent. Voir aussi alias, parent.  
  
 relation parent-enfant  
 Une relation dans une structure hiérarchique dans laquelle le parent est un niveau supérieur et directement associé à un ou plusieurs enfants. Un enfant est un niveau inférieur et doit avoir un seul parent. Voir aussi parent, enfant.  
  
 verrouillage pessimiste  
 Type de verrouillage dans lequel la page contenant un ou plusieurs enregistrements, notamment l’enregistrement en cours de modification, n’est pas disponible à d’autres utilisateurs pour vous assurer qu’une mise à jour sera établie. Comportement du verrouillage pessimiste est défini par le fournisseur OLE DB. En règle générale, les enregistrements sont verrouillés lors de la modification et reste indisponibles jusqu'à ce que le **mise à jour** méthode est terminée.  
  
 Verrouillage pessimiste est activé lorsque la **Recordset** objet est ouvert avec le **LockType** paramètre ou la valeur de propriété **adLockPessimistic**. Consultez également le verrouillage optimiste.  
  
 le regroupement  
 Optimisation des performances basée sur l’utilisation de collections de ressources préallouées, telles que des objets ou des connexions de base de données. Il est plus efficace d’extraire une ressource existante du pool que la création d’une nouvelle ressource.  
  
 ProgID (identificateur programmatique)  
 Un nom unique mappé au Registre Windows par une application COM. Le ProgID pour une connexion ADO est « ADODB. Connexion ». Voir aussi CLSID, COM.  
  
 Proxy  
 Un objet spécifique de l’interface qui fournit le marshaling des paramètres et la communication nécessaires à un client pour appeler un objet d’application est en cours d’exécution dans un environnement d’exécution différents, tels que sur un thread différent ou dans un autre processus. Le proxy est installé sur le client et communique avec un stub correspondant qui se trouve à l’objet d’application qui est appelée. Voir aussi stub.  
  
## <a name="r"></a>R  
 URL relative  
 Une URL partiellement qualifiée qui spécifie une ressource sur Internet ou sur un intranet dont l’emplacement est relatif à un point de départ spécifié par une URL absolue ou d’un objet connexion ADO équivalent. En effet, le concaténée absolu et relatif URL constituer une URL complète. Voir aussi URL et URL absolue.  
  
 source de données distante  
 Une source de données qui existe sur un autre ordinateur, plutôt que sur le système local (sur lequel l’application cliente s’exécute).  
  
 enregistrement de ressource  
 Un enregistrement à partir d’un fournisseur de source de document qui contient des champs pour la définition et la description d’un dossier ou un document. Le document lui-même n’est pas contenu dans l’enregistrement de ressource, mais généralement accessibles par le flux par défaut ou un champ dans l’enregistrement de ressource contenant une URL. Voir aussi fournisseur de source de document, URL du flux par défaut.  
  
 ensemble de lignes  
 Un ensemble de lignes à partir d’une source de données, tous ayant le même schéma de champ. Un ensemble de lignes peut représenter la totalité ou une partie des champs à partir d’une table. Un ensemble de lignes peut également représenter une table virtuelle, créée par une requête ou une jointure de deux ou plusieurs tables. Dans ADO, les ensembles de lignes sont représentés par **Recordset** objets.  
  
## <a name="s"></a>S  
 Portée  
 La plage de référence pour un objet ou une variable ou une plage d’enregistrements dans une table ou vue. Par exemple, les variables locales peuvent être référencées uniquement dans la procédure dans laquelle ils ont été définis. Les variables publiques sont accessibles à partir de n’importe où dans l’application. Objets, tels que la base de données en cours sont dans la portée si elles sont dans le chemin d’accès de recherche définis. Plages d’enregistrements peuvent être spécifiés avec une clause Scope dans de nombreuses commandes.  
  
 fournisseur de services  
 Logiciel qui encapsule un service en production et la consommation des données, étendant les fonctionnalités de vos applications ADO. Il s’agit d’un fournisseur qui n’expose pas directement les données, au lieu de cela, il fournit un service, telles que le traitement des requêtes. Le fournisseur de services peut traiter les données fournies par un fournisseur de données. Consultez également le fournisseur de données.  
  
 jeu d’enregistrements mis en forme  
 A **Recordset** dont les colonnes ont été spécifiquement définies pour contenir des données, mais également références (appelés chapitres) à d’autres **Recordset** objets et/ou des valeurs calculées basé sur les autres **Recordset** objets.  
  
 frère  
 Les deux ou plusieurs nœuds dans une structure hiérarchique qui se trouvent sur le même niveau dans la hiérarchie. Le nœud racine dans une hiérarchie de possède pas de frères.  
  
 procédure stockée  
 Collection précompilée du code tel que des instructions SQL et d’instructions de flux de contrôle facultatives stockées sous un nom et traitées en tant qu’unité. Procédures stockées sont stockées dans une base de données ; ils peuvent être exécutées avec un appel à partir d’une application et autorisent des variables déclarées par l’utilisateur, l’exécution conditionnelle et autres fonctionnalités de programmation puissantes.  
  
 stub  
 Un objet spécifique de l’interface qui fournit le marshaling des paramètres et la communication nécessaires pour un objet d’application à recevoir des appels à partir d’un client qui est en cours d’exécution dans un environnement d’exécution différents, tels que sur un thread différent ou dans un autre processus. Le stub se trouve à l’objet application et communique avec un proxy correspondant qui est situé sur le client qui l’appelle. Voir aussi proxy.  
  
 sous-nœud  
 Voir enfant.  
  
 opération synchrone  
 Une opération démarrée par du code qui se termine avant que l’opération suivante puisse démarrer. Voir aussi une opération asynchrone.  
  
## <a name="t-z"></a>T-Z  
 Arborescence  
 Structure représentant une relation hiérarchique entre des éléments (nœuds). Il existe un nœud au niveau supérieur d’une arborescence (racine). Sous la racine, il peut y avoir plusieurs enfants. Chaque enfant peut être à son tour le parent d’autres enfants, donc créer une branche comme un arbre. Un dossier contenant des documents et autres dossiers est un exemple classique d’une structure arborescente. Voir aussi hiérarchie, nœud, racine, enfant, parent.  
  
 Serveur Web  
 Un ordinateur qui fournit des services Web et des pages pour les utilisateurs intranet et Internet.

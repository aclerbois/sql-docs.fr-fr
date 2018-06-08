---
title: Glossaire ODBC | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- ODBC [ODBC], glossary
- glossary [ODBC]
ms.assetid: e8227000-1944-42e5-a881-1f549e1ff9d1
caps.latest.revision: 7
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 0bd26d6d8d95399bfe1126f56f6f9b8c2d176e71
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="odbc-glossary"></a>Glossaire ODBC
## <a name="a"></a>Un  
 **plan d’accès**  
 Un plan généré par le moteur de base de données pour exécuter une instruction SQL. Équivalent au code exécutable compilé à partir d’un langage de troisième génération tel que C.  
  
 **Fonction d’agrégation**  
 Une fonction qui génère une valeur unique à partir d’un groupe de valeurs, souvent utilisé avec **GROUP BY** et **HAVING** clauses. Incluront des fonctions d’agrégation **AVG**, **nombre**, **MAX**, **MIN**, et **somme**. Également appelé *fonctions de jeu*. *Voir aussi* fonction scalaire.  
  
 **ANSI**  
 American National Standards Institute. L’API ODBC est basée sur l’Interface de niveau d’appel ANSI.  
  
 **APD**  
 *Consultez* descripteur de paramètre d’application (APD).  
  
 **API**  
 Interface de programmation d’application. Un ensemble de routines qu’une application utilise pour demander et exécuter des services de niveau inférieur. L’API ODBC est composé des fonctions ODBC.  
  
 **Application**  
 Un programme exécutable qui appelle les fonctions de l’API ODBC.  
  
 **Descripteur de paramètre d’application (APD)**  
 Un descripteur qui décrit les paramètres dynamiques dans une instruction SQL avant toute conversion spécifiée par l’application.  
  
 **Descripteur de ligne d’application (ARD)**  
 Un descripteur qui représente les métadonnées de colonne et les données des mémoires tampon de l’application, qui décrit une ligne de données à la suite d’une conversion de données spécifiée par l’application.  
  
 **ARD**  
 *Consultez* descripteur de ligne d’application (ARD).  
  
 **mode de validation automatique**  
 Mode de validation de transaction dans laquelle les transactions sont validées immédiatement après leur exécution.  
  
## <a name="b"></a>B  
 **changement de comportement**  
 Une modification de certaines fonctionnalités à partir d’ODBC 3 *.x* comportement ODBC 2. *x* comportement, ou vice versa. Dû en modifiant l’attribut d’environnement SQL_ATTR_ODBC_VERSION.  
  
 **Objet binaire volumineux (BLOB)**  
 Toutes les données binaires pour un certain nombre d’octets, telles que 255. En général beaucoup plus longue. Ce type de données est généralement envoyé à et récupérée à partir de la source de données dans les parties. Également appelé *données long*.  
  
 **Liaison**  
 Comme un verbe, le fait d’association d’une colonne dans un jeu de résultats ou un paramètre dans une instruction SQL à une variable d’application. Comme un nom, l’association.  
  
 **décalage de la liaison**  
 Une valeur ajoutée pour les adresses de mémoire tampon de données et les adresses de mémoire tampon de longueur / d’indicateur pour tous les liée au paramètre ou la colonne des données, produisant nouvelles adresses.  
  
 **curseur de bloc**  
 Un curseur capable d’extraire plusieurs lignes de données à la fois.  
  
 **Mémoire tampon**  
 Une partie de la mémoire de l’application utilisée pour passer des données entre l’application et le pilote. Mémoires tampons sont souvent fournis par paires : un *tampon de données* et un *tampon de longueur de données*.  
  
 **byte**  
 Huit bits ou un octet. *Voir aussi* octet.  
  
## <a name="c"></a>C  
 **Type de données C**  
 Le type de données d’une variable dans un programme C, dans ce cas, l’application.  
  
 **catalog**  
 Ensemble de tables système dans une base de données qui décrivent la forme de la base de données. Également appelé un *schéma* ou *dictionnaire de données*.  
  
 **fonction de catalogue**  
 Une fonction ODBC utilisée pour récupérer les informations de catalogue de la base de données.  
  
 **CLI**  
 *Consultez* API.  
  
 **client/serveur**  
 Une stratégie d’accès de base de données dans laquelle un ou plusieurs clients accéder aux données via un serveur. Les clients implémentent généralement l’interface utilisateur, tandis que les contrôles de serveur de base de données access.  
  
 **column**  
 Conteneur pour un élément d’information dans une ligne unique. Également appelé *champ*.  
  
 **Validation**  
 Pour conserver les modifications dans une transaction.  
  
 **accès concurrentiel**  
 La capacité de plusieurs transactions à accéder aux mêmes données en même temps.  
  
 **Niveau de conformité**  
 Un ensemble discret de fonctionnalités prises en charge par une source de données ou de pilote. ODBC définit des niveaux de conformité d’API et les niveaux de conformité SQL.  
  
 **connexion**  
 Une instance particulière d’une source de données et le pilote.  
  
 **navigation de la connexion**  
 Rechercher sur le réseau pour se connecter à des sources de données. Navigation de la connexion peut impliquer plusieurs étapes. Par exemple, l’utilisateur peut tout d’abord parcourir le réseau pour les serveurs, puis parcourez un serveur particulier pour une base de données.  
  
 **handle de connexion**  
 Handle vers une structure de données qui contient des informations sur une connexion.  
  
 **Ligne actuelle**  
 La ligne actuellement pointé par le curseur. Opérations positionnées agissent sur la ligne actuelle.  
  
 **cursor**  
 Un élément logiciel qui retourne des lignes de données à l’application. Probablement nommé d’après le curseur clignotant sur un ordinateur Terminal Server ; comme ce curseur indique la position actuelle sur l’écran, un curseur sur un jeu de résultats indique la position actuelle dans le jeu de résultats.  
  
## <a name="d"></a>D  
 **mémoire tampon de données**  
 Une mémoire tampon utilisée pour passer des données. Souvent associées avec des données mémoire tampon est un *tampon de longueur de données*.  
  
 **dictionnaire de données**  
 *Consultez* catalogue.  
  
 **mémoire tampon de longueur de données**  
 Une mémoire tampon utilisée pour transmettre la longueur de la valeur correspondante *tampon de données*. Le tampon de longueur de données est également utilisé pour stocker des indicateurs, tels que si la valeur de données est terminée.  
  
 **Source de données**  
 Les données que l’utilisateur souhaite accéder et son système d’exploitation associé, SGBD et de plateforme de réseau (le cas échéant).  
  
 **Type de données**  
 Le type d’un élément de données. ODBC définit des types de données C et SQL. *Voir aussi* indicateur de type.  
  
 **colonne de données d’exécution**  
 Une colonne pour laquelle les données sont envoyées une fois **SQLSetPos** est appelée. Ce nom, car les données sont envoyées au moment de l’exécution, plutôt que de soit placé dans une mémoire tampon d’ensemble de lignes. Données de type long sont généralement envoyées dans les parties au moment de l’exécution.  
  
 **paramètre de Data-at-execution**  
 Un paramètre pour lequel les données sont envoyées une fois **SQLExecute** ou **SQLExecDirect** est appelée. Ce nom, car les données sont envoyées lors de l’exécution de l’instruction SQL au lieu d’être placé dans une mémoire tampon de paramètre. Données de type long sont généralement envoyées dans les parties au moment de l’exécution.  
  
 **database**  
 Collection de données dans un SGBD discrète. Également un SGBD.  
  
 **Moteur de base de données**  
 Le logiciel dans un SGBD qui analyse et exécute des instructions SQL et accède aux données physiques.  
  
 **SGBD**  
 Système de gestion de base de données. Couche logicielle entre la base de données physique et l'utilisateur. Le SGBD (système de gestion de base de données) gère tous les accès à la base de données.  
  
 **Pilote SGBD**  
 Un pilote qui accède à des données physiques via un moteur de base de données autonome.  
  
 **DDL**  
 Langage de définition de données. Instructions SQL qui définissent, par opposition à manipuler des données. Par exemple, **CREATE TABLE**, **CREATE INDEX**, **GRANT**, et **RÉVOQUER**.  
  
 **identificateur délimité**  
 Un identificateur est placé entre guillemets identificateur afin qu’il peut contenir des caractères spéciaux ou correspondre aux mots clés (également appelé un identificateur entre guillemets).  
  
 **descripteur**  
 Structure de données qui conserve des informations sur les données de la colonne ou de paramètres dynamiques. La représentation physique du descripteur n’est pas définie ; applications obtenir un accès direct à un descripteur uniquement par la manipulation de ses champs en appelant les fonctions ODBC avec le handle du descripteur.  
  
 **base de données de bureau**  
 Un SGBD est conçu pour s’exécuter sur un ordinateur personnel. En général, ces SGBD ne fournit pas d’un moteur de base de données autonome et doit être accessible via un pilote de fichier. Les moteurs de ces pilotes ont généralement réduite de prise en charge pour SQL et les transactions. Par exemple, dBASE, Paradox, Btrieve ou Microsoft® FoxPro.  
  
 **Diagnostic**  
 Un enregistrement contenant les informations de diagnostic sur la dernière fonction appelée et utilisé un handle donné. Les enregistrements de diagnostic sont associés de l’environnement, connexion, l’instruction et les handles de descripteur.  
  
 **DML**  
 Langage de Manipulation de données. Instructions SQL qui manipulent, par opposition à définir, des données. Par exemple, **insérer**, **mise à jour**, **supprimer**, et **sélectionnez**.  
  
 **Pilote**  
 Une bibliothèque de routine qui expose les fonctions de l’API ODBC. Les pilotes sont spécifiques à un système SGBD unique.  
  
 **Gestionnaire de pilotes**  
 Une bibliothèque de routine qui gère l’accès aux pilotes pour l’application. Le Gestionnaire de pilotes charge et décharge (ou se connecte et se déconnecte) pilotes et passe les appels aux fonctions ODBC pour le pilote approprié.  
  
 **DLL d’installation du pilote**  
 Une DLL qui contient des fonctions d’installation et de configuration spécifiques au pilote.  
  
 **curseur dynamique**  
 Un curseur de défilement capable de détecter les lignes mises à jour, supprimées ou insérées dans le jeu de résultats.  
  
 **Instructions SQL dynamiques**  
 Type d’embedded SQL dans SQL instructions sont créées et compilées au moment de l’exécution. *Voir aussi* SQL statique.  
  
## <a name="e"></a>E  
 **Embedded SQL**  
 Les instructions SQL qui sont incluses directement dans un programme écrit dans un autre langage, tels que COBOL ou C. ODBC n’utilise pas embedded SQL. *Voir aussi* SQL statique *et* SQL dynamique.  
  
 **Environnement**  
 Un contexte global de l’accès aux données ; associé à l’environnement est toute information globale par nature, telle qu’une liste de toutes les connexions dans cet environnement.  
  
 **handle d’environnement**  
 Handle vers une structure de données qui contient des informations sur l’environnement.  
  
 **clause d’échappement**  
 Une clause dans une instruction SQL.  
  
 **execute**  
 Pour exécuter une instruction SQL.  
  
## <a name="f"></a>F  
 **curseur FAT**  
 *Consultez* curseur de bloc.  
  
 **extraction**  
 Pour récupérer une ou plusieurs lignes à partir d’un jeu de résultats.  
  
 **field**  
 *Consultez* colonne.  
  
 **fichiers de pilote**  
 Un pilote qui accède directement aux données physiques. Dans ce cas, le pilote contient un moteur de base de données et sert de pilote et de source de données.  
  
 **source de données fichier**  
 Une source de données pour quelle connexion les informations sont stockées dans un fichier .dsn.  
  
 **clé étrangère**  
 Une ou plusieurs colonnes dans une table qui correspondent à la clé primaire dans une autre table.  
  
 **Curseur avant uniquement**  
 Un curseur qui peut uniquement mettre en œuvre via le jeu de résultats et généralement n'extrait qu’une seule ligne à la fois. La plupart des bases de données relationnelles prennent en charge uniquement les curseurs avant uniquement.  
  
## <a name="h"></a>H  
 **Handle**  
 Une valeur qui identifie de façon unique un élément comme une structure de fichiers ou de données. Poignées sont uniquement explicites pour le logiciel qui crée et utilise les mais qui est passé par d’autres logiciels pour identifier les éléments. ODBC définit des handles pour les environnements, les connexions, les instructions et les descripteurs.  
  
## <a name="i"></a>I  
 **Descripteur de paramètre d’implémentation (IPD)**  
 Un descripteur qui décrit les paramètres dynamiques dans une instruction SQL après une conversion définie par l’application.  
  
 **Descripteur de ligne d’implémentation (IRD)**  
 Un descripteur qui décrit une ligne de données avant toute conversion spécifiée par l’application.  
  
 **Programme d’installation DLL**  
 Une DLL qui installe des composants ODBC et configure les sources de données.  
  
 **Integrity Enhancement Facility**  
 Un sous-ensemble de SQL conçue pour maintenir l’intégrité d’une base de données.  
  
 **niveau de conformité d’interface**  
 Le niveau de l’interface ODBC 3.7 pris en charge par un pilote. peut être Core, de niveau 1 ou de niveau 2.  
  
 **Interopérabilité**  
 Capacité d’une application à utiliser le même code lors de l’accès aux données dans le SGBD différents.  
  
 **IPD**  
 *Consultez* descripteur de paramètre d’implémentation (IPD).  
  
 **IRD**  
 *Consultez* descripteur de ligne d’implémentation (IRD).  
  
 **ISO/IEC**  
 Commission électrotechnique d’organisation/International normes internationales. L’API ODBC est basée sur l’Interface au niveau de l’appel de norme ISO/IEC.  
  
## <a name="j"></a>J  
 **Joindre**  
 Une opération dans une base de données relationnelle qui lie les lignes de deux ou plusieurs tables en faisant correspondre les valeurs dans les colonnes spécifiées.  
  
## <a name="k"></a>K  
 **clé**  
 Une ou plusieurs colonnes dont les valeurs identifient une ligne. *Voir aussi* clé étrangère *et* clé primaire.  
  
 **Jeu de clés**  
 Un jeu de clés utilisé par un curseur mixte ou pilotés par pour extraire des lignes.  
  
 **curseur keyset**  
 Un curseur de défilement qui détecte les lignes mises à jour et supprimées à l’aide d’un jeu de clés.  
  
## <a name="l"></a>L  
 **literal**  
 Représentation sous forme de caractères de la valeur réelle des données dans une instruction SQL.  
  
 **Verrouillage**  
 Le processus par lequel un SGBD limite l’accès à une ligne dans un environnement multi-utilisateur. Le SGBD définit généralement un bit sur une ligne ou de la page physique qui contient une ligne qui indique la ligne ou page est verrouillée.  
  
 **données de type long**  
 Toutes les données binaires ou de caractères sur une certaine longueur, telles que 255 octets ou de caractères. En général beaucoup plus longue. Ce type de données est généralement envoyé à et récupérée à partir de la source de données dans les parties. Également appelé *BLOB*s ou *CLOB*s.  
  
## <a name="m"></a>M  
 **source de données machine**  
 Une source de données pour quelle connexion les informations sont stockées sur le système (par exemple, le Registre).  
  
 **mode de validation manuelle**  
 Un mode de validation de transaction dans laquelle les transactions doivent être explicitement validées en appelant **SQLTransact**.  
  
 **Métadonnées**  
 Données qui décrivent un paramètre dans une instruction SQL ou une colonne dans un jeu de résultats. Par exemple, le type de données, longueur d’octet et la précision d’un paramètre.  
  
 **pilotes à plusieurs niveaux**  
 *Consultez* pilote SGBD.  
  
## <a name="n"></a>N  
 **Valeur NULL**  
 N’avoir aucune valeur explicitement affectée. En particulier, une valeur NULL est différente de zéro ou une valeur vide.  
  
## <a name="o"></a>O  
 **octet**  
 Huit bits ou un octet. *Voir aussi* octets.  
  
 **longueur en octets**  
 La longueur en octets d’une mémoire tampon ou les données qu’il contient.  
  
 **ODBC**  
 Connectivité de base de données ouverte. Spécification d’API qui définit un ensemble standard de routines avec lequel une application peut accéder aux données dans une source de données.  
  
 **Administrateur ODBC**  
 Un programme exécutable qui appelle la DLL pour configurer des sources de données de programme d’installation.  
  
 Ouvrir le groupe  
 Une société qui publie des normes. En particulier, il publie des normes de groupe d’accès SQL (trous).  
  
 **Accès concurrentiel optimiste**  
 Une stratégie pour accroître la simultanéité dans laquelle les lignes ne sont pas verrouillées. Au lieu de cela, avant d’être mis à jour ou supprimés, un curseur vérifie si elles ont été modifiés depuis leur dernière lecture. Dans ce cas, la mise à jour ou la suppression échoue. *Voir aussi* d’accès concurrentiel pessimiste.  
  
 **jointure externe**  
 Jointure dans les lignes sans correspondance et correspondants sont retournés. Les valeurs de toutes les colonnes de la table sans correspondance dans les lignes non correspondantes sont définies avec la valeur NULL.  
  
 **propriétaire**  
 Le propriétaire d’une table.  
  
## <a name="p"></a>P  
 **parameter**  
 Une variable dans une instruction SQL, marqués avec un marqueur de paramètre ou d’un point d’interrogation ( ?). Paramètres liés aux variables d’application et leurs valeurs récupérées lors de l’instruction est exécutée.  
  
 **descripteur de paramètre**  
 Un descripteur qui décrit les paramètres d’exécution utilisés dans une instruction SQL, soit avant toute conversion spécifié par l’application (APD ou un descripteur de paramètre d’application) ou après une conversion définie par l’application (un descripteur de paramètre d’implémentation ou IPD).  
  
 **tableau d’opération de paramètres**  
 Un tableau contenant les valeurs pour une application peut définir pour indiquer que le paramètre correspondant doit être ignoré dans une **SQLExecDirect** ou **SQLExecute** opération.  
  
 **tableau de paramètres état**  
 Un tableau qui contient l’état d’un paramètre après un appel à **SQLExecDirect** ou **SQLExecute**.  
  
 **l’accès concurrentiel pessimiste**  
 Une stratégie pour l’implémentation de la sérialisation, dans lequel les lignes sont verrouillées afin que les autres transactions ne peuvent pas les modifier. *Voir aussi* d’accès concurrentiel optimiste *et* la capacité à sérialiser.  
  
 **mise à jour positionnée**  
 Toute opération qui agit sur la ligne actuelle. Par exemple, positionné mise à jour et supprimer des instructions, **SQLGetData**, et **SQLSetPos**.  
  
 **instruction de mise à jour positionnée**  
 Une instruction SQL utilisée pour mettre à jour les valeurs dans la ligne actuelle.  
  
 **instruction positionnée delete**  
 Une instruction SQL utilisée pour supprimer la ligne actuelle.  
  
 **Préparer**  
 Pour compiler une instruction SQL. Un plan d’accès est créé par la préparation d’une instruction SQL.  
  
 **Clé primaire**  
 Une ou plusieurs colonnes qui identifie de façon unique une ligne dans une table.  
  
 **procedure**  
 Un groupe d’un ou plusieurs précompilé d’instructions SQL qui sont stockées sous la forme d’un objet nommé dans une base de données.  
  
 **colonne de procédure**  
 Un argument dans un appel de procédure, la valeur retournée par une procédure ou une colonne dans un jeu de résultats créé par une procédure.  
  
## <a name="q"></a>Q  
 **Qualificateur**  
 Une base de données qui contient une ou plusieurs tables.  
  
 **Requête**  
 Une instruction SQL. Parfois utilisé pour signifier un **sélectionnez** instruction.  
  
 **identificateur entre guillemets**  
 Un identificateur est placé entre guillemets identificateur afin qu’il peut contenir des caractères spéciaux ou correspondre aux mots clés (également appelées dans SQL-92 comme un identificateur délimité).  
  
## <a name="r"></a>R  
 **radical**  
 La base d’un système de nombre. Généralement de 2 ou 10.  
  
 **Enregistrement**  
 *Consultez* ligne.  
  
 **jeu de résultats**  
 L’ensemble de lignes créé en exécutant un **sélectionnez** instruction.  
  
 **Code de retour**  
 La valeur retournée par une fonction ODBC.  
  
 **restaurer**  
 Pour retourner les valeurs modifiées par une transaction à leur état d’origine.  
  
 **Ligne**  
 Un ensemble de colonnes associées qui décrivent une entité spécifique. Également appelé un *enregistrement*.  
  
 **descripteur de ligne**  
 Un descripteur qui décrit les colonnes d’un jeu de résultats, avant toute conversion spécifié par l’application (un descripteur de ligne d’implémentation ou IRD) ou après toute conversion spécifié par l’application (un descripteur de ligne d’application ou ARD).  
  
 **tableau d’opérations de ligne**  
 Un tableau contenant les valeurs pour une application peut définir pour indiquer que la ligne correspondante doit être ignorée dans un **SQLSetPos** opération.  
  
 **tableau d’état de ligne**  
 Un tableau qui contient l’état d’une ligne après un appel à **SQLFetch**, **SQLFetchScroll**, ou **SQLSetPos**.  
  
 **ensemble de lignes**  
 L’ensemble de lignes retournées par une seule extraction en un curseur de bloc.  
  
 **tampons de l’ensemble de lignes**  
 Les mémoires tampons liées aux colonnes d’un jeu de résultats et dans les données pour un ensemble de lignes entier sont retournées.  
  
## <a name="s"></a>S  
 **TROUS**  
 *Consultez* groupe d’accès SQL (trous).  
  
 **fonction scalaire**  
 Une fonction qui génère une valeur unique à partir d’une valeur unique. Par exemple, une fonction qui modifie la casse des données de type caractère.  
  
 **schema**  
 *Consultez* catalogue.  
  
 **curseur de défilement**  
 Un curseur que vous pouvez vous déplacer vers l’avant ou vers l’arrière dans le jeu de résultats.  
  
 **Sérialisation**  
 Si deux transactions s’exécutant simultanément produisent un résultat qui est identique à l’exécution de série (ou séquentielle) de ces transactions. Les transactions sont nécessaires pour préserver l’intégrité de base de données.  
  
 **base de données du serveur**  
 Un SGBD est conçu pour être exécuté dans un environnement client/serveur. Ces SGBD fournit un moteur de base de données autonome qui fournit la prise en charge complète pour SQL et les transactions. Ils sont accessibles via les pilotes basés sur SGBD. Par exemple, Oracle, Informix, DB/2 ou Microsoft® SQL Server.  
  
 **Set (fonction)**  
 *Consultez* fonction d’agrégation.  
  
 **DLL d’installation**  
 *Consultez* DLL d’installation du pilote *et* DLL d’installation du traducteur.  
  
 **pilote de niveau unique**  
 *Consultez* basée sur le fichier de pilote.  
  
 **SQL**  
 Langage de requêtes structurées. Langage utilisé par les bases de données relationnelles pour interroger, mettre à jour et gérer les données.  
  
 **Groupe d’accès SQL (trous)**  
 Un consortium industriel de sociétés concernées par le SGBD de SQL. Interface de niveau d’appel de The Open Group est basée sur le travail effectué à l’origine par le groupe d’accès SQL.  
  
 **Niveau de conformité SQL**  
 Le niveau de grammaire SQL-92 pris en charge par un pilote. peut être entrée, FIPS transitoires, intermédiaire ou intégral.  
  
 **Type de données SQL**  
 Le type de données d’une colonne ou un paramètre tel qu’il est stocké dans la source de données.  
  
 **SQLSTATE**  
 Une valeur de cinq caractères qui indique une erreur particulière.  
  
 **Instruction SQL**  
 Une phrase complète dans SQL qui commence par un mot clé et complètement décrit une action à entreprendre. Par exemple, sélectionnez * à partir de commandes. Instructions SQL ne doivent pas être confondues avec les instructions.  
  
 **state**  
 Une condition bien définie d’un élément. Par exemple, une connexion a sept États, y compris les données non allouées, allouées, connectées et nécessitant. Certaines opérations peuvent être effectuées uniquement lorsqu’un élément est dans un état particulier. Par exemple, une connexion peut être libérée uniquement lorsqu’il est dans un état alloué et non, par exemple, lorsqu’il est en état connecté.  
  
 **transition d’état**  
 Déplacement d’un élément d’un état à un autre. ODBC définit des transitions d’état rigoureux pour les environnements, les connexions et les instructions.  
  
 **instruction**  
 Un conteneur pour toutes les informations relatives à une instruction SQL. Les instructions ne doivent pas être confondues avec des instructions SQL.  
  
 **descripteur d’instruction**  
 Handle vers une structure de données qui contient des informations sur une instruction.  
  
 **curseur statique**  
 Un curseur de défilement qui ne peut pas détecter les mises à jour, supprime ou insère dans le jeu de résultats. Généralement implémentée en effectuant une copie du jeu de résultats.  
  
 **SQL statique**  
 Type d’embedded SQL dans SQL instructions sont codées en dur et compilées lorsque le reste du programme est compilé. *Voir aussi* SQL dynamique.  
  
 **Procédure stockée**  
 *Consultez* procédure.  
  
## <a name="t"></a>T  
 **table**  
 Une collection de lignes.  
  
 **Thunking**  
 La conversion de 16 bits résout les adresses 32 bits, ou inversement, lorsque les applications 16 bits sont utilisées avec les pilotes ODBC 32 bits.  
  
 **transaction**  
 Une unité atomique de travail. Le travail dans une transaction doit être effectué dans son ensemble ; Si une partie de la transaction échoue, la transaction entière échoue.  
  
 **Isolement des transactions**  
 Le fait de l’isolation des transactions entre les effets de toutes les autres transactions.  
  
 **Niveau d’isolation de transaction**  
 Mesure de la manière dont une transaction est isolée. Il existe cinq niveaux d’isolation de transaction : Read Uncommitted, Read Committed, Repeatable Read, Serializable et le contrôle de version.  
  
 **DLL de conversion**  
 Une DLL utilisé pour convertir des données à partir d’un jeu de caractères à un autre.  
  
 **DLL d’installation du convertisseur**  
 Une DLL qui contient des fonctions d’installation et de configuration spécifiques au traducteur.  
  
 **validation en deux phases**  
 Le processus de validation d’une transaction distribuée en deux phases. Dans la première phase, le processeur de transaction vérifie que toutes les parties de la transaction peuvent être validées. Dans la deuxième phase, toutes les parties de la transaction sont validées. Si une partie de la transaction indique dans la première phase, qu’il ne peut pas être validée, la deuxième phase n’a pas lieu. ODBC ne prend pas en charge les validations en deux phases.  
  
 **indicateur de type**  
 Une valeur entière passée à ou retournée à partir d’une fonction ODBC pour indiquer le type de données d’une variable d’application, un paramètre ou une colonne. ODBC définit des indicateurs de type pour les types de données C et SQL.  
  
## <a name="v"></a>V  
 **Vue**  
 Un autre moyen de consulter les données dans une ou plusieurs tables. Une vue est généralement créée comme un sous-ensemble des colonnes à partir d’une ou plusieurs tables. Dans ODBC, les vues sont généralement équivalentes aux tables.

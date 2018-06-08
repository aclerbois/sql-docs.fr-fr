---
title: Fonction SQLBrowseConnect | Documents Microsoft
ms.custom: ''
ms.date: 01/19/2017
ms.prod: sql
ms.prod_service: connectivity
ms.reviewer: ''
ms.suite: sql
ms.technology: connectivity
ms.tgt_pltfrm: ''
ms.topic: conceptual
apiname:
- SQLBrowseConnect
apilocation:
- sqlsrv32.dll
apitype: dllExport
f1_keywords:
- SQLBrowseConnect
helpviewer_keywords:
- SQLBrowseConnect function [ODBC]
ms.assetid: b7f1be66-e6c7-4790-88ec-62b7662103c0
caps.latest.revision: 36
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: 66daf3ce016b8688ada1d584865a97675a7dbc01
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="sqlbrowseconnect-function"></a>Fonction SQLBrowseConnect
**Mise en conformité**  
 Version introduite : La mise en conformité des normes 1.0 ODBC : ODBC  
  
 **Résumé**  
 **SQLBrowseConnect** prend en charge une méthode itérative de détection et d’énumération des attributs et des valeurs d’attribut requises pour se connecter à une source de données. Chaque appel à **SQLBrowseConnect** renvoie les niveaux successifs d’attributs et valeurs d’attribut. Lorsque tous les niveaux ont été énumérés, une connexion à la source de données est terminée et une chaîne de connexion complète est retournée par **SQLBrowseConnect**. Un code de retour de SQL_SUCCESS ou SQL_SUCCESS_WITH_INFO indique que toutes les informations de connexion a été spécifiées et que l’application est maintenant connectée à la source de données.  
  
## <a name="syntax"></a>Syntaxe  
  
```  
  
SQLRETURN SQLBrowseConnect(  
     SQLHDBC         ConnectionHandle,  
     SQLCHAR *       InConnectionString,  
     SQLSMALLINT     StringLength1,  
     SQLCHAR *       OutConnectionString,  
     SQLSMALLINT     BufferLength,  
     SQLSMALLINT *   StringLength2Ptr);  
```  
  
## <a name="arguments"></a>Arguments  
 *Handle de connexion*  
 [Entrée] Handle de connexion.  
  
 *InConnectionString*  
 [Entrée] Rechercher la chaîne de connexion de requête (voir «*InConnectionString* Argument » dans « Commentaires »).  
  
 *StringLength1*  
 [Entrée] Longueur de **InConnectionString* en caractères.  
  
 *OutConnectionString*  
 [Sortie] Pointeur vers une mémoire tampon de caractères dans lequel retourner la chaîne de connexion résultats Parcourir (voir «*OutConnectionString* Argument » dans « Commentaires »).  
  
 Si *OutConnectionString* est NULL, *StringLength2Ptr* retourne toujours le nombre total de caractères (sans le caractère de fin de la valeur null pour les données de type caractère) disponibles à renvoyer dans la mémoire tampon vers laquelle pointée *OutConnectionString*.  
  
 *BufferLength*  
 [Entrée] Longueur, en caractères, de la **OutConnectionString* mémoire tampon.  
  
 *StringLength2Ptr*  
 [Sortie] Le nombre total de caractères (à l’exclusion des arrêts de null) disponibles à renvoyer dans \* *OutConnectionString*. Si le nombre de caractères à retourner est supérieur ou égal à *BufferLength*, la chaîne de connexion dans \* *OutConnectionString* est tronqué à *BufferLength* moins la longueur d’un caractère de fin de la valeur null.  
  
## <a name="returns"></a>Valeur renvoyée  
 SQL_SUCCESS, SQL_SUCCESS_WITH_INFO, SQL_NEED_DATA, SQL_ERROR, SQL_INVALID_HANDLE ou SQL_STILL_EXECUTING.  
  
## <a name="diagnostics"></a>Diagnostics  
 Lorsque **SQLBrowseConnect** retourne SQL_NEED_DATA, une valeur SQLSTATE associée, SQL_SUCCESS_WITH_INFO ou SQL_ERROR peut être obtenu en appelant **SQLGetDiagRec** avec un *HandleType* de SQL_HANDLE_STMT et un *gérer de handle de connexion*. Le tableau suivant répertorie les valeurs SQLSTATE généralement retournées par **SQLBrowseConnect** et explique chacune d’elles dans le contexte de cette fonction ; la notation « (DM) » précède les descriptions de SQLSTATE retournée par le Gestionnaire de pilotes. Le code de retour associé à chaque valeur SQLSTATE est SQL_ERROR, sauf indication contraire.  
  
|SQLSTATE|Erreur| Description|  
|--------------|-----------|-----------------|  
|01000|Avertissement général|Message d’information de spécifiques au pilote. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|01004|Données de type chaîne, droite tronquées|La mémoire tampon \* *OutConnectionString* n’est pas suffisamment grande pour retourner la chaîne de connexion résultats parcourir ensemble, donc la chaîne a été tronquée. La mémoire tampon **StringLength2Ptr* contient la longueur de la chaîne de connexion de résultat non tronqué Parcourir. (La fonction retourne SQL_NEED_DATA.)|  
|01 S 00|Attribut de chaîne de connexion non valide|Un mot clé d’attribut non valide a été spécifié dans la chaîne de connexion de demande de navigation (*InConnectionString*). (La fonction retourne SQL_NEED_DATA.)<br /><br /> Un mot clé d’attribut a été spécifié dans la chaîne de connexion de demande de navigation (*InConnectionString*) qui ne s’applique pas au niveau de la connexion actuelle. (La fonction retourne SQL_NEED_DATA.)|  
|01S02|Valeur modifiée|Le pilote ne prenait pas en charge la valeur spécifiée de la *ValuePtr* argument dans **SQLSetConnectAttr** et une valeur similaire. (La fonction retourne SQL_SUCCESS_WITH_INFO).|  
|08001|Impossible d’établir la connexion du client|Le pilote n’a pas pu établir une connexion avec la source de données.|  
|08002|Nom de la connexion en cours d’utilisation|(DM) la connexion spécifiée a déjà été utilisée pour établir une connexion avec une source de données et la connexion a été ouverte.|  
|08004|Le serveur a rejeté la connexion|La source de données a rejeté l’établissement de la connexion pour des raisons de défini par l’implémentation.|  
|08S01|Échec de lien de communication|Échec de la liaison de communication entre le pilote et la source de données à laquelle le pilote a tenté de se connecter avant le traitement de la fonction a été exécutée.|  
|28000|Spécification d’autorisation non valide|L’identificateur d’utilisateur ou la chaîne d’autorisation ou les deux, comme spécifié dans l’option Parcourir demander la chaîne de connexion (*InConnectionString*), violation des restrictions définies par la source de données.|  
|HY000|Erreur générale|Une erreur s’est produite pour laquelle aucun code SQLSTATE spécifique est survenu et pour lequel aucune SQLSTATE spécifique à l’implémentation a été définie. Le message d’erreur retourné par **SQLGetDiagRec** dans les  *\*MessageText* tampon décrit l’erreur et sa cause.|  
|HY001|Erreur d’allocation de mémoire|Le Gestionnaire de pilotes (DM) n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou à l’achèvement de la fonction.<br /><br /> Le pilote n’a pas pu allouer la mémoire requise pour prendre en charge l’exécution ou à l’achèvement de la fonction.|  
|HY008|Opération annulée|Une opération asynchrone a été annulée en appelant [SQLCancelHandle fonction](../../../odbc/reference/syntax/sqlcancelhandle-function.md). Ensuite, la fonction d’origine a été appelée à nouveau sur le *handle de connexion*.<br /><br /> Une opération a été annulée en appelant **SQLCancelHandle** sur la *handle de connexion* à partir d’un autre thread dans une application multithread.|  
|HY010|Erreur de séquence de fonction|(DM), une fonction de façon asynchrone en cours d’exécution (pas celui-ci) a été appelée pour le *handle de connexion* et toujours en cours d’exécution lorsque cette fonction a été appelée.|  
|HY013|Erreur de gestion de mémoire|L’appel de fonction n’a pas pu être traité, car les objets sous-jacents de la mémoire ne sont pas accessible, éventuellement en raison d’une mémoire insuffisante.|  
|HY090|Longueur de chaîne ou une mémoire tampon non valide|(DM) la valeur spécifiée pour l’argument *StringLength1* était inférieure à 0 et n’est pas égale à SQL_NTS.<br /><br /> (DM) la valeur spécifiée pour l’argument *BufferLength* était inférieure à 0.|  
|HY114|Pilote ne prend pas en charge l’exécution de fonction asynchrone au niveau de connexion|(DM) l’application activée à l’opération asynchrone sur le handle de connexion avant d’établir la connexion. Toutefois, le pilote ne prend pas en charge l’opération asynchrone sur le handle de connexion.|  
|HYT00|Délai d'expiration dépassé|Le délai d’expiration de connexion a expiré avant la connexion à la source de données s’est terminée. Le délai d’expiration est défini par le **SQLSetConnectAttr**, SQL_ATTR_LOGIN_TIMEOUT.|  
|HYT01|Délai de connexion a expiré.|Le délai d’expiration de connexion a expiré avant que la source de données a répondu à la demande. Le délai d’expiration de connexion est défini par le **SQLSetConnectAttr**, SQL_ATTR_CONNECTION_TIMEOUT.|  
|IM001|Pilote ne prend pas en charge cette fonction|(DM) le pilote correspondant au nom de source de données spécifié ne prend pas en charge la fonction.|  
|IM002|Source de données introuvable et aucun pilote par défaut spécifié|(DM) les source de données spécifiée dans la chaîne de connexion de demande de navigation (*InConnectionString*) est introuvable dans les informations système, ni existait une spécification de pilote par défaut.<br /><br /> (DM) ODBC par défaut et la source de pilote de données est introuvable dans les informations système.|  
|IM003|Pilote spécifié n’a pas pu être chargé.|(DM) le pilote répertorié dans la spécification de source de données dans les informations système ou spécifié par le **pilote** mot clé est introuvable ou ne peut pas être chargé pour une raison quelconque.|  
|IM004|Le pilote **SQLAllocHandle** sur _ENV SQL_HANDLE a échoué|(DM) au cours de **SQLBrowseConnect**, le Gestionnaire de pilote appelé du pilote **SQLAllocHandle** fonctionne avec un *HandleType* de SQL_HANDLE_ENV et le pilote a renvoyé une erreur.|  
|IM005|Le pilote **SQLAllocHandle** sur SQL_HANDLE_DBC a échoué|(DM) au cours de **SQLBrowseConnect**, le Gestionnaire de pilote appelé du pilote **SQLAllocHandle** fonctionne avec un *HandleType* de SQL_HANDLE_DBC et le pilote a renvoyé une erreur.|  
|IM006|Le pilote **SQLSetConnectAttr** a échoué|(DM) au cours de **SQLBrowseConnect**, le Gestionnaire de pilote appelé du pilote **SQLSetConnectAttr** fonction et le pilote a renvoyé une erreur.|  
|IM009|Impossible de charger la DLL de traduction|Le pilote n’a pas pu charger la DLL a été spécifiée pour la source de données ou pour la connexion de la traduction.|  
|IM010|Nom de source de données trop long|(DM) la valeur d’attribut pour le mot clé DSN a été plue SQL_MAX_DSN_LENGTH caractères.|  
|IM011|Nom de pilote trop long|(DM) la valeur d’attribut pour le mot clé DRIVER a plus de 255 caractères.|  
|IM012|Erreur de syntaxe du mot clé DRIVER|(DM) la paire mot clé-valeur pour le mot clé DRIVER contenait une erreur de syntaxe.|  
|IM014|La source de données spécifié contient une incompatibilité d’architecture entre le pilote et l’Application|Application de 32 bits (DM) utilise un DSN de la connexion à un pilote 64 bits. ou vice versa.|  
|IM017|L’interrogation est désactivée en mode de notification asynchrone|Chaque fois que le modèle de notification est utilisé, l’interrogation est désactivée.|  
|IM018|**SQLCompleteAsync** n’a pas été appelé pour terminer l’opération asynchrone précédente sur ce handle.|Si l’appel de fonction précédente sur le handle retourne SQL_STILL_EXECUTING et si le mode de notification est activé, **SQLCompleteAsync** doit être appelée sur le handle de post-traitement et terminer l’opération.|  
|S1118|Pilote ne prend pas en charge la notification asynchrone|Lorsque le pilote ne prend pas en charge la notification asynchrone, Impossible de définir SQL_ATTR_ASYNC_DBC_EVENT ou SQL_ATTR_ASYNC_DBC_RETCODE_PTR.|  
  
## <a name="inconnectionstring-argument"></a>Argument de InConnectionString  
 Une chaîne de connexion Parcourir présente la syntaxe suivante :  
  
 *chaîne de connexion* :: = *attribut*[`;`] &#124; *attribut* `;` *chaîne de connexion*;<br>
 *attribute* ::= *attribute-keyword*`=`*attribute-value* &#124; `DRIVER=`[`{`]*attribute-value*[`}`]<br>
 *mot clé de l’attribut* :: = `DSN` &#124; `UID` &#124; `PWD` &#124; *-défini-attribut-mot clé driver*<br>
 *attribute-value* ::= *character-string*<br>
 *défini-attribut-mot clé Driver* :: = *identificateur*<br>
  
 où *chaîne de caractères* a zéro ou plusieurs caractères ; *identificateur* a un ou plusieurs caractères ; *mot clé de l’attribut* ne respecte pas la casse ; *attribut-valeur* peut respecter la casse ; et la valeur de la **DSN** mot clé n’est pas constitué uniquement d’espaces. En raison de l’initialisation et la chaîne de fichier grammaire, mots clés et l’attribut valeurs de connexion qui contient les caractères **[]{}(), ? \*= ! @** doit être évitée. En raison de la grammaire dans les informations système, les noms de sources de données et les mots clés ne peut pas contenir la barre oblique inverse (\\) caractères. Pour une application ODBC 2. *x* pilote, accolades sont obligatoires pour la valeur d’attribut pour le mot clé DRIVER.  
  
 Si tous les mots clés sont répétés dans la chaîne de connexion de demande de parcourir, le pilote utilise la valeur associée à la première occurrence du mot clé. Si le **DSN** et **pilote** mots clés sont inclus dans la même chaîne de connexion de demande de parcourir, le Gestionnaire de pilotes et le pilote utilisent le mot clé qui apparaît en premier.  
  
 Pour plus d’informations sur la façon dont une application choisit un pilote ou une source de données, consultez [choisir une Source de données ou le pilote](../../../odbc/reference/develop-app/choosing-a-data-source-or-driver.md).  
  
## <a name="outconnectionstring-argument"></a>Argument de OutConnectionString  
 La chaîne de connexion Parcourir résultat est une liste des attributs de connexion. Un attribut de connexion se compose d’un mot clé d’attribut et une valeur d’attribut correspondant. La chaîne de connexion résultats Parcourir présente la syntaxe suivante :  
  
 *chaîne de connexion* :: = *attribut*[`;`] &#124; *attribut* `;` *chaîne de connexion*<br>
 *attribute* ::= [`*`]*attribute-keyword*`=`*attribute-value*<br>
 *attribute-keyword* ::= *ODBC-attribute-keyword* &#124; *driver-defined-attribute-keyword*<br>
 *Mot clé ODBC attribut* = {`UID` &#124; `PWD`} [`:`*identificateur localisée*] *-défini-attribut-mot clé driver* :: = *identificateur*[`:`*identificateur localisée*] *attribut-valeur* :: = `{` *attribut-valeur-list* `}` &#124; `?` (Les accolades sont littéral ; ils sont retournés par le pilote).<br>
 *liste de valeurs d’attribut* :: = *chaîne de caractères* [`:`*chaîne de caractères localisés*] &#124; *chaîne de caractères* [`:` *chaîne de caractères localisés*] `,` *liste de valeurs d’attribut*<br>
  
 où *chaîne de caractères* et *chaîne de caractères localisés* avoir zéro ou plusieurs caractères ; *identificateur* et *identificateur localisée* ont un ou plusieurs caractères ; *mot clé de l’attribut* ne respecte pas la casse ; et *attribut-valeur* peut respecter la casse. En raison de la connexion chaîne et l’initialisation du fichier grammaire, les mots clés et les identificateurs localisées et valeurs d’attribut qui contient les caractères **[]{}(), ? \*= ! @** doit être évitée. En raison de la grammaire dans les informations système, les noms de sources de données et les mots clés ne peut pas contenir la barre oblique inverse (\\) caractères.  
  
 La syntaxe de chaîne de connexion résultats Parcourir est utilisée selon les règles sémantiques suivantes :  
  
-   Si un astérisque (\*) précède un *mot clé de l’attribut*, le *attribut* est facultatif et peut être omis dans l’appel suivant à **SQLBrowseConnect**.  
  
-   Les mots clés d’attribut **UID** et **PWD** ont la même signification, tel que défini dans **SQLDriverConnect**.  
  
-   A *-défini-attribut-mot clé driver* désigne le type d’attribut pour lequel une valeur d’attribut peut être fournie. Par exemple, il peut être **SERVER**, **base de données**, **hôte**, ou **SGBD**.  
  
-   *Mots clés ODBC attribut* et *pilote--attribut-mots clés définis par* comprennent une version localisée ou conviviale du mot clé. Cela peut être utilisé par les applications en tant qu’étiquette dans une boîte de dialogue. Toutefois, **UID**, **PWD**, ou *identificateur* seul doit être utilisé lors du passage d’une chaîne de requête de navigation pour le pilote.  
  
-   Le {*liste de valeurs d’attribut*} est une énumération de valeurs réelles valides pour les *mot clé de l’attribut*. Notez que les accolades ({}) n’indiquent pas une liste de choix ; ils sont retournés par le pilote. Par exemple, il peut être une liste de noms de serveur ou une liste de noms de base de données.  
  
-   Si le *attribut-valeur* est un point d’interrogation ( ?), une seule valeur correspond à la *mot clé de l’attribut*. Par exemple, UID = JohnS ; PWD = sésame.  
  
-   Chaque appel à **SQLBrowseConnect** retourne uniquement les informations nécessaires pour satisfaire le niveau suivant le processus de connexion. Le pilote associe les informations d’état avec le handle de connexion afin que le contexte peut toujours être déterminé sur chaque appel.  
  
## <a name="using-sqlbrowseconnect"></a>À l’aide de SQLBrowseConnect  
 **SQLBrowseConnect** nécessite une connexion allouée. Le Gestionnaire de pilotes charge le pilote qui a été spécifié dans ou qui correspond au nom de source de données spécifié dans la chaîne de connexion initiale Parcourir demande ; Pour plus d’informations sur lorsque cela se produit, consultez la section « Commentaires » dans [fonction SQLConnect](../../../odbc/reference/syntax/sqlconnect-function.md). Le pilote peut établir une connexion avec la source de données pendant le processus de navigation. Si **SQLBrowseConnect** retourne SQL_ERROR, en attente, les connexions sont interrompues et la connexion est retournée à un état non connecté.  
  
> [!NOTE]  
>  **SQLBrowseConnect** ne prend pas en charge le regroupement de connexions. Si **SQLBrowseConnect** est appelée alors que le regroupement de connexions est activé, SQLSTATE HY000 (erreur générale) est retournée.  
  
 Lorsque **SQLBrowseConnect** est appelée pour la première fois sur une connexion, doit contenir la chaîne de connexion de demande de parcourir le **DSN** mot clé ou le **pilote** (mot clé). Si la chaîne de connexion de demande de navigation contient le **DSN** (mot clé), le Gestionnaire de pilotes localise une spécification de source de données correspondant dans les informations système :  
  
-   Si le Gestionnaire de pilote détecte la spécification de source de données correspondante, il charge le pilote associé DLL ; le pilote peut récupérer des informations sur la source de données à partir des informations système.  
  
-   Si le Gestionnaire de pilotes ne peut pas trouver la spécification de source de données correspondante, il localise la spécification de source de données par défaut et charge le pilote associé DLL ; le pilote peut récupérer des informations sur la source de données par défaut à partir des informations système. « DEFAULT » est passé au pilote pour la source de données.  
  
-   Si le Gestionnaire de pilotes ne peut pas trouver la spécification de source de données correspondante, et il n’existe aucune spécification de source de données par défaut, elle retourne SQL_ERROR avec SQLSTATE IM002 (source de données introuvable et aucun pilote par défaut spécifié).  
  
 Si la chaîne de connexion de demande de navigation contient le **pilote** (mot clé), le Gestionnaire de pilotes charge le pilote spécifié ; il ne tente pas de trouver une source de données dans les informations système. Étant donné que la **pilote** mot-clé n’utilise pas les informations à partir des informations système, le pilote doit définir suffisamment de mots clés afin qu’un pilote peut se connecter à une source de données à l’aide uniquement les informations dans les chaînes de connexion de demande de parcourir.  
  
 Sur chaque appel à **SQLBrowseConnect**, l’application spécifie les valeurs d’attribut de connexion dans la chaîne de connexion de demande de navigation. Le pilote retourne les niveaux successifs d’attributs et valeurs d’attribut dans la chaîne de connexion résultats de navigation ; Il retourne SQL_NEED_DATA tant qu’il existe des attributs de connexion qui n’ont pas encore été énumérés dans la chaîne de connexion de demande de navigation. L’application utilise la valeur de la chaîne de connexion résultats Parcourir pour générer la chaîne de connexion de demande de navigation pour le prochain appel à **SQLBrowseConnect**. Tous les attributs obligatoires (ceux non précédé d’un astérisque dans le *OutConnectionString* argument) doit être inclus dans le prochain appel à **SQLBrowseConnect**. Notez que l’application ne peut pas utiliser le contenu de chaînes de connexion de résultat Parcourir précédente lors de la création de la chaîne de connexion de la demande de navigation en cours ; Autrement dit, il ne peut pas spécifier des valeurs différentes pour les attributs définis dans des niveaux précédents.  
  
 Lorsque tous les niveaux de connexion et leurs attributs associés ont été énumérés, le pilote retourne SQL_SUCCESS, la connexion à la source de données est terminée et une chaîne de connexion complète est retournée à l’application. La chaîne de connexion est recommandée d’utiliser conjointement avec **SQLDriverConnect**, avec l’option SQL_DRIVER_NOPROMPT pour établir une autre connexion. La chaîne de connexion complète ne peut pas être utilisée dans un autre appel à **SQLBrowseConnect**, toutefois, si **SQLBrowseConnect** était de nouveau appelée, l’intégralité de la séquence d’appels devrait être répété.  
  
 **SQLBrowseConnect** également retourne SQL_NEED_DATA s’il existe des erreurs non récupérables pendant le processus de parcourir ; par exemple, un mot de passe non valide ou le mot clé d’attribut fourni par l’application. Lorsque SQL_NEED_DATA est retourné et la chaîne de connexion résultats Parcourir est inchangée, une erreur s’est produite et l’application peut appeler **SQLGetDiagRec** pour retourner la valeur SQLSTATE pour les erreurs de navigation. Cela permet à l’application pour corriger l’attribut et continuer au Parcourir.  
  
 Une application peut mettre fin au processus de parcourir à tout moment en appelant **SQLDisconnect**. Le pilote se mettre fin à toutes les connexions en attente et la connexion à un état non connecté.  
  
 Si les opérations asynchrones sont activées sur le handle de connexion, **SQLBrowseConnect** peut également retourner SQL_STILL_EXECUTING. Lorsqu’elle retourne SQL_NEED_DATA, une application doit utiliser **SQLDisconnect** pour annuler le processus de navigation. Si **SQLBrowseConnect** retourne SQL_STILL_EXECUTING, une application doit utiliser **SQLCancelHandle** pour annuler l’opération en cours. Appel de **SQLCancelHandle** une fois que la fonction retourne SQL_NEED_DATA n’a aucun effet.  
  
 Pour plus d’informations, consultez [connexion avec SQLBrowseConnect](../../../odbc/reference/develop-app/connecting-with-sqlbrowseconnect.md).  
  
 Si un pilote prend en charge **SQLBrowseConnect**, la section de mot clé driver dans les informations système pour le pilote doit contenir le **ConnectFunctions** mot clé par le troisième caractère de la valeur « Y ».  
  
## <a name="code-example"></a>Exemple de code  
  
> [!NOTE]  
>  Si vous vous connectez à un fournisseur de source de données qui prend en charge l’authentification Windows, vous devez spécifier `Trusted_Connection=yes` au lieu des informations d’ID et mot de passe utilisateur dans la chaîne de connexion.  
  
 Dans l’exemple suivant, une application appelle **SQLBrowseConnect** à plusieurs reprises. Chaque fois que **SQLBrowseConnect** retourne SQL_NEED_DATA, il passe des informations sur les données nécessaires dans \* *OutConnectionString*. L’application passe *OutConnectionString* à sa routine **GetUserInput** (non affichée). **GetUserInput** analyse les informations, génère et affiche une boîte de dialogue et retourne les informations entrées par l’utilisateur dans \* *InConnectionString*. L’application transmet les informations d’utilisateur pour le pilote dans le prochain appel à **SQLBrowseConnect**. Une fois que l’application a fourni toutes les informations nécessaires pour le pilote pour se connecter à la source de données **SQLBrowseConnect** retourne SQL_SUCCESS et l’application se poursuit.  
  
 Pour obtenir un exemple plus détaillé de la connexion à un pilote de SQL Server en appelant **SQLBrowseConnect**, consultez [exemple de navigation de SQL Server](../../../odbc/reference/develop-app/sql-server-browsing-example.md).  
  
 Par exemple, pour vous connecter aux données de ventes de la source, les actions suivantes peuvent se produire. Tout d’abord, l’application passe la chaîne suivante à **SQLBrowseConnect**:  
  
```  
"DSN=Sales"  
```  
  
 Le Gestionnaire de pilotes charge le pilote associé à la source de données Sales. Il appelle ensuite du pilote **SQLBrowseConnect** fonction avec les mêmes arguments qu’il a reçu à partir de l’application. Le pilote retourne la chaîne suivante dans **OutConnectionString*:  
  
```  
"HOST:Server={red,blue,green};UID:ID=?;PWD:Password=?"  
```  
  
 L’application transmet cette chaîne à sa **GetUserInput** routine, ce qui génère une boîte de dialogue qui demande à l’utilisateur pour sélectionner le serveur vert, rouge ou bleu et entrez un ID utilisateur et un mot de passe. La routine transmet les informations suivantes spécifié par l’utilisateur de nouveau \* *InConnectionString*, dont l’application passe à **SQLBrowseConnect**:  
  
```  
"HOST=red;UID=Smith;PWD=Sesame"  
```  
  
 **SQLBrowseConnect** utilise ces informations pour se connecter au serveur rouge comme Smith avec le mot de passe sésame, puis retourne la chaîne suivante dans **OutConnectionString*:  
  
```  
"*DATABASE:Database={SalesEmployees,SalesGoals,SalesOrders}"  
```  
  
 L’application transmet cette chaîne à sa **GetUserInput** routine, qui génère une boîte de dialogue qui demande à l’utilisateur de sélectionner une base de données. L’empdata sélectionne utilisateur et l’application appelle **SQLBrowseConnect** une dernière fois par cette chaîne :  
  
```  
"DATABASE=SalesOrders"  
```  
  
 Voici la dernière partie des informations que le pilote doit se connecter à la source de données ; **SQLBrowseConnect** retourne SQL_SUCCESS, et **OutConnectionString* contient la chaîne de connexion terminée :  
  
```  
// SQLBrowseConnect_Function.cpp  
// compile with: odbc32.lib  
#include <windows.h>  
#include <sqltypes.h>  
#include <sqlext.h>  
  
#define BRWS_LEN 100  
SQLHENV henv;  
SQLHDBC hdbc;  
SQLHSTMT hstmt;  
SQLRETURN retcode;  
SQLCHAR szConnStrIn[BRWS_LEN], szConnStrOut[BRWS_LEN];  
SQLSMALLINT cbConnStrOut;  
  
void GetUserInput(SQLCHAR * szConnStrOut, SQLCHAR * szConnStrIn) {}  
  
int main() {  
   // Allocate the environment handle.  
   retcode = SQLAllocHandle(SQL_HANDLE_ENV, SQL_NULL_HANDLE, &henv);        
   if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
  
      // Set the version environment attribute.  
      retcode = SQLSetEnvAttr(henv, SQL_ATTR_ODBC_VERSION, (SQLPOINTER*)SQL_OV_ODBC3, 0);  
      if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
  
         // Allocate the connection handle.  
         retcode = SQLAllocHandle(SQL_HANDLE_DBC, henv, &hdbc);  
         if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO) {  
            // Call SQLBrowseConnect until it returns a value other than SQL_NEED_DATA   
            // (pass data source name the first time).  If SQL_NEED_DATA is returned, call GetUserInput   
            // (not shown) to build a dialog from the values in szConnStrOut.  The user-supplied values   
            // are returned in szConnStrIn, which is passed in the next call to SQLBrowseConnect.  
  
            strcpy_s((char*)szConnStrIn, _countof(szConnStrIn), "DSN=Sales");  
            do {  
               retcode = SQLBrowseConnect(hdbc, szConnStrIn, SQL_NTS,  
                  szConnStrOut, BRWS_LEN, &cbConnStrOut);  
               if (retcode == SQL_NEED_DATA)  
                  GetUserInput(szConnStrOut, szConnStrIn);  
            } while (retcode == SQL_NEED_DATA);  
  
            if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO){  
  
               // Allocate the statement handle.  
               retcode = SQLAllocHandle(SQL_HANDLE_STMT, hdbc, &hstmt);  
  
               if (retcode == SQL_SUCCESS || retcode == SQL_SUCCESS_WITH_INFO)  
                  // Process data after successful connection  
                  SQLFreeHandle(SQL_HANDLE_STMT, hstmt);  
               SQLDisconnect(hdbc);  
            }  
         }  
         SQLFreeHandle(SQL_HANDLE_DBC, hdbc);  
      }  
   }  
   SQLFreeHandle(SQL_HANDLE_ENV, henv);  
}  
```  
  
## <a name="related-functions"></a>Fonctions connexes  
  
|Pour obtenir des informations sur|Consultez|  
|---------------------------|---------|  
|Allocation d’un descripteur de connexion|[SQLAllocHandle, fonction](../../../odbc/reference/syntax/sqlallochandle-function.md)|  
|Connexion à une source de données|[SQLConnect, fonction](../../../odbc/reference/syntax/sqlconnect-function.md)|  
|Déconnexion d’une source de données|[SQLDisconnect, fonction](../../../odbc/reference/syntax/sqldisconnect-function.md)|  
|Connexion à une source de données à l’aide d’une boîte de dialogue ou la chaîne de connexion|[SQLDriverConnect, fonction](../../../odbc/reference/syntax/sqldriverconnect-function.md)|  
|Renvoi de descriptions de pilote et attributs|[SQLDrivers, fonction](../../../odbc/reference/syntax/sqldrivers-function.md)|  
|La libération d’un handle de connexion|[SQLFreeHandle, fonction](../../../odbc/reference/syntax/sqlfreehandle-function.md)|  
  
## <a name="see-also"></a>Voir aussi  
 [Référence de l’API ODBC](../../../odbc/reference/syntax/odbc-api-reference.md)   
 [Fichiers d’en-tête ODBC](../../../odbc/reference/install/odbc-header-files.md)

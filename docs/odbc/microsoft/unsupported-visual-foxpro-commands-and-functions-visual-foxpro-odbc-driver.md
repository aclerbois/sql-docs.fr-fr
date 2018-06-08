---
title: Non pris en charge les fonctions et les commandes de Visual FoxPro | Documents Microsoft
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
- FoxPro ODBC driver [ODBC], commands and functions
- functions [ODBC], Visual FoxPro
- Visual FoxPro ODBC driver [ODBC], commands and functions
- Visual FoxPro commands and functions
- FoxPro ODBC driver
ms.assetid: afdb6b7e-738d-42ca-8053-67ae50873ca6
caps.latest.revision: 5
author: MightyPen
ms.author: genemi
manager: craigg
ms.openlocfilehash: cfef52f471f9b87e7f6560b76e191aca1ba26172
ms.sourcegitcommit: 1740f3090b168c0e809611a7aa6fd514075616bf
ms.translationtype: MT
ms.contentlocale: fr-FR
ms.lasthandoff: 05/03/2018
---
# <a name="unsupported-visual-foxpro-commands-and-functions-visual-foxpro-odbc-driver"></a>Les commandes non prises en charge Visual FoxPro et les fonctions (le pilote ODBC Visual FoxPro)
Le tableau suivant répertorie les commandes de FoxPro et les fonctions qui ne sont pas pris en charge par le pilote ODBC Visual FoxPro, mais sont pris en charge par Microsoft® Visual FoxPro.  
  
 Si votre application interagit avec les données dont les règles, les déclencheurs, les valeurs par défaut, ou d’appellent de procédures stockées de ces commandes de Visual FoxPro ou fonctions, le pilote peut générer une erreur.  
  
## <a name="unsupported-visual-foxpro-commands-and-functions"></a>Fonctions et les commandes non prises en charge Visual FoxPro  
  
||||  
|-|-|-|  
|#DEFINE... #UNDEF|#IF... #ENDIF Directive de préprocesseur|#IFDEF &AMP;#124; #IFNDEF|  
|#INCLUDE, Directive du préprocesseur|: Opérateur de résolution de portée|! Commande (voir exécution &#124; ! Commande)|  
|? &#124; ?? Command|??? Command|\ &#124; \\\ Commande|  
|@ ... Commande de la zone|@ ... Commande de la classe|@ ... Effacer, commande|  
|@ ... MODIFIER - modifier des zones commande|@ ... REMPLIR, commande|@ ... GET|  
|@ ... Commande de MENU|@ ... Commande d’invite de commandes|@ ... Par exemple de commande|  
|@ ... Commande de défilement|@ ... COMMANDE||  
  
## <a name="a"></a>Un  
  
||||  
|-|-|-|  
|ACCEPTER la commande|ACLASS (fonction))|Activer la commande de MENU|  
|Activer les commandes de menu contextuel|Activer la commande de l’écran|Activer la fenêtre commande|  
|ActivateCell (méthode)|AJOUTER des commandes de la classe|ADIR (fonction))|  
|AFONT (fonction))|AINSTANCE (fonction))|Variable de mémoire système _ALIGNMENT|  
|AMEMBERS (fonction))|ANSITOOEM (fonction))|APRINTERS (fonction))|  
|ASELOBJ (fonction))|Commande d’assistance||  
  
## <a name="b"></a>B  
  
||||  
|-|-|-|  
|BARRE (fonction))|BARCOUNT (fonction))|BARPROMPT (fonction))|  
|Variable de mémoire système _BEAUTIFY|Variable de mémoire système _BOX|Parcourir les commandes|  
|Variable de mémoire système _BROWSER|GÉNÉRER la commande de l’application|GÉNÉRER la commande EXE|  
|GÉNÉREZ le projet (commande)|Variable de mémoire système _BUILDER||  
  
## <a name="c"></a>C  
  
||||  
|-|-|-|  
|Variable de mémoire système _CALCVALUE|Variable de mémoire système _CLIPTEXT|Variable de mémoire système _CONVERTER|  
|Variable de mémoire système _CUROBJ|Appelez la commande|Annule la commande|  
|CAPSLOCK (fonction))|Commande CD|Commande de modification|  
|Commande CHDIR|CHRSAW (fonction))|Commande d’avoir fermer|  
|CNTBAR (fonction))|CNTPAD (fonction))|COL (fonction))|  
|COMPILATION de commande|COMPILE la commande de base de données|COMPILATION de la commande|  
|COMPOBJ (fonction))|Objet conteneur|Objet de contrôle|  
|Copiez le fichier (commande)|Commande de copie Mémo|CRÉER des commandes de la classe|  
|CRÉER une commande CLASSLIB|CRÉER des commandes de jeu de couleurs|CRÉER une commande|  
|CRÉER la commande de connexion|CRÉER une commande de base de données|CRÉER des commandes de formulaire|  
|CRÉER à partir de la commande|CRÉER des commandes d’étiquette|CRÉER la commande de MENU|  
|CRÉER le projet (commande)|CRÉER la commande de requête|CRÉER des commandes de rapport|  
|CRÉER des commandes de l’écran|CRÉER la commande de vue SQL|CRÉER la commande de déclencheur|  
|CRÉER des commandes d’affichage|CREATEOBJECT (fonction))|CURDIR, fonction)|  
  
## <a name="d"></a>D  
  
||||  
|-|-|-|  
|Variable de mémoire système _DBLCLICK|Variable de mémoire système _DIARYDATE|DBSETPROP (fonction))|  
|Fonctions DDE|DÉSACTIVER la commande de MENU|DÉSACTIVER les commandes de menu contextuel|  
|DÉSACTIVER la fenêtre commande|DÉCLARER - DLL commande|DÉCLARER des commandes|  
|DÉFINIR la barre de commandes|DÉFINIR la zone commande|DÉFINIR des commandes de la classe|  
|DÉFINIR la commande de MENU|DÉFINIR des commandes de remplissage|DÉFINIR des commandes de menu contextuel|  
|DÉFINIR la fenêtre commande|SUPPRIMER la commande de connexion|SUPPRIMER la commande de base de données|  
|SUPPRIMER fichier (commande)|Commande de déclencheur DELETE|SUPPRIMER la commande de vue|  
|Commande DIR|Commande de répertoire|Commande d’affichage|  
|Commande de connexions d’affichage|Commande de base de données d’affichage|Commande DLL|  
|FICHIERS d’affichage (commande)|AFFICHER la mémoire, commande|AFFICHER les objets commande|  
|Commande de procédures d’affichage|Commande état d’affichage|Commande STRUCTURE d’affichage|  
|Commande de TABLES d’affichage|Commande de vues d’affichage|NE forment pas commande|  
  
## <a name="e"></a>E  
  
||||  
|-|-|-|  
|MODIFIER la commande|Commande de l’erreur||  
|Effacer (commande)|Commande externe|Commande d’exportation|  
|ÉJECTER de commande|ÉJECTER de commande de PAGE||  
  
## <a name="f"></a>F  
  
||||  
|-|-|-|  
|Variable de mémoire système _FOXDOC|Variable de mémoire système _FOXGRAPH|FEOF (fonction))|  
|FCLOSE (fonction))|FCREATE (fonction))|FGETS (fonction))|  
|FERROR (fonction))|FFLUSH (fonction))|FKLABEL (fonction))|  
|Commande de serveur de fichiers|TROUVER la commande|FOPEN (fonction))|  
|FKMAX (fonction))|FONTMETRIC (fonction))|FSEEK (fonction))|  
|FPUTS (fonction))|FREAD (fonction))||  
|FWRITE (fonction))|FCHSIZE (fonction))||  
  
## <a name="g"></a>G  
  
||||  
|-|-|-|  
|Variable de mémoire système _GENGRAPH|Variable de mémoire système _GENMENU|Variable de mémoire système _GENPD|  
|Variable de mémoire système _GENSCRN|Variable de mémoire système _GENXTAB|GETBAR (fonction))|  
|GETCOLOR (fonction))|GETDIR (fonction))|Commande GETEXPR|  
|GETFILE (fonction))|GETFONT (fonction))|GETOBJECT (fonction))|  
|GETPAD (fonction))|GETPICT (fonction))|GETPRINTER (fonction))|  
  
## <a name="h"></a>H  
  
||||  
|-|-|-|  
|Commande HELP|Masquer la commande de MENU|Masquer les commandes de menu contextuel|  
|Masquer la fenêtre commande|Accueil () (fonction)||  
  
## <a name="i"></a>I  
  
||||  
|-|-|-|  
|IMESTATUS (fonction))|Commande d’importation|Commandes d’entrée|  
|INDEX de commande|Fonction INKEY)|ISCOLOR (fonction))|  
|Commande INSERT|INSMODE (fonction))||  
|ISMOUSE (fonction))|Variable de mémoire système _INDENT||  
  
## <a name="j"></a>J  
  
||||  
|-|-|-|  
|JOINDRE, commande|||  
  
## <a name="k"></a>K  
  
||||  
|-|-|-|  
|Commande de clavier|||  
  
## <a name="l"></a>L  
  
||||  
|-|-|-|  
|Variable de mémoire système _LMARGIN|Commande LABEL|LASTKEY (fonction))|  
|LINENO (fonction))|LISTE des commandes|Commande de connexions de liste|  
|Commande de chargement|Fichier LOCFILE (fonction))||  
  
## <a name="m"></a>M  
  
||||  
|-|-|-|  
|MCOL (fonction))|Commande MD|MENU de commande|  
|MÉMOIRE (fonction))|Commande de MENU|Commande MKDIR|  
|MENU (fonction))|MESSAGEBOX (fonction))|MODIFIER la commande de connexion|  
|MODIFIER la commande de la classe|MODIFIER la commande|MODIFIER la commande|  
|MODIFIER la commande de base de données|MODIFIER fichier (commande)|MODIFIER la commande de facture|  
|MODIFIER les commandes général|MODIFIER la commande de l’étiquette|MODIFIER le projet (commande)|  
|MODIFIER la commande de MENU|Commande de la procédure de modification|MODIFIER la commande de l’écran|  
|MODIFIER la commande de requête|MODIFIER la commande de rapport|MODIFIER la fenêtre commande|  
|MODIFIER la commande STRUCTURE|MODIFIER la commande d’affichage|DÉPLACER la fenêtre commande|  
|Commande de la souris|DÉPLACEZ la commande de menu contextuel|MROW (fonction))|  
|MRKBAR (fonction))|MRKPAD (fonction))||  
|MWINDOW (fonction))|MDOWN (fonction))||  
  
## <a name="n"></a>N  
  
||||  
|-|-|-|  
|Verr. NUM (fonction))|||  
  
## <a name="o"></a>O  
  
||||  
|-|-|-|  
|OBJNUM (fonction))|OBJTOCLIENT (fonction))|SUR la barre commande|  
|OEMTOANSI (fonction))|SUR la commande APLABOUT|Commande MENU quitter ON|  
|SUR la commande d’échappement|SUR la barre de commandes de sortie|CLÉ = commande|  
|ON remplissage EXIT (commande)|Commande de menu contextuel ON EXIT|Commande de remplissage ON|  
|SUR la commande de l’étiquette de clé|SUR la commande MACHELP|SUR la sélection de barre de commandes|  
|SUR la PAGE de commande|SUR la commande READERROR|SUR la commande de menu contextuel de la sélection|  
|SUR la commande de MENU de sélection|SUR la commande de remplissage de la sélection||  
|SUR la commande d’arrêt|OBJVAR (fonction))||  
  
## <a name="p"></a>P  
  
||||  
|-|-|-|  
|Variable de mémoire système _PADVANCE|Variable de mémoire système _PAGENO|Variable de mémoire système _PBPAGE|  
|Variable de mémoire système _PCOLNO|Variable de mémoire système _PCOPIES|Variable de mémoire système _PDRIVER|  
|Variable de mémoire système _PDSETUP|Variable de mémoire système _PECODE|Variable de mémoire système _PEJECT|  
|Variable de mémoire système _PEPAGE|Variable de mémoire système _PLENGTH|Variable de mémoire système _PLINENO|  
|Variable de mémoire système _PLOFFSET|Variable de mémoire système _PPITCH|Variable de mémoire système _PQUALITY|  
|Variable de mémoire système _PRETEXT|Variable de mémoire système _PSCODE|Variable de mémoire système _PSPACING|  
|Variable de mémoire système _PWAIT|Commande de base de données PACK|Fonction de remplissage)|  
|PCOL (fonction))|PEMSTATUS (fonction))|Commande de lecture (macro)|  
|Fenêtre contextuelle de touches|Commande de MENU POP|Commande de menu contextuel POP|  
|Menu contextuel (fonction))|PRINTJOB... Commande ENDPRINTJOB|PRINTSTATUS (fonction))|  
|PRMBAR (fonction))|PRMPAD (fonction))|Fonction de l’invite de commandes)|  
|PROW (fonction))|PRTINFO (fonction))|CLÉ de commande|  
|Commande MENU|Commande de menu contextuel|PUTFILE (fonction))|  
  
## <a name="q"></a>Q  
  
||||  
|-|-|-|  
|Commande QUIT|||  
  
## <a name="r"></a>R  
  
||||  
|-|-|-|  
|Variable de mémoire système _RMARGIN|Commande de bureau à distance|READKEY (fonction))|  
|Commande de lecture|LIRE la commande MENU|VERSION de barre de commandes|  
|Fonction Refresh()|RÉINDEXER la commande|BIBLIOTHÈQUE de commandes de mise en production|  
|Commande CLASSLIB de version|Commande de mise en production|Commande de remplissage de version|  
|Commandes de MENUS de mise en production|Commande MODULE de mise en production|Commande de WINDOWS version|  
|Commande POPUPS de version|PROCÉDURE de mise en production, commande|La commande Renommer|  
|SUPPRIMER la commande de la classe|RENOMMER une commande de classe|RENOMMER une commande de vue|  
|RENOMMER une commande de connexion|RENOMMER une commande de TABLE|RESTAURER à partir de la commande|  
|Commande de rapport|REQUERY () (fonction)|RESTAURATION de la fenêtre commande|  
|RESTAURER les commandes de MACROS|ÉCRAN commande RESTORE|RGBSCHEME (fonction))|  
|Commande de reprise|RVB (fonction))|EXÉCUTEZ &AMP;#124; ! Command|  
|Commande RMDIR|Fonction de ligne)||  
|Commande RUNSCRIPT|RDLEVEL (fonction))||  
  
## <a name="s"></a>S  
  
||||  
|-|-|-|  
|ENREGISTRER des MACROS, commande|Enregistrer l’écran, commande|Enregistrer dans la commande|  
|ENREGISTRER les commandes WINDOWS|Fonction de schéma)|SCOLS (fonction))|  
|Commande de défilement|Variable de mémoire système _SCREEN|Commande SET|  
|Commande autre SET|Commande SET ANSI|Commande SET APLABOUT|  
|Commande SET enregistrement automatique|Commande de cloche SET|Commande CLIGNOTEMENT de SET|  
|Commande de bordure SET|Commande BRSTATUS SET|Commande SET CLASSLIB|  
|Commande Effacer SET|Commande d’horloge de jeu|DÉFINIR la couleur de commande|  
|DÉFINIR la couleur de la commande de schéma|Commande de jeu de couleurs SET|DÉFINIR la couleur de commande|  
|Commande COMPATIBLE SET|Commande SET confirmer|Commande de CONSOLE de jeu|  
|JEU DE CPCOMPILE|JEU DE CPDIALOG|Commande de devise SET|  
|CURSEUR de jeu (commande)|Commande de session SET|Commande de débogage de jeu|  
|Commande SET décimales|Commande de DÉLIMITEURS SET|Commande de développement de jeu|  
|Commande de périphérique de jeu|Commande d’affichage de jeu|Commande SET DOHISTORY|  
|Commande ECHO de jeu|Commande d’échappement de SET|Commande FORMAT de jeu|  
|Commande SET (fonction)|Commande d’en-têtes SET|Commande aide de SET|  
|Commande SET HELPFILTER|Commande d’intensité SET|Commande SET KEY|  
|Commande SET KEYCOMP|Commande SET LOGERRORS|Commande SET MACDESKTOP|  
|Commande SET MACHELP|Commande SET MACKEY|Commande de marge de SET|  
|Marquer les ensemble de commande|Marquer pour la commande|Commande de largeur Mémo SET|  
|Commande MESSAGE SET|Commande de la souris de jeu|Commande de compteur SET|  
|JEU de classes OLEOBJECT commande|Commande PALETTE SET|Commande SET PDSETUP|  
|Commande de définir le POINT|Commande d’imprimante SET|Commande SET READBORDER|  
|Commande d’actualisation de SET|Commande d’une ressource de jeu|Commande de sécurité de SET|  
|Commande du tableau de bord SET|Commande SET secondes|Commande de séparateur de SET|  
|Commande d’ombres SET|ENSEMBLE de SKIP de commande|Commande d’un espace de jeu|  
|Commande d’état de SET|DÉFINIR l’état de barre de commandes|Commande d’étape de jeu|  
|Commande RÉMANENTES SET|Commande SET SYSFORMATS|Commande SET SYSMENU|  
|Commande de parler de SET|Commande de la fusion de texte SET|Commande de DÉLIMITEURS SET la fusion de texte|  
|Commande de rubrique SET|Commande ID de rubrique SET|Commande SET TRBETWEEN|  
|Commande de tampon clavier SET|Commande de vue d’ensemble|DÉFINIR une fenêtre de commande de facture|  
|Commande SET XCMDFILE|Variable de mémoire système _SHELL|AFFICHER la commande GET|  
|Afficher Obtient la commande|AFFICHER la commande MENU|AFFICHER la commande de l’objet|  
|AFFICHER la commande de menu contextuel|AFFICHER la fenêtre commande|Commande de menu contextuel de taille|  
|Taille fenêtre commande|SKPBAR (fonction))|SKPPAD (fonction))|  
|Fonction SOUNDEX)|Variable de mémoire système _SPELLCHK|Fonctions SQL|  
|SROWS (fonction))|Variable de mémoire système _STARTUP|La commande d’interruption|  
|Fonctions sys() sauf SYS(2011)|SYSMETRIC (fonction))||  
  
## <a name="t"></a>T  
  
||||  
|-|-|-|  
|Variable de mémoire système _TABS|TEXTE... ENDTEXT commande|TXTWIDTH (fonction))|  
|TRANSFORM (fonction))|Variable de mémoire système _TRANSPORT||  
|Commande TYPE|Variable de mémoire système _THROTTLE||  
  
## <a name="u"></a>U  
  
||||  
|-|-|-|  
|Fonction de mise à jour (de)|Utilisez la commande||  
  
## <a name="v"></a>V  
  
||||  
|-|-|-|  
|VALIDER la commande de base de données|VARREAD (fonction))|VERSION (fonction))|  
  
## <a name="w"></a>W  
  
||||  
|-|-|-|  
|Variable de mémoire système _WINDOWS|Variable de mémoire système _WIZARD|WCHILD (fonction))|  
|ATTENTE de commande|WBORDER (fonction))|WFONT (fonction))|  
|WCOLS (fonction))|WEXIST (fonction))|WLROW (fonction))|  
|AVEC... Commande ENDWITH|WLAST (fonction))|WONTOP (fonction))|  
|W maximum (fonction))|WLCOL (fonction))|WREAD (fonction))|  
|WOUTPUT (fonction))|WMINIMUM (fonction))|WVISIBLE (fonction))|  
|WPARENT (fonction))|WTITLE (fonction))||  
|WROWS (fonction))|Variable de mémoire système _WRAP||  
  
## <a name="z"></a>Z  
  
||||  
|-|-|-|  
|FENÊTRE de ZOOM (commande)|||

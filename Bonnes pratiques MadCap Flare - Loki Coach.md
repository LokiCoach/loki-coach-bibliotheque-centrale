# Bonnes pratiques MadCap Flare — Loki Coach

Document de référence interne, basé sur l'analyse du projet **Bibliothèque-Entreprise** (dossier `Mes projets\Bibliothèque-Entreprise`) et sur la documentation officielle de MadCap Software. À faire évoluer au fil de la croissance de l'équipe de rédaction.

## Diagnostic du projet actuel

Avant les recommandations générales, voici ce que l'inspection du projet a révélé concrètement — c'est ce qui doit guider les priorités ci-dessous plutôt qu'une liste générique.

Le projet est globalement bien organisé au premier niveau : un seul projet central plutôt qu'une multitude de petits projets épars, un dossier `Resources` qui respecte la structure standard de Flare (Branding, Images, PageLayouts, Snippets, Stylesheets, TableStyles, TemplatePages), et une bonne convention de codes pour les projets spéciaux (LS005-S110648, CC001, SR26-001, etc.) qui facilite la traçabilité. C'est une base saine à préserver.

Trois problèmes ressortent cependant clairement des fichiers eux-mêmes et méritent d'être corrigés avant que l'équipe ne grandisse :

Le premier est la duplication de fichiers avec des suffixes numérotés — par exemple `LF001.1 - Rapport de traçabilité_FR.fltoc`, `_FR1.fltoc` et `_FR2.fltoc`, chacun avec sa propre feuille de style associée. C'est le signe d'un versionnement fait « à la main » par copier-coller plutôt que par un vrai système de contrôle de version, ce qui rend impossible de savoir quelle version est la bonne sans ouvrir chaque fichier, et multiplie les risques d'erreur lors des mises à jour.

Le deuxième est qu'il existe onze feuilles de style (CSS) quasi identiques, une par document (`Aide-mémoire...css`, `Campagne_rappel_LOKI.css`, `CC001 Starlink...css`, etc.), en plus d'un `Styles.css` générique qui semble peu utilisé. En pratique, cela veut dire qu'un changement de mise en page ou d'image de marque doit être répété onze fois, avec un risque élevé d'incohérence visuelle entre documents et d'oubli.

Le troisième est que les jeux de variables (`Variables.flvar`, 223 octets) et de snippets (un seul snippet, « En-tête ») sont quasiment vides, alors que le contenu — noms de produits, coordonnées de l'entreprise, clauses de garantie, avis de sécurité — est probablement répété tel quel dans chaque topic. À cela s'ajoute un seul jeu de conditions (`Default.flcts`) et un seul utilisateur Flare enregistré (`frederic.djeki`), et un projet stocké dans OneDrive plutôt que dans un système de contrôle de version — viable pour un rédacteur seul, mais fragile dès qu'une deuxième personne édite le projet en même temps.

Rien de tout cela n'est grave à ce stade, mais chaque point coûtera de plus en plus cher à corriger à mesure que le nombre de documents et de rédacteurs augmente. Le plan d'action à la fin de ce document reprend ces trois priorités.

## 1. Structure de projet et single-sourcing

Le principe central de Flare est de rédiger un contenu une seule fois et de le publier vers plusieurs sorties (PDF, HTML5, etc.) et, idéalement, plusieurs langues, sans dupliquer les fichiers sources. Trois outils de Flare servent précisément à ça, et gagneraient à être davantage exploités chez Loki Coach.

Les **variables** (`Project > VariableSets`) devraient contenir tout ce qui est amené à se répéter ou à changer avec le temps : le nom légal de l'entreprise, l'adresse, les numéros de téléphone du support, les noms et numéros de modèles de véhicules, l'année courante, les numéros de version de document. Le jour où une coordonnée change, une seule mise à jour de la variable suffit au lieu de chercher dans tous les topics.

Les **snippets** (`Content\Resources\Snippets`) devraient héberger tout bloc de texte qui revient identique dans plusieurs documents : clauses de garantie standard, avis de sécurité réglementaires, procédures de mise en service communes, mentions légales de bas de page. Vu la nature du contenu (rappels de sécurité, garantie, procédures véhicule), c'est probablement le gain le plus important à court terme : un snippet corrigé une fois se corrige partout où il est inséré, ce qui réduit fortement le risque d'un avis de sécurité obsolète publié par erreur.

Les **balises de condition** (`Project > ConditionTagSets`) permettent de gérer des variantes de contenu (FR/EN, version interne vs externe, brouillon vs final) à l'intérieur des mêmes topics plutôt qu'en dupliquant des fichiers entiers. La séparation actuelle par dossiers `EN`/`FR` reste une approche valide recommandée par MadCap quand les deux langues divergent complètement, mais elle suppose une vraie discipline de synchronisation manuelle ; les duplications `_FR1`/`_FR2` observées montrent que cette discipline n'est pas encore tenue. Pour les projets qui grandissent, MadCap propose aussi le *Master Project Linking*, qui permet de scinder un gros projet en plusieurs tout en partageant un seul jeu de feuilles de style, gabarits et snippets — une option à garder en tête si la bibliothèque continue de croître.

Sur la taille du projet elle-même, la recommandation de MadCap est simple : un projet unique reste préférable tant que vous n'êtes pas obligé de répéter la même modification dans plusieurs projets différents — c'est justement le signal qu'ils devraient être fusionnés. Le choix actuel d'un projet central « Bibliothèque-Entreprise » est donc le bon; il ne faut pas cherche à le fragmenter par équipe ou par année.

## 2. Collaboration et contrôle de version

Dès qu'une deuxième personne va contribuer au projet, OneDrive cessera d'être une solution suffisante : les fichiers de Flare sont en grande partie du XML, et une synchronisation de fichiers classique gère mal les modifications simultanées (conflits silencieux, absence d'historique de versions fiable, aucun moyen de revenir à un état antérieur précis).

MadCap recommande d'utiliser un vrai système de contrôle de version — Git étant le plus courant et nativement supporté par Flare — avec un dépôt par projet. Cela apporte trois choses concrètes : un historique complet de qui a changé quoi et quand (particulièrement utile pour des documents réglementaires comme les rappels de sécurité), la possibilité d'annuler une modification erronée sans tout perdre, et une vraie fusion des contributions de plusieurs rédacteurs plutôt que des copies parallèles.

À mesure que l'équipe grandit, chaque rédacteur devrait avoir son propre utilisateur Flare (`Project > Users`) avec les droits appropriés, plutôt que de partager un compte unique. Étant donné la nature du contenu — garantie, sécurité, rappels — un flux de relecture/approbation avant publication (via des commentaires Flare ou un outil externe) vaut la peine d'être formalisé, ne serait-ce que pour garder une trace de qui a validé quoi.

## 3. Rédaction et guide de style

La priorité immédiate ici est de revenir à une architecture de styles centralisée. Flare fonctionne bien avec une seule feuille de style de base (ou un petit nombre), appliquée à tous les topics, avec des classes pour les cas particuliers plutôt qu'une feuille de style par document. Le `Styles.css` déjà présent dans le projet est un bon point de départ : l'objectif serait que les onze documents actuels y reviennent progressivement, et que tout nouveau document parte de cette base commune au lieu d'en copier une existante.

Les gabarits de page (`PageLayouts`, `TemplatePages`) déjà présents dans `Resources` vont dans le même sens et méritent d'être systématiquement utilisés pour les en-têtes, pieds de page et pages de garde, plutôt que recréés document par document.

Un dossier `Glossaries` existe déjà dans le projet ; le peupler avec la terminologie technique bilingue (termes véhicule, garantie, sécurité) aide à garder une cohérence FR/EN, surtout si plusieurs personnes traduisent à l'avenir. Sur l'accessibilité, veillez à toujours renseigner un texte alternatif sur les images et à garder une hiérarchie de titres cohérente (H1 > H2 > H3) — Flare s'appuie sur cette structure pour générer automatiquement tables des matières et navigation.

## 4. Publication et maintenance

Le projet ne compte actuellement qu'une seule cible de publication, `PDF.fltar`. C'est adapté pour des documents formels comme les avis de rappel ou les rapports de traçabilité, mais pour du contenu comme la formation clients ou l'expérience client, une cible HTML5 (consultable en ligne, recherchable, plus facile à tenir à jour) pourrait mieux servir l'usage réel du contenu — à évaluer selon qui consulte ces documents et comment.

Le dossier `Analyzer` déjà présent à la racine du projet est l'outil intégré de Flare pour détecter le contenu orphelin, les liens brisés et les variables ou styles inutilisés : en faire une étape systématique avant chaque publication évite que des incohérences s'accumulent silencieusement, surtout une fois que les recommandations ci-dessus (variables, snippets, styles centralisés) seront en place.

Enfin, un nettoyage ponctuel des fichiers dupliqués actuels (`_FR1`, `_FR2` et équivalents) est recommandé dès que possible : une fois le contrôle de version en place, ces copies successives n'auront plus de raison d'être, et les garder ne fait qu'augmenter le risque de publier par erreur une version obsolète.

## Plan d'action prioritaire

1. Mettre en place un contrôle de version (Git) pour le projet avant d'ajouter un deuxième rédacteur — c'est le prérequis à tout le reste.
2. Consolider les onze feuilles de style vers `Styles.css` et supprimer les duplicatas `_FR1` / `_FR2` une fois l'historique de versions disponible dans Git.
3. Créer des snippets pour les clauses de garantie et avis de sécurité récurrents, et enrichir le jeu de variables (coordonnées de l'entreprise, noms de modèles, année en cours).
4. Documenter la convention de nommage déjà en usage (LS0xx, CC0xx, SR26-xxx) pour qu'elle reste cohérente quand l'équipe grandira.
5. Réévaluer si une cible HTML5 serait pertinente pour le contenu de formation et d'expérience client.

## Sources

- [7 Best Practices for Starting Your First MadCap Flare Project (MadCap Software)](https://www.madcapsoftware.com/resources/white-papers/starting-first-madcap-flare-project/)
- [MadCap Flare Best Practices — Project Size (MadCap Software)](https://www.madcapsoftware.com/blog/madcap-flare-best-practices-project-size/)
- [My Adventures with MadCap Flare and Git Source Control (MadCap Software)](https://www.madcapsoftware.com/blog/adventures-madcap-flare-git-source-control/)
- [MadCap Flare Source Control Guide: Git — Flare 2025 r2 (MadCap documentation, PDF)](https://docs.madcapsoftware.com/flare2025r2/Flare-Source-Control-Guide-Git.pdf)
- [Single-sourcing Best Practices with Multiple MadCap Flare Projects (webinaire MadCap Software)](https://www.madcapsoftware.com/webinars/single-sourcing-best-practices-multiple-madcap-flare-projects)

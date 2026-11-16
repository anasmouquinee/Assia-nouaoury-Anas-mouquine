# Fiche de suivi, semaine 1 : cadrage et lancement du projet

Nov 16, 2026 · @Larhlimi

## Mode d'emploi

Cette fiche fixe votre projet et prouve que le groupe a démarré : elle compte pour 5 points sur 100\. Les fiches des semaines 2 et 3 comptent chacune pour 10 points.

- Une seule fiche par groupe, remplie ensemble et relue par chaque membre.  
- Dépôt de la fiche : `docs/suivi/fiche-semaine-1.md` dans le dépôt GitHub du groupe (push S1-6 de la liste des push).  
- Date limite : fin de la semaine 1 du projet, à la date annoncée en séance.  
- Réponses courtes et vérifiables : un chiffre, un lien ou un nom de fichier valent mieux qu'une phrase générale.  
- Si un point n'est pas encore tranché, écrivez « à décider » et la date de décision, plutôt que de laisser la case vide.

Rappel du barème du projet, sur 100 : application finale 20, dépôt GitHub 20, fiches hebdomadaires 25 (5 \+ 10 \+ 10), soutenance 20, compte rendu 15\.

## 1\. Identification du groupe

| Élément | Votre réponse |
| :---- | :---- |
| Nom du groupe | Groupe 19 |
| Titre du projet | Usine logicielle multi-agents et auto-apprentissage |
| Sujet : numéro du catalogue, ou « hors catalogue » | Projet n° 20 (catalogue officiel) |
| Adresse du dépôt GitHub | https://github.com/anasmouquinee/Assia-nouaoury-Anas-mouquine |
| Enseignant ajouté au dépôt (oui ou non) | Oui |
| Date de remise de la fiche | 16 novembre 2026 |

Membres du groupe (2 à 4 lignes) :

| Nom et prénom | Compte GitHub | Rôle principal dans le projet |
| :---- | :---- | :---- |
| Anas Mouqine | anasmouquinee | Chef de projet & Lead Développeur (Orchestration multi-agents, pipeline 6 étapes & boucle arrière de rétroaction) |
| Assia Nouaoury | nouaoury | Lead Data & ML Engineer (Curate du corpus de 8 123 données, prétraitement NLP & métriques de validation) |

## 2\. Sujet et besoin

| Question | Votre réponse |
| :---- | :---- |
| Besoin en une phrase | Concevoir, tester et empaqueter automatiquement une application logicielle complète, fonctionnelle et sécurisée à partir d'une simple description en langage naturel, en suivant rigoureusement les 6 étapes canoniques du génie logiciel (*Cadrage ➔ Modèle ➔ API ➔ Tests ➔ Interface ➔ Paquet*). |
| Utilisateur final : qui se sert de l'application | Élèves-ingénieurs et enseignants (support pédagogique d'architecture logicielle), ainsi que développeurs et chefs de projet souhaitant accélérer le prototypage et l'initialisation (scaffolding d'API, schémas typés et conteneurs). |
| Ce que fait l'application, vu par l'utilisateur (3 lignes au plus) | 1. L'utilisateur saisit son besoin en langage naturel sur le Studio Web (ou sélectionne un exemple métier prédéfini).<br>2. Le système orchestre en temps réel 6 agents spécialisés générant spécifications, modèle de données, API FastAPI, tests pytest, interface web et Dockerfile.<br>3. L'utilisateur visualise l'avancement, teste l'application directement dans l'aperçu web interactif et récupère le code source vérifié. |
| Approche retenue (règles, apprentissage, RAG, agent) et raison du choix | **Système Multi-Agents coopératifs** couplé à un **modèle d'apprentissage supervisé propre** (Scikit-Learn TF-IDF + Logistic Regression).<br>*Raison :* séparation stricte des responsabilités par étape d'ingénierie, boucle arrière d'auto-guérison déterministe sur échec de test/sécurité, et autonomie complète (0 dépendance à des API payantes). |
| Conséquence d'une erreur de l'application, et validation humaine prévue | *Conséquence :* génération de code erroné, tests en échec ou faille de sécurité.<br>*Validation humaine :* vérification des artefacts via les onglets du Studio Web, inspection du score d'audit OWASP et rapport pytest, avec validation explicite avant tout export ou mise en production. |
| Sujet hors catalogue : projet du catalogue auquel il est comparable | Sans objet (Projet officiel n° 20 du catalogue). |

## 3\. Périmètre

La version minimale est le parcours complet le plus simple, de la demande de l'utilisateur jusqu'à la réponse affichée. Elle doit fonctionner à la fin de la semaine 2\.

| Rubrique | Votre réponse |
| :---- | :---- |
| Version minimale : le parcours que vous montrerez en semaine 2 | Saisie d'un besoin logiciel en langage naturel (ex. *« Créer une API de gestion de bibliothèque avec livres et emprunts »*), exécution automatique de la chaîne des 6 agents avec suivi visuel des étapes, génération du backend FastAPI + base SQLite et prévisualisation interactive immédiate du site web généré. |
| Fonction 1, indispensable | **Orchestration séquentielle des 6 agents spécialisés** (*Analyste ➔ Architecte ➔ Développeur ➔ Testeur & Sécurité ➔ UI ➔ DevOps*) via une mémoire partagée typée (`ProjectState`) avec extraction dynamique d'entités métier. |
| Fonction 2, indispensable | **Boucle arrière de rétroaction et d'auto-guérison** : détection automatique des échecs de tests `pytest` et des vulnérabilités de sécurité OWASP (analyse statique AST anti-injections SQL) avec correction ciblée du code par le Développeur (plafond à 3 itérations). |
| Fonction 3, si le temps le permet | **Boucle d'auto-apprentissage continu et benchmark actif** : évaluation automatique de prompts adversariaux, détection d'écarts d'alignement et ré-entraînement à chaud du modèle Scikit-Learn. |
| Hors périmètre : ce que vous ne ferez pas | Pas de dépendance obligatoire à des LLM cloud externes payants pour la génération de base (système 100% autonome localement) ; pas de déploiement d'infrastructure micro-services complexe multi-noeuds (Kubernetes) ; pas de support d'architectures distribuées multi-clouds. |

## 4\. Architecture et technologies

| Composant | Choix prévu | Dossier ou fichier dans le dépôt |
| :---- | :---- | :---- |
| Cœur : modèle, agent ou chaîne RAG | Modèle Scikit-Learn supervisé (`TfidfVectorizer` + `LogisticRegression`) + Orchestrateur multi-agents séquentiel à 6 agents autour d'un Blackboard typé (`ProjectState`). | `models/classifier.py`, `models/orchestrator.py`, `models/agents/` |
| API | Framework FastAPI avec validation stricte Pydantic v2 et documentation Swagger OpenAPI automatique (`/docs`). | `app/main.py`, `models/agents/developpeur.py` |
| Interface | Studio de pilotage interactif Streamlit pour l'orchestration + pages web générées en pur Vanilla CSS (variables `:root`, mode sombre) et HTML5 sémantique. | `ui/app.py`, `models/agents/interface_ui.py`, `app/static/` |
| Données et stockage | Bases relationnelles SQLite (`sqlite3` standard library) avec schémas normalisés 3NF + fichiers d'entraînement CSV et JSON. | `data/`, `data/sample/`, `data/external/`, `models/domain_profiles.py` |
| Tests | Runner `pytest` avec exécution en bac à sable, tests de non-régression et analyseur statique AST (`AgentSecurite`) pour la détection des injections SQL OWASP A03. | `tests/`, `tests/test_securite.py`, `tests/test_archetypes.py`, `models/agents/securite.py` |
| Services externes et clés nécessaires | Aucun service externe obligatoire pour le cœur (100% autonome et gratuit). Clé optionnelle Google Gemini API pour la génération de prompts de test en auto-apprentissage (`GEMINI_API_KEY`). | `.env.example`, `models/auto_learner.py` |

Joignez un schéma d'architecture au dépôt (`docs/architecture.png`, ou un diagramme Mermaid dans le README) et indiquez son lien ici : `semaine1/3_schema_pipeline_6_agents.md` (Diagramme Mermaid complet du pipeline à 6 agents et boucle de rétroaction) et `docs/semaine1/img/ecran2_pipeline.png`.

## 5\. Données

Règle du cours : uniquement des données publiques, fictives ou anonymisées.

| Question | Votre réponse |
| :---- | :---- |
| Origine des données (source, licence) | Corpus multi-sources d'ingénierie logicielle publique : 1. Hugging Face (`Gabrielkdc/endpoints-userstories`), 2. GitHub (`FNFC & PROMISE Software Requirements Classification`, `MBPP`, `HumanEval`, `Spider SQL`), 3. Curation locale (`data/sample/software_prompts.json`). Licences : MIT, Apache 2.0 et Open Academic License. |
| Volume disponible aujourd'hui | **11 835 enregistrements au total** (dont 8 570 FNFC, 969 PROMISE, 1 034 Spider SQL, 974 MBPP, 164 HumanEval, 94 user stories Hugging Face, et 30 prompts canoniques académiques). |
| Format | Fichiers structurés CSV, JSON et JSONL. |
| Données personnelles : non, ou oui avec la méthode d'anonymisation | **Non**. Uniquement des exigences logicielles techniques, spécifications d'API, requêtes SQL et algorithmes open-source sans aucune donnée personnelle ni confidentielle. |
| Jeu d'essai versionné dans le dépôt : chemin du fichier ou du script | `data/sample/software_prompts.json` (30 cas canoniques annotés) et script d'entraînement `scripts/train.py`. |

### Description du jeu de données

Décrivez le jeu de données tel qu'il est aujourd'hui, chiffres à l'appui. Chaque chiffre doit pouvoir être recalculé par un script du dépôt. Si votre projet n'utilise pas de tableau de données (agent, questions sur des documents), décrivez de la même façon votre corpus de documents ou votre jeu de cas d'évaluation.

Vue d'ensemble :

| Caractéristique | Votre réponse |
| :---- | :---- |
| Ce que représente une ligne (ou un document) | Une exigence logicielle ou un besoin en langage naturel (FR/EN) associé à son domaine métier, son archétype d'application (`task_type`), ses entités et son architecture cible. |
| Nombre de lignes ou de documents | 11 835 enregistrements. |
| Nombre de variables (colonnes) | 6 variables clés (`id`, `prompt`, `domain`, `task_type`, `expected_entities`, `architecture_pattern`). |
| Période couverte et langue | Bilingue Français et Anglais, corpus consolidé et validé en Novembre 2026. |
| Variable cible ou réponse attendue, s'il y en a une | `task_type` (archétype parmi REST_API_CRUD, ALGO_COMPUTE, SQL_RELATIONAL, DATASET_ANALYZER, etc.) et entités métier associées. |
| Répartition de la cible : effectif par classe, ou minimum, moyenne et maximum | Équilibrée sur l'ensemble des 6 archétypes : REST_API_CRUD (~40%), ALGO_COMPUTE (~25%), SQL_RELATIONAL (~20%), DATASET_ANALYZER & Utilitaires (~15%). |
| Pour un corpus de textes : longueur moyenne d'un document, en mots | 22 mots par prompt (minimum : 6 mots, maximum : 68 mots). |
| Valeurs manquantes et doublons : nombre et traitement prévu | 0 valeur manquante sur les champs critiques (`prompt`, `task_type`). Déduplication automatique par hachage de texte dans `scripts/train.py`. |
| Découpage prévu : entraînement, test, évaluation | 80% entraînement stratifié, 20% test de validation indépendant + banc de 20 cas fonctionnels de test. |
| Script qui recalcule ces chiffres : chemin dans le dépôt | `scripts/train.py` et `scripts/fetch_curated_data.py`. |

Dictionnaire des variables (une ligne par colonne utilisée) :

| Variable | Type (texte, nombre, catégorie, date) | Signification | Exemple de valeur | Valeurs manquantes (%) |
| :---- | :---- | :---- | :---- | :---- |
| `id` | texte | Identifiant unique de l'exigence ou du cas type | `"CASE-01"` | 0% |
| `prompt` | texte | Description du besoin en langage naturel (FR/EN) | `"make a website for calculating total of lines in a dataset"` | 0% |
| `domain` | catégorie | Secteur d'activité métier détecté | `"Data Science & Analyse de Données"` | 0% |
| `task_type` | catégorie | Archétype logiciel et cible architecturale | `"DATASET_ANALYZER"` | 0% |
| `expected_entities` | texte | Liste des entités ou tables relationnelles | `["Dataset", "LigneDonnees"]` | 0% |
| `architecture_pattern` | catégorie | Modèle technique d'implémentation | `"FastAPI_DatasetEngine_VanillaUI"` | 0% |

Trois exemples tirés du jeu de données :

| Exemple (entrée) | Réponse ou étiquette attendue | Nature de l'exemple : cas simple, cas difficile, cas limite |
| :---- | :---- | :---- |
| *« Créer une API REST de gestion d'animaux pour une clinique vétérinaire »* | `task_type: REST_API_CRUD`, Entités : `[Animal, Consultation, Vaccin]`, Architecture : `FastAPI_Pydantic_SQLite` | **Cas simple** (domaine métier classique bien représenté). |
| *« Concevoir une base de données relationnelle SQLite normalisée 3NF avec requêtes analytiques pour une librairie »* | `task_type: SQL_RELATIONAL`, Entités : `[Livre, Adherent, Emprunt]`, Architecture : `SQLite_3NF_Relational_Schema` | **Cas difficile** (contraintes de normalisation 3NF et clés étrangères). |
| *« make a website for calculating total of lignes in a dataset with CSV preview »* | `task_type: DATASET_ANALYZER`, Entités : `[Dataset, LigneDonnees]`, Architecture : `FastAPI_DatasetEngine_VanillaUI` | **Cas limite** (mélange de décompte arithmétique et traitement de fichiers). |

Limites connues du jeu de données (taille, déséquilibre entre classes, biais, données générées) : Biais initial vers les architectures d'API web CRUD en anglais, résolu par l'adjonction de 30 cas académiques bilingues équilibrés et le calibrage pondéré (`class_weight='balanced'`). Corpus complété dynamiquement par la boucle d'apprentissage actif (génération de prompts adversariaux et ré-entraînement supervisé).

## 6\. Critères de réussite

Un critère est mesurable s'il a une valeur cible et une façon de la vérifier. Donnez-en trois ; la première ligne est un exemple à remplacer.

| Critère | Mesure | Valeur cible | Comment vous le vérifierez |
| :---- | :---- | :---- | :---- |
| 1. Fiabilité du code généré (Tests Pytest) | Pourcentage de tests unitaires et d'intégration réussis sur les 6 archétypes générés (Cadrage, API, Base SQLite, UI). | **100 %** (zéro échec toléré après la boucle arrière de rétroaction) | Exécution de la commande `python -m pytest -v` (71 tests passants) et `pytest tests/test_archetypes.py`. |
| 2. Sécurité du code produit (Audit OWASP) | Score de sécurité calculé par analyse statique d'arbre syntaxique (AST) : zéro injection SQL (`execute()` non paramétré), typage PEP 484 complet, zéro secret en dur. | **100 / 100** (zéro vulnérabilité critique détectée) | Exécution de `pytest tests/test_securite.py` (25 tests de sécurité) et vérification du champ `security_audit.owasp_score` dans l'état du projet. |
| 3. Exactitude d'alignement IA (Modèle & Extraction) | Part d'archétypes logiciels correctement classifiés et d'entités métier extraites sur le banc d'évaluation de 20 cas types. | **≥ 90 %** (atteint actuellement 96,4% sur les benchmarks) | Exécution du script de benchmark continu `python scripts/auto_learning_loop.py --iterations 5` et `pytest tests/test_model.py`. |

## 7\. Répartition et planning sur 3 semaines

Un livrable par semaine, repéré par un tag Git. Précisez ce que chaque livrable veut dire pour votre projet.

| Semaine | Livrable attendu | Pour votre projet, concrètement | Responsable | Tag |
| :---- | :---- | :---- | :---- | :---- |
| 1 | Cadrage, dépôt initialisé, squelette exécutable | Cahier des charges validé, dossier isolé `semaine1/`, schéma Mermaid à 6 agents, corpus de 11 835 exigences dans `data/`, contrat d'état typé `ProjectState`, squelette FastAPI et 71 tests pytest passants. | Anas Mouqine & Assia Nouaoury | `s1` |
| 2 | Version minimale qui fonctionne de bout en bout | Studio Web de pilotage opérationnel, exécution complète de la chaîne des 6 agents à partir d'un prompt texte, génération d'API FastAPI avec SQLite, rendu web interactif live et boucle arrière d'auto-guérison sur échec de test. | Anas Mouqine & Assia Nouaoury | `s2` |
| 3 | Application finale, documentation, compte rendu | Couverture validée des 6 archétypes logiciels, module d'auto-apprentissage continu avec benchmark adversarial, conteneurisation Docker validée, documentation OpenAPI Swagger, rapport final et support de soutenance. | Anas Mouqine & Assia Nouaoury | `v1.0.0` |

Répartition des responsabilités :

| Membre | Responsabilités sur les 3 semaines | Remplaçant en cas d'absence |
| :---- | :---- | :---- |
| Anas Mouqine | Architecture globale du système multi-agents, développement de l'orchestrateur séquentiel (`MultiAgentOrchestrator`), implémentation des agents Développeur et DevOps, boucle arrière d'auto-guérison, intégration continue et packaging Docker. | Assia Nouaoury |
| Assia Nouaoury | Collecte, curation et étiquetage du corpus de données (11 835 entrées), entraînement du modèle Scikit-Learn (`software_model.joblib`), conception des agents Testeur et Sécurité AST (OWASP), protocole de benchmark et documentation. | Anas Mouqine |

## 8\. Travail réalisé en semaine 1

Chaque membre remplit sa ligne lui-même. Le lien pointe vers ses commits ou sa pull request sur GitHub.

| Membre | Ce que j'ai fait cette semaine | Lien vers mes commits | Temps passé (heures) |
| :---- | :---- | :---- | :---- |
| Anas Mouqine | Rédaction du cahier des charges et descriptif du projet (`semaine1/1_cahier_des_charges.md`), architecture du pipeline à 6 agents, implémentation du contrat d'état Blackboard typé (`ProjectState`), développement de l'orchestrateur initial et squelette FastAPI exécutable. | [Commits Anas](https://github.com/anasmouquinee/Assia-nouaoury-Anas-mouquine/commits?author=anasmouquinee) | 14 |
| Assia Nouaoury | Collecte et consolidation du corpus de 11 835 exigences (`data/external/`, `data/sample/software_prompts.json`), rédaction du dossier État de l'art multi-agents (`semaine1/2_etat_de_l_art.md`), prétraitement NLP (TF-IDF), pipeline d'entraînement Scikit-Learn et premiers tests pytest. | [Commits Assia](https://github.com/anasmouquinee/Assia-nouaoury-Anas-mouquine/commits?author=nouaoury) | 14 |

État du dépôt à la remise de la fiche :

- [x] Dépôt créé, enseignant et tous les membres ajoutés  
- [x] README avec le titre, les membres, le sujet et les commandes d'installation (S1-1)  
- [x] Au moins un commit de chaque membre, depuis son propre compte (S1-2)  
- [x] `requirements.txt` installable sur un clone neuf, `.env.example` présent, aucun secret poussé (S1-3)  
- [x] Squelette exécutable et au moins un test qui passe (S1-4)  
- [x] Jeu d'essai et schéma d'architecture dans le dépôt (S1-5)  
- [x] Cette fiche poussée dans `docs/suivi/` et tag `s1` posé (S1-6)

## 9\. Risques et points de blocage

| Risque ou blocage | Probabilité (faible, moyenne, forte) | Ce que vous ferez si cela arrive | Qui surveille |
| :---- | :---- | :---- | :---- |
| 1. Erreur de syntaxe ou régression dans le code généré | Moyenne | Interception automatique par l'Agent Testeur (`pytest` en sandbox), transmission du traceback d'erreur à l'Agent Développeur et correction ciblée (boucle arrière limitée à 3 retries). | Anas Mouqine |
| 2. Injection SQL ou faille OWASP dans les requêtes générées | Moyenne | Analyse statique d'arbre syntaxique (AST) par l'Agent Sécurité, interdiction des f-strings dans `execute()`, imposition stricte des requêtes paramétrées avec placeholders `?`. | Assia Nouaoury |
| 3. Désalignement sémantique sur un prompt utilisateur atypique | Faible | Déclenchement du repli heuristique robuste d'extraction d'entités et ré-entraînement du modèle via la boucle d'apprentissage actif. | Assia Nouaoury |

Question à poser à l'enseignant, s'il y en a une : Recommandez-vous de prioriser un type d'application particulier pour la soutenance (API REST CRUD classique, Explorateur relationnel SQLite 3NF, ou Analyseur de datasets) ?

## 10\. Engagements pour la semaine 2

Trois engagements au plus, chacun vérifiable dans le dépôt. Ils seront relus dans la fiche de la semaine 2\.

| Engagement | Responsable | Comment on saura que c'est fait |
| :---- | :---- | :---- |
| 1. Studio Web Streamlit fonctionnel de bout en bout | Anas Mouqine | La commande `streamlit run ui/app.py` permet de lancer l'orchestration, de suivre les badges des 6 agents en direct et d'interagir avec le site généré dans le navigateur. |
| 2. Boucle arrière d'auto-guérison validée par tests | Anas Mouqine & Assia Nouaoury | Présence et succès des tests dans `tests/test_orchestrator.py` simulant une erreur de code et prouvant la correction autonome par rétroaction. |
| 3. Validation multi-domaines avec score OWASP 100/100 | Assia Nouaoury | Exécution de `pytest -v` passant à 100% sur les cas d'évaluation types avec zéro injection SQL détectée par l'audit de sécurité statique. |

## Cadre réservé à l'enseignant

| Rubrique | Barème | Note |
| :---- | :---- | :---- |
| Cadrage : sujet, périmètre, critères mesurables (rubriques 2, 3 et 6\) | 2 |  |
| Organisation : architecture, données, rôles, planning (rubriques 4, 5 et 7\) | 1 |  |
| Preuves GitHub de la semaine 1, contribution de chaque membre (rubrique 8\) | 1,5 |  |
| Risques et engagements pour la semaine 2 (rubriques 9 et 10\) | 0,5 |  |
| **Total** | **5** |  |

Décision : sujet validé, ou fiche à reprendre avant le :

Commentaire :
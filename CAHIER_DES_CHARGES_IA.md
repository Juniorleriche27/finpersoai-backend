# FinPersoAI - Cahier des charges IA

Version : operationnelle Codex  
Date : 2026-03-18  
Statut : document de reference pour l'equipe produit, backend, data et IA

## 1. Objet du document

Ce document definit les exigences fonctionnelles, techniques et organisationnelles du systeme d'intelligence artificielle de FinPersoAI.

Le systeme doit :

- transformer des donnees financieres en analyses exploitables ;
- produire des resultats explicables, fiables et actionnables ;
- fonctionner dans un cadre securise, tracable et supervise ;
- rester controlable par l'equipe et par les utilisateurs ;
- s'integrer proprement dans l'architecture globale du produit.

## 2. Perimetre de la partie IA

Le systeme IA couvre les modules suivants :

- ingestion et normalisation des donnees ;
- auto-categorisation des transactions ;
- prevision de tresorerie ;
- detection d'anomalies ;
- scoring budgetaire explicable ;
- recommandations personnalisees ;
- assistant documentaire base sur le RAG ;
- orchestration intelligente ;
- matching experts-utilisateurs ;
- monitoring des modeles ;
- gestion de la confidentialite et du consentement ;
- gestion du cold start.

Ces modules constituent le moteur d'intelligence financiere de FinPersoAI.

## 3. Cible d'architecture : 9 composants IA

FinPersoAI part sur une cible de neuf composants IA distincts. Tous ne sont pas necessairement des modeles entraines au sens strict. Certains sont des moteurs hybrides ou des composants de supervision, mais ils font partie du systeme IA global.

| # | Composant IA | Role principal | Nature du composant | Priorite |
| --- | --- | --- | --- | --- |
| 1 | Categorisation des transactions | Affecter automatiquement une categorie a chaque transaction avec un score de confiance. | Modele ML supervise | V1 |
| 2 | Prevision de tresorerie | Estimer les soldes futurs, les tensions de cash et l'incertitude. | Modele ML / time series | V1 |
| 3 | Detection d'anomalies | Identifier les depenses inhabituelles et signaux suspects. | Modele hybride regles + ML | V1 |
| 4 | Scoring budgetaire explicable | Produire un score interpretable, une classe de risque et les facteurs explicatifs. | Modele ML explicable | V1 |
| 5 | Moteur de recommandations | Generer des actions concretes, priorisees et estimees en impact. | Moteur hybride regles + ranking | V2 |
| 6 | Matching experts-utilisateurs | Associer un utilisateur au bon expert selon son profil et son besoin. | Modele de ranking / matching | V2 |
| 7 | Analyse du besoin utilisateur | Comprendre l'intention, le contexte et le type d'accompagnement attendu. | Classification NLP legere | V2 |
| 8 | Assistant documentaire RAG | Repondre a partir de documents valides avec citations et refus en cas d'insuffisance. | Systeme RAG embeddings + LLM | V1 |
| 9 | Monitoring, drift et retraining intelligence | Surveiller la qualite des modeles, detecter les derives et declencher les retrainings. | Composant IA de supervision | V1 |

Principes de cadrage :

- la cible est de neuf composants IA, mais seulement cinq a sept seront de vrais modeles entraines au depart ;
- les autres composants restent IA au sens produit, car ils pilotent des decisions, du retrieval, du ranking ou de la supervision ;
- chaque composant doit rester independant, observable, testable et remplacable.

### 3.1 Cartographie technique des 9 composants

| # | Composant IA | Inputs principaux | Outputs attendus | Type de modele / moteur | Metriques principales | Dossier cible |
| --- | --- | --- | --- | --- | --- | --- |
| 1 | Categorisation des transactions | Libelle, montant, date, canal, contrepartie, historique utilisateur | Categorie predite, score de confiance, top categories alternatives | Classification supervisee tabulaire + texte leger | Accuracy, Macro F1, taux de correction utilisateur | `ai/categorization/` |
| 2 | Prevision de tresorerie | Historique des flux, periodicite revenus/charges, solde courant, saisonnalite | Solde predit, intervalle d'incertitude, alerte deficit | Time series + features metier | MAE, RMSE, MAPE, taux d'alerte juste | `ai/forecasting/` |
| 3 | Detection d'anomalies | Transactions recentes, historique utilisateur, categories, contexte temporel | Score d'anomalie, motif, alerte explicable, statut a valider | Regles metier + detection outliers | Precision alerte, recall, faux positifs | `ai/anomaly_detection/` |
| 4 | Scoring budgetaire explicable | Revenus, charges, ratio d'endettement, regularite flux, incidents | Score 0-100, classe de risque, facteurs explicatifs | Modele tabulaire explicable | AUC, KS, calibration, stabilite du score | `ai/scoring/` |
| 5 | Moteur de recommandations | Scores IA, anomalies, categories, objectifs utilisateur, contexte financier | Recommandations priorisees, estimation d'impact, justification | Regles + ranking | CTR, adoption, impact estime vs observe | `app/services/` |
| 6 | Matching experts-utilisateurs | Profil utilisateur, besoin, urgence, langue, domaine expert, disponibilite | Liste d'experts, score d'adequation, justification de matching | Ranking / matching | Precision@K, taux de prise en charge, satisfaction | `app/services/` |
| 7 | Analyse du besoin utilisateur | Texte libre, formulaire, historique d'interactions, contexte financier | Intention, type de besoin, niveau d'urgence, routage | NLP leger / classification | Accuracy, Macro F1, taux de routage correct | `ai/inference/` |
| 8 | Assistant documentaire RAG | Question utilisateur, base documentaire validee, contexte conversationnel | Reponse, citations, score de confiance, refus motive si besoin | Retrieval + embeddings + LLM | Precision retrieval, groundedness, taux de reponse avec source | `rag/` |
| 9 | Monitoring, drift et retraining intelligence | Predictions, labels reels, feedback, distributions de features, logs | Alertes drift, rapports de performance, decision de retraining | Supervision analytique | Drift score, performance decay, delai de detection | `ai/evaluation/` |

Regle d'implementation :

- les composants `1`, `2`, `3` et `4` sont les priorites de modelisation ;
- les composants `5` et `6` peuvent commencer en logique hybride avant d'etre raffines ;
- le composant `7` peut rester simple au depart ;
- le composant `8` s'appuie sur le sous-systeme `rag/` ;
- le composant `9` doit etre branche des la V1 pour surveiller les autres.

## 4. Architecture fonctionnelle IA

Le systeme IA est organise en quatre couches principales.

### 4.1 Data Layer

Responsabilite : preparer des donnees propres et exploitables.

Fonctions attendues :

- ingestion des transactions, SMS, OCR, CSV et autres entrees supportees ;
- parsing et mapping vers un format transaction standard ;
- normalisation des champs ;
- validation de qualite et detection d'incoherences ;
- stockage structure des donnees.

Sortie attendue :

- transactions propres et pretes pour analyse.

### 4.2 Analytics Layer

Responsabilite : transformer les donnees en intelligence exploitable.

Modules attendus :

- categorisation automatique ;
- prevision financiere ;
- detection d'anomalies ;
- scoring budgetaire ;
- recommandations.

Sortie attendue :

- indicateurs ;
- signaux IA ;
- scores ;
- alertes ;
- recommandations actionnables.

### 4.3 RAG Layer

Responsabilite : fournir des reponses fiables basees sur des documents.

Fonctions attendues :

- ingestion de documents ;
- indexation vectorielle ;
- recherche semantique ;
- generation de reponse avec sources.

Contraintes obligatoires :

- utilisation exclusive de documents valides ;
- reponses avec citations explicites ;
- refus de repondre si les donnees sont insuffisantes ou non fiables.

### 4.4 Orchestration et supervision

Responsabilite : coordonner et superviser l'ensemble du systeme.

Fonctions attendues :

- centralisation des signaux IA ;
- priorisation des alertes ;
- monitoring des modeles ;
- journalisation ;
- supervision humaine.

## 5. Exigences fonctionnelles

| ID | Domaine | Attendus operationnels |
| --- | --- | --- |
| EF1 | Traitement des donnees | Parser plusieurs formats, normaliser les transactions, detecter les incoherences. |
| EF2 | Categorisation | Proposer une categorie, fournir un score de confiance, integrer la correction utilisateur, apprendre via le feedback. |
| EF3 | Prevision | Predire le solde futur, afficher l'incertitude, detecter le risque de deficit. |
| EF4 | Anomalies | Detecter les depenses inhabituelles, generer une alerte explicable, permettre la validation utilisateur. |
| EF5 | Scoring | Produire un score de 0 a 100, une classe de risque et les facteurs explicatifs. Le score doit rester explicable. |
| EF6 | Recommandations | Proposer des actions concretes, les prioriser et estimer leur impact. |
| EF7 | RAG | Fournir des reponses documentees, afficher les sources, refuser en cas d'incertitude elevee. |
| EF8 | Orchestration | Agreger les signaux IA, declencher des actions internes et generer des rapports. |
| EF9 | Matching experts | Analyser le besoin utilisateur, proposer des experts et fournir un score d'adequation. |
| EF10 | Monitoring | Mesurer la performance des modeles, detecter le drift et declencher le retraining si necessaire. |

## 6. Gouvernance, securite et supervision humaine

### 6.1 Transparence

- chaque resultat IA doit etre explicable ;
- aucune boite noire ne doit etre exposee sans explication exploitable ;
- les facteurs qui influencent un score ou une recommandation doivent etre consultables.

### 6.2 Confidentialite

- acces restreint aux donnees sensibles ;
- consentement obligatoire pour les usages qui le requierent ;
- audit des acces et des traitements.

### 6.3 Tracabilite

Les elements suivants doivent etre journalises :

- scores ;
- decisions ;
- acces ;
- requetes API ;
- sorties critiques des modules IA.

### 6.4 Supervision humaine

Les actions suivantes ne doivent pas etre automatisees sans validation humaine :

- decision de credit ;
- suppression de donnees ;
- modification critique d'un dossier ou d'une configuration sensible.

## 7. Contraintes techniques

### 7.1 Performance

- temps de reponse faible ;
- latence minimale sur les parcours critiques.

### 7.2 Scalabilite

- support de la croissance du nombre d'utilisateurs ;
- support de l'augmentation du volume de donnees ;
- capacite a faire evoluer les pipelines et les modeles sans refonte globale.

### 7.3 Modularite

Chaque module doit etre :

- independant ;
- testable ;
- remplacable ;
- observable.

### 7.4 Logging et audit

- toutes les actions doivent etre tracees ;
- les decisions IA doivent etre enregistrees ;
- les erreurs et refus doivent etre auditables.

## 8. Gestion du cold start

Cas couverts :

- nouvel utilisateur ;
- historique insuffisant ;
- faible volume de donnees.

Approche attendue :

- appliquer des regles simples par defaut ;
- produire un score partiel si necessaire ;
- afficher un message d'incertitude lorsque la confiance est insuffisante ;
- enrichir progressivement les signaux a mesure que les donnees augmentent.

## 9. KPIs IA

Les mesures suivantes doivent etre suivies :

- precision de categorisation ;
- qualite des previsions ;
- taux de faux positifs sur les anomalies ;
- adoption des recommandations ;
- satisfaction utilisateur ;
- stabilite du score.

## 10. Tests et validation

Avant mise en production :

- tests sur dataset controle ;
- tests sur cas reels ;
- validation produit ;
- verification des garde-fous de securite et d'explicabilite.

Apres mise en production :

- tests continus ;
- monitoring ;
- revue des incidents et des refus ;
- suivi de drift et de degradation de performance.

## 11. Evolution continue

Le systeme doit evoluer via :

- amelioration des modeles ;
- enrichissement du RAG ;
- ajout de nouveaux signaux ;
- optimisation des recommandations.

Contraintes de gouvernance :

- versioning ;
- documentation ;
- validation avant deploiement ;
- compatibilite avec l'architecture existante.

## 12. Conclusion

Le systeme IA de FinPersoAI est un systeme modulaire, explicable et supervise qui transforme les donnees financieres en :

- analyses ;
- alertes ;
- recommandations ;
- score ;
- aide a la decision.

Il repose sur :

- une couche data ;
- une couche analytique ;
- une couche documentaire ;
- une couche d'orchestration.

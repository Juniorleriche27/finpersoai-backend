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

## 3. Architecture fonctionnelle IA

Le systeme IA est organise en quatre couches principales.

### 3.1 Data Layer

Responsabilite : preparer des donnees propres et exploitables.

Fonctions attendues :

- ingestion des transactions, SMS, OCR, CSV et autres entrees supportees ;
- parsing et mapping vers un format transaction standard ;
- normalisation des champs ;
- validation de qualite et detection d'incoherences ;
- stockage structure des donnees.

Sortie attendue :

- transactions propres et pretes pour analyse.

### 3.2 Analytics Layer

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

### 3.3 RAG Layer

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

### 3.4 Orchestration et supervision

Responsabilite : coordonner et superviser l'ensemble du systeme.

Fonctions attendues :

- centralisation des signaux IA ;
- priorisation des alertes ;
- monitoring des modeles ;
- journalisation ;
- supervision humaine.

## 4. Exigences fonctionnelles

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

## 5. Gouvernance, securite et supervision humaine

### 5.1 Transparence

- chaque resultat IA doit etre explicable ;
- aucune boite noire ne doit etre exposee sans explication exploitable ;
- les facteurs qui influencent un score ou une recommandation doivent etre consultables.

### 5.2 Confidentialite

- acces restreint aux donnees sensibles ;
- consentement obligatoire pour les usages qui le requierent ;
- audit des acces et des traitements.

### 5.3 Tracabilite

Les elements suivants doivent etre journalises :

- scores ;
- decisions ;
- acces ;
- requetes API ;
- sorties critiques des modules IA.

### 5.4 Supervision humaine

Les actions suivantes ne doivent pas etre automatisees sans validation humaine :

- decision de credit ;
- suppression de donnees ;
- modification critique d'un dossier ou d'une configuration sensible.

## 6. Contraintes techniques

### 6.1 Performance

- temps de reponse faible ;
- latence minimale sur les parcours critiques.

### 6.2 Scalabilite

- support de la croissance du nombre d'utilisateurs ;
- support de l'augmentation du volume de donnees ;
- capacite a faire evoluer les pipelines et les modeles sans refonte globale.

### 6.3 Modularite

Chaque module doit etre :

- independant ;
- testable ;
- remplacable ;
- observable.

### 6.4 Logging et audit

- toutes les actions doivent etre tracees ;
- les decisions IA doivent etre enregistrees ;
- les erreurs et refus doivent etre auditables.

## 7. Gestion du cold start

Cas couverts :

- nouvel utilisateur ;
- historique insuffisant ;
- faible volume de donnees.

Approche attendue :

- appliquer des regles simples par defaut ;
- produire un score partiel si necessaire ;
- afficher un message d'incertitude lorsque la confiance est insuffisante ;
- enrichir progressivement les signaux a mesure que les donnees augmentent.

## 8. KPIs IA

Les mesures suivantes doivent etre suivies :

- precision de categorisation ;
- qualite des previsions ;
- taux de faux positifs sur les anomalies ;
- adoption des recommandations ;
- satisfaction utilisateur ;
- stabilite du score.

## 9. Tests et validation

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

## 10. Evolution continue

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

## 11. Conclusion

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

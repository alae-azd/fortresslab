# Mission 0.2 — Analyse de 3 incidents de sécurité réels

## Introduction

L’étude d’incidents réels est essentielle pour comprendre concrètement les enjeux de la cybersécurité. Ces événements permettent d’illustrer comment des failles techniques ou organisationnelles peuvent être exploitées à grande échelle.
Dans ce document, nous analysons trois incidents majeurs : SolarWinds (2020), Colonial Pipeline (2021) et Log4Shell (2021), en mettant en évidence leur impact sur les trois piliers fondamentaux de la sécurité : confidentialité, intégrité et disponibilité.

---

## Incident 1 — SolarWinds 2020

### Ce qui s'est passé

SolarWinds, éditeur du logiciel Orion utilisé pour la supervision réseau, a été victime d’une attaque sophistiquée de type supply chain.
Les attaquants ont réussi à compromettre le pipeline de développement et à injecter un code malveillant dans une version officielle du logiciel.

Cette version compromise a ensuite été distribuée sous forme de mise à jour légitime, signée par SolarWinds. En installant cette mise à jour, les clients ont involontairement déployé une backdoor (SUNBURST) dans leurs systèmes.

Les attaquants ont ainsi obtenu un accès discret à de nombreuses organisations pendant plusieurs mois, leur permettant d’observer et d’exfiltrer des données sensibles.

### Piliers CIA Triad violés

* **Confidentialité** : fortement compromise, car les attaquants ont pu accéder à des emails et des données internes.
* **Intégrité** : compromise, le code source a été modifié et les mises à jour distribuées étaient altérées.
* **Disponibilité** : non impactée directement, l’objectif étant de rester furtif.

### Leçons apprises

* Sécuriser les pipelines CI/CD est critique pour éviter les attaques supply chain.
* La signature numérique ne garantit pas l’absence de code malveillant.
* Mettre en place des contrôles d’intégrité sur le code avant déploiement.
* Surveiller les comportements réseau anormaux (communications externes suspectes).

---

## Incident 2 — Colonial Pipeline 2021

### Ce qui s'est passé

Colonial Pipeline a subi une attaque par ransomware après qu’un attaquant ait utilisé des identifiants VPN compromis.
Le compte utilisé ne disposait pas d’authentification multifacteur, ce qui a facilité l’accès au réseau interne.

Une fois à l’intérieur, les attaquants ont déployé un ransomware qui a chiffré des données critiques. Par mesure de précaution, l’entreprise a stoppé ses opérations, provoquant une perturbation majeure de l’approvisionnement en carburant aux États-Unis.

### Piliers CIA Triad violés

* **Confidentialité** : violée, des données ont été exfiltrées avant chiffrement.
* **Intégrité** : compromise, les données ont été altérées via chiffrement.
* **Disponibilité** : fortement impactée, arrêt des opérations pendant plusieurs jours.

### Leçons apprises

* Activer le MFA sur tous les accès distants est indispensable.
* Surveiller les fuites de credentials sur internet et le dark web.
* Mettre en place une segmentation réseau stricte entre IT et OT.
* Disposer de sauvegardes isolées et testées régulièrement.

---

## Incident 3 — Log4Shell 2021

### Ce qui s'est passé

Log4Shell est une vulnérabilité critique découverte dans la bibliothèque Java Log4j.
Elle permet à un attaquant d’exécuter du code à distance simplement en envoyant une chaîne spécialement conçue qui sera loggée par l’application.

Cette vulnérabilité a été massivement exploitée dans le monde entier en très peu de temps, affectant un grand nombre de services et infrastructures.

### Piliers CIA Triad violés

* **Confidentialité** : compromise, un attaquant peut accéder aux données du système.
* **Intégrité** : compromise, possibilité de modifier ou injecter du code.
* **Disponibilité** : menacée, car l’attaquant peut perturber ou arrêter les services.

### Leçons apprises

* Maintenir un inventaire précis des dépendances logicielles (SBOM).
* Mettre en place un processus de patch management rapide.
* Utiliser des WAF pour bloquer les patterns d’exploitation connus.
* Limiter les connexions sortantes des serveurs pour réduire les risques.

---

## Conclusion

Ces trois incidents illustrent des vecteurs d’attaque différents : chaîne d’approvisionnement, compromission d’accès et vulnérabilité logicielle.
Ils montrent que la sécurité ne repose pas uniquement sur des outils, mais sur une combinaison de bonnes pratiques : gestion des accès, surveillance, mise à jour et architecture sécurisée.

L’analyse de ces cas met en évidence l’importance d’une approche globale de la cybersécurité, intégrant à la fois des mesures techniques et organisationnelles.

# Note d'identification des sources — Acerox Métallurgie

> Document remis à **Sébastien Marchand** (chef de projet industrialisation
> Acerox). **2-3 pages max.** Public : décideur métier non-technique —
> langage courant, pas de jargon scikit-learn ou SQL.
> Auteur : `Célia` — Date : `30-06-2026`

## 1. Contexte

Acerox est une entreprise de Metallurgie, qui possède 3 site de production.

Ils ont mis en place un modèle il y a 2 ans pour prédire les NC et faire une maintenance par anticipation.

Il souhaiterait améliorer ce modèle, notamment pour mieux détecter sur le site de Roubaix qui semble avoir beaucoup plus de NC non détectée.

## 2. Demande métier reformulée

Ce que Sébastien a demandé : Réduire de 20% les NC non détectée sur le site de Roubaix.

Ce que je comprends qu'il cherche vraiment : Mieux prédire les NC sur les 3 sites de production et mieux comprendre les causes qui font que le site de Roubaix a des taux supérieurs de NC comparé aux autres sites.

## 3. Inventaire des sources

> Une ligne par source que **vous** avez identifiée (fichiers reçus +
> ce que vous découvrez en explorant les données). Le nom exact du fichier
> fait partie de l'inventaire à dresser.

| Source | Format | Volume | Période / Fréquence | Qualité observée | Risques RGPD | Pertinence métier |
|---|---|---|---|---|---|---|
| *`capteurs_iot.csv`* | CSV | 51 000 mesures — 3 sites, 8 capteurs (Lyon×1, Saint-Étienne×3, Roubaix×4) | Avril 2026 — quasi-continu (~1 750 mesures/capteur) | 749 valeurs manquantes sur `vibration_mms` (~1,5 %). Pics aberrants : température max 160 °C, vibration max 12 mm/s. Lyon très sous-représenté (1 seule ligne). | Aucun — pas de donnée personnelle | **Haute** — température, vibration et débit directement liés à la détection de NC |
| *`erp_export.json`* | JSON | 2 000 ordres (Roubaix 1 108 / Saint-Étienne 892 — Lyon absent) | Avril 2026 — ~70 ordres/jour en moyenne | 109 valeurs manquantes sur `ouvrier_id` (~5,5 %). Statuts : 78 % terminé, 10 % en cours, 7 % suspendu, 5 % annulé. | **Moyen** — `ouvrier_id` est un identifiant nominatif à pseudonymiser | **Modérée** — statuts suspendu/annulé potentiellement corrélés à des arrêts NC |
| *`logs_machines.log`* | TXT (parsing regex) | 30 000 événements — 3 sites, 4 lignes | Avril 2026 — ~1 événement/minute | 100 % parsé. 75 % INFO / 19 % WARN / 6 % ERROR. Roubaix LINE-3 : **153 emergency_stop** vs 27-44 pour les autres lignes. Lyon sous-représenté (1 ligne). | Aucun — pas de donnée personnelle | **Très haute** — arrêts d'urgence, alertes vibration/température corrélables avec les capteurs IoT |

## 4. Recommandations

> 3-5 puces. Quelles sources ingérer en priorité ? Lesquelles écarter et
> pourquoi ?

- ...
- ...
- ...

## 5. Points à clarifier avec Sébastien

> 3-5 questions ouvertes restantes — preuve de lucidité sur ce qu'on ne
> sait pas encore.

1. ...
2. ...
3. ...

## 6. Limites de cette note

> Ce qu'on n'a **pas** fait, et qu'il faudrait faire plus tard.

- Pas d'analyse statistique fouillée des sources (M3-B1 = identification,
  pas EDA complète)
- Pas d'AIPD juridique formelle (recommandation : escalader au DPO Acerox)
- ...

---

*Note produite par <prénom>, <date>, dans le cadre du brief M3-B1 ATOS.*

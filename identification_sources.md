# Note d'identification des sources — Acerox Métallurgie

> Document remis à **Sébastien Marchand** (chef de projet industrialisation
> Acerox). **2-3 pages max.** Public : décideur métier non-technique —
> langage courant, pas de jargon scikit-learn ou SQL.
> Auteur : `Célia` — Date : `30-06-2026`

## 1. Contexte

Acerox est une entreprise de Metallurgie, qui possède 3 site de production sur: Lyon, Saint-Etienne et Roubaix.

Nous avons réalisé un entretien avec le client le 30-06-2026 pour recueillir son besoin, ses attentes et mieux comprendre les enjeux de la demande.

Lors de l'entretien, le client nous a indiqué que les 3 sites de production sont équivalents et ont chacun 1 seule ligne de production.

La société a mis en place un modèle il y a 2 ans pour prédire les défauts et permettra une maintenance préventive.

Le client souhaiterait améliorer ce modèle, notamment pour mieux détecter les défauts sur le site de Roubaix qui semble avoir beaucoup plus de non conformités (NC) non détectées que les autres sites.

## 2. Demande métier reformulée

Le client souhaite réduire de 20% les NC non détectées sur le site de Roubaix.

Au travers de sa demande, nous proposons de mieux prédire les NC sur les 3 sites de production pour avoir une meilleure anticipation, ainsi que de mieux comprendre les causes qui font que le site de Roubaix a des taux supérieurs de NC comparé aux autres sites.

## 3. Inventaire des sources

Le client nous a fourni 3 sources de données :

| Source | Format | Volume | Période / Fréquence | Qualité observée | Risques RGPD | Pertinence métier |
|---|---|---|---|---|---|---|
| *`capteurs_iot.csv`* | CSV | 51 000 mesures — 3 sites, 8 capteurs (Lyon×1, Saint-Étienne×3, Roubaix×4) | Avril 2026 — quasi-continu (~1 750 mesures/capteur) | 749 valeurs manquantes sur `vibration_mms` (~1,5 %). Pics aberrants : température max 160 °C, vibration max 12 mm/s. Lyon très sous-représenté (1 seule ligne). | Aucun — pas de donnée personnelle | **Haute** — température, vibration et débit directement liés à la détection de NC |
| *`erp_export.json`* | JSON | 2 000 ordres (Roubaix 1 108 / Saint-Étienne 892 — Lyon absent) | Avril 2026 — ~70 ordres/jour en moyenne | 109 valeurs manquantes sur `ouvrier_id` (~5,5 %). Statuts : 78 % terminé, 10 % en cours, 7 % suspendu, 5 % annulé. | **Moyen** — `ouvrier_id` est un identifiant nominatif à pseudonymiser | **Modérée** — statuts suspendu/annulé potentiellement corrélés à des arrêts NC |
| *`logs_machines.log`* | TXT (parsing regex) | 30 000 événements — 3 sites, 4 lignes | Avril 2026 — ~1 événement/minute | 100 % parsé. 75 % INFO / 19 % WARN / 6 % ERROR. Roubaix LINE-3 : **153 emergency_stop** vs 27-44 pour les autres lignes. Lyon sous-représenté (1 ligne). | Aucun — pas de donnée personnelle | **Très haute** — arrêts d'urgence, alertes vibration/température corrélables avec les capteurs IoT |

## 4. Recommandations

Après une rapide analyse des données concernant ces 3 sources, voici nos recommandations : 
- Les 2 sources à ingérer en priorité sont capteurs_iot.csv et logs_machines.log. Elles contiennent toutes deux des informations capitales sur les lignes de production pour prévenir les non conformités, et anticiper les périodes de maintenance.
- La source erp_export.json donne des informations sur les commandes et les statuts, mais il ne semble pas y avoir de corrélation entre les statuts des commandes et les arrets, ni entre les salariés et les arrêts. C'est donc une source à écarter.

La qualité des données est satisfaisante : 
* Pas de données manquante significative (Seulement ouvrier_id sur erp_export.json qui n'est pas signifitcatif)
* Volume de données suffisant pour un premier EDA
* Les 3 sources sont sur une même période temporelle, ce qui permet de faire des corrélations
* Il sera nécessaire de formatter les données issues des logs machines pour les structurer et pouvoir s'en servir

Il y a cependant un point déterminant qu'il va falloir approfondir: contrairement au contexte initial, il s'avère qu'il y a plusieurs lignes de productions sur différents sites. Nous aurons donc besoin d'élements complémentaires pour mieux évaluer le dispositif et ainsi pondérer correctement le modèle pour éviter les biais.
 

## 5. Points à clarifier avec Sébastien

1. Il y a 4 lignes de production sur Roubaix, 3 sur St Etienne, et 1 seule sur Lyon. Les 3 sites ne sont donc pas équivalents. Peut-on avoir des indications plus précises sur ces différentes lignes de production, leurs spécificités (équipement ou contraintes spécifiques) et leur ancienneté ?
2. Quelle est la température nominale de fonctionnement des lignes en générale si elles sont comparables ? Et en particulier de la LINE-3 de Roubaix ?
3. Quelle est la vibration nominale de fonctionnement des lignes en générale si elles sont comparables ? Et en particulier de la LINE-3 de Roubaix ?
4. Les seuils de détection des capteurs sont différents selon les sites : sur Lyon 1 seul capteur fait 10 000 mesures, contre 3 capteurs sur aint Etienne qui font respectivement 7000 mesures, et contre les 4 capteurs de Roubaix qui font chacun seulement 5 mesures. Est-ce qu'il y a une raison pour expliquer ces différences ? Par exemple par rapport au volume de données que cela représente et donc à la capacité de stockage ?
5. Quel est le critère dans les éléments donnés qui permet d'identifier un arret suite à une NC sur la ligne de production ? Peut-on utiliser les "emergency_stop" comme evenement de ce type ? En effet, nous avons besoin d'identifier quel est le critère cible pour mesurer les détections des incidents que l'on souhaite éviter et surtout d'évaluer l'amélioration approtée par le nouveau modèle.

## 6. Limites de cette note

- Pas d'analyse statistique fouillée des sources, pas encore une EDA complète, pas d'identification claire des biais.
- Pas d'AIPD juridique formelle (recommandation : escalader au DPO Acerox)
- Pas d'analyse du modèle existant, ni de ses données d'entrainement.

---

*Note produite par Célia, 30-06-2026, dans le cadre du brief M3-B1 ATOS.*

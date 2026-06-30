# Notes d'entretien — Sébastien Marchand (Acerox)

> Notes prises au fil de l'eau pendant l'entretien fictif 9h30-10h00.
> **5 catégories à pré-remplir** avant l'entretien. Tu complètes au fil
> de l'eau. Distinction explicite *dit* (ce que Sébastien a effectivement
> formulé) vs *interprété* (ta lecture).
>
> Conservé en livrable du brief.

**Date** : `30-06-2026` — **Durée** : 30 min — **Présents** : Sébastien Marchand (chef de projet industrialisation Acerox) + `Célia` (FastIA)

---

## 1. Besoin métier

> Quelle décision Sébastien veut-il améliorer ? Quel KPI métier ?
> - Interpreté : Le client souhaite un modèle plus prédictif, qui permette d'anticiper les défauts pour faire une maintenance 48h à l'avance. L'objectif est de faire baisser le taux de NC sur le site de Roubaix dans les 3 mois avec une baisse significative (-20% de NC)


* Combien de sites de production ? Est-ce qu'ils produisent tous la meme chose ?
- Dit : 3 sites de production (lyon, roubais, saint etienne), meme production, IT sur Lyon, même volume de production. Lignes comparables. 1 ligne de production par site.
- Interpreté : 3 site de production équivalents, mais plus de NC sur le site de Roubaix.

* Quel est le rythme de production ?
- Dit : production en continue

* Y a-t-il plusieurs type de défaut à détecter ?
- Dit : peu important, ce qui est important c'est qu'il y a un défaut, une non conformité (NC)
- Interpreté : pas de catégorisation des défauts nécessaire, mais ce qui compte c'est la prédiction d'un défaut quel qu'il soit.

* Que produit le modèle aujourd'hui ?
- Dit : éviter les défaut des pièces produites dans les 3 sites. Si un défaut est constaté, il faut tout arreté pour faire la maintenance de la ligne.
- Interprété : Le taux de prédiction n'est pas suffisant et cela entraine des couts de mise à l'arret.


* Comparaison des sites ?
- Dit : les 3 sites ont le meme age, mais taux de NC plus élevé sur le site de roubaix (ligne 3)

* Evolution dans les lignes de production ?
- Dit: augmentation significative des NC sur le site roubaix mais on ne sait pas pourquoi
- Interpreté : Les sites sont équivalents, mais il se passe quelque chose sur le site de roubaix qu'il va falloir mieux prédire dans le modèle.

* Quel est l'objectif du modèle :
- Dit : nous souhaitons mieux anticiper les défauts pour prévoir une maintenance 48h à l'avance, car si une pièce non conforme passe, il faut tout arreter et coute au global env 12K€/pièce
- Interpreté: la mise à l'arret suite à une NC non détectée leur coute chere. La maintenance dure 48h.

## 2. Sources et formats

> Qu'a-t-il à disposition ? Où ? Sous quel format ?
> Interpreté : Le client a a disposition : logs machine (TXT) + logs des capteurs (CSV) en continue sur les 3 sites. Le stockage est 100% sur le site de Lyon (IT).

* Avez vous des capteurs ?
- Dit: Oui, des capteurs sur  les lignes, on a des logs machine (TXT) et logs des capteurs (CSV) .
- Interpreté : plusieurs sources par ligne.

* Personnes qui ont mis en place le modèle sont-elles consultables ?
- Dit : les personnes ne sont plus là
- Interprété : il va falloir analyser l'existant et espérer qu'il soit bien documenté.

## 3. Volumétrie et fréquence

> Combien de données ? À quelle cadence arrivent-elles ?

* Depuis quand le modèle est-il en place ? Depuis quand avous des données ?
- Dit : Depuis 2 ans, je suppose que vous aurez accès aux données depuis 2 ans.
- Interpreté : on a potentiellement le volume de données des 2 années précédentes sur les 3 lignes de production.

* Combien de lignes ?
- Dit : 3 lignes de production

* Y a-t-il des périodes de mise à l'arret ou au contraire des impossibilités de mise à l'arret ?
- Dit : non, production continue

## 4. Contraintes (RGPD, sécurité, propriété)

* Lignes semi automatisées :
- Dit : on sait quel individu est sur la ligne
- Interpreté : attention probablement une donnée sensible.

* Avez vous des données sensibles ?
- Dit : Oui, l'identifiant ouvrier (EMP-1)
- Interpreté : attention donnée sensible sur les employés

* Stockage données ?
- Dit : 100% interne, pour des questions de souveraineté. Pas de modèle cloud.
- Interpeté : attention aux stockage des données et du modèle, enjeux de souveraineté.

## 5. Critères de succès

> Comment Sébastien saura-t-il qu'on a réussi ?
> Interpreté : Faire baisser le taux de NC sur le site de Roubaix dans les 3 mois avec une baisse significative (-20% de NC)

* Quel seuil de faux positif est acceptable ? 
- Dit : Fausse alerte peu couteux par rapport à pièce non conforme, mais il ne faut pas qu'il y en ait tous les secondes non plus.
- Interpreté : il vaut mieux de fausses alertes qu'une absence de détection d'une NC.

* Type de décision ?
- Dit : Alerte qui permettent à l'humain de déciser si arret de la ligne remontées
- Interpreté : le résultat du modèle est soumis à une décision humaine, il y a donc un controle sur l'action, ce n'est pas tout automatisé.

* Quel est votre objectif ? Qu'est-ce que vous attendez de l'amélioration ?
- Dit : Y avoir plus clair sur ce qui est en place pour savoir comment avancer. On ne constate pas de dérive technique du modèle, mais il rate des NC.
- Interpeté : il faut améliorer le modèle pour qu'il détecte mieux les NC.

---

## Questions restées sans réponse

* Combien de défauts sont actuellement détectés par le modèle existant ?
=> Ne sait pas (NSP)

* Volume des maintenances ou des NC actuellement ?
=> NSP

* Combien de temps sont conservées les données
=> NSP

* Combien de capteurs par ligne ?
=> NSP

* Volumétrie 
=> NSP, capteur de ligne = continue donc important

* Taux de NC actuel ?
=> NSP

* volume de pièces produit ?
=> NSP

## Mes impressions à chaud (10 min après)

> Le modèle a besoin d'etre améliorer sur le taux de détection des NC. Il se passe quelque chose sur le site de roubaix qu'il va falloir essayer de mettre en lumière.

---

*Notes d'entretien produites par Célia, 30-06-2026.*

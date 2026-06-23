---
name: iterer-qualite
description: >-
  Élève la qualité d'un agent IA SmartFunnel au-delà de la simple conformité, dans deux
  cas. (1) Itération experte sur un livrable : utilise ce skill dès que quelqu'un veut
  améliorer, affiner, sharpener, retravailler ou faire passer un cap à un script, une
  persona ou des réponses d'agent (« ça c'est pas ouf », « rends ça meilleur »).
  (2) Analyse de conversations réelles : utilise-le dès qu'on veut analyser, décortiquer
  ou tirer des leçons de vraies conversations d'un agent en prod pour le faire progresser.
  Le mode itération passe le livrable au crible de plusieurs lentilles expertes (répartie,
  conversion, naturel, lead difficile) ; le mode analyse récupère les conversations dans
  Setteo (list-conversations, list-conversation-messages, get-agent-stats) et en sort des
  améliorations concrètes. Ce skill améliore, il ne remplace pas l'audit bloquant
  (auditer-agent).
---

# Itérer sur la qualité d'un agent

Ce skill sert à **élever** la qualité, pas à valider. C'est sa différence avec
`auditer-agent` :

- **`auditer-agent`** = porte bloquante. « A-t-on le droit de pousser ? » Repère les
  violations.
- **`iterer-qualite`** = élévation. « Est-ce aussi bon que ça pourrait l'être ? » Affine
  la répartie, la conversion, le naturel — au-delà de la simple conformité.

Toute amélioration repasse ensuite par l'audit avant un nouveau push : améliorer ne
court-circuite pas le verrou.

---

## Mode 1 — Tours experts (itération sur un livrable)

Pour un script ou une persona en cours, repérer les failles « ça c'est pas ouf » et
affiner. Passer le livrable au crible de **plusieurs lentilles expertes**, chacune
proposant une version plus tranchante :

1. **Lentille répartie / copywriting** — chaque message est-il net, naturel, dans la voix
   du client ? Le wording peut-il être plus serré, moins tiède ?
2. **Lentille conversion / vente** — chaque étape fait-elle réellement avancer vers le
   CTA ? Les objections sont-elles traitées avec la meilleure répartie ? Le CTA RDV (les 3
   ingrédients) est-il vraiment percutant, pas juste présent ?
3. **Lentille naturel / anti-bot** — ça se lit comme un humain ? Cette lentille attrape le
   « ça fait bot » subtil qui n'est pas une violation dure.
4. **Lentille lead difficile** — stress-test : comment ça tient face à un lead évasif,
   sceptique ou hostile ?

Pour chaque lentille : pointer ce qui faiblit, puis **proposer une réécriture**. Itérer
jusqu'à ce que le livrable soit tranchant, pas seulement conforme. Charger
`golden-agents-conventions` pour rester dans les clous pendant qu'on affine.

**Sortie** : les messages retravaillés + une note de ce qui a été amélioré et pourquoi.

---

## Mode 2 — Analyse de conversations réelles

Pour un agent en prod, apprendre de ce qui se passe vraiment et en sortir des
améliorations concrètes.

1. **Identifier l'agent cible** (depuis le contexte, ou via `list-agents` / `list-tenants`
   si besoin — ne pas deviner le mauvais tenant).
2. **Récupérer les conversations** : `list-conversations` (filtrer par agent / période),
   puis `list-conversation-messages` pour le contenu réel. `get-agent-stats` pour la vue
   agrégée.
3. **Analyser** selon ces axes :
   - **Points de décrochage** — où les leads arrêtent-ils de répondre ?
   - **Patterns d'échec** — l'agent lâche-t-il un prix ? sonne-t-il bot ? ne recadre-t-il
     pas une objection ? double question ? (recouper avec les conventions)
   - **Objections non gérées** — les objections récurrentes que l'agent rate.
   - **Qualité de qualification** — la qualification progresse-t-elle vraiment vers le CTA ?
   - **Ce qui marche** — les schémas gagnants, à renforcer.
4. **Restituer** des améliorations **concrètes et actionnables** (pas un constat vague) :
   quel node, quel message, quel correctif.

**Sortie** : un diagnostic court + une liste d'améliorations précises, prêtes à réinjecter
dans `generer-script-chatbot` (ou en édition directe via `pousser-agent-setteo`), puis
re-auditer.

---

## Garde-fou commun

Ne jamais dégrader un message déjà bon au nom de « l'amélioration », et **pas de stats
inventées** : tout chiffre vient de `get-agent-stats` ou d'un scan réel. Mieux vaut trois
améliorations justes qu'une refonte gratuite.

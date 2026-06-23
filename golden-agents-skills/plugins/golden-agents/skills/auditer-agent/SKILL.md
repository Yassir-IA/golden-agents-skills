---
name: auditer-agent
description: >-
  Audit qualité bloquant d'un agent IA SmartFunnel avant mise en ligne. Utilise ce skill
  dès qu'un script, un prompt, une persona ou un agent chatbot doit être relu, vérifié,
  contrôlé ou audité, ou dès que quelqu'un demande « est-ce prêt à pousser ? », « est-ce
  conforme ? », « qu'est-ce qui cloche ? », ou veut un go / no-go avant activation.
  Fonctionne sur un prompt collé OU sur un agent récupéré depuis Setteo via MCP. Renvoie
  des flags classés par gravité (bloquant / à corriger / suggestion), avec où, pourquoi
  et comment corriger, puis un verdict GO / NO-GO basé sur la check-list pré-activation.
  C'est l'étape de contrôle obligatoire : rien ne se pousse sans être passé par cet audit.
---

# Audit d'un agent (étape bloquante avant push)

Ce skill contrôle un agent fini avant sa mise en ligne et rend un **verdict GO / NO-GO**.
C'est le filet de sécurité qualité : il rattrape ce qui « fait bot » ou viole les
conventions avant que l'agent parte en production.

## Entrée acceptée

- Un **prompt / script / persona collé** dans la conversation, ou
- Un **agent existant dans Setteo** : le récupérer via le MCP Setteo (`view-agent`) avant
  d'auditer. Ne jamais auditer de mémoire un agent en prod, toujours partir de l'état réel.

## Principe directeur (à ne pas oublier)

**Ne flaguer que les vrais problèmes.** Comme le validateur Setteo est un juge non
déterministe, l'audit ne doit pas dégrader des messages déjà conformes ni inventer des
reproches. Mieux vaut trois flags justes que vingt flags de bruit. Si un message respecte
les conventions, on le laisse.

## Déroulé en 3 temps

### Temps 1 — Patterns bot universels (passe rapide)

Scanner ces 7 patterns. Ils trahissent un agent au premier coup d'œil :

1. **Openers d'accusé de réception** — messages qui s'ouvrent par « Bien noté »,
   « Parfait », « Je comprends ».
2. **Jargon interne** — vocabulaire métier / technique qui fuite vers le lead.
3. **Bombardement multi-bulles** — plusieurs messages d'affilée, ou un message en
   plusieurs bulles, au lieu d'un échange tour par tour.
4. **IA têtue** — l'agent répète la même relance / la même question sans tenir compte de
   la réponse du lead.
5. **Anti-répétition mécanique** — formules de transition recyclées à l'identique d'un
   message à l'autre.
6. **Qualification vague** — questions trop générales qui ne font pas avancer la
   qualification vers le CTA.
7. **Complaisance face aux fausses réponses** — l'agent valide ou enchaîne sur une
   réponse incohérente ou évasive au lieu de recadrer.

### Temps 2 — Dimensions QA (passe approfondie)

Charger le skill `golden-agents-conventions`, puis lire
`references/dimensions-qa.md` et auditer **dimension par dimension** (8 dimensions +
2 bonus techniques). Pour chaque écart : noter **où** (persona ou node précis), **quoi**,
**pourquoi ça compte**, et le **correctif**.

### Temps 3 — Verdict et check-list

Dérouler `references/checklist-pre-activation.md` (18 points). **GO** uniquement si aucune
case n'est non conforme. **Un seul « non » → NO-GO**, on n'active pas.

## Gravité des flags

- **Bloquant** — on n'active pas tant que ce n'est pas corrigé. Exemples : prix chiffré,
  promesse sans mécanisme réel, double question, lien en markdown, verbe de réservation,
  claim chiffré non validé, stop qui argumente, `width` manquant, tenant non confirmé.
- **À corriger** — dégrade la qualité, à fixer avant push. Exemples : formule bannie,
  accusé de réception mécanique, message trop long, qualification vague.
- **Suggestion** — amélioration non bloquante.

## Format de sortie (toujours le même)

```
VERDICT : GO / NO-GO — [une phrase de justification]

BLOQUANTS (NO-GO tant qu'ils sont présents)
- [dimension] — [problème] · où : [persona / node X] · correctif : [...]

À CORRIGER
- [dimension] — [problème] · où : [...] · correctif : [...]

SUGGESTIONS
- [...]

CHECK-LIST PRÉ-ACTIVATION
[les 18 points avec [x] / [ ] / [s.o.]]
```

Si l'entrée est trop incomplète pour trancher (ex. persona absente, pas de doc client),
le dire explicitement et lister ce qui manque, plutôt que de rendre un faux GO.

## Exemples de flags bien formulés

**Exemple 1 — bloquant**
Message audité : « Le programme est à 1 500 €, tu veux que je te book un call ? »
Flag : *Anti-pitch + verbe de réservation* — où : node step_cta · problème : prix chiffré
en conversation, et « je te book ». · correctif : retirer le prix (tout vers le CTA), et
remplacer par « Je te garde un créneau ? ».

**Exemple 2 — à corriger**
Message audité : « Parfait, bien noté ! Super, je reviens vers toi. »
Flag : *Ton et formules* — où : node step_relance · problème : accusé de réception
mécanique + formules bannies (« parfait », « super », « bien noté »). · correctif :
réécrire sans accusé de réception ni formule bannie.

---
name: brief
description: >-
  Transforme une conversation brute (transcript d'appel, notes, loom, formulaire
  d'onboarding) en brief client structuré, prêt à alimenter la création d'un agent.
  Utilise ce skill dès que quelqu'un a un appel, un transcript, des notes ou un onboarding
  à transformer en brief, à structurer ou à synthétiser, ou dès qu'on démarre un nouveau
  client et qu'il faut clarifier l'offre, l'objectif funnel, les claims autorisés, les
  interdictions et les témoignages avant de créer l'agent. Le skill extrait et structure
  tout ce dont la génération a besoin, signale ce qui manque pour débloquer la discovery
  (étape 0), et ne reproduit jamais les coquilles d'un transcript. Lève un flag si un
  claim chiffré ou une promesse « fait-à-ta-place » apparaît sans validation.

---

# Brief client à partir d'une conversation brute

Ce skill prend un matériau brut (transcript d'appel, notes, transcript de loom,
formulaire d'onboarding) et le transforme en **brief structuré**, prêt à nourrir le choix
du Golden Agent et la génération. C'est la porte d'entrée de la chaîne (étapes 0 et 2).

## Hiérarchie des sources de vérité

Quand les sources se contredisent :

1. Le formulaire d'onboarding = brief initial.
2. **Les décisions orales en call priment** sur le formulaire.
3. (Plus tard, une fois l'agent pushé, Setteo devient la source de vérité — hors scope ici.)

## Garde-fou transcripts

Les artefacts de transcription sont fréquents (« CTO » / « CETO » = Setteo, « Webbook » =
webhook, noms propres déformés). Un transcript est une **source, pas une vérité
littérale** :

- Ne jamais reproduire une coquille de transcription dans le brief.
- En cas de doute sur un terme ou un chiffre, le **signaler comme ambigu** plutôt que de
  trancher au hasard.

## Structure du brief (toujours la même)

```
BRIEF CLIENT — [nom]

## Client & offre
- Client / marque :
- Offre (nom, promesse principale) :
- Persona cible :

## Objectif funnel
- Étape funnel : [before webinar / after event / evergreen / prise de RDV / bridge VSL / relance post-VSL]
- Canal : [WhatsApp / Instagram]
- CTA visé : [booking / redirection VSL] + lien

## Ton & style
- Ton du client :
- Politique identité IA (« t'es un bot ? ») : [choix du client]
- Prénom souhaité pour l'agent (à vérifier, collisions) :

## Claims & preuves
- Claims chiffrés AUTORISÉS et validés : [liste, ou « aucun validé »]
- Témoignages validés : [liste, ou « aucun »]

## Interdictions formelles (ce qu'on ne dit JAMAIS)
- [liste tirée des CGV / du doc commercial]

## Mécanismes d'exécution disponibles (pour les promesses tenables)
- [scenario / cron / template / humain — ce que l'agent peut réellement promettre]

## Sources fournies
- [onboarding / transcript / copy tunnel / Thank You Page / etc.]

---
À VALIDER / MANQUANT POUR DÉBLOQUER L'ÉTAPE 0
- [ce qui manque : doc commercial, claims non validés, interdictions absentes,
  témoignages, pitch closer type…]
- FLAGS : [tout claim chiffré ou promesse « fait-à-ta-place » repéré mais non validé]
```

## Le verrou de l'étape 0

L'étape 0 (discovery) est **bloquante**. Le brief ne « complète » pas les trous tout
seul : il les **expose**. La règle absolue : tout claim chiffré public ou promesse
« fait-à-ta-place » sans doc commercial validé → on lève le flag et on s'arrête. Le brief
doit donc toujours rendre lisible ce qui est validé et ce qui ne l'est pas.

Un brief complet alimente ensuite `choisir-golden-agent` puis `generer-script-chatbot`.

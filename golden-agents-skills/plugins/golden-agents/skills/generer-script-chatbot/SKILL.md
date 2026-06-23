---
name: generer-script-chatbot
description: >-
  Génère la persona ET le script (flow) d'un agent IA conversationnel SmartFunnel, prêt à
  auditer puis pousser dans Setteo. Utilise ce skill dès que quelqu'un veut créer, écrire,
  produire, cloner ou adapter un agent, un chatbot, un script de tunnel de vente ou une
  persona pour un client, quelle que soit l'étape funnel (before webinar, after event,
  evergreen, prise de RDV, bridge vers VSL, relance post-VSL). Le skill clone la structure
  du Golden Agent le plus proche, réécrit tout le wording dans le ton du client, applique
  les conventions, et marque par un placeholder tout claim factuel manquant (jamais
  inventé). Produit directement le livrable, sans questions. Charge
  golden-agents-conventions et enchaîne ensuite sur auditer-agent.
---

# Génération d'un agent (persona + script)

Ce skill produit directement les deux livrables d'un agent — sa **persona** et son
**script / flow** — en clonant un Golden Agent existant et en l'adaptant au nouveau
client. Il ne repart jamais de zéro et ne pose pas de questions : il produit, puis
signale ce qui reste à compléter.

## Avant de produire : recap des inputs (pas de questions)

Ne pas interroger l'utilisateur. À la place, **récapituler en deux lignes** ce qui a été
fourni et ce qui manque, puis produire quand même. Inputs attendus :

- Formulaire d'onboarding (brief initial par défaut).
- Copywriting complet du tunnel + Thank You Page.
- Offre, persona cible, transcripts d'appels (discovery et vente).
- Doc commercial : claims chiffrés autorisés, interdictions, témoignages.

**Hiérarchie des sources de vérité** : (1) le formulaire d'onboarding = brief initial ;
(2) les décisions orales en call **priment** sur le formulaire ; (3) une fois l'agent
pushé, **Setteo devient la source de vérité** (re-fetch avant toute édition, jamais de
re-push d'un vieux prompt local).

**Attention aux transcripts** : les artefacts de transcription sont fréquents (« CTO » /
« CETO » = Setteo, « Webbook » = webhook, noms propres déformés). Un transcript est une
source, pas une vérité littérale : ne jamais reproduire ses coquilles.

## La méthode : clonage intelligent

1. **Partir du Golden Agent le plus proche** de l'objectif funnel (identifié à l'étape
   « choisir-golden-agent », ou fourni par l'utilisateur). L'objectif funnel prime sur la
   niche.
2. **Cloner le squelette, pas le wording** : calquer la structure logique du Golden Agent
   (étapes, détections, branches d'objections), mais **réécrire chaque message** dans le
   ton du nouveau client.
3. **Appliquer les conventions** : charger le skill `golden-agents-conventions` et s'y
   tenir sur toute la production (structure des messages, ton, anti-pitch, CTA 3
   ingrédients, dé-temporalisation si after/evergreen, interdits gravés à 2 niveaux).
4. **Construire la persona** selon `references/structure-persona.md`.

C'est le squelette du Golden Agent qui porte la logique propre à l'étape funnel : le skill
n'invente pas une structure « before » ou « after » abstraite, il adapte une vraie
structure éprouvée.

## Règle d'or : inventer le ton, jamais les faits

Le skill **invente librement** ce qui relève du style : ton, transitions, formulation,
enchaînement, wording du squelette. C'est ce qui permet de produire un brouillon complet
sans aller-retour.

En revanche, il **n'invente JAMAIS un fait** : prix, chiffre, résultat client, garantie,
témoignage, ou promesse « fait-à-ta-place » précise. Tout claim factuel absent du contexte
fourni est remplacé par un **placeholder visible**, jamais par une valeur fabriquée :

```
[[À COMPLÉTER : claim chiffré à valider par le client]]
[[À COMPLÉTER : témoignage validé]]
```

Raison : faire promettre à l'agent un chiffre non validé peut violer les CGV du client.
Ces placeholders seront **bloqués par l'audit** (point « zéro placeholder ») : c'est
voulu. L'AM les remplit depuis le doc commercial réel avant de pousser.

## Auto-test avant de rendre

Simuler mentalement les 5 conversations les plus probables et corriger les trous avant de
livrer :

1. Lead chaud qui va au bout sans friction (chemin nominal).
2. Lead qui demande le prix → l'agent ne donne jamais de prix, recadre vers le CTA.
3. Lead sceptique / objection type de la niche.
4. Lead qui demande « t'es un bot ? » → politique d'identité IA du client.
5. Lead qui dit stop / se désinscrit → exit immédiat, sans argumenter, sans lien.

## Format de sortie

```
PARTIE 1 — PERSONA
[la persona, selon structure-persona.md]

PARTIE 2 — SCRIPT / FLOW
[les étapes / nodes, avec pour chacun : message court + instruction détaillée]

PARTIE 3 — À COMPLÉTER AVANT AUDIT
- [liste des placeholders [[À COMPLÉTER]] et des inputs manquants]
```

Ne pas pousser ce livrable tel quel. **Prochaine étape obligatoire : `auditer-agent`**
(étape bloquante), puis seulement la mise en ligne via `pousser-agent-setteo`.

---
name: choisir-golden-agent
description: >-
  Aide à choisir le Golden Agent à cloner comme point de départ d'un nouvel agent. Utilise
  ce skill dès que quelqu'un démarre un nouvel agent et se demande par quoi commencer, quel
  agent prendre comme base, quel modèle ou référence cloner, ou quel Golden Agent
  correspond à un objectif funnel (before webinar, after event, evergreen, prise de RDV,
  bridge VSL, relance post-VSL, DM Instagram). Le skill applique la règle « l'objectif
  funnel prime sur la niche », consulte la bibliothèque des Golden Agents, et vérifie
  l'état réel et les performances directement dans Setteo (list-tenants, list-agents,
  get-agent-stats) plutôt que de se fier à des chiffres figés. Renvoie l'agent recommandé
  avec son ID, son tenant et ce qui reste à confirmer.
---

# Choisir le Golden Agent de départ

Ce skill aide à choisir le **point de départ** d'un nouvel agent : le Golden Agent le plus
proche, qu'on clonera ensuite. Le principe du système est de **ne jamais repartir de
zéro**.

## La règle de sélection : l'objectif funnel prime sur la niche

Choisir d'abord selon l'**objectif funnel** (before webinar, after event, evergreen, prise
de RDV, bridge VSL, relance post-VSL, DM Insta), **avant** la niche. Un flow before
webinar d'une autre niche est un meilleur point de départ qu'un flow VSL de la même niche.

## Déroulé

1. **Identifier l'objectif funnel** du nouveau client (depuis le brief si disponible).
2. **Consulter la bibliothèque** : `references/bibliotheque-golden-agents.md`, organisée
   par objectif funnel. Repérer le ou les candidats.
3. **Respecter les verrous de niche** : certains flows sont spécifiques et ne se clonent
   pas hors contexte (Cœur Patrimoine = patrimoine / investissement uniquement ; Big Wave
   = agences marketing uniquement). La bibliothèque les signale.
4. **Vérifier l'état réel dans Setteo** — ne jamais se fier aux chiffres figés de la
   bibliothèque :
   - `list-tenants` → confirmer le bon tenant (méfiance sur les homonymes, ex. Scalezia
     209 vide vs 210, NP corp 199 vs 200).
   - `list-agents` → confirmer que l'agent existe encore et est actif dans ce tenant.
   - `get-agent-stats` → récupérer les **performances réelles à jour**, plutôt que le
     relevé daté de la bibliothèque.
5. **Recommander** un agent, avec une alternative si pertinent.

## Format de sortie

```
GOLDEN AGENT RECOMMANDÉ
- Agent : [nom] (agent X · tenant Y)
- Objectif funnel : [...]  ·  Niche : [...]
- Pourquoi : [ce qui le rend le plus proche du besoin]
- Perf à jour (Setteo) : [chiffres get-agent-stats, ou « à récupérer »]
- À confirmer : [tenant exact, état actif, points « à confirmer » de la bibliothèque]

ALTERNATIVE (si pertinent)
- [...]
```

Quand le meilleur point de départ est un agent encore marqué « à confirmer » dans la
bibliothèque, le dire clairement et proposer de le recouper dans Setteo avant de s'en
servir.

L'agent choisi alimente ensuite `generer-script-chatbot`, qui en clone le squelette.

## Faire vivre la bibliothèque

Un nouvel agent rejoint la bibliothèque quand il tient ses KPI sur plusieurs semaines de
scan. **Pas de stats inventées** : tout chiffre vient d'un scan hebdo ou de
`get-agent-stats`. Si le calcul exact est impossible, donner une fourchette et le signal
utilisé.

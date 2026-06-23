# Bibliothèque des Golden Agents (index curé)

> **Comment lire ce fichier.** C'est un **index curé**, organisé par objectif funnel.
> La curation — *quel* agent est Golden et *pour quoi* il sert de base — se maintient
> ici. L'**état réel** (existe encore, actif) et les **chiffres de performance** se
> vérifient toujours dans Setteo (`list-agents`, `get-agent-stats`, `list-tenants`),
> jamais en se fiant à cette page. Les chiffres ci-dessous sont des **relevés datés**, à
> reconfirmer.
>
> Un agent devient Golden quand il tient ses KPI sur plusieurs semaines de scan
> (taux de réponse, complétion, leads bookés). Quand c'est le cas, on l'ajoute ici avec
> niche, objectif et ce qui le rend clonable.

Les IDs sont au format `agent X · tenant Y`. En cas de doute sur un tenant, `list-tenants`
fait foi.

---

## VSL & acquisition directe

**Scalezia — agent_starterpack (agent 813 · tenant 210)**
- Niche : généraliste / haut de gamme (entrepreneurs B2B, offre StarterPack).
- Référence absolue pour l'acquisition / VSL. Persona « Victor ». Sa persona implémente
  déjà toutes les conventions (1 question/message, step_prix, disqualification B2C, liens
  bruts). **C'est de cet agent que vient le pattern CTA RDV en 3 ingrédients.**
- Attention : deux tenants « Scalezia » existent, le 209 est vide, l'agent vit dans le 210.
- Perf [relevé 12/06/2026, à reconfirmer] : ~10 % de leads bookés, 406 conversations.

**Cœur Patrimoine — agent_vsl (agent 703 · tenant 195)**
- Niche : investissement / finances. **À utiliser exclusivement pour le patrimoine et
  l'investissement** (flow très spécifique, ne pas cloner ailleurs).
- Complété par l'agent 733 « follow-up lien rdv ».
- Perf [relevé 12/06/2026, à reconfirmer] : ~10 % de conversion, 2 277 conversations.

**Creative Group — agent_creativeprods (agent 833 · tenant 202)**
- Niche : B2B / agence (offre Creative Prods, persona « Emma »).
- Excellente référence globale en production. Un doublon « creative group » existe en 121.

**Méthode Big Wave (tenant 208)**
- Niche : marketing digital. Flow mentalement différent des autres VSL, **adapté aux
  agences marketing uniquement — ne pas cloner hors de ce contexte.**

**VSL historiques — à confirmer**
- L'agent 482 (« VSL Whatsapp - 19122025 », créé déc. 2025) existe mais est inactif et
  sans persona attachée. Noms et tenant **à confirmer auprès de l'équipe** avant usage.

---

## Before webinar & relances

**LegalPlace (tenant 256, affiché « ScalePlace IA »)**
- **Meilleure référence actuelle pour le Before Webinar** (agent « cheaté »).
- Perf [relevé, à reconfirmer] : ~50 % de taux de réponse, ~35 % de complétion.

**La Fabrique à Clients (tenant 75)**
- Canal WhatsApp (Challenge IA de Christophe, persona « Julia »).
- Agents 836/837 « before connu / inconnu », 216 et 915 « after ».
- Flow atypique avec sélection directe des liens de groupe WhatsApp.
- Perf : taux de passage très élevés *[la métrique « 4 885 » du référentiel est ambiguë :
  préciser de quoi et sur quelle période avant de la citer]*.

**Implémentations récentes de référence (before / after)**
- David Villain / DV Immobilier (tenant 179, agents 1453/1452) et Papa Inshape
  (tenant 159, agent 1459) : les implémentations **les plus récentes** des conventions
  (width 260, dé-temporalisation, tags T1). Bon point de départ « propre ».

---

## After webinar / show-up / no-show

**NP Corp (tenants 199 et 200)**
- Objectif : after / challenge. **Deux tenants quasi homonymes** (« NP corp » 199 et
  « np Corp » 200) : vérifier lequel porte l'agent avant tout push.

**Mindeo — Théo Ritzy (tenant 133)**
- Objectif : show-up / IClosed.

**Runz (tenant 148)**
- Objectif : no-show / IClosed.

> Rappel : tout agent after / evergreen multi-relances doit être **dé-temporalisé** (voir
> `golden-agents-conventions`). Cas vécu : Mon Plan Immo (tenant 156, agent 1451).

---

## Réseaux sociaux (Instagram)

**Bodytime**
- Objectif : flows DM Insta (Agent Flow 1, Flow 2).

**Runz (tenant 148)**
- Objectif : automatisation Comments & Posts. Pour les funnels commentaires/stories IG
  avec bascule vers Setteo, voir le skill `funnel-insta-amorce-manychat` (non couvert ici).

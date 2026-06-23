---
name: pousser-agent-setteo
description: >-
  Pousse un agent IA validé dans Setteo via le MCP : création de la persona et de l'agent,
  sandbox, et préparation à l'activation. Utilise ce skill dès que quelqu'un veut pousser,
  créer, déployer, mettre en ligne, publier ou installer un agent dans Setteo, ou éditer
  un agent existant dans Setteo. Vérifie d'abord que l'audit est passé et qu'il ne reste
  aucun placeholder, confirme le tenant via list-tenants AVANT tout create (aucun delete
  n'est possible via MCP), applique les conventions techniques, crée l'agent inactif,
  rejoue les 5 conversations en sandbox, puis laisse l'activation manuelle en UI. Charge
  golden-agents-conventions pour les conventions techniques.
---

# Mise en ligne d'un agent dans Setteo (via MCP)

Ce skill prend un agent **déjà audité** et le pousse dans Setteo. Il effectue de vraies
écritures, et **aucun delete n'est possible via le MCP** : une création dans le mauvais
tenant se nettoie à la main. La prudence sur le tenant n'est donc pas optionnelle.

Charger d'abord `golden-agents-conventions` et suivre
`references/conventions-techniques-setteo.md` sur toute la procédure.

## Pré-vol : 3 verrous avant toute écriture

Ne créer **rien** tant que ces trois points ne sont pas vrais :

1. **Audit passé** — l'agent a un verdict GO de `auditer-agent`. On ne pousse jamais un
   NO-GO.
2. **Zéro placeholder** — plus aucun `[[À COMPLÉTER]]` dans la persona ni dans le flow.
3. **Tenant confirmé** — appeler `list-tenants` et identifier le bon tenant **avant tout
   create**. Méfiance sur les homonymes (ex. deux « Scalezia » dont un vide, deux
   « NP corp » quasi identiques) : vérifier l'ID exact, pas juste le nom.

## Avant de construire les nodes

Lire la ressource de schéma de flow Setteo (`prompt-flow-schema`) avant de construire
`prompt_nodes` / `prompt_edges`, et la ressource `agent-settings-schema` pour l'objet
`data`. Appliquer les conventions techniques, en particulier :

- `"width": 260` à la racine de **chaque** node (au même niveau que `id` / `type` /
  `position`, pas dans `data`).
- Un `step_0001` explicite avec `is_start: true`.
- Aucun `inPath` / `hasPath` (auto-purgés).
- Nouveaux nodes en préfixe `NEW_`, jamais suffixe `_v2`.
- `message` court (1-5 lignes) / `instruction` détaillée. Aucun node de pur routage.

## Séquence de création

1. **`create-persona`** — `name`, `tenant_id`, et `description` (la persona en markdown :
   ton, sujets interdits, style, langue). Récupérer le `persona_id` retourné. Les
   `validation_rules` sont extraites automatiquement de la persona.
2. **`create-agent`** — `label`, `platform` (`whatsapp` ou `instagram`), `tenant_id`,
   `persona_id`, plus `prompt_nodes`, `prompt_edges`, `prompt_variables`. L'agent est créé
   **inactif** par défaut. Le `prompt_compiled` est **auto-généré** à partir des nodes :
   ne jamais le saisir à la main.
3. **Tags** — `list-tags` d'abord, puis `create-tag` pour 1 tag « [label agent] - reply »
   + 1 tag par lien sortant (analytics). Pour le pattern « template + agent », régler le
   tag en auto-assign sur le keyword du template.
4. **Templates WhatsApp** — si des relances partent hors fenêtre 24h, un template
   Meta-approved est obligatoire (sinon Meta bloque l'envoi).

## Validation en sandbox

- **`create-sandbox`**, puis rejouer via **`send-sandbox-message`** les 5 conversations
  types : chemin nominal, demande de prix, objection de la niche, « t'es un bot ? »,
  stop / désinscription.
- **`view-sandbox`** pour relire le comportement. **`reset-sandbox-conversation`** entre
  deux scénarios si besoin.
- Vérifier que le comportement respecte les conventions (pas de prix, exit propre sur
  stop, une question à la fois, etc.).

## Activation

- L'agent reste `is_active=false`. **L'activation est manuelle, dans l'UI Setteo** — elle
  ne passe pas par le MCP.
- La **timezone est laissée en UTC** par le MCP : la fixer dans l'UI après création.

## Éditer un agent existant (cycle de vie)

- `update-agent` ne fonctionne **que si `is_active=false`**. Workflow : (1) UI → toggle
  off, (2) `update-agent` via MCP, (3) UI → toggle on.
- `data`, `prompt_nodes`, `prompt_edges` sont des **remplacements complets** : faire
  `view-agent` d'abord, reconstruire l'objet entier, puis pousser. Un envoi partiel efface
  le reste.
- Les conversations en cours pendant la fenêtre continuent avec l'ancien prompt jusqu'à
  leur fin.
- Le validateur Setteo est un juge IA non déterministe et non bloquant : ne corriger que
  les vrais flags, ne pas dégrader des messages conformes. Vérifier les `validation_rules`
  héritées du tenant après ré-extraction.

## Point à compléter par l'équipe

Le référentiel signale que `send_exact: true` ne garantit rien au niveau du LLM (il ne
voit pas le wording exact) et renvoie à un **workaround** propre à ce skill — workaround
qui n'est pas documenté dans le référentiel. **À récupérer auprès de l'équipe et à
insérer ici.**

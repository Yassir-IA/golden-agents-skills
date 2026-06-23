# Conventions techniques Setteo (push via MCP)

Les pièges ci-dessous ont tous été vécus en production. Chaque push qui les ignore coûte
un aller-retour de correction. Cette référence est surtout utile à l'étape de mise en
ligne (push et édition d'un agent dans Setteo).

## 1. Structure des nodes et du flow

- **`"width": 260` OBLIGATOIRE à la racine de CHAQUE node** (au même niveau que
  `id` / `type` / `position`, **PAS** dans `data`). Sans `width`, Setteo affiche les
  nodes en barres horizontales géantes au lieu de cartes verticales façon SMS. (Erreur
  vécue : agent Optima 1455, 26/05/2026.)
- **Convention message / instruction** : le champ `message` reste court (1-5 lignes,
  c'est ce qui s'affiche dans le graph) ; le champ `instruction` porte le détail
  (variantes, règles, comportement LLM au runtime). Ne JAMAIS mettre l'instruction
  longue dans `message`, sinon le graph devient illisible.
- **Pas de nodes de pur routage** (un node dont le message est du type « Branchement →
  step_xxx ») : Setteo route nativement via les edges + detections.
- **Toujours un `step_0001` explicite avec `is_start: true`**, même silencieux.
- **Jamais d'`inPath` / `hasPath`** dans les nodes/edges envoyés (auto-purge silencieuse
  côté Setteo).
- **Nouveaux nodes : préfixe `NEW_`, jamais suffixe `_v2`** (collision d'IDs Setteo).
- **Positions standards** : main flow x=400, gap y=320 ; objections à partir de x=950,
  gap x=550, gap y=320 entre `_start` et `step`.

## 2. Cycle de vie d'un agent

- **`update-agent` : le champ `data` d'un node est un *full replacement*.** Repasser
  TOUS les settings du node ; un `data` partiel efface le reste. Au niveau agent :
  `view-agent` d'abord, reconstruire le `data` complet, puis pousser.
- **`update-agent` ne fonctionne que si `is_active=false`.** Le toggle off/on se fait
  **manuellement dans l'UI Setteo**. Workflow : (1) UI → toggle off, (2) `update-agent`
  via MCP, (3) UI → toggle on. Les conversations en cours pendant la fenêtre continuent
  avec l'ancien prompt jusqu'à leur fin.
- **`send_exact: true` ne garantit RIEN au niveau LLM** (le LLM ne voit pas le wording
  exact).
- **`prompt_compiled` est régénéré automatiquement** à partir des nodes/edges : ne
  jamais le saisir à la main.
- **Pas de `delete-agent` / `delete-persona` via MCP** : une création dans le mauvais
  tenant se nettoie à la main. **Confirmer le tenant via `list-tenants` AVANT tout
  `create`.**
- **Validateur Setteo = juge IA non déterministe, non bloquant** : ne corriger que les
  vrais flags (prix chiffré, double question, markdown de lien, verbe de réservation),
  ne pas dégrader des messages déjà conformes. Vérifier les `validation_rules` héritées
  du tenant après une ré-extraction.

## 3. Tags, templates et fenêtre WhatsApp

- **Un node ne peut pas poser de tag** : le seul levier est le tag auto-assign sur
  mot-clé du message agent. Convention : 1 tag « [label agent] - reply » par agent, 1
  tag par lien sortant (pour l'analytics). `list-tags` avant tout `create-tag`.
- **Hors fenêtre 24h WhatsApp Business** : tout message proactif passe par un template
  Meta-approved obligatoire (sinon Meta bloque l'envoi).
- **Pattern « template + agent »** : un keyword dans le template active un tag
  `auto_assign_message_type=agent` → l'agent prend la conversation.
- **1 template suffit** pour les broadcasts internes au funnel (show-up, relance
  booking). Les variantes pack (v2/v3) ne servent que pour les premiers messages à haut
  volume (opt-in, réactivation).

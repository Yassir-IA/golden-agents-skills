# Dimensions QA de l'audit (passe approfondie)

> **Note.** Le référentiel mentionne « 8 dimensions + 2 bonus » sans toutes les
> détailler. Les dimensions ci-dessous sont **dérivées des conventions**
> (`golden-agents-conventions`) et constituent une base cohérente, à confirmer ou
> ajuster par l'équipe. Pour le détail de chaque règle, charger le skill
> `golden-agents-conventions` : cette liste dit **quoi vérifier**, le skill conventions
> dit **pourquoi** et donne les formulations exactes.

Pour chaque dimension : ce qu'on vérifie, et la gravité d'une violation.

## Dimension 1 — Structure des messages

- 1 seule question par message, toujours ouverte. Deux interrogatives → **bloquant**.
- 2-3 phrases max, proportionnel au lead. Message-pavé → **à corriger**.
- Liens en brut, sur leur propre ligne. Lien en markdown / hyperlien / entre crochets →
  **bloquant**.
- Aucun tiret long (cadratin — ou demi-cadratin –). Présence → **à corriger**.

## Dimension 2 — Ton et formules

- Aucune formule bannie (voir `formules-bannies.md` du skill conventions). Présence →
  **à corriger**.
- Aucun accusé de réception mécanique (« Bien noté », « Parfait », « Je comprends »), ni
  sa répétition d'un message à l'autre. Présence → **à corriger**.

## Dimension 3 — Identité IA

- Posture floue par défaut (assistant·e de [figure]) ; l'agent ne clame jamais être une
  IA spontanément. Violation → **à corriger**.
- Politique en cas de « t'es un bot ? » conforme au choix du client (vérifier le doc du
  client). Non conforme → **bloquant**.
- Prénom de l'agent sans collision (équipe du client + SmartFunnel). Collision →
  **à corriger**.

## Dimension 4 — Anti-pitch et logique de vente

- Aucun prix, aucune garantie, aucun détail de module en conversation. Présence →
  **bloquant**.
- Tout converge vers le CTA (booking ou redirection VSL). Dérive → **à corriger**.
- CTA RDV : les 3 ingrédients présents (pression sociale douce + promesse opérationnelle
  + CTA soft). Manque → **à corriger**.
- Aucun verbe de réservation (« je te book », « je réserve »). Présence → **bloquant**.

## Dimension 5 — Promesses tenables (dimension explicitement bloquante)

- Toute promesse de l'agent (« je te renvoie X », « tu recevras Y ») a un mécanisme
  d'exécution réel : scenario, cron, template, ou humain. Promesse sans mécanisme →
  **bloquant**. C'est la dimension nommée comme bloquante dans le référentiel.

## Dimension 6 — Compliance stop / désinscription

- Sur stop / désinscription : exit immédiat, sans argumenter, sans redonner de lien.
  L'agent qui argumente ou renvoie un lien après un stop → **bloquant** (compliance
  WhatsApp).

## Dimension 7 — Dé-temporalisation (agents after / evergreen uniquement)

- Aucun repère temporel relatif (« ce soir », « hier », « demain matin ») dans les
  messages ni les instructions. Présence → **bloquant** pour ce type d'agent.
- Directive de dé-temporalisation présente dans la persona. Absente → **à corriger**.
- Sans objet (s.o.) si l'agent n'est pas un after / evergreen multi-relances.

## Dimension 8 — Implémentation des interdits à 2 niveaux

- Chaque interdit critique (style ET comportement) est gravé dans la persona **ET** dans
  l'instruction des nodes concernés. Interdit posé dans la persona seule → **à corriger**
  (le LLM l'oubliera au profit du contexte local du node).

---

## Bonus 1 — Conformité structurelle Setteo

- `"width": 260` à la racine de chaque node. Manquant → **bloquant** (affichage cassé).
- `step_0001` explicite avec `is_start: true`. Manquant → **bloquant**.
- Aucun `inPath` / `hasPath` dans les nodes/edges. Présence → **à corriger**.
- Nouveaux nodes en préfixe `NEW_`, jamais suffixe `_v2`. Violation → **à corriger**.
- `message` court (1-5 lignes) / `instruction` détaillée. Instruction longue dans
  `message` → **à corriger**.
- Aucun node de pur routage. Présence → **à corriger**.

## Bonus 2 — Tags, templates et fenêtre WhatsApp

- Tags conventionnels présents : 1 tag « [label agent] - reply » + 1 tag par lien
  sortant. Manquant → **à corriger**.
- Si relances hors fenêtre 24h : template Meta-approved en place. Manquant → **bloquant**
  (Meta bloquera l'envoi).

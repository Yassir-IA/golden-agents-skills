# SmartFunnel — Création d'agents IA (instructions projet)

Ce dossier sert à créer des agents IA conversationnels (chatbots de tunnel de vente)
pour les clients SmartFunnel, via les skills installés. Les Account Managers décrivent
leur besoin en langage naturel ; les skills s'enchaînent. Ce fichier garde le **process
dans l'ordre**, le **router** vers le bon skill, et un **filet de sécurité minimal**.

## Le process (dans l'ordre)

0. **Discovery client** — exiger le doc commercial (CGV, interdictions, claims
   autorisés, témoignages). **BLOQUANT** : pas de claim chiffré ni de promesse
   « fait-à-ta-place » sans ce doc. → skill `brief`
1. **Choisir le point de départ** — le Golden Agent le plus proche de l'objectif funnel
   (l'objectif prime sur la niche). → skill `choisir-golden-agent`
2. **Générer** — produire persona + script en clonant la structure, pas le wording.
   → skill `generer-script-chatbot`
3. **Auditer** — QA complète + 7 patterns bot + simulation des 5 conversations types +
   check-list. **BLOQUANT** : rien ne se pousse sans audit passé.
   → skill `auditer-agent`
4. **Mettre en ligne** — push Setteo via MCP, puis sandbox des 5 conversations, puis
   activation **manuelle dans l'UI** (l'agent se crée `is_active=false`). Confirmer le
   tenant via `list-tenants` AVANT tout `create`. → skill `pousser-agent-setteo`
5. **(Optionnel) Améliorer** — itérer sur le livrable, analyser les vraies conversations.
   → skill `iterer-qualite`

Les deux étapes **BLOQUANTES** (0 et 3) ne se sautent pas. Après une génération,
enchaîne toujours vers l'audit avant d'envisager une mise en ligne.

## Quel skill pour quel besoin (router)

- « créer / écrire / cloner un agent, un script, une persona » → `generer-script-chatbot`
- « relire, vérifier, auditer, est-ce conforme » → `auditer-agent`
- « quel agent comme base, par quoi commencer » → `choisir-golden-agent`
- « pousser, mettre en ligne, créer dans Setteo » → `pousser-agent-setteo`
- « transformer un appel / des notes en brief » → `brief`
- « améliorer un agent existant, analyser des conversations » → `iterer-qualite`
- question sur les règles, le ton, les formules bannies, le style →
  `golden-agents-conventions`

## Règles catastrophiques (filet de sécurité)

Liste complète et à jour dans le skill `golden-agents-conventions` (source de vérité).
Ce qui suit est un rappel minimal, toujours présent :

- Jamais de prix, garantie ou détail de module en conversation ; tout converge vers le
  CTA.
- Jamais inventer un claim chiffré sans doc commercial validé.
- 1 question ouverte par message, 2-3 phrases max, liens en brut sur leur propre ligne.
- Toute promesse de l'agent = mécanisme d'exécution réel, sinon on ne pousse pas.
- Stop / désinscription = sortie immédiate, sans argumenter, sans lien.
- Interdit critique gravé à 2 niveaux : persona ET instruction des nodes.
- `"width": 260` à la racine de chaque node ; confirmer le tenant via `list-tenants`
  avant tout `create` (pas de delete via MCP).

## Pour les conventions détaillées

Les skills de génération et d'audit appliquent automatiquement le skill
`golden-agents-conventions`. Ne pas recopier ces règles ici : ce fichier ne garde qu'un
filet de sécurité minimal.

---

*Installation : en **Cowork**, ce contenu va dans les **Folder Instructions** du dossier
(Réglages > Cowork, ou instructions de dossier). En **Claude Code**, il reste à la racine
du dépôt sous le nom `CLAUDE.md`.*

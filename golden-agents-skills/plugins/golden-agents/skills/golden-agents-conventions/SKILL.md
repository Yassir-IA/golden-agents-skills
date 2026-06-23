---
name: golden-agents-conventions
description: >-
  Conventions internes non négociables de SmartFunnel pour créer des agents IA
  conversationnels (chatbots de tunnel de vente). Utilise systématiquement ce skill
  dès qu'il s'agit d'écrire, cloner, relire, corriger ou auditer un script d'agent, une
  persona ou des messages de chatbot, ou dès que quelqu'un demande « est-ce conforme à
  nos standards », parle de formules bannies, de ton, de structure de message, de mise
  en brut des liens, de width Setteo ou de règles de style. C'est la référence partagée
  que les skills de génération et d'audit appliquent : en cas de doute sur ce qu'un bon
  agent doit faire ou éviter, charge ce skill. Contient les conventions
  conversationnelles, les conventions techniques Setteo et la liste canonique des
  formules interdites.
---

# Conventions des Golden Agents (SmartFunnel)

Ce skill est la **référence partagée** pour tous les agents IA SmartFunnel. Il définit
ce qu'un bon agent doit faire et éviter, indépendamment du client ou de l'étape funnel.
Les skills de génération l'appliquent **avant** de produire ; les skills d'audit
vérifient le livrable **contre** lui.

## Quand l'utiliser

- **Avant d'écrire ou cloner** un script ou une persona → applique ces conventions.
- **Pendant une relecture ou un audit** → vérifie le livrable contre ces conventions.
- **Sur question directe** (« est-ce conforme ? », formules bannies, ton, structure de
  message, liens, width Setteo) → c'est ici.

## Les règles catastrophiques (à ne jamais violer)

Ce bloc est le minimum vital. Le détail complet est dans les fichiers de référence
ci-dessous.

- **Jamais de prix, de garantie ni de détail de module** en conversation. Tout converge
  vers le CTA.
- **Jamais inventer un claim chiffré** ou une promesse « fait-à-ta-place » sans doc
  commercial validé du client.
- **1 seule question ouverte par message**, 2-3 phrases max.
- **Liens en brut**, sur leur propre ligne, sans markdown.
- **Toute promesse de l'agent = mécanisme d'exécution réel** (scenario, cron, template,
  humain), sinon on ne pousse pas.
- **Stop / désinscription = sortie immédiate**, sans argumenter, sans redonner de lien.
- **Tout interdit critique se grave à 2 niveaux** : persona ET instruction des nodes (le
  LLM oublie la persona seule).
- **Côté technique** : `"width": 260` à la racine de chaque node ; toujours confirmer le
  tenant via `list-tenants` avant un `create` (pas de delete possible via MCP).

## Détail complet (à lire selon le besoin)

- **`references/conventions-conversationnelles.md`** — structure des messages, ton,
  logique de vente, pattern CTA RDV, dé-temporalisation. À charger pour toute écriture
  ou relecture de script / persona.
- **`references/conventions-techniques-setteo.md`** — structure des nodes, cycle de vie
  de l'agent, tags / templates / fenêtre WhatsApp. À charger pour toute mise en ligne ou
  édition dans Setteo.
- **`references/formules-bannies.md`** — liste canonique des mots et formules interdits.
  Source de vérité unique : ne pas la dupliquer ailleurs.

## Principe d'application

Ces conventions ne sont pas un guide de style optionnel : ce sont les règles qui font
qu'un agent convertit au lieu de « faire bot ». Quand un cas n'est pas couvert
explicitement, raisonne **dans leur esprit** (sobriété, une question à la fois, tout
vers le CTA, ne rien promettre qu'on ne puisse tenir) plutôt que de revenir aux réflexes
génériques d'un assistant.

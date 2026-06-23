# Golden Agents — Skills SmartFunnel

Chaîne d'outils Claude pour créer des agents IA conversationnels **standardisés**, du
brief à la mise en ligne. On installe une fois, on reçoit les mises à jour via GitHub.

## Ce que ça contient

Un plugin (`golden-agents`) qui regroupe **7 skills** :

- **golden-agents-conventions** — le socle : les conventions non négociables (source de
  vérité unique).
- **brief** — transforme un appel, des notes ou un onboarding en brief client structuré.
- **choisir-golden-agent** — choisit le meilleur agent existant comme point de départ.
- **generer-script-chatbot** — génère la persona et le script de l'agent.
- **auditer-agent** — audit qualité **bloquant** avant toute mise en ligne.
- **pousser-agent-setteo** — pousse l'agent dans Setteo via le MCP.
- **iterer-qualite** — affine un livrable, ou analyse les conversations réelles en prod.

La chaîne : `brief → choisir-golden-agent → generer-script-chatbot → auditer-agent →
pousser-agent-setteo`, avec `iterer-qualite` en boucle d'amélioration.

## Installation (une fois par personne)

### Claude Code

```
/plugin marketplace add Yassir-IA/golden-agents-skills
/plugin install golden-agents@smartfunnel
```

### Cowork

Ajouter la même marketplace via l'interface plugins de Cowork (Réglages > plugins), puis
installer le plugin `golden-agents`. Voir l'aide « Use plugins in Cowork » pour les étapes
exactes de l'interface.

Une fois installé, **les skills se déclenchent tout seuls** : il suffit de décrire son
besoin en langage naturel, le bon outil vient. En secours, taper `/` liste les skills pour
en forcer un.

## Étape importante : l'orchestrateur (`CLAUDE.md`)

Le fichier `CLAUDE.md` (à la racine de ce repo) tient le **process dans l'ordre**, le
**router** vers les skills et le **filet de règles**. Il n'est pas distribué par le plugin
(c'est un autre mécanisme de Claude), donc à installer séparément :

- **Cowork** : copier le contenu de `CLAUDE.md` dans les **Global instructions**
  (Réglages > Cowork > Global instructions). À faire une fois, ça s'applique partout.
- **Claude Code** : placer `CLAUDE.md` à la racine du dossier de travail (ou le committer
  dans le repo où l'équipe crée les agents).

Sans cet orchestrateur, les skills fonctionnent quand même, mais le respect du process
(et de l'étape d'audit bloquante) est moins fiable.

## Prérequis : le connecteur Setteo (MCP)

Les skills **choisir-golden-agent**, **pousser-agent-setteo** et le mode analyse de
**iterer-qualite** ont besoin du connecteur Setteo (MCP) activé dans Claude. Les autres
(brief, génération, audit, conventions) fonctionnent sans.

## À compléter par l'équipe

Trois zones laissées volontairement ouvertes, à remplir avec vos données internes :

- La **persona-type « 7 blocs »** →
  `plugins/golden-agents/skills/generer-script-chatbot/references/structure-persona.md`
- Les **8 dimensions QA** si vous en aviez de précises →
  `plugins/golden-agents/skills/auditer-agent/references/dimensions-qa.md`
- Le **workaround `send_exact`** →
  `plugins/golden-agents/skills/pousser-agent-setteo/SKILL.md`

Et dans la durée : la **bibliothèque des Golden Agents** →
`plugins/golden-agents/skills/choisir-golden-agent/references/bibliotheque-golden-agents.md`
(la curation se maintient là ; les chiffres viennent de Setteo).

## Mettre à jour

Le plugin n'a pas encore de mise à jour automatique. Quand un skill est modifié et poussé
sur GitHub, chaque personne réinstalle pour récupérer la dernière version :

```
/plugin install golden-agents@smartfunnel
```

En tant que mainteneur du repo : une convention corrigée une fois est récupérée par toute
l'équipe à la réinstallation.

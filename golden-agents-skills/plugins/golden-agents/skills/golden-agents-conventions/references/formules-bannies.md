# Formules bannies — source de vérité unique

Ce fichier est **le seul endroit** où vit la liste des mots et formules interdits.
Ne pas en maintenir de copie ailleurs (ni dans une persona, ni dans un autre skill,
ni dans le doc Word) : tout pointe ici. Quand l'équipe décide de bannir une nouvelle
formule, on l'ajoute **ici uniquement**.

## Pourquoi ces formules sont bannies

Ce sont les tics qui font « sonner bot » : tournures de service client, accusés de
réception robotiques, enthousiasme corporate. Un lead les repère inconsciemment et le
message perd en naturel et en conversion.

## Mots et formules interdits

À bannir dans le wording des messages comme dans les instructions :

- parfait
- super
- ravi / ravie
- c'est noté
- excellente question
- n'hésitez pas / n'hésite pas

Cette liste reprend les formules explicitement bannies à ce jour. **Elle n'est pas
exhaustive** : enrichis-la au fil de la prod dès qu'un nouveau tic est repéré.

## Accusés de réception mécaniques

Bannir aussi les ouvertures de message en accusé de réception, **et leur répétition
d'un message à l'autre** :

- « Bien noté »
- « Parfait »
- « Je comprends »

Le problème n'est pas seulement le mot isolé : c'est le réflexe d'ouvrir chaque réponse
par un accusé de réception. Un agent qui commence trois messages d'affilée par « Je
comprends… » se trahit immédiatement.

## Comment faire respecter cette liste

Un interdit posé seulement dans la persona ne tient pas : le LLM l'oublie au profit du
contexte local du node. **Graver l'interdit à 2 niveaux** — dans la persona ET dans
l'instruction des nodes concernés. (Voir la règle d'implémentation des interdits dans
`conventions-conversationnelles.md`.)

## À noter — ponctuation

Les tirets longs (cadratin `—` et demi-cadratin `–`) sont également bannis, mais c'est
une règle de **format**, pas de lexique : elle vit dans `conventions-conversationnelles.md`
(section « Structure des messages »).

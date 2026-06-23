# Check-list pré-activation

À dérouler **intégralement** avant de passer un agent `is_active=true`. **Un seul
« non » = on n'active pas.** L'audit remplit chaque case : `[x]` conforme, `[ ]` non
conforme (= NO-GO), `[s.o.]` sans objet.

- [ ] 1. Doc commercial du client reçu et lu (CGV, interdictions, claims autorisés,
  témoignages).
- [ ] 2. Tenant confirmé via `list-tenants` avant le `create`.
- [ ] 3. Zéro placeholder dans la persona ET dans le flow.
- [ ] 4. Tous les nodes ont `"width": 260` à la racine.
- [ ] 5. `step_0001` explicite avec `is_start: true` ; aucun `inPath` / `hasPath`.
- [ ] 6. Aucun prix, garantie ni module dans les messages ; tout converge vers le CTA.
- [ ] 7. 1 seule question par message, messages de 2-3 phrases max.
- [ ] 8. Liens en brut, sur leur propre ligne, sans markdown.
- [ ] 9. Aucun cadratin ni demi-cadratin ; aucun mot de la liste bannie (voir
  `formules-bannies.md`).
- [ ] 10. Interdits critiques posés à 2 niveaux (persona + instruction des nodes).
- [ ] 11. Toute promesse de l'agent a un mécanisme d'exécution réel
  (scenario / cron / template / humain).
- [ ] 12. Branche stop / désinscription : exit immédiat, sans argumenter, sans lien.
- [ ] 13. Politique d'identité IA conforme au choix du client ; prénom de l'agent sans
  collision.
- [ ] 14. Agent after / evergreen : prompt dé-temporalisé à 2 niveaux (nodes + directive
  persona).
- [ ] 15. Dimensions QA et 7 patterns bot passés, flags corrigés.
- [ ] 16. Sandbox : les 5 conversations types rejouées dans Setteo, comportement validé.
- [ ] 17. Tags posés (« [label] - reply » + 1 par lien) ; templates Meta approuvés si
  relances hors 24h.
- [ ] 18. Timezone fixée en UI (le MCP la laisse en UTC).

**Verdict :** GO uniquement si aucune case n'est `[ ]`. Un seul « non » → NO-GO, on
n'active pas.

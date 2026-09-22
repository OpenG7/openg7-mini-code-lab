# OpenG7 Mini Code Lab — architecture

## Mission et état

Expérimenter, entraîner et spécialiser North Mini Code à partir de trajectoires OpenG7 vérifiées.
Le dépôt contient actuellement cadrage et gouvernance. Les modules décrits
dans le [README](../README.md) sont une architecture cible, pas du code livré.
Aucun build applicatif n’est disponible avant ajout de ses manifests et sources.

## Frontières

Le laboratoire possède datasets d’entraînement, expériences, adaptateurs et publication d’artefacts. Il réutilise AI Evals pour les évaluations et ne s’attribue aucun droit de production.

API/dashboard → expériences et workflows → domaine/datasets → ports de calcul, artefacts et AI Evals. Le GPU peut être distant; les métadonnées et preuves restent portables et versionnées.

## Invariants de conception

Appliquer les [invariants du projet](../AGENTS.md#périmètre-local) aux contrats,
aux adaptateurs et à leurs tests; ils restent définis à cet endroit unique.

## Évolution

Garder les contrats de domaine indépendants des fournisseurs et les effets dans
les adaptateurs. Pour matérialiser un module, documenter ses entrées/sorties,
consommateurs, permissions, état d’implémentation et validations disponibles.
Mettre à jour cette frontière si elle change; les consignes d’exécution restent
dans [AGENTS.md](../AGENTS.md), sans recopier une autre stack.

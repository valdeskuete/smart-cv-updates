# smart-cv-updates

Dépôt d'artefacts de mise à jour pour **Smart CV Desktop** (Tauri 2).

## Structure

```
smart-cv-updates/
├── windows-x86_64/
│   ├── latest.json
│   ├── Smart-CV_<version>_x64-setup.msi
│   └── Smart-CV_<version>_x64-setup.msi.sig
├── (darwin-x86_64/ … plus tard)
└── (linux-x86_64/  … plus tard)
```

## URL consommée par l'app

```
https://raw.githubusercontent.com/valdeskuete/smart-cv-updates/main/{{target}}/latest.json
```

## Important — comportement HTTP

GitHub raw renvoie **toujours 200** si le fichier existe.
La comparaison SemVer (version actuelle >= latest -> pas de mise à jour) est
réalisée **côté frontend** dans `useUpdater.ts` (Phase 4).

## Comment publier une nouvelle version

1. Build signé : `npm run desktop:build:signed`
2. Générer `latest.json` :

   ```bash
   python scripts/generate-latest-json.py \
     --version 1.1.0 \
     --notes "Corrections + améliorations UX" \
     --msi path/to/Smart-CV_1.1.0_x64-setup.msi \
     --sig  path/to/Smart-CV_1.1.0_x64-setup.msi.sig \
     --out  release-endpoint-structure/windows-x86_64/latest.json
   ```

3. Copier le `.msi` et le `.sig` dans le même dossier.
4. Commit + push sur la branche `main`.

## Signature

La clé publique est déjà configurée dans `tauri.conf.json` -> `plugins.updater.pubkey`.
Ne jamais committer la clé privée.

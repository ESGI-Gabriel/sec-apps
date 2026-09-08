# Kit "fork de reference" - Juice Shop, bloc A du module Securite des applications (5ESGI-AL)

Ce dossier remplace le "fork de reference" a preparer sur GitHub. Il contient uniquement les
fichiers a **ajouter par-dessus** un clone d'OWASP Juice Shop v20.1.1. Le code de Juice Shop
n'est pas inclus (trop volumineux, et il evolue : on part toujours d'un tag officiel).

## Contenu

```
docker-compose.yml            lancement local sur http://localhost:3000 (S1 a S6)
.github/workflows/ci.yml       workflow d'amorce : jobs build + lint, vert au depart
.npmrc                         reactive la generation du package-lock (upstream la desactive)
package-lock.json              lock file racine genere sur v20.1.1 (1458 entrees) - voir plus bas
docs/security/.gitkeep         dossier ou s'accumulent les livrables du fil rouge
```

Arborescence `docs/security/` visee en fin de bloc A (produite par les etudiants) :
`contexte.md` (S1), `threat-model.md` (S2), `triage-s3.md` (S3), `politique-maj.md` (S4),
puis `docs/adr/0001-correctif-conception.md` (S5).

## Pourquoi un `package-lock.json` fourni

Juice Shop **ne versionne pas** son lock file : `.npmrc` porte `package-lock=false` et
`.gitignore` (ligne 10) exclut `package-lock.json`. Sans lock committe :
- le job `build` de `ci.yml` echoue (`npm ci` exige un lock file),
- `npm audit` du TP de S4 sort en `ENOLOCK`.

Le lock fourni a ete genere le 2026-09-08 avec Node 22 / npm 10.9.2 :

```
npm install --package-lock-only --ignore-scripts --no-audit --no-fund
```

Verification (memes chiffres que le TP de S4) :
`npm audit` -> 48 paquets affectes (7 critical, 21 high, 17 moderate, 3 low), exit 1 sur
`--audit-level=high`. Chaine transitive de reference du TP4 : `pdfkit` (directe) tire
`crypto-js 3.3.0` (GHSA-xwcq-pm8m-c4vf), correctif = montee majeure de `pdfkit`.

Ces chiffres derivent avec le temps. Regenerer le lock juste avant le module et revalider les
nombres cites dans le deck et le TP de S4 si l'ecart est important.

## Montage du fork de reference (formateur, avant le module)

```bash
# 1. Partir du tag officiel
git clone --depth 1 --branch v20.1.1 https://github.com/juice-shop/juice-shop.git juice-shop
cd juice-shop
rm -rf .git && git init -b main            # on repart d'un historique propre

# 2. Deposer ce kit par-dessus (depuis le dossier decompresse)
cp -r /chemin/vers/juice-shop-fork-reference/. .

# 3. Permettre le suivi du lock file
sed -i '/^package-lock\.json$/d' .gitignore   # retire la ligne 10

# 4. Premier commit
git add -A
git commit -m "Juice Shop v20.1.1 + amorce CI du module securite applicative"

# 5. Pousser en depot PUBLIC sur le compte GitHub du module, puis
#    Settings > Actions > General : autoriser les workflows.
#    Les etudiants forkent ce depot (bouton Fork) et activent Actions sur leur fork
#    ("I understand my workflows, go ahead and enable them").
```

Verifier apres le premier push que les deux jobs `build` et `lint` passent au vert
(compter 8 a 12 min : le `postinstall` de Juice Shop compile le frontend Angular).

## A ne PAS ajouter ici

Les workflows `sast.yml` (S3), `supply-chain.yml` (S4) et `dast.yml` (S5) sont **construits
par les etudiants en seance**. Les gabarits d'amorce correspondants sont dans
`supports/gabarits/workflows/` du depot module, pas ici.

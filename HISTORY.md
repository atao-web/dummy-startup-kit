```bash

git --version
# git version 2.17.1

jq --version
# jq-1.5-1-a5b5cbe

### pour sponge :
dpkg -l moreutils
# Souhait=inconnU/Installé/suppRimé/Purgé/H=à garder
# | État=Non/Installé/fichier-Config/dépaqUeté/échec-conFig/H=semi-installé/W=attend-traitement-déclenchements
# |/ Err?=(aucune)/besoin Réinstallation (État,Err: majuscule=mauvais)
# ||/ Nom                                            Version                      Architecture                 Description
# +++-==============================================-============================-============================-==================================================================================================
# ii  moreutils                                      0.60-1                       amd64                        additional Unix utilities

node --version
# v12.3.1

npm list -g --depth 0 2>&1
# /usr/lib
# ├── @angular/cli@8.2.0
# ├── @nrwl/cli@8.4.13
# ├── @nrwl/schematics@8.4.13
# ├── js-beautify@1.10.2
# ├── json-parse-cli@0.3.0
# ├── npm@6.9.0
# ├── npx@10.2.0
# ├── rxjs@6.5.2
# ├── rxjs-tslint@0.1.7
# ├── UNMET PEER DEPENDENCY tslint@^5.0.0
# └── UNMET PEER DEPENDENCY typescript@>=2.1.0 || >=2.1.0-dev || >=2.2.0-dev || >=2.3.0-dev || >=2.4.0-dev || >=2.5.0-dev || >=2.6.0-dev || >=2.7.0-dev || >=2.8.0-dev || >=2.9.0-dev
#
# npm ERR! peer dep missing: tslint@^5.0.0, required by rxjs-tslint@0.1.7
# npm ERR! peer dep missing: typescript@>=2.1.0 || >=2.1.0-dev || >=2.2.0-dev || >=2.3.0-dev || >=2.4.0-dev || >=2.5.0-dev || >=2.6.0-dev || >=2.7.0-dev || >=2.8.0-dev || >=2.9.0-dev, required by rxjs-tslint@0.1.7
# npm ERR! peer dep missing: rxjs@^6.4.0, required by jasmine-marbles@0.6.0
# npm ERR! peer dep missing: webpack@^4.18.1, required by @cypress/webpack-preprocessor@4.1.0
# npm ERR! peer dep missing: typescript@*, required by ts-loader@5.3.1
# npm ERR! peer dep missing: webpack@>=2, required by babel-loader@8.0.6

mkdir -pv atao-dummy-startup-kit && cd atao-dummy-startup-kit
mkdir: création du répertoire 'atao-dummy-startup-kit'

git init
Dépôt Git vide initialisé dans /home/pierre/DevSpace/aktea-explo/atao-dummy-startup-kit/.git/

touch HISTORY.md

echo "# A Dummy Startup Kit" >> README.md

git add . && git commit -m "Initial commit"
[master (commit racine) 8a6ab08] Initial commit
 2 files changed, 1 insertion(+)
 create mode 100644 HISTORY.md
 create mode 100644 README.md

code .

git remote add origin https://github.com/atao-web/dummy-startup-kit.git

git push -u origin master
# Username for 'https://github.com': atao-web
# Password for 'https://atao-web@github.com': xxxxxxxxx
# Décompte des objets: 4, fait.
# Delta compression using up to 4 threads.
# Compression des objets: 100% (2/2), fait.
# Écriture des objets: 100% (4/4), 280 bytes | 280.00 KiB/s, fait.
# Total 4 (delta 0), reused 0 (delta 0)
# To https://github.com/atao-web/dummy-startup-kit.git
#  * [new branch]      master -> master
# La branche 'master' est paramétrée pour suivre la branche distante 'master' depuis 'origin'.


cat > README.md <<___EOF
# A Dummy Startup Kit

See [Step by step: Building and publishing an NPM Typescript package](https://itnext.io/step-by-step-building-and-publishing-an-npm-typescript-package-44fe7164964c), Carl-Johan Kihl, May 30, 2018

___EOF

npm init -y
# Wrote to /home/pierre/DevSpace/aktea-explo/atao-dummy-startup-kit/package.json:
#
# {
#   "name": "atao-dummy-startup-kit",
#   "version": "1.0.0",
#   "description": "See [Step by step: Building and publishing an NPM Typescript package](https://itnext.io/step-by-step-building-and-publishing-an-npm-typescript-package-44fe7164964c), Carl-Johan Kihl, May 30, 2018",
#   "main": "index.js",
#   "scripts": {
#     "test": "echo \"Error: no test specified\" && exit 1"
#   },
#   "repository": {
#     "type": "git",
#     "url": "git+https://github.com/atao-web/dummy-startup-kit.git"
#   },
#   "keywords": [],
#   "author": "",
#   "license": "ISC",
#   "bugs": {
#     "url": "https://github.com/atao-web/dummy-startup-kit/issues"
#   },
#   "homepage": "https://github.com/atao-web/dummy-startup-kit#readme"
# }

jq '.name="dummy-startup-kit"' package.json | sponge package.json

echo "node_modules" >> .gitignore

npm install --save-dev typescript
# npm notice created a lockfile as package-lock.json. You should commit this file.
# + typescript@3.7.3
# added 1 package from 1 contributor and audited 1 package in 9.951s
# found 0 vulnerabilities

cat > tsconfig.json << ___EOF
{
  "compilerOptions": {
    "target": "es5",
    "module": "commonjs",
    "declaration": true,
    "outDir": "./lib",
    "strict": true
  },
  "include": ["src"],
  "exclude": ["node_modules", "**/__tests__/*"]
}
___EOF

mkdir src

cat > src/index.ts << ___EOF
export const Greeter = (name: string) => \`Hello \${name}\`; 
___EOF

jq '.scripts["build"]="tsc"' package.json | sponge package.json

npm run build
#
# > atao-dummy-startup-kit@1.0.0 build /home/pierre/DevSpace/aktea-explo/atao-dummy-startup-kit
# > tsc

ls -al lib
# total 16
# drwxr-xr-x 2 pierre pierre 4096 déc.  19 17:56 .
# drwxr-xr-x 6 pierre pierre 4096 déc.  19 17:56 ..
# -rw-r--r-- 1 pierre pierre   56 déc.  19 17:56 index.d.ts
# -rw-r--r-- 1 pierre pierre  140 déc.  19 17:56 index.js

cat >> .gitignore << ___EOF
/lib
___EOF

npm install --save-dev prettier tslint tslint-config-prettier
# + tslint-config-prettier@1.18.0
# + prettier@1.19.1
# + tslint@5.20.1
# added 39 packages from 23 contributors and audited 55 packages in 3.293s
# found 0 vulnerabilities

cat > tslint.json <<___EOF
{
   "extends": ["tslint:recommended", "tslint-config-prettier"]
}
___EOF

cat > .prettierrc << ___EOF
{
  "printWidth": 120,
  "trailingComma": "all",
  "singleQuote": true
}
___EOF


jq '.scripts["format"]="prettier --write \"src/**/*.ts\" \"src/**/*.js\""' package.json | sponge package.json \
  && jq '.scripts["lint"]="tslint -p tsconfig.json"' package.json | sponge package.json

killall vsc

code .

npm run lint
# 
# > atao-dummy-startup-kit@1.0.0 lint /home/pierre/DevSpace/aktea-explo/atao-dummy-startup-kit
# > tslint -p tsconfig.json

npm run format
#
# > atao-dummy-startup-kit@1.0.0 format /home/pierre/DevSpace/aktea-explo/atao-dummy-startup-kit
# > prettier --write "src/**/*.ts" "src/**/*.js"
#
# src/index.ts 209ms
 
jq '.files=["lib/**/*"]' package.json | sponge package.json  # whitelist for npm packaging

npm install --save-dev jest ts-jest @types/jest
# npm WARN deprecated left-pad@1.3.0: use String.prototype.padStart()
# npm WARN optional SKIPPING OPTIONAL DEPENDENCY: fsevents@1.2.11 (node_modules/fsevents):
# npm WARN notsup SKIPPING OPTIONAL DEPENDENCY: Unsupported platform for fsevents@1.2.11: wanted {"os":"darwin","arch":"any"} (current: {"os":"linux","arch":"x64"})
#
# + @types/jest@24.0.24
# + jest@24.9.0
# + ts-jest@24.2.0
# added 442 packages from 381 contributors and audited 877614 packages in 24.351s
# found 0 vulnerabilities

cat > jestconfig.json << ___EOF
{
  "transform": {
    "^.+\\\\.(t|j)sx?\$": "ts-jest"
  },
  "testRegex": "(/__tests__/.*|(\\\\.|/)(test|spec))\\\\.(jsx?|tsx?)\$",
  "moduleFileExtensions": ["ts", "tsx", "js", "jsx", "json", "node"]
}
___EOF

jq '.scripts["test"]="jest --config jestconfig.json"' package.json | sponge package.json

mkdir -pv src/__tests__
# mkdir: création du répertoire 'src/__tests__'

cat > src/__tests__/Greeter.test.ts <<___EOF
import { Greeter } from '../index';

test('My Greeter', () => {
  expect(Greeter('Carl')).toBe('Hello Carl');
});
___EOF

npm test
#
# > dummy-startup-kit@1.0.0 test /home/pierre/DevSpace/aktea-explo/atao-dummy-startup-kit
# > jest --config jestconfig.json

#  PASS  src/__tests__/Greeter.test.ts
#   ✓ My Greeter (4ms)

# Test Suites: 1 passed, 1 total
# Tests:       1 passed, 1 total
# Snapshots:   0 total
# Time:        2.499s
# Ran all test suites.


jq '.scripts["prepare"]="npm run build"' package.json | sponge package.json \
  && jq '.scripts["prepublishOnly"]="npm test && npm run lint"' package.json | sponge package.json \
  && jq '.scripts["preversion"]="npm run lint"' package.json | sponge package.json \
  && jq '.scripts["version"]="npm run format && git add -A src"' package.json | sponge package.json \
  && jq '.scripts["postversion"]="git push && git push --tags"' package.json | sponge package.json

jq '.description="A nice greeter. See [Step by step: Building and publishing an NPM Typescript package](https://itnext.io/step-by-step-building-and-publishing-an-npm-typescript-package-44fe7164964c), Carl-Johan Kihl, May 30, 2018"' package.json | sponge package.json

jq '.main="lib/index.js"' package.json | sponge package.json \
  && jq '.types="lib/index.d.ts"' package.json | sponge package.json \
  && jq '.keywords=["Hello", "Greeter"]' package.json | sponge package.json \
  && jq '.author="C-J & PR"' package.json | sponge package.json

git add -A && git commit -m "Setup Package"
git push


```


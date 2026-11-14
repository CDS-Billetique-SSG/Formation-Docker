---
theme: default
title: Introduction à Docker
info: |
  Introduction aux conteneurs : différences avec les VM, pourquoi les utiliser,
  et des cas pratiques simples pour le quotidien.
colorSchema: dark
transition: slide-left
mdc: true
fonts:
  sans: Inter
  mono: Fira Code
layout: center
class: text-center
---

<div class="text-8xl mb-4"><logos-docker-icon /></div>

# Introduction à <span class="text-docker">Docker</span>

Comprendre les conteneurs et s'en servir au quotidien

<div class="mt-10 flex justify-center gap-3">
  <span class="tag">Conteneurs</span>
  <span class="tag">VM vs Docker</span>
  <span class="tag">Cas pratiques</span>
</div>

<!--
Objectif : à la fin, chacun doit savoir ce qu'est un conteneur, en quoi c'est différent d'une VM, et être capable de lancer une base de données ou un outil sans rien installer.
-->

---
layout: default
---

# Au programme

<div class="grid grid-cols-2 gap-5 mt-10">
  <FeatureCard v-click icon="🤕" title="1. Le problème">
    « Ça marche sur ma machine » et l'enfer des dépendances
  </FeatureCard>
  <FeatureCard v-click icon="📦" title="2. C'est quoi un conteneur ?" color="#a78bfa">
    L'analogie du conteneur maritime, VM vs conteneurs
  </FeatureCard>
  <FeatureCard v-click icon="🧱" title="3. Le vocabulaire" color="#f59e0b">
    Dockerfile, image, conteneur, registry, ports, volumes
  </FeatureCard>
  <FeatureCard v-click icon="🛠️" title="4. Cas pratiques" color="#10b981">
    BDD sans installation, outils jetables, plusieurs versions, Compose
  </FeatureCard>
</div>

---
clicks: 3
---

# « Ça marche sur ma machine… » 🤷

Le même code, trois environnements différents

<div class="mt-8">
  <WorksOnMyMachine :step="$clicks" />
</div>

<div v-click="3" class="mt-8 text-center text-lg">
  Le problème n'est pas le code : c'est <span class="text-docker font-bold">l'environnement</span> autour.
</div>

---

# L'enfer des dépendances 🔥

Installer un projet sur un nouveau poste, en vrai :

<div class="grid grid-cols-[1fr_1fr] gap-8 mt-6 items-center">
<div>

<v-clicks>

- 📥 Installer **Node 20**… mais un autre projet veut **Node 16**
- 🐘 Installer **Postgres**, créer un user, configurer le service
- 🔴 Installer **Redis** juste pour le cache en local
- 🐍 Un script a besoin de **Python 3.11** (le système a 3.9)
- 🧹 Six mois plus tard : tout ça traîne encore sur ta machine

</v-clicks>

</div>
<div v-click class="panel p-6 text-center">
  <div class="text-6xl mb-3">🍝</div>
  <div class="font-bold text-lg">Des heures d'onboarding</div>
  <div class="text-sm opacity-70 mt-2">Des versions qui se marchent dessus,<br>une machine polluée, une doc d'installation jamais à jour.</div>
</div>
</div>

---
clicks: 5
---

# Un conteneur, c'est quoi ? 📦

Une boîte standard qui embarque l'application **et tout ce dont elle a besoin**

<div class="mt-8">
  <ShippingContainer :step="$clicks" />
</div>

<!--
Analogie : avant les conteneurs maritimes, chaque marchandise était chargée différemment. Le conteneur standardise l'emballage : peu importe ce qu'il y a dedans, la grue, le bateau et le camion savent le manipuler.
-->

---
layout: center
class: text-center
---

# Concrètement 🔍

<div class="grid grid-cols-3 gap-6 mt-10 text-left">
  <FeatureCard icon="🏃" title="Un processus isolé">
    Un conteneur est un <b>processus</b> normal de ta machine… mais qui croit être seul au monde.
  </FeatureCard>
  <FeatureCard icon="🧱" title="Ses propres murs" color="#a78bfa">
    Son système de fichiers, son réseau, ses processus. Il ne voit pas le reste.
  </FeatureCard>
  <FeatureCard icon="🤝" title="Un noyau partagé" color="#10b981">
    Il utilise le <b>noyau</b> de l'hôte : pas de système d'exploitation complet à démarrer.
  </FeatureCard>
</div>

<div v-click class="mt-10 opacity-80">
  👉 C'est cette dernière différence qui le sépare d'une machine virtuelle.
</div>

---
clicks: 3
---

# Machine virtuelle vs Conteneur

<div class="grid grid-cols-2 gap-10 mt-4">
  <StackDiagram mode="vm" :step="$clicks" />
  <StackDiagram mode="container" :step="$clicks" />
</div>

<div v-click="3" class="mt-4 grid grid-cols-2 gap-10 text-sm text-center">
  <div class="text-vm">Chaque VM embarque <b>un OS complet</b> : lourd, lent à démarrer</div>
  <div class="text-docker">Les conteneurs <b>partagent le noyau</b> : légers, quasi instantanés</div>
</div>

<!--
Clic 1 : OS hôte. Clic 2 : la couche de virtualisation (hyperviseur vs Docker Engine). Clic 3 : les applis. Insister sur le bloc orange "OS invité" qui est dupliqué dans chaque VM.
-->

---
clicks: 4
---

# En chiffres 📊

Ordres de grandeur typiques

<div class="mt-8">
<MetricBars :step="$clicks" :metrics="[
  { name: '⏱️ Démarrage', vm: 75, ctr: 2, vmLabel: '30 s – plusieurs minutes', ctrLabel: '< 1 seconde' },
  { name: '💾 Taille', vm: 75, ctr: 8, vmLabel: '5 – 20 Go', ctrLabel: '5 – 500 Mo' },
  { name: '🧠 RAM au repos', vm: 60, ctr: 5, vmLabel: '1 – 2 Go par VM', ctrLabel: 'quelques Mo' },
  { name: '📦 Densité sur un laptop', vm: 10, ctr: 75, vmLabel: '2 – 3 VM', ctrLabel: 'des dizaines de conteneurs' },
]" />
</div>

---

# Alors, les VM c'est fini ? 🤔

Non : ce sont deux outils différents

<div class="grid grid-cols-2 gap-8 mt-8">
<div class="panel p-5" style="border-color: var(--vm)">

### <span class="text-vm">🖥️ Plutôt une VM quand…</span>

- Il faut un **autre OS** (Windows sur un Mac)
- On veut une **isolation forte** (sécurité, multi-clients)
- On teste un **noyau** ou un OS complet

</div>
<div class="panel p-5" style="border-color: var(--docker-light)">

### <span class="text-docker">🐳 Plutôt un conteneur quand…</span>

- On veut lancer une **application** et ses dépendances
- On veut un environnement **reproductible**
- On veut démarrer / jeter **vite et souvent**

</div>
</div>

<div v-click class="mt-6 panel p-4 text-sm">
  💡 Sur Mac et Windows, <b>Docker Desktop fait tourner une petite VM Linux</b> en coulisse : les conteneurs ont besoin d'un noyau Linux. Les deux technologies cohabitent très bien !
</div>

---
clicks: 3
---

# Le vocabulaire Docker 🧱

<div class="mt-6">
  <DockerFlow :step="$clicks" />
</div>

<!--
Analogie de la cuisine : le Dockerfile est la recette, l'image est le plat surgelé prêt à l'emploi, le conteneur est le plat réchauffé et servi. On peut en servir autant qu'on veut à partir du même surgelé.
-->

---
clicks: 7
---

# Une image = des couches 🥞

Chaque instruction du Dockerfile ajoute une couche

<div class="mt-6">
  <ImageLayers :step="$clicks" />
</div>

<div v-click="7" class="mt-4 text-sm text-center opacity-80">
  Les couches sont <b>mises en cache</b> et <b>partagées</b> entre images : on ne retélécharge / reconstruit que ce qui a changé ⚡
</div>

---
clicks: 2
---

# Ports & volumes 🔌📁

Un conteneur est isolé… il faut lui ouvrir des portes

<div class="mt-6">
  <PortVolume :step="$clicks" />
</div>

<div v-click="2" class="mt-4 text-sm text-center opacity-80">
  ⚠️ Sans volume, tout ce qui est écrit dans le conteneur <b>disparaît</b> quand on le supprime.
</div>

---
layout: center
class: text-center
---

# Pourquoi utiliser des conteneurs ? 🚀

<div class="grid grid-cols-3 gap-5 mt-10 text-left">
  <FeatureCard v-click icon="🔁" title="Reproductible">
    Le même environnement partout : dev, collègue, CI, prod.
  </FeatureCard>
  <FeatureCard v-click icon="🧼" title="Machine propre" color="#10b981">
    Rien n'est installé sur l'hôte. On supprime le conteneur, il ne reste rien.
  </FeatureCard>
  <FeatureCard v-click icon="⚡" title="Rapide" color="#f59e0b">
    Démarre en une seconde. Parfait pour tester et jeter.
  </FeatureCard>
  <FeatureCard v-click icon="🧩" title="Isolé" color="#a78bfa">
    Plusieurs versions d'un même outil côte à côte, sans conflit.
  </FeatureCard>
  <FeatureCard v-click icon="🤝" title="Partageable" color="#22d3ee">
    Un fichier versionné avec le code remplace des pages de doc d'installation.
  </FeatureCard>
  <FeatureCard v-click icon="🌍" title="Écosystème" color="#ef4444">
    Des milliers d'images officielles prêtes à l'emploi sur Docker Hub.
  </FeatureCard>
</div>

---
layout: center
class: text-center
---

<div class="text-7xl mb-6">🛠️</div>

# Cas pratiques

Des usages simples, utiles dès demain

---
clicks: 3
---

# Cas n°1 — Une base de données sans l'installer 🐘

<span class="tag">Besoin d'un Postgres pour le dev local</span>

<div class="mt-6">
<BeforeAfter :step="$clicks" :before="[
  'Télécharger l\'installeur ou <code>brew install postgresql@16</code>',
  'Démarrer et configurer le service système',
  'Créer un utilisateur, un mot de passe, une base',
  'Gérer les conflits avec la version d\'un autre projet',
  'Penser à tout désinstaller proprement un jour…',
]" :after="[
  'Une commande. C\'est tout.',
]">
<div class="mt-4">
<Terminal :step="$clicks - 1" :lines="[
  { cmd: 'docker run -d --name db -e POSTGRES_PASSWORD=dev -p 5432:5432 postgres:16', out: 'a3f9c2e1b7d4…' },
  { comment: 'Fini ? on supprime, il ne reste rien', cmd: 'docker rm -f db' },
]" />
</div>
</BeforeAfter>
</div>

---

# Cas n°1 — Décortiquons la commande 🔬

<div class="mt-10 text-base font-mono flex flex-wrap justify-center gap-2">
  <span v-click="1" class="whitespace-nowrap px-3 py-2 rounded bg-slate-700">docker run</span>
  <span v-click="2" class="whitespace-nowrap px-3 py-2 rounded" style="background:#334155">-d</span>
  <span v-click="3" class="whitespace-nowrap px-3 py-2 rounded" style="background:#5b21b6">--name db</span>
  <span v-click="4" class="whitespace-nowrap px-3 py-2 rounded" style="background:#b45309">-e POSTGRES_PASSWORD=dev</span>
  <span v-click="5" class="whitespace-nowrap px-3 py-2 rounded" style="background:#0e7490">-p 5432:5432</span>
  <span v-click="6" class="whitespace-nowrap px-3 py-2 rounded" style="background:#1d63ed">postgres:16</span>
</div>

<div class="grid grid-cols-3 gap-4 mt-12 text-sm">
  <div v-click="1" class="panel p-3"><b>docker run</b> — crée et démarre un conteneur</div>
  <div v-click="2" class="panel p-3"><b>-d</b> — en arrière-plan (detached)</div>
  <div v-click="3" class="panel p-3"><b>--name</b> — un nom lisible au lieu d'un ID</div>
  <div v-click="4" class="panel p-3"><b>-e</b> — une variable d'environnement (config)</div>
  <div v-click="5" class="panel p-3"><b>-p hôte:conteneur</b> — expose un port</div>
  <div v-click="6" class="panel p-3"><b>image:tag</b> — quelle image, quelle version</div>
</div>

---

# Cas n°2 — Des outils « jetables » 🧹

Utiliser un outil **une fois** sans l'installer : `--rm` supprime le conteneur à la fin

<div class="grid grid-cols-2 gap-6 mt-6">
<div class="flex flex-col gap-3">
  <FeatureCard compact icon="🎞️" title="Convertir une vidéo">
    Pas besoin d'installer ffmpeg et ses 50 dépendances.
  </FeatureCard>
  <FeatureCard compact icon="🐍" title="Lancer un script Python" color="#f59e0b">
    Sans toucher au Python du système.
  </FeatureCard>
  <FeatureCard compact icon="📄" title="Servir un dossier en HTTP" color="#10b981">
    Un serveur web en une ligne pour partager une démo.
  </FeatureCard>
</div>

<Terminal title="outils sans installation" :lines="[
  { comment: 'ffmpeg sans l\'installer', cmd: 'docker run --rm -v $PWD:/w -w /w jrottenberg/ffmpeg -i video.mov video.mp4' },
  { comment: 'un script Python 3.12', cmd: 'docker run --rm -v $PWD:/app -w /app python:3.12 python script.py' },
  { comment: 'servir le dossier courant sur :8080', cmd: 'docker run --rm -p 8080:80 -v $PWD:/usr/share/nginx/html nginx' },
]" />
</div>

<div v-click class="mt-5 text-sm text-center opacity-80">
  🔑 Le combo magique : <code>--rm</code> (on jette après) + <code>-v $PWD:/…</code> (on travaille sur mes fichiers)
</div>

---
clicks: 3
---

# Cas n°3 — Plusieurs versions en parallèle 🔀

<span class="tag">Tester son app sur Node 18, 20 et 22</span>

<div class="grid grid-cols-[1.1fr_1fr] gap-6 mt-6 items-start">
  <VersionMatrix :step="$clicks" />
  <Terminal :step="$clicks" :lines="[
    { cmd: 'docker run --rm node:18 node -v', out: 'v18.20.4' },
    { cmd: 'docker run --rm node:20 node -v', out: 'v20.18.0' },
    { cmd: 'docker run --rm node:22 node -v', out: 'v22.11.0' },
  ]" />
</div>

<div v-click="3" class="mt-6 text-center text-sm opacity-80">
  Fini <code>nvm</code>, <code>pyenv</code>, <code>sdkman</code>… pour les tests ponctuels : on change juste le <b>tag</b> de l'image.
</div>

---

# Cas n°4 — Docker Compose 🎼

Décrire plusieurs conteneurs dans **un seul fichier**, versionné avec le projet

<div class="grid grid-cols-[1.1fr_1fr] gap-8 mt-4">
<div>

```yaml {all|2-6|7-10|11-14|all}
services:
  db:
    image: postgres:16
    environment:
      POSTGRES_PASSWORD: dev
    volumes: [db-data:/var/lib/postgresql/data]
  cache:
    image: redis:7
    ports: ["6379:6379"]

  adminer:            # interface web pour la BDD
    image: adminer
    ports: ["8080:8080"]

volumes:
  db-data:
```

</div>
<div class="flex flex-col gap-4">

<Terminal title="compose" :lines="[
  { comment: 'tout démarrer', cmd: 'docker compose up -d', out: '✔ db       Started\n✔ cache    Started\n✔ adminer  Started' },
  { comment: 'tout arrêter et nettoyer', cmd: 'docker compose down' },
]" />

<div v-click class="panel p-4 text-sm">
  🎉 Un nouvel arrivant fait <code>git clone</code> puis <code>docker compose up</code> : son environnement est prêt.
</div>

</div>
</div>

---

# Antisèche 📝

<div class="grid grid-cols-3 gap-4 mt-6 text-sm">
<div class="panel p-4 cheat"><h3>▶️ Lancer</h3><ul><li><code>docker run IMAGE</code></li><li><code>docker run -d -p 80:80 nginx</code></li><li><code>docker run --rm -it ubuntu bash</code></li><li><code>docker compose up -d</code></li></ul></div>
<div class="panel p-4 cheat"><h3>🔍 Observer</h3><ul><li><code>docker ps</code> <span>— actifs</span></li><li><code>docker ps -a</code> <span>— tous</span></li><li><code>docker logs -f NOM</code></li><li><code>docker exec -it NOM sh</code></li></ul></div>
<div class="panel p-4 cheat"><h3>🧹 Nettoyer</h3><ul><li><code>docker stop NOM</code></li><li><code>docker rm NOM</code></li><li><code>docker rmi IMAGE</code></li><li><code>docker system prune</code></li></ul></div>
</div>

<div class="grid grid-cols-4 gap-3 mt-6 text-xs text-center">
  <div class="panel p-2"><code>-d</code><br>arrière-plan</div>
  <div class="panel p-2"><code>-p h:c</code><br>port</div>
  <div class="panel p-2"><code>-v h:c</code><br>volume</div>
  <div class="panel p-2"><code>-e K=V</code><br>variable</div>
  <div class="panel p-2"><code>--rm</code><br>auto-suppression</div>
  <div class="panel p-2"><code>-it</code><br>terminal interactif</div>
  <div class="panel p-2"><code>--name</code><br>nommer</div>
  <div class="panel p-2"><code>-w</code><br>dossier de travail</div>
</div>

---

# Les pièges classiques ⚠️

<div class="grid grid-cols-2 gap-5 mt-8">
  <FeatureCard v-click icon="💾" title="Oublier les volumes" color="#ef4444">
    Les données écrites dans le conteneur disparaissent avec lui. Utiliser <code>-v</code> pour ce qui doit survivre.
  </FeatureCard>
  <FeatureCard v-click icon="🏷️" title="Le tag latest" color="#f59e0b">
    <code>postgres:latest</code> change sans prévenir. Fixer une version : <code>postgres:16</code>.
  </FeatureCard>
  <FeatureCard v-click icon="🧱" title="Ce n'est pas une VM" color="#a78bfa">
    Un conteneur = un processus principal. On ne s'y connecte pas en SSH, on utilise <code>docker exec</code>.
  </FeatureCard>
  <FeatureCard v-click icon="🗑️" title="Le disque qui se remplit" color="#22d3ee">
    Images et conteneurs s'accumulent. <code>docker system prune</code> de temps en temps.
  </FeatureCard>
</div>

---
layout: center
class: text-center
---

# À retenir 🎯

<div class="grid grid-cols-3 gap-6 mt-10 text-left">
  <FeatureCard v-click icon="📦" title="Conteneur">
    Une application + ses dépendances, isolée, qui partage le noyau de l'hôte.
  </FeatureCard>
  <FeatureCard v-click icon="⚖️" title="≠ VM" color="#f59e0b">
    Pas d'OS invité : plus léger, plus rapide, isolation moins forte.
  </FeatureCard>
  <FeatureCard v-click icon="🧼" title="Au quotidien" color="#10b981">
    Lancer BDD et outils sans rien installer, et partager un environnement identique.
  </FeatureCard>
</div>

<div v-click class="mt-12 text-2xl">
  À vous de jouer : <code>docker run hello-world</code> 👋
</div>

---
layout: center
class: text-center
---

<div class="text-8xl mb-6"><logos-docker-icon /></div>

# Questions ?

<div class="mt-8 text-sm opacity-60">
  docs.docker.com · hub.docker.com · labs.play-with-docker.com
</div>


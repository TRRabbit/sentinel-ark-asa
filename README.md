# Sentinel - ARK ASA

Sentinel is a Discord bot for ARK: Survival Ascended servers. It runs on a
machine you keep online and connects your ARK server(s) to your Discord: raid
alerts, tribe and player leaderboards, a live map, tickets, server status and
more.

This repository is the official download point. It contains no source code, only
ready-to-use compiled programs published under [Releases](../../releases) and this
documentation.

> **Public beta:** every module is currently free — no license key needed —
> until 3 August 2026. When the beta ends, premium modules require a license key
> and you install the new version.

Version francaise plus bas.

---

## English

### What you download

Go to [Releases](../../releases), download the file
`Sentinel-ARK-ASA-v<version>.zip` and extract it. You get a single folder with
everything you need:

- `config.exe` - a local setup tool. It opens a page in your browser where you
  enter everything (Discord token, database, servers, modules) without touching
  any file by hand.
- `bot.exe` - the bot itself.
- `configExemple.jsonc` - a documented example of the most common settings
  (config.exe shows every setting, with help text).
- `maps/` - the calibrated map backgrounds used by the live map (you can add or
  replace images in this folder later).

Free core, paid modules: the foundation (configuration panel, health, variables,
account linking, cluster tribes) and a few modules (server status, RCON console,
timers) are free. The premium modules (raid alerts, leaderboards, live map,
tickets, anti-cheat, dino arsenal, analytics, battle pass) are unlocked with a
license key, one module at a time.

### The modules

Turn each module on or off in `config.exe`. A module marked "needs a mod" only
uses it for its live data — see "Server mods" below for the links.

Free:

- **Configuration panel** (`/config`): edit every bot setting from Discord.
- **Health**: heartbeat, "new version available" notice, and operator alerts.
- **Variables**: custom text shortcuts reused across your messages and panels.
- **Account linking**: links a Discord member to their in-game player. Works
  better with Awesome Admin Tools (it then sees offline players too).
- **Cluster tribes**: keeps each tribe's identity consistent across a
  multi-server cluster.
- **Server status**: a live status panel, and an optional voice-channel player
  counter.
- **RCON console**: run the admin commands you approved on a server, from Discord.
- **Timers**: scheduled, recurring messages and announcements.

Premium (license key):

- **Raid alerts**: detects raids from the game log and reports them, with a raid
  score. (Needs the server launch flags below.)
- **Leaderboards**: player and tribe rankings (kills, tames, play-time, and
  more), posted and refreshed automatically.
- **Live map**: a private web map of your server. **Needs two mods**: Awesome
  Admin Tools (to show bases) and Livemap / HTTPLocation (for live player
  positions). The map background and dinos come from the save file, no mod
  needed.
- **Tickets**: a support-ticket system with panels and transcripts (optional AI
  help when you provide your own key).
- **Anti-cheat**: detectors and alerts for suspicious activity on your server.
- **Dino arsenal**: an overview of notable creatures across the cluster (read
  from the save file).
- **Analytics**: activity and population statistics for your server.
- **Battle Pass**: seasonal progression with in-game and Discord rewards.

### Requirements

- A Windows machine kept online (the bot watches your server continuously).
- A free Discord application: <https://discord.com/developers/applications> ->
  New Application -> Bot -> copy the token.
- A MySQL or MariaDB database. Nitrado provides one; otherwise install MariaDB or
  use any MySQL host.
- For the premium map features: FTP access to your server save file, and RCON for
  the rest. Nitrado provides both.
- A license key for each paid module you use (from your purchase on the shop).

### Server launch flags (the #1 setting — for combat scores and raids)

The bot reads kills, tames, deaths, structure destructions and raids from your
ARK server's log file. ARK only writes that log in the format the bot needs when
the server is launched with these three options:

```
-servergamelog -servergamelogincludetribelogs -ServerRCONOutputTribeLogs
```

Add them to your server's launch command line (next to `-mods=`,
`-WinLiveMaxPlayers`, etc.), then restart the server. On Nitrado, paste them in
the "Additional command-line arguments" field. This is set on the SERVER side,
not in config.exe.

- With all three (recommended): the full combat ranking — kills, tames, deaths,
  **structures and raids** — with reliable tribe identity.
- With only `-servergamelog`: the bot still counts kills, tames and deaths (by
  tribe name), but **structure destructions and raids are not recorded**, and a
  renamed tribe shows up as a new entry.
- With none: the combat ranking stays empty. The dino arsenal, bases, tribes and
  live map still work — those come from the save file, not the log.

So if "the kills don't show up", check these flags first.

### Installation

1. Download the latest release and extract the folder wherever you like (keep
   `bot.exe` and `config.exe` together).
2. Run `config.exe`. It opens on a Quick Start page: paste your Discord token in
   its step 1 and save — the tool stores it for you (no file to create by hand).
3. Still in `config.exe`, fill in the other pages:
   - General: the foundation settings (always on, free — no license here).
   - Database: your MySQL/MariaDB host, user, password and database name.
   - Servers: your ARK server's RCON access (and FTP for the premium features).
   - Each module page: turn the module on or off. For a paid module, paste its
     license key at the top of its page and click Activate.

   Click Save on each page. This writes your settings into a local `config`
   folder next to the tool.
4. Invite the bot to your Discord (Developer Portal -> OAuth2 -> URL Generator ->
   scopes `bot` and `applications.commands`; permissions: Manage Channels, Manage
   Roles, Send Messages, Embed Links, Read Message History).
5. Run `bot.exe`. It reads the `config`, `logs` and `cache` folders next to
   itself. The window shows "Connected as ..." once it is online.
6. In Discord, type `/` to see the commands (`/config`, `/link-panel`,
   `/leaderboard`, and more).

### Configuration is done in config.exe only

All configuration, license entry and module activation happen in `config.exe`.
Discord is used only to run and use the bot (showing panels, actions). There is
no configuration command on Discord.

### Tips: Discord lists and leaderboards

- A Discord dropdown shows at most 25 choices (e.g. a tribe or a dino species).
  Start typing the first letters of the name to filter the list and reach an
  entry further down — the search always finds it.
- `/dino-top` with no species lists every species at once; pick a species only
  to see its per-stat record detail.
- In the dino tables, the "Dino" column shows the dino's custom name, or its
  level (e.g. "Lvl 238") when it was never renamed — never a bare "?".
- The leaderboard has TWO boards: choose `subject: Tribes` or `subject: Players`
  on `/leaderboard`. They are configured on two SEPARATE screens in `config.exe`
  ("Tribes leaderboard" and "Players leaderboard"), each with its own metrics
  and weights. `/player` shows one player's own stats.

### RCON password on self-hosted ASA servers

Put the admin/RCON password in `GameUserSettings.ini`, under `[ServerSettings]`
(`ServerAdminPassword=...`). Do not pass `?ServerAdminPassword=` on the server's
command line: the ASA launcher swallows everything after it and RCON
authentication fails.

### Keeping the license key off disk (optional)

Instead of entering the key in `config.exe`, you can add it to the `.env` file:
`SENTINEL_LICENSE_KEY=your-key`. The environment variable wins when both are set.

### Server mods for live data (optional)

Some premium features read extra data from your server through two free mods:

- **Awesome Admin Tools** — CurseForge ID `941450`
  (<https://www.curseforge.com/ark-survival-ascended/mods/awesome-admin-tools>).
  Feeds the **Live map** bases and the player identity used by account linking,
  leaderboards, dino arsenal, battle pass and status (it sees offline players and
  exact levels). Add `941450` to your server's `-mods=` argument and restart. The
  bot reads its cache over the same FTP access, read-only.
- **Livemap / HTTPLocation** — CurseForge ID `934290`
  (<https://www.curseforge.com/ark-survival-ascended/mods/livemap>). Sends
  real-time player positions for the **Live map**. Add `934290` to `-mods=`, then
  in `GameUserSettings.ini` add an `[HTTPLocation]` block with `privateid`
  matching your bot's livemap secret and
  `url` pointing at the bot. Restart the server.

### Importing an already-running server

When you install Sentinel on a server that is already running and populated, it
imports the current state on its own at start-up: tribes, bases, dinos and the
player roster appear within the first cycle. Run `/import` (admins only) to pull
everything immediately.

What cannot be imported, and why (an ARK limit, not the bot's): combat scores,
PvP duels and play-time have no past history. ARK exposes the tribe log only as a
live, self-emptying buffer, so rankings and play-time start counting from the
moment you install.

### Live map: keep it private

The live map shows every base on your server, so by default it only listens on
your own machine (`127.0.0.1`). Pointing it at a public address is ignored unless
you explicitly allow public access, so a typo can never expose your whole map.

### Keep the bot running (restart after a reboot)

`bot.exe` is a normal program: if Windows restarts (updates!) or the window is
closed, the bot stops until someone launches it again. To make it start by
itself, create a Scheduled Task once:

1. Open the Start menu, type "Task Scheduler" and open it.
2. Action -> Create Basic Task -> name it "Sentinel".
3. Trigger: "When the computer starts". Action: "Start a program" -> browse to
   your `bot.exe`. In "Start in", put the folder that contains `bot.exe`
   (important: that is where it finds your `config` folder).
4. Finish, then open the task's Properties and, in Settings, tick "If the task
   fails, restart every 1 minute" for automatic recovery after a crash.

If your database (MariaDB/MySQL) runs on the same machine, the bot now waits
for it by itself at boot — no start-order tweaking needed.

### Updating

To update, download the new release and replace `bot.exe` (and `config.exe`).
Your configuration and data are kept.

### Hosting

Supported: Nitrado and self-hosted Windows. Any host that exposes FTP and RCON
works.

---

## Francais

> **Beta publique :** tous les modules sont actuellement gratuits — aucune cle
> de licence necessaire — jusqu'au 3 aout 2026. A la fin de la beta, les modules
> premium demanderont une cle de licence et vous installerez la nouvelle version.

### Ce que vous telechargez

Allez dans [Releases](../../releases), telechargez le fichier
`Sentinel-ARK-ASA-v<version>.zip` et extrayez-le. Vous obtenez un seul dossier
avec tout le necessaire :

- `config.exe` - un outil de reglage local. Il ouvre une page dans votre
  navigateur ou vous saisissez tout (token Discord, base de donnees, serveurs,
  modules) sans toucher a aucun fichier a la main.
- `bot.exe` - le bot lui-meme.
- `configExemple.jsonc` - un exemple documente des reglages les plus courants
  (config.exe montre TOUS les reglages, avec leur aide).
- `maps/` - les fonds de carte calibres utilises par la carte en direct (vous
  pourrez ajouter ou remplacer des images dans ce dossier plus tard).

Coeur gratuit, modules payants : le socle (panneau de configuration, sante,
variables, liaison des comptes, tribus du cluster) et quelques modules (statut du
serveur, console RCON, minuteurs) sont gratuits. Les modules premium (alertes de
raid, classements, carte en direct, tickets, anti-triche, arsenal de dinos,
analytique, battle pass) se debloquent avec une cle de licence, un module a la
fois.

### Les modules

Activez ou desactivez chaque module dans `config.exe`. Un module marque
« a besoin d'un mod » ne s'en sert que pour ses donnees en direct — voir la
section « Mods serveur » plus bas pour les liens.

Gratuits :

- **Panneau de configuration** (`/config`) : reglez tout le bot depuis Discord.
- **Sante** : battement de coeur, avis « nouvelle version disponible » et alertes
  a l'operateur.
- **Variables** : des raccourcis de texte personnalises reutilisables dans vos
  messages et panneaux.
- **Liaison des comptes** : relie un membre Discord a son joueur en jeu.
  Fonctionne mieux avec Awesome Admin Tools (il voit alors aussi les joueurs
  hors-ligne).
- **Tribus du cluster** : garde l'identite de chaque tribu coherente sur un
  reseau de plusieurs serveurs.
- **Statut du serveur** : un panneau de statut en direct, et un compteur de
  joueurs en salon vocal (optionnel).
- **Console RCON** : lancez depuis Discord les commandes admin que vous avez
  approuvees.
- **Minuteurs** : des messages et annonces recurrents programmes.

Premium (cle de licence) :

- **Alertes de raid** : detecte les raids dans le journal de jeu et les signale,
  avec une note de raid. (Necessite les options de lancement du serveur, plus bas.)
- **Classements** : classements joueurs et tribus (kills, tames, temps de jeu,
  etc.), postes et rafraichis automatiquement.
- **Carte en direct** : une carte web privee de votre serveur. **Necessite deux
  mods** : Awesome Admin Tools (pour afficher les bases) et Livemap / HTTPLocation
  (pour les positions des joueurs en direct). Le fond de carte et les dinos
  viennent de la sauvegarde, sans mod.
- **Tickets** : un systeme de tickets de support avec panneaux et transcriptions
  (aide par IA optionnelle si vous fournissez votre propre cle).
- **Anti-triche** : detecteurs et alertes sur les activites suspectes.
- **Arsenal de dinos** : une vue des creatures notables du cluster (lue dans la
  sauvegarde).
- **Analytique** : statistiques d'activite et de population de votre serveur.
- **Battle Pass** : progression par saisons avec des recompenses en jeu et sur
  Discord.

### Pre-requis

- Un PC Windows allume en continu (le bot surveille le serveur en permanence).
- Une application Discord gratuite : <https://discord.com/developers/applications>
  -> New Application -> Bot -> copier le token.
- Une base de donnees MySQL ou MariaDB. Nitrado en fournit une ; sinon installez
  MariaDB ou utilisez un hebergeur MySQL.
- Pour les fonctions premium de carte : un acces FTP au fichier de sauvegarde, et
  le RCON pour le reste. Nitrado fournit les deux.
- Une cle de licence pour chaque module payant utilise (issue de votre achat sur
  la boutique).

### Reglages de lancement du serveur (le reglage n°1 — pour le combat et les raids)

Le bot lit les kills, tames, morts, structures detruites et raids dans le fichier
journal de votre serveur ARK. ARK n'ecrit ce journal au bon format QUE si le
serveur est lance avec ces trois options :

```
-servergamelog -servergamelogincludetribelogs -ServerRCONOutputTribeLogs
```

Ajoutez-les sur la ligne de commande de lancement du serveur (a cote de `-mods=`,
`-WinLiveMaxPlayers`, etc.), puis redemarrez. Sur Nitrado, collez-les dans le
champ "Arguments de ligne de commande supplementaires". Cela se regle cote
SERVEUR, pas dans config.exe.

- Avec les trois (recommande) : le classement de combat COMPLET — kills, tames,
  morts, **structures et raids** — avec une identite de tribu fiable.
- Avec seulement `-servergamelog` : le bot compte quand meme les kills, tames et
  morts (par nom de tribu), mais **les structures detruites et les raids ne sont
  pas enregistres**, et une tribu renommee apparait comme une nouvelle ligne.
- Sans aucun : le classement de combat reste vide. L'arsenal des dinos, les
  bases, les tribus et la carte en direct fonctionnent quand meme — ils viennent
  de la sauvegarde, pas du journal.

Donc si "les kills ne remontent pas", verifiez d'abord ces options.

### Installation

1. Telechargez la derniere version et extrayez le dossier ou vous voulez
   (gardez `bot.exe` et `config.exe` ensemble).
2. Lancez `config.exe`. Il s'ouvre sur la page Demarrage rapide : collez votre
   token Discord dans son etape 1 et enregistrez — l'outil le stocke pour vous
   (aucun fichier a creer a la main).
3. Toujours dans `config.exe`, remplissez les autres pages :
   - General : les reglages du socle (toujours actif, gratuit — pas de licence ici).
   - Base de donnees : hote, utilisateur, mot de passe et nom de la base.
   - Serveurs : l'acces RCON de votre serveur ARK (et le FTP pour le premium).
   - Page de chaque module : activez ou desactivez le module. Pour un module
     payant, collez sa cle de licence en haut de sa page puis cliquez sur
     Activer.

   Cliquez sur Enregistrer sur chaque page. Les reglages sont ecrits dans un
   dossier `config` a cote de l'outil.
4. Invitez le bot sur votre Discord (Developer Portal -> OAuth2 -> URL Generator
   -> scopes `bot` et `applications.commands` ; permissions : Gerer les salons,
   Gerer les roles, Envoyer des messages, Integrer des liens, Voir l'historique).
5. Lancez `bot.exe`. Il lit les dossiers `config`, `logs` et `cache` situes a
   cote de lui. La fenetre affiche "Connected as ..." une fois en ligne.
6. Sur Discord, tapez `/` pour voir les commandes (`/config`, `/link-panel`,
   `/leaderboard`, et d'autres).

### La configuration se fait uniquement dans config.exe

Toute la configuration, la saisie des licences et l'activation des modules se
font dans `config.exe`. Discord sert uniquement a faire fonctionner et utiliser
le bot (afficher les panneaux, actions). Il n'y a aucune commande de
configuration sur Discord.

### Astuces : listes Discord et classements

- Un menu deroulant Discord n'affiche que 25 choix (par ex. une tribu ou une
  espece de dino). Tapez les premieres lettres du nom pour filtrer la liste et
  atteindre une entree plus bas — la recherche la retrouve toujours.
- `/dino-top` sans espece liste toutes les especes d'un coup ; ne choisissez une
  espece que pour voir le detail de ses records par stat.
- Dans les tableaux de dinos, la colonne "Dino" montre le nom personnalise du
  dino, ou son niveau (par ex. "Niv. 238") s'il n'a jamais ete renomme — jamais
  un simple "?".
- Le classement a DEUX tableaux : choisissez `subject: Tribes` ou
  `subject: Players` sur `/leaderboard`. Ils se configurent sur deux ecrans
  SEPARES dans `config.exe` ("Classement Tribus" et "Classement Joueurs"),
  chacun avec ses propres mesures et poids. `/player` affiche les stats perso
  d'un joueur.

### Mot de passe RCON sur serveurs ASA auto-heberges

Mettez le mot de passe admin/RCON dans `GameUserSettings.ini`, section
`[ServerSettings]` (`ServerAdminPassword=...`). Ne passez jamais
`?ServerAdminPassword=` sur la ligne de commande du serveur : le lanceur ASA
avale tout ce qui suit et l'authentification RCON echoue.

### Garder la cle de licence hors du disque (optionnel)

Au lieu de saisir la cle dans `config.exe`, ajoutez-la au fichier `.env` :
`SENTINEL_LICENSE_KEY=votre-cle`. La variable d'environnement gagne si les deux
sont definies.

### Mods serveur pour les donnees en direct (optionnel)

Certaines fonctions premium lisent des donnees supplementaires sur votre serveur
via deux mods gratuits :

- **Awesome Admin Tools** — ID CurseForge `941450`
  (<https://www.curseforge.com/ark-survival-ascended/mods/awesome-admin-tools>).
  Alimente les bases de la **Carte en direct** et l'identite des joueurs utilisee
  par la liaison des comptes, les classements, l'arsenal de dinos, le battle pass
  et le statut (il voit les joueurs hors-ligne et leurs niveaux exacts). Ajoutez
  `941450` a l'argument `-mods=` du serveur puis redemarrez. Le bot lit son cache
  via le meme acces FTP, en lecture seule.
- **Livemap / HTTPLocation** — ID CurseForge `934290`
  (<https://www.curseforge.com/ark-survival-ascended/mods/livemap>). Envoie les
  positions des joueurs en temps reel pour la **Carte en direct**. Ajoutez
  `934290` a `-mods=`, puis dans `GameUserSettings.ini` ajoutez un bloc
  `[HTTPLocation]` avec `privateid` egal au secret livemap du bot et `url`
  pointant vers le bot. Redemarrez le serveur.

### Importer un serveur deja lance

Quand vous installez Sentinel sur un serveur deja lance et peuple, il importe
l'etat actuel tout seul au demarrage : tribus, bases, dinos et liste des joueurs
apparaissent des le premier cycle. Lancez `/import` (admins seulement) pour tout
recuperer immediatement.

Ce qui ne peut pas etre importe, et pourquoi (une limite d'ARK, pas du bot) : les
scores de combat, les duels PvP et le temps de jeu n'ont aucun historique passe.
ARK n'expose le journal de tribu que comme un tampon en direct qui se vide a
chaque lecture ; les classements et le temps de jeu comptent donc a partir du
moment ou vous installez.

### Carte en direct : gardez-la privee

La carte en direct montre toutes les bases de votre serveur : par defaut elle
n'ecoute que sur votre propre machine (`127.0.0.1`). La pointer vers une adresse
publique est ignore sauf autorisation explicite, donc une faute de frappe ne peut
jamais exposer votre carte entiere.

### Garder le bot lance (relance apres redemarrage)

`bot.exe` est un programme normal : si Windows redemarre (mises a jour !) ou si
la fenetre est fermee, le bot s'arrete jusqu'a ce que quelqu'un le relance. Pour
qu'il demarre tout seul, creez une tache planifiee une fois pour toutes :

1. Menu Demarrer, tapez "Planificateur de taches" et ouvrez-le.
2. Action -> Creer une tache de base -> nommez-la "Sentinel".
3. Declencheur : "Au demarrage de l'ordinateur". Action : "Demarrer un
   programme" -> choisissez votre `bot.exe`. Dans "Commencer dans", mettez le
   dossier qui contient `bot.exe` (important : c'est la qu'il trouve votre
   dossier `config`).
4. Terminez, puis ouvrez les Proprietes de la tache et, dans Parametres, cochez
   "Si la tache echoue, recommencer toutes les 1 minute" pour la relance
   automatique apres un plantage.

Si votre base de donnees (MariaDB/MySQL) tourne sur la meme machine, le bot
l'attend desormais tout seul au demarrage — aucun ordre de lancement a regler.

### Mises a jour

Pour mettre a jour, telechargez la nouvelle version et remplacez `bot.exe` (et
`config.exe`). Votre configuration et vos donnees sont conservees.

### Hebergement

Supportes : Nitrado et serveur Windows auto-heberge. Tout hebergeur qui fournit
FTP et RCON fonctionne.

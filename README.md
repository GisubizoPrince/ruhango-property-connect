# Ruhango Property Connect

## Database ya dashboard (WAMP)

1. Tangiza WAMP, urebe ko MySQL/MariaDB iri gukora.
2. Fungura `http://localhost/phpmyadmin`.
3. Hitamo **Import**, uhitemo [schema.sql](./server/schema.sql), ukande **Import**. Ibi bikora database `ruhango_property_connect` n’imbonerahamwe `properties`, `interests`, na `team_members`. Server nayo ikora `team_members` yonyine igihe itangiye niba database yari isanzweho.
4. Server isoma amakuru ya kera ari muri `server/data/properties.json` na `server/data/interests.json`, ikayimurira muri database igihe imbonerahamwe zikiri ubusa.

`properties` ibika amatangazo dashboard yerekana kandi ihindura. `interests` ibika amazina, telefoni n’umutungo umuntu yashimishijwe na wo. Amakuru y’abashimishijwe ntasibwa iyo itangazo ry’umutungo risibwe.

Mu guhindura itangazo muri dashboard, ushobora kongeraho, gusimbuza cyangwa gusiba amafoto. Amafoto abikwa kuri database hamwe n’itangazo; yemerewe kuba hagati ya rimwe na 12 kuri buri mutungo.

Tab ya **Ikipe** iri ku rubuga rugaragara na buri wese. Muri Admin Dashboard, fungura tab yihariye ya **Ikipe** kugira ngo wongereho, uhindure cyangwa usibe abagize ikipe; buri muntu agira amazina, inshingano, ibisobanuro n’ifoto itari itegeko. Amakuru abikwa muri `team_members` kandi ahita agaragara ku rubuga.

## Gukoresha urubuga na server

Fungura terminals ebyiri uri mu mizi y’umushinga:

1. Tangiza API:
   ```powershell
   cd server
   npm install
   npm run dev
   ```
2. Tangiza urubuga:
   ```powershell
   cd client
   npm run dev
   ```

Urubuga rukoresha API kuri `http://localhost:7000` binyuze kuri Vite proxy. Server ihuza na database ya WAMP ikoresheje ibisanzwe `localhost`, `root`, nta password, na `ruhango_property_connect`. Niba MySQL yawe ifite ibindi byinjira, shyiraho `.env` muri `server`:

```env
DB_HOST=localhost
DB_PORT=3306
DB_USER=root
DB_PASSWORD=
DB_NAME=ruhango_property_connect
PORT=7000
```

Dashboard irinzwe na login; server isoma `ADMIN_USERNAME` na `ADMIN_PASSWORD` muri `server/.env`. Session ibikwa muri cookie ya `HttpOnly` ifite igihe ntarengwa cy’amasaha 8, kandi logout iyihagarika kuri server. API yo guhindura/gusiba amatangazo n’iyo kubona abashimishijwe zisaba admin login.

`server/.env` irimo amakuru yo kwinjira kuri mudasobwa yawe kandi iri muri `.gitignore`; ntuyisangize cyangwa ngo uyishyire muri Git. Hindura password n’urufunguzo rwa session mbere yo kuyishyira kuri server rusange, kandi ukoreshe HTTPS.

Umubare wa favorites dashboard yerekana ubu ukomoka muri browser iri gukoresha urubuga; ntuhuzwa na MySQL kuko favorites zitagira konti z’abakoresha cyangwa endpoint ya server.

Amafoto asanzwe akoreshwa n’urubuga abitswe muri `client/public/images/properties` kandi agaragara no muri build ya Vite. Iyo wimuriye umushinga kuri indi laptop, koporora uyu mushinga wose kugira ngo ayo mafoto ajyane na code. Amatangazo n’amafoto mashya washyize muri dashboard byo bibikwa muri MySQL ya WAMP; niba ushaka no kuyimura, export database `ruhango_property_connect` muri phpMyAdmin hanyuma uyimportinge kuri laptop nshya.

## Kuyishyira kuri internet (Render)

`render.yaml` itegura Render Web Service imwe ikora build ya client, igatanga website na API kuri domain imwe, kandi igakoresha `/healthz` mu kugenzura service. Render ntikoresha WAMP ya laptop; banza ugire MySQL/MariaDB iboneka kuri internet, uyishyiremo imbonerahamwe ukoresheje `server/schema.sql`, hanyuma uhitemo values zayo muri Render Environment Variables.

1. Bika umushinga muri GitHub repository yawe. Root `.gitignore` na `server/.gitignore` birinda `.env`, `node_modules`, na build files kujya muri repository. Ntushyiremo cyangwa ngo wohereze `server/.env`.
2. Kuri Render, fungura **New → Blueprint**, uhuze na GitHub repository, uhitemo `render.yaml`, maze ukore service.
3. Niba ukoresha Railway MySQL, fungura MySQL service → **Settings → Networking → TCP Proxy** maze uyikore (public access). Reba `MYSQL_PUBLIC_URL` muri service variables; uyigabanyemo hostname, port, username, password na database. Muri Render Environment, uzuze `DB_HOST`, `DB_PORT`, `DB_USER`, `DB_PASSWORD`, `ADMIN_USERNAME`, na `ADMIN_PASSWORD`. `DB_SSL` uyishyire `true` gusa niba Railway MySQL connection yawe ishyigikira TLS; ntukohereze URL yose kuko irimo password.
4. Import `server/schema.sql` muri MySQL yo kuri internet. Niba ushaka kuzana amakuru asanzwe ari muri WAMP, export database ya WAMP muri phpMyAdmin uyimportinge muri MySQL yo kuri internet mbere yo gukoresha amakuru mashya.
5. Render izakora deployment kandi iguhe URL ya `*.onrender.com`; custom domain si ngombwa. Ku plan ya free, service ishobora gusinzira iyo imaze igihe idakoreshwa, maze gufungura bwa mbere bigatinda.

Reset-password ikoresheje email ntiyashyizweho; ntukishingikirize kuri yo. Niba wibagiwe admin password, uyihindure muri Render Environment Variables, hanyuma wongere utangize service.

# Outdoor Freedom

> **Find your tribe. Live your adventure.**
> La plateforme qui réunit les passionnés d'outdoor : trouver des partenaires, organiser ses sorties, vivre plus d'expériences — sans jongler entre dix applications.

Ce dépôt contient le **prototype visuel cliquable** (`/prototype/index.html`) et le
**blueprint stratégique** ci-dessous. Le prototype prouve le niveau de design visé
(« rivaliser avec Apple ») ; le blueprint justifie chaque décision produit.

👉 **Ouvrir le prototype** : `prototype/index.html` (aucune dépendance, un seul fichier).
Navigation réelle via la barre d'onglets · light/dark · tout est cliquable.

---

## 0. Principe directeur

> **Une seule question filtre chaque fonctionnalité : « Est-ce que ça fait passer l'utilisateur
> de *organiser* à *vivre* plus vite ? »** Si non → reporté à une version ultérieure.

Le concurrent n'est pas Instagram. C'est **le groupe WhatsApp bordélique + 4 applis météo +
le forum local**. On ne gagne pas en ajoutant des features, on gagne en supprimant des frictions.

---

## 1. Le problème (validé par la douleur, pas par la techno)

| Douleur réelle | Conséquence | Réponse Outdoor Freedom |
|---|---|---|
| « Je ne connais personne à mon niveau » | On ne sort pas, ou seul | Passeport + Carte des aventuriers proches |
| « Je ne sais pas où ni quand sortir » | Sessions ratées, mauvaises conditions | Conditions intégrées + Spots + Sorties datées |
| « Organiser prend plus de temps que la sortie » | Abandon | Créer une aventure en < 30 s, la tribu rejoint |
| « J'utilise 5 applis » | Charge mentale | Un seul endroit : trouver, organiser, discuter, se souvenir |
| « Les infos ne sont pas fiables » | Confiance en berne | Score de fiabilité + guides locaux + niveaux vérifiés par les pairs |

---

## 2. Personas (4 archétypes, pas 4 sports)

Le sport est une variable, pas un persona. Ce qui compte : **le rapport au groupe et au temps**.

### 🏄 Manea — « Le local généreux » (le cœur de réacteur)
- **28 ans, guide/artisan, revenu moyen, Tahiti.** Sort 4×/semaine, connaît les spots.
- **Frustration** : personne de fiable pour partager les bonnes conditions au bon moment.
- **Motivation** : transmettre, ne pas surfer seul, être reconnu comme référence locale.
- **Rôle produit** : **crée l'offre**. Sans lui, pas de sorties. On le choie (badges Guide, Premium offert au mérite).

### 🥾 Léa — « La nouvelle arrivante » (le carburant de croissance)
- **31 ans, cadre, revenu élevé, vient d'emménager.** Motivée, zéro réseau local.
- **Frustration** : les groupes existants sont fermés, intimidants, ou masculins.
- **Motivation** : rencontrer, progresser en sécurité, appartenir.
- **Rôle produit** : **consomme l'offre puis en crée**. Cible payante prioritaire.

### 🎣 Teiki — « Le passionné mono-sport » (la rétention)
- **42 ans, indépendant, revenu variable.** Pêche exclusivement, exigeant sur les conditions.
- **Frustration** : le bruit social, les « influenceurs », le superflu.
- **Motivation** : les bonnes marées, un binôme fiable, zéro chichi.
- **Rôle produit** : **valide qu'on n'est pas Instagram**. S'il reste, le produit est honnête.

### 🏂 Hina — « La multi-outdoor nomade » (l'ambassadrice)
- **26 ans, freelance/remote, revenu moyen, voyage.** Surf, snow, rando selon la saison.
- **Frustration** : repartir de zéro à chaque nouveau lieu.
- **Motivation** : trouver une tribu partout, capitaliser sa réputation.
- **Rôle produit** : **prouve la valeur du Passeport portable** (réputation qui voyage). Moteur d'expansion géographique.

> **Auto-critique** : j'ai écarté « le débutant total » comme persona MVP. Il a besoin de cours/coaching
> (marketplace), pas de partenaires — c'est une autre entreprise. Reporté à V4.

---

## 3. Parcours utilisateur — optimisé friction par friction

```
Téléchargement → Inscription → Onboarding → Passeport → Découverte
   → Créer/Rejoindre une aventure → Discussion → Sortie → Carnet → Retour
```

Décisions clés (chaque étape = un point d'abandon potentiel) :

1. **Inscription** — Apple / Google en 1 tap. Email en secours. *Jamais* de mot de passe imposé à l'entrée.
2. **Onboarding = valeur, pas formulaire.** 3 écrans max : *« Tes sports ? »* → *« Ton niveau ? »* → *« Où ? »*.
   À la fin, on montre **immédiatement 3 aventuriers et 1 sortie près de toi**. La preuve avant l'effort.
3. **Passeport** — pré-rempli au maximum (photo Apple/Google, ville géo). L'utilisateur *confirme*, ne *remplit* pas.
4. **Aha moment ciblé** : *rejoindre sa première sortie en < 5 min après l'install*. C'est LA métrique d'activation.
5. **Retour** — déclenché par un signal utile (bonnes conditions demain + 2 amis y vont), pas par du engagement-bait.

> **Auto-critique** : la tentation était un onboarding « complet » (bio, dispos, tous les sports).
> Refusé : chaque champ obligatoire coûte ~10 % d'activation. On enrichit le profil *après* le premier succès.

---

## 4. Scope MVP — ce qu'on construit, et surtout ce qu'on refuse

### ✅ Dans le MVP (V1)
1. **Auth** (Apple/Google/email) + **Passeport aventurier** (photo, sports, niveau, ville, dispos).
2. **Carte** : sorties + spots + aventuriers, avec filtres sport/niveau/date.
3. **Créer / rejoindre une aventure** (sport, date, lieu, places, description, visibilité).
4. **Messagerie de groupe** liée à chaque sortie (pas de DM 1-1 au départ — voir critique).
5. **Carnet d'aventure** : publications-récits, **sans likes ni compteurs**.
6. **Notifications** ciblées (nouvelle sortie proche, message, demande rejoint).
7. **Sécurité de base** : signalement, blocage, RGPD, modération.

### ❌ Explicitement reporté (avec raison)
| Idée | Pourquoi pas au MVP | Version |
|---|---|---|
| DM privés 1-à-1 | Risque harcèlement + charge modération avant d'avoir une communauté. Le chat de groupe suffit à s'organiser. | V2 |
| Fil d'actualité algorithmique | On n'est pas un réseau social. Le Carnet chronologique suffit. | Jamais (par principe) |
| Événements pro / clubs / assos | B2B ≠ B2C, autre cycle de vente. | V3 |
| Marketplace (matos, coaching, guides payants) | Complexité paiement + confiance + logistique. | V4 |
| Tracking GPS temps réel type Strava | Feature de performance, pas de rencontre. Hors mission. | V3 (léger) |
| Stories éphémères | Copie d'Instagram, contraire à la mission « garder ce qu'on a vécu ». | Jamais |

> **La discipline du refus est la fonctionnalité principale du MVP.**

---

## 5. Les 5 fonctionnalités — justification

**F1 · Passeport Aventurier.** *Le* différenciateur. Une identité outdoor **portable et réputationnelle**
(niveau par sport, badges mérités, score de fiabilité). C'est ce qui crée la confiance nécessaire pour
partir avec un inconnu — et le coût de sortie (« ma réputation me suit »).

**F2 · Carte.** L'outdoor est spatial. La carte répond aux trois « où » (où sont les gens, les sorties, les spots)
en un écran. Filtres = pertinence. *Auto-critique* : on n'affiche pas la position exacte des utilisateurs
(sécurité) — seulement une zone approximative et les sorties publiques.

**F3 · Créer une aventure.** L'unité de valeur atomique. Objectif : **< 30 secondes** pour publier.
Sport → date → lieu → places → visibilité. Tout est pré-suggéré à partir du profil et du contexte.

**F4 · Messagerie de groupe.** Rattachée à la sortie, éphémère par nature (utile avant/pendant, archivée après).
Simple et rapide. Un message épinglé = le RDV. Pas de fonctionnalités de chat superflues.

**F5 · Carnet d'aventure.** **Sans likes, sans followers, sans compteurs.** On raconte une expérience,
pas on collectionne des validations. C'est le pari anti-Instagram : la rétention vient du *souvenir* et de la
*tribu*, pas de la dopamine. *Auto-critique* : sans likes, comment mesurer l'engagement ? Par les
**sorties réalisées** et les **membres de tribu**, pas par des vanity metrics.

---

## 6. Design System — « Deep Water & Sunrise »

Implémenté en tokens CSS dans le prototype (`:root`, light + dark).

| Token | Light | Dark | Usage |
|---|---|---|---|
| `--sea` | `#0E4C4A` | `#159089` | Marque, confiance, actions primaires |
| `--sun` | `#FF7A45` | `#FF875B` | **Accent rare** : action, badges, niveau (jamais en aplat massif) |
| `--glacier` | `#6FA9AF` | `#79B4BA` | Eau, progression, secondaire |
| `--foam / --ink` | `#E7EBE3 / #0C1B1A` | `#070F0E / #EAF1EC` | Fonds & texte, neutres **biaisés vert-mer** |
| sémantiques | `good #1E9E6A` · `warn #E4A93B` · `crit #E2543B` | | États, distincts de l'accent |

- **Typographie** : sans système (qualité SF Pro sur Apple) pour l'UI ; **serif de carnet**
  (`Iowan/Palatino/Georgia`) pour les récits et titres d'aventure — le contraste raconte la marque.
- **Rayons** 10 → 30 px · **Ombres** douces en 2 couches · **Animations** courtes (< 340 ms),
  `prefers-reduced-motion` respecté.
- **Dark mode** conçu, pas inversé : contraste et accent revalidés sur fond océan-nuit.

---

## 7. Architecture (cible V1, pragmatique et scalable)

```
Mobile (React Native / Expo)  ─┐
Web responsive (Next.js)       ─┼──► API (tRPC/REST) ──► Postgres (+ PostGIS)
                                │                     ├─► Auth (Apple/Google/OTP)
                                │                     ├─► Realtime (chat/notifs)
                                │                     ├─► Storage (photos, WebP/CDN)
                                └──► Push (APNs/FCM)   └─► Paiement (RevenueCat + Stripe)
```

**Choix & justification**
- **Expo + React Native** : une base iOS/Android, itération rapide, OTA updates. Priorité mobile (outdoor = terrain).
- **Supabase (Postgres + PostGIS + Auth + Realtime + Storage)** pour le MVP : couvre 80 % des besoins,
  géo natif (requêtes « autour de moi »), coût quasi nul au démarrage. *Auto-critique* : lock-in modéré —
  acceptable car tout reste du Postgres standard, migrable.
- **RevenueCat** au-dessus des paiements natifs App Store / Play : gère abonnements & essais sans back-office lourd.

---

## 8. Base de données (schéma essentiel)

```
users(id, handle, name, avatar_url, bio, home_lat, home_lng, city,
      reliability_score, is_premium, created_at)
sports(id, key, label, emoji)
user_sports(user_id, sport_id, level ENUM[debutant..expert])   -- niveau PAR sport
availabilities(user_id, weekday, part_of_day)                  -- créneaux
spots(id, name, sport_id, lat, lng, difficulty, description)   -- PostGIS geography

adventures(id, host_id, sport_id, spot_id, title, description,
           starts_at, capacity, visibility ENUM[public,tribe,invite],
           lat, lng, status ENUM[open,full,done,cancelled], created_at)
participants(adventure_id, user_id, status ENUM[joined,requested,left], joined_at)

messages(id, adventure_id, sender_id, body, pinned, created_at)

journal_entries(id, author_id, adventure_id?, sport_id, title, story,
                cover_url, location, created_at)   -- pas de table "likes"
journal_media(entry_id, url, order)

badges(id, key, label, emoji, criteria)
user_badges(user_id, badge_id, earned_at)

reports(id, reporter_id, target_type, target_id, reason, status, created_at)
blocks(blocker_id, blocked_id, created_at)
notifications(id, user_id, type, payload_json, read_at, created_at)
```

**Contraintes / perfs** : `participants` unique `(adventure_id,user_id)` ; index GIST sur `adventures(lat,lng)`
et `spots` pour les requêtes géo ; index sur `adventures(starts_at) WHERE status='open'` ;
`capacity` vérifiée en transaction au join (anti-surbooking).

---

## 9. API (REST, extrait — GraphQL non justifié à ce stade)

| Méthode | Endpoint | Rôle |
|---|---|---|
| `POST` | `/auth/apple` · `/auth/google` · `/auth/otp` | Connexion |
| `GET/PATCH` | `/me` | Passeport (profil, sports, dispos) |
| `GET` | `/map?bbox=&sports=&level=&date=` | Sorties + spots + aventuriers dans la zone |
| `POST` | `/adventures` | Créer une aventure |
| `GET` | `/adventures/:id` | Détail |
| `POST` | `/adventures/:id/join` · `/leave` | Participer (transaction capacity) |
| `GET/POST` | `/adventures/:id/messages` | Chat de groupe (WS pour le temps réel) |
| `GET/POST` | `/journal` · `/journal/:id` | Carnet |
| `POST` | `/reports` · `/blocks` | Sécurité |
| `POST` | `/billing/checkout` · webhook | Premium |

> **Auto-critique GraphQL** : séduisant pour la carte (données hétérogènes), mais surcoût de mise en place
> et de cache non justifié pour ~15 endpoints. REST + un endpoint `/map` agrégé suffit. Réévaluable en V3.

---

## 10. Sécurité, confiance & RGPD

- **RGPD** : consentement géoloc explicite et révocable ; export + suppression de compte en 1 écran ;
  minimisation (position *approximative* stockée, jamais le temps réel) ; hébergement UE.
- **Sécurité communautaire** : signalement, blocage, masquage de position exacte, **score de fiabilité**
  (no-show / annulations tardives le dégradent) ; modération sur signalement + heuristiques anti-spam
  (rate-limit création, détection de liens).
- **Sûreté physique** (spécifique outdoor) : rappel conditions/niveau avant de rejoindre une sortie à risque ;
  visibilité `invite` pour les sorties sensibles ; jamais de position domicile publique.

---

## 11. Business model — Freemium honnête

**Principe** : le *nécessaire pour vivre l'aventure* reste **gratuit à vie**. Premium enlève les
**frictions des utilisateurs actifs** — on ne rançonne pas la mission.

| | **Free** | **Premium** — 7,99 €/mois ou 4,90 €/mois (annuel), 7 j d'essai |
|---|---|---|
| Rejoindre des sorties | ✅ illimité | ✅ illimité |
| Créer des sorties | 2 actives | ✅ illimité |
| Carte & filtres | de base | complète (conditions détaillées, filtres illimités) |
| Passeport | standard | signature (bio étendue, badges rares, thème) |
| Priorité locale | — | sa sortie remonte auprès de la tribu |
| Carnet | ✅ | ✅ + export souvenirs |

**Autres leviers (plus tard, sans dénaturer)** : Premium offert aux **guides locaux méritants** (acquisition d'offre),
partenariats marques outdoor sur le Carnet (natif, non intrusif, V3), marketplace commissionnée (V4).
*Refusé* : publicité display (tue le premium), vente de données (tue la confiance).

---

## 12. Roadmap V1 → V5

| Version | Focus | Fonctions clés | Effort | Impact utilisateur |
|---|---|---|---|---|
| **V1 — MVP** | *Trouver & organiser* | Auth, Passeport, Carte, Créer/Rejoindre, Chat groupe, Carnet, sécurité | ~3–4 mois | 🔴🔴🔴 Validation marché |
| **V2** | *Rétention & confiance* | Score fiabilité affiné, badges, DM 1-1 modérés, conditions temps réel, invitations | ~2 mois | 🔴🔴🔴 Boucle de rétention |
| **V3** | *Croissance & lieux* | Réputation portable multi-régions, clubs/assos, partenariats marques, i18n | ~3 mois | 🔴🔴 Expansion géo |
| **V4** | *Monétisation élargie* | Marketplace (guides, coaching, matos), débutants encadrés | ~4 mois | 🔴🔴 Nouveaux revenus |
| **V5** | *Référence mondiale* | Ligues/défis communautaires légers, API partenaires, offline terrain | ~4 mois | 🔴 Réseau & moat |

*Les coûts se raisonnent en équipe : un noyau de 2 dev + 1 designer produit livre V1 en ~3–4 mois.*

---

## 13. Ce que contient ce dépôt

```
prototype/index.html   → prototype cliquable, autonome (design system + 5 écrans + chat + premium)
README.md              → ce blueprint stratégique
docs/                  → (à venir) specs détaillées par domaine
```

**Prochaine itération suggérée** : transformer le prototype en scaffold Expo + Supabase réel
(auth + passeport + création d'aventure d'abord), en gardant le scope MVP strict ci-dessus.

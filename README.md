# TaxPilot

> **Les impôts deviennent simples.**
> Permettre à n'importe quel particulier de remplir sa déclaration d'impôts grâce à une IA conversationnelle — plus simple que Taxfix, beaucoup plus moderne.

Ce dépôt contient le **prototype visuel cliquable** (`prototype/index.html`) et le **blueprint d'ingénierie**
ci-dessous. Le prototype prouve le niveau de design visé (« rivaliser avec Stripe, Linear, Notion, Apple ») ;
le blueprint définit l'architecture, le schéma de base, l'arborescence, les user flows et la roadmap
**avant** d'écrire la première ligne de production.

👉 **Ouvrir le prototype** : `prototype/index.html` — un seul fichier, aucune dépendance.
Landing complète · authentification · dashboard · assistant IA · documents + OCR · dossier fiscal · historique ·
paramètres · support · admin · light/dark · responsive. Tout est cliquable.

---

## 0. Principe directeur

> **Une seule question filtre chaque décision : « Est-ce que l'utilisateur remplit un formulaire, ou l'IA fait le travail ? »**
> Si l'utilisateur doit remplir → on repense l'interaction. Le concurrent n'est pas l'administration fiscale.
> C'est **la pile de papiers + les 40 cases obscures + la peur de se tromper.** On ne gagne pas en ajoutant des
> champs, on gagne en supprimant la friction et l'incertitude.

Trois engagements non négociables, hérités du brief :

1. **Aucun faux backend, aucune base simulée.** Toute fonctionnalité livrée est réellement implémentée.
2. **L'IA n'invente jamais.** Chaque réponse porte un **niveau de confiance**. En cas d'incertitude → elle demande confirmation.
3. **Le moteur fiscal est indépendant de l'interface.** La logique métier vit dans `/tax-engine`, jamais dans les composants React.

---

## 1. Le problème (validé par la douleur, pas par la techno)

| Douleur réelle | Conséquence | Réponse TaxPilot |
|---|---|---|
| « Je ne comprends pas les cases » | Erreurs, stress, report au dernier jour | Conversation en français clair, l'IA remplit à ma place |
| « Je ne sais pas si j'oublie une déduction » | Trop d'impôt payé | Détection automatique des crédits & oublis |
| « Chercher chaque justificatif » | Abandon | Import + OCR haute précision, extraction auto |
| « J'ai peur de me tromper » | Anxiété, procrastination | Détection d'incohérences + niveau de confiance affiché |
| « L'outil ne me fait pas confiance / je ne lui fais pas confiance » | Défiance | Sources citées, confiance visible, données chiffrées UE |

---

## 2. Personas

Le sport n'est pas le persona ; ici, c'est **le rapport au temps et à la complexité fiscale**.

| Persona | Situation | Besoin dominant |
|---|---|---|
| **Le pressé** (Camille, salariée) | Salaire simple, veut en finir vite | Rapidité, zéro jargon, confiance |
| **Le complexe** (Marc, indépendant) | Revenus multiples, TVA, frais | Ne rien oublier, régime optimal |
| **Le foyer** (Sarah, propriétaire bailleur) | Enfants, foncier, crédits | Optimisation, parts, déductions |
| **L'anxieux** (Julien, primo-déclarant) | Première fois, peur de l'erreur | Pédagogie, réassurance, contrôle final |

---

## 3. Architecture (Clean Architecture + DDD)

```
┌──────────────────────────────────────────────────────────────┐
│  PRÉSENTATION  · Next.js 15 App Router · React · shadcn/ui     │
│  Server Components + Suspense/streaming · React Hook Form + Zod │
└───────────────┬──────────────────────────────────────────────┘
                │  TanStack Query (cache) · Server Actions / API routes
┌───────────────▼──────────────────────────────────────────────┐
│  APPLICATION  · Services · Use-cases · Validation Zod          │
│  (chat, ocr, tax-file, billing, notifications, audit)          │
└───────┬───────────────────────┬──────────────────────┬────────┘
        │                       │                      │
┌───────▼────────┐   ┌──────────▼─────────┐   ┌────────▼─────────┐
│  DOMAINE        │   │  TAX-ENGINE         │   │  INFRASTRUCTURE  │
│  Entities       │   │  (indépendant, pur) │   │  Repositories    │
│  Value Objects  │   │  /france/rules ...  │   │  Supabase/PG/RLS │
│  Interfaces     │   │  calculations ...   │   │  Storage · Auth  │
└─────────────────┘   └─────────────────────┘   │  Stripe·OpenAI·  │
                                                 │  OCR·Resend·     │
                                                 │  PostHog·Sentry  │
                                                 └──────────────────┘
```

**Règles de dépendance :** la Présentation dépend de l'Application ; l'Application dépend du Domaine et
d'interfaces (Repository Pattern). Le **Tax-Engine est pur** (aucune I/O, aucune dépendance framework) →
testable à 100 % et portable vers d'autres pays. L'Infrastructure implémente les interfaces du Domaine.

---

## 4. Stack technique

| Couche | Choix | Justification |
|---|---|---|
| Frontend | **Next.js 15** (App Router), React, TypeScript strict | Server Components, streaming, SEO natif |
| UI | **Tailwind CSS + shadcn/ui**, Radix | Design system cohérent, accessibilité WCAG AA |
| Forms | **React Hook Form + Zod** | Validation partagée client/serveur (une seule source de vérité) |
| Data client | **TanStack Query** | Cache, invalidation, optimistic updates |
| Backend | **Supabase** (PostgreSQL, Auth, Storage) | RLS natif, UE, auth multi-provider |
| Paiement | **Stripe** (Checkout, Billing, Customer Portal, Webhooks) | Standard SaaS, abonnements + factures |
| IA | **OpenAI** (function/tool calling, streaming) | Chat + extraction structurée + confiance |
| OCR | **OCR haute précision** (service dédié via adaptateur) | Extraction champs + score de confiance |
| Emails | **Resend** | Transactionnels + relances |
| Analytics | **PostHog** | Funnels, feature flags |
| Monitoring | **Sentry** | Erreurs front + back |
| Déploiement | **Vercel** | Edge, preview deployments |

---

## 5. Schéma de la base de données (PostgreSQL + RLS Supabase)

Toutes les tables métier portent `user_id uuid references auth.users` et une **policy RLS** :
`user_id = auth.uid()` (lecture/écriture). Les tables d'admin/log sont accessibles via le rôle `admin`.
`created_at`/`updated_at` (trigger `moddatetime`) sur chaque table. Aucune table inutile.

```
auth.users (Supabase)
  └─ profiles (1-1)                fiscal_country, locale, role, stripe_customer_id
       ├─ tax_households (1-N)     parts, situation, conjoint
       │    └─ dependents (1-N)    enfants / personnes à charge (nom, date_naissance)
       ├─ documents (1-N)          storage_path, type, mime, ocr_status
       │    └─ ocr_extractions (1-N)  field_key, value, confidence, corrected
       ├─ tax_files (1-N)          année, status, progress, estimated_tax  ← "dossier fiscal"
       │    ├─ incomes (1-N)       type, amount, source_document_id
       │    ├─ deductions (1-N)    type, amount, credit_amount, source_document_id
       │    └─ tax_file_events (1-N)  audit: actor(user|ai), action, before/after
       ├─ conversations (1-N)      titre, tax_file_id
       │    └─ messages (1-N)      role, content, confidence, tool_calls
       ├─ subscriptions (1-1)      stripe_sub_id, plan, status, current_period_end
       ├─ invoices (1-N)           stripe_invoice_id, amount, pdf_url, status
       ├─ notifications (1-N)      type, payload, read_at
       └─ audit_logs (1-N)         action, ip, user_agent  (sécurité/RGPD)

checklist_items (générés par le tax-engine par tax_file)  key, status(done|verify|missing)
```

**Contraintes clés :** FK `on delete cascade` depuis `profiles`, `check` sur les enums
(`document.type`, `income.type`, `subscription.status`…), `unique(user_id, year)` sur `tax_files`,
montants en `numeric(12,2)`, `confidence` en `numeric(4,3)` (0–1). Suppression de compte =
suppression en cascade + purge Storage (droit à l'effacement RGPD).

---

## 6. Arborescence du dépôt

```
taxpilot/
├─ prototype/index.html          # prototype cliquable (ce livrable)
├─ app/                          # Next.js App Router
│  ├─ (marketing)/               # landing, pricing, faq, blog
│  ├─ (auth)/login, /signup, /reset, /verify
│  ├─ (app)/dashboard, /assistant, /documents, /dossier, /history, /settings, /support
│  ├─ (admin)/admin/...
│  ├─ api/v1/                    # REST versionnée (chat, documents, ocr, tax-file, stripe/webhook)
│  └─ error.tsx, not-found.tsx   # 500 / 404
├─ src/
│  ├─ domain/                    # entities, value-objects, repository interfaces
│  ├─ application/               # services / use-cases (Zod schemas)
│  ├─ infrastructure/            # supabase repos, stripe, openai, ocr, resend adapters
│  ├─ components/ui              # shadcn/ui
│  └─ lib/                       # hooks, query client, utils, env (zod-validated)
├─ tax-engine/                   # MOTEUR FISCAL INDÉPENDANT (pur, testé)
│  └─ france/
│     ├─ rules/                  # barèmes, plafonds, seuils par année
│     ├─ calculations/           # impôt, parts, quotient familial
│     ├─ deductions/             # charges déductibles
│     ├─ credits/                # crédits & réductions (dons art.200, garde, emploi domicile…)
│     └─ validators/             # cohérence, doublons, incohérences
├─ supabase/migrations/          # SQL: tables, contraintes, RLS, triggers
├─ tests/                        # unit (tax-engine, services) · integration (api) · e2e (Playwright)
└─ ...config (eslint, prettier, tsconfig strict, playwright, vitest)
```

---

## 7. User flows principaux

**Onboarding → premier document (le "aha" en < 2 min)**
`Landing → Inscription (Google/Apple/Email/Magic Link) → Vérification email →
Dashboard vide → "Importez un document" → OCR → champs extraits + confiance → Valider →
checklist se remplit toute seule → l'IA engage la conversation.`

**Boucle de valeur (assistant + dossier)**
`Assistant pose une question ciblée → réponse utilisateur → l'IA met à jour le dossier fiscal
(event d'audit) → checklist recalculée par le tax-engine → estimation d'impôt rafraîchie →
détection d'oublis/erreurs → répéter jusqu'à 100 %.`

**Finalisation → paiement**
`Dossier à 100 % → récap + estimation → Stripe Checkout (essai → plan) → dossier exportable
prêt à reporter → notifications (email de confirmation).`

**RGPD**
`Paramètres → Export de mes données (job → email) · Suppression définitive (confirmation email →
cascade + purge Storage + audit_log).`

---

## 8. Maquettes

Livrées et navigables dans `prototype/index.html` :

- **Landing** : hero + CTA, capture produit, comparaison classique/TaxPilot, 3 étapes, features, avis, tarifs (toggle mensuel/annuel), FAQ, bande CTA, footer, SEO/OG prêts.
- **Auth** : modal Google / Apple / Email / Magic Link, bascule connexion↔inscription.
- **Dashboard** : anneau de progression, estimation d'impôt, checklist intelligente (complété/à vérifier/manquant), documents récents, dernière conversation IA.
- **Assistant IA** : conversation réelle avec mémoire, réponses contextuelles, **niveau de confiance** par message, suggestions, l'IA demande confirmation quand elle n'est pas sûre.
- **Documents** : drag & drop, scanner mobile, table d'extraction OCR éditable avec barres de confiance, liste des documents.
- **Dossier fiscal** : onglets Identité / Foyer / Revenus / Charges & crédits / Documents — tout modifiable.
- **Historique** : déclarations par année + journal d'audit (acteur : vous / IA).
- **Paramètres** : compte, sécurité (2FA, appareils connectés), notifications, abonnement (Stripe), RGPD (export/suppression).
- **Support** · **Admin** (KPI, utilisateurs, logs, config & rôles) · **404 / 500**.

---

## 9. Sécurité & conformité

- **RGPD** : hébergement UE, export & suppression définitive, registre des traitements, minimisation.
- **Chiffrement** : TLS en transit, AES-256 au repos (Supabase), Storage privé signé.
- **RLS partout** : isolation stricte par `user_id`, aucune donnée transverse sans rôle `admin`.
- **Validation serveur systématique** (Zod) ; jamais confiance au client.
- **Rate limiting** sur API sensibles (chat, OCR, auth), protection CSRF, échappement XSS, requêtes paramétrées (anti-injection).
- **Headers** : CSP, HSTS, X-Frame-Options, Referrer-Policy, Permissions-Policy.
- **Audit logs** (connexions, modifications de dossier), **2FA**, gestion des sessions/appareils.

---

## 10. Moteur fiscal (`/tax-engine`)

- **Pur & indépendant** : entrée = état structuré du dossier ; sortie = impôt estimé + checklist + anomalies. Aucune I/O.
- **France v1** : quotient familial & parts, barème progressif, dons (art. 200 CGI), garde d'enfants, emploi à domicile, revenus fonciers (micro vs réel), retenue à la source.
- **Validators** : documents manquants, revenus incohérents, doublons, formats invalides, informations contradictoires → messages d'explication simples pour l'utilisateur.
- **Extensibilité** : ajouter un pays = ajouter `/{pays}/{rules,calculations,deductions,credits,validators}` sans toucher à l'app.

---

## 11. Plan de développement (par phases, chaque phase complète avant la suivante)

1. **Fondations** — repo, TS strict, ESLint/Prettier, env Zod, Supabase, migrations + RLS, CI.
2. **Auth & compte** — providers, vérification email, 2FA, sessions, profils, RGPD (export/suppression).
3. **Documents & OCR** — upload, Storage privé, pipeline OCR, extraction + confiance, correction.
4. **Tax-engine France** — rules/calculations/deductions/credits/validators + tests unitaires exhaustifs.
5. **Dossier fiscal & checklist** — CRUD, audit, recalcul par le tax-engine, détection d'erreurs.
6. **Assistant IA** — chat streaming, mémoire, tool calling relié au dossier, confiance, garde-fous anti-invention.
7. **Paiements** — Stripe Checkout/Billing/Portal, webhooks, factures.
8. **Notifications** — Resend + in-app (relances, import/OCR terminé, abonnement).
9. **Admin** — utilisateurs, abonnements, logs, config, rôles.
10. **Qualité & lancement** — tests intégration + E2E, SEO/JSON-LD/sitemap, Lighthouse >95, Sentry/PostHog, perf.

---

## 12. Roadmap

| Horizon | Contenu |
|---|---|
| **MVP (v1)** | France, salaires + dons + foyer + foncier simple, assistant IA, OCR, Stripe, dashboard, admin |
| **v1.1** | Revenus indépendants/BIC-BNC, plus de crédits, pré-remplissage avancé |
| **v1.2** | Import connecté (banques/paie), rappels d'échéance intelligents |
| **v2** | 2e pays (nouveau module tax-engine), app mobile, conseil fiscal augmenté |

---

## 13. Tests & qualité

- **Unitaires** : tax-engine (barèmes, parts, crédits, validators), services, value objects.
- **Intégration** : routes API v1, RLS (accès inter-utilisateurs refusé), webhooks Stripe.
- **E2E (Playwright)** : onboarding → import → OCR → chat → dossier 100 % → checkout.
- **Non négociable** : types stricts, aucun `any`, aucun TODO, aucun code mort, Lighthouse perf/SEO/a11y > 95.

---

*© 2026 TaxPilot — prototype & blueprint. Hébergé et conçu pour l'Union européenne. 🇪🇺*

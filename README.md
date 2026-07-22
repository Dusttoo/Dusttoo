# Dusty Mumphrey

Senior engineer, 9+ years building production systems that ship under real constraints, serve real users, and carry real money. I've designed data pipelines for federal agencies, maintained HIPAA-compliant healthcare infrastructure, led a team of 7, and now run a software studio where I own three products end to end: architecture, deployment, payments, on-call, and the support inbox.

I'm also an active animal breeder. I run a crested gecko breeding program, and I build software for breeders and breed registries because I needed it before I sold it. When I design a pedigree model or a registration flow, I'm not guessing at the workflow.

**[builtbydusty.com](https://builtbydusty.com)** · [dmumphrey@builtbydusty.com](mailto:dmumphrey@builtbydusty.com) · [LinkedIn](https://www.linkedin.com/in/dusty-mumphrey) · [dev.to](https://dev.to/dusttoo)

---

## What I Work With

[![Python](https://img.shields.io/badge/-Python-3776AB?style=flat-square&logo=Python&logoColor=white)]()
[![TypeScript](https://img.shields.io/badge/-TypeScript-007ACC?style=flat-square&logo=typescript&logoColor=white)]()
[![FastAPI](https://img.shields.io/badge/-FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white)]()
[![Node.js](https://img.shields.io/badge/-Node.js-339933?style=flat-square&logo=node.js&logoColor=white)]()
[![React](https://img.shields.io/badge/-React-45b8d8?style=flat-square&logo=react&logoColor=white)]()
[![React Native](https://img.shields.io/badge/-React%20Native-45b8d8?style=flat-square&logo=react&logoColor=white)]()
[![Next.js](https://img.shields.io/badge/-Next.js-000000?style=flat-square&logo=next.js&logoColor=white)]()
[![PostgreSQL](https://img.shields.io/badge/-PostgreSQL-336791?style=flat-square&logo=postgresql&logoColor=white)]()
[![Redis](https://img.shields.io/badge/-Redis-DC382D?style=flat-square&logo=redis&logoColor=white)]()
[![AWS](https://img.shields.io/badge/-AWS-232F3E?style=flat-square&logo=amazon-aws&logoColor=white)]()
[![Docker](https://img.shields.io/badge/-Docker-46a2f1?style=flat-square&logo=docker&logoColor=white)]()
[![Kubernetes](https://img.shields.io/badge/-Kubernetes-326CE5?style=flat-square&logo=kubernetes&logoColor=white)]()
[![Terraform](https://img.shields.io/badge/-Terraform-623CE4?style=flat-square&logo=terraform&logoColor=white)]()

**Certifications:** AWS Certified Solutions Architect, Associate · Microsoft Certified: Azure Developer Associate

---

## Open Source

### [claude-orchestrator](https://github.com/Dusttoo/claude-orchestrator)

A multi-agent orchestration harness for Claude Code, extracted from the system I run nightly against a production multi-tenant SaaS codebase. Each ticket is implemented by an agent in an isolated git worktree, then reviewed by a separate code-review agent and a separate security-review agent that never see the implementer's reasoning, and merged only when every gate is green.

The design rule is that the author never reviews their own work, because an agent asked to review its own diff confirms its implementation instead of testing the requirement. The enforcement rule matters more: the merge gate is a `PreToolUse` hook that can veto the merge command outright, rather than an instruction the agent is asked to follow. Branch protection can enforce CI-green, but it cannot know whether the review agents actually returned PASS. Making that constraint mechanical is the difference between "never merge on red" being discipline and being a mechanism.

Packaged as a Claude Code plugin. Repo-specific facts live in a per-repo config file so the harness itself stays repo-agnostic.

### [deploykit](https://github.com/Dusttoo/deploykit)

A Bash-native deployment toolkit for machines where you cannot assume a runtime beyond Bash and standard Unix tooling. Handles releases, environment config, HTTP and TCP health checks with retry and backoff, symlink-based rollback, an AES-256 encrypted secret vault with versioned rotation slots and a grace period, and an append-only audit log with checksum verification. Ships with a pure-Bash test suite and a Dockerfile so the whole thing runs reproducibly in a container.

### [reptile-genetics-engine](https://github.com/Dusttoo/reptile-genetics-engine)

A rule-based genetics engine that resolves an animal's visible traits from its genotype, covering seven dominance patterns, same-locus suppression, lethal homozygous combinations, and polygenic traits. The interesting design work is in the epistemics rather than the Mendelian math. Inheritance rules live in the database rather than in code, so updating a trait's behavior is a data migration instead of a deploy. `UNKNOWN` is a first-class value handled conservatively rather than a missing field. A sigmoid confidence function gradually displaces textbook genetics with observed breeding outcomes as sample size grows.

Companion code for [a writeup on modeling a domain whose experts disagree](https://dev.to/dusttoo/i-built-a-phenotype-generator-for-crested-gecko-genetics-heres-how-i-modeled-a-hobby-that-cant-34kl).

### [morph-finder](https://github.com/Dusttoo/morph-finder)

A React Native Expo app that walks crested gecko keepers through identifying their animal's morph via a guided question flow, running on the same genetics engine as ReptiDex. Expo, Supabase, TanStack Query, Zustand, React Native Web, TypeScript, Jest.

### [react-native-expo-supabase-starter](https://github.com/Dusttoo/react-native-expo-supabase-starter)

A production-ready React Native starter with Supabase, RevenueCat, and TanStack Query. The part most starters skip is the one worth having: a tested, injectable Supabase service layer with a chainable mock helper for Jest.

---

## What I've Built

### [Breed Ledger](https://breedledger.co)

Multi-tenant SaaS for breeders, breed clubs, and registries. Launched May 2026 and my primary focus. Individual breeder tenants and breed organizations each get their own admin experience, public surface, billing model, and access rules on one shared architecture.

Tenant and organization context resolves from the hostname and passes through server components and actions so every request operates inside the correct account boundary. Every table has Row Level Security enabled, with policies separating tenant data, org data, public read paths, member access, registrar access, and admin-only workflows. Mutations run through server actions and server-side Supabase access, keeping privileged logic out of the client bundle. Organizations collect dues, registration fees, and event entries through Stripe Connect, managed separately from platform subscriptions.

First organizational client is the [Gold Standard Gecko Club](https://www.builtbydusty.com/case-studies/gold-standard-gecko-club), live since June 2026 with membership, animal registry, event entry, and payments running through a custom-branded tenant bolted onto the public site they already had. Their first show under the system is NRBE 2026 in Daytona Beach.

Next.js · TypeScript · Supabase · PostgreSQL · Stripe Connect · Vercel

### [ReptiDex](https://reptidex.com)

Reptile husbandry and breeding records app, built and shipped solo, live on iOS and web since March 2026. 229 registered keepers tracking 560 animals across 225 collections, with 686 feeding events logged and 66 active breeding pairs recorded.

Multi-generation pedigree trees resolved in PostgreSQL, cross-collection lineage linking, QR-code-to-live-record animal profiles, genetic calculators across eight species, and AI morph identification running a TensorFlow model trained on 22,000+ labeled images and served from SageMaker at 88 percent accuracy. Records sync automatically into Breed Ledger club portals so keepers maintain data once.

React Native · Expo · FastAPI · PostgreSQL · Supabase · AWS · SageMaker

### [Built By Dusty](https://builtbydusty.com)

The studio. Custom builds for breeders and small businesses ([breeder websites](https://builtbydusty.com/services/breeder-websites), [records and genetics apps](https://builtbydusty.com/services/breeding-records-app), [sales platforms](https://builtbydusty.com/services/breeder-sales-platform), [registry and pedigree software](https://builtbydusty.com/services/registry-pedigree-platform)) plus a [template and contract kit shop](https://builtbydusty.com/shop) and a set of free tools. The [COI calculator](https://builtbydusty.com/tools/coi-calculator) and breed color calculators run on the same genetics engine that powers the products.

### Scrollbook (sunset, 2025 to 2026)

An AI-powered D&D campaign platform serving 1,000+ active users across 867 Discord servers, taken from idea to production in two months as sole engineer. Six FastAPI microservices, a Next.js frontend, PostgreSQL, a Discord bot with 33 slash commands, an RBAC subscription system, and AWS CDK infrastructure with zero-downtime deployments.

The engineering result I'm proudest of: prompt caching and context partitioning cut inference cost by roughly 90 percent, taking average session cost from about $10.00 to $1.30. It is no longer running. At that user count the infrastructure cost outran what I could carry solo alongside the rest of the studio, so I shut it down rather than run it badly. [Writeup here](https://dev.to/dusttoo/i-cut-claude-api-costs-by-90-with-prompt-caching-heres-what-i-learned-before-i-had-to-shut-it-564d).

---

## Writing

- [Building a Multi-Generation Pedigree Tree in PostgreSQL](https://dev.to/dusttoo/building-a-multi-generation-pedigree-tree-in-postgresql-288)
- [I cut Claude API costs by 90% with prompt caching. Here's what I learned before I had to shut it down.](https://dev.to/dusttoo/i-cut-claude-api-costs-by-90-with-prompt-caching-heres-what-i-learned-before-i-had-to-shut-it-564d)
- [How I solved Supabase's chainable query builder problem in React Native tests](https://dev.to/dusttoo/how-i-solved-supabases-chainable-query-builder-problem-in-react-native-tests-oa7)
- [I built a phenotype generator for crested gecko genetics. Here's how I modeled a hobby that can't agree on its own rules.](https://dev.to/dusttoo/i-built-a-phenotype-generator-for-crested-gecko-genetics-heres-how-i-modeled-a-hobby-that-cant-34kl)
- [I built and launched a mobile app in 3 months as a solo engineer. Here's exactly what happened.](https://dev.to/dusttoo/i-built-and-launched-a-mobile-app-in-3-months-as-a-solo-engineer-heres-exactly-what-happened-479e)

Longer build stories: [Case Studies](https://builtbydusty.com/case-studies)

---

## Professional Background

**Built By Dusty** (2017 to present), Founder and Full Stack Engineer
Own architecture through deployment, payments, and support for three live products. Signed and support the first organizational client on Breed Ledger.

**Alignerr and Handshake AI** (2025 to present), AI Training Contributor and Reviewer
Expert-level training data, evaluations, and annotations for frontier LLM development across coding and technical domains. Promoted to reviewer on multiple projects; audit contributor submissions against project rubrics and deliver corrective feedback.

**DealerOn** (2024 to 2025), Senior Engineer
Led a team of 7. Designed and shipped an event-driven personalization engine reaching 87 percent dealer enrollment. Raised test coverage from roughly 50 percent to 85 percent across 10 applications in 6 weeks.

**GovStar** (2025), Full Stack Engineer and Data Scientist
Event-driven ingestion and analytics for federal clients including CBP, ICE, and FEMA. Multi-million-row datasets, automated 309-column reporting workflows. Python, Snowflake, SageMaker, Neptune graph database, Tableau.

**Clockwork** (2022 to 2024), Senior Engineer
Agency work across healthcare, insurance, retail, and government clients, running several concurrent projects on different stacks. React, Python, Docker, Kubernetes, and custom e-commerce. Authored architecture RFCs and drove CI/CD performance improvements.

**Vault Health** (2021 to 2022), Reliability Engineer
HIPAA-compliant Python and FastAPI services processing millions of clinical tests weekly. Held under a 1 percent incident rate across multi-region Kubernetes clusters.

**Education:** B.S. Computer Science, The University of Texas at Austin

---

## Currently

Building Breed Ledger toward its next registry clients, shipping ReptiDex updates, extracting claude-orchestrator into something anyone can run, and writing up the parts worth writing up.

Open to senior and staff engineering roles, and to projects in breeding, animal health, niche e-commerce, or AI integration.

[![Portfolio](https://img.shields.io/badge/Portfolio-builtbydusty.com-2D5016?style=for-the-badge)](https://builtbydusty.com)
[![Email](https://img.shields.io/badge/Email-dmumphrey%40builtbydusty.com-C4783A?style=for-the-badge)](mailto:dmumphrey@builtbydusty.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-Connect-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/dusty-mumphrey)

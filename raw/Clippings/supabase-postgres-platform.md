---
title: "supabase/supabase: The Postgres development platform. Supabase gives you a dedicated Postgres database to build your web, mobile, and AI applications."
source: "https://github.com/supabase/supabase"
author:
published:
created: 2026-06-16
description: "The Postgres development platform. Supabase gives you a dedicated Postgres database to build your web, mobile, and AI applications. - supabase/supabase"
tags:
  - "clippings"
---
[![](https://user-images.githubusercontent.com/8291514/213727234-cda046d6-28c6-491a-b284-b86c5cede25d.png#gh-light-mode-only)](https://user-images.githubusercontent.com/8291514/213727234-cda046d6-28c6-491a-b284-b86c5cede25d.png#gh-light-mode-only) [![](https://user-images.githubusercontent.com/8291514/213727225-56186826-bee8-43b5-9b15-86e839d89393.png#gh-dark-mode-only)](https://user-images.githubusercontent.com/8291514/213727225-56186826-bee8-43b5-9b15-86e839d89393.png#gh-dark-mode-only)

## Supabase

[Supabase](https://supabase.com/) is the Postgres development platform. We're building the features of Firebase using enterprise-grade open source tools.

- Hosted Postgres Database. [Docs](https://supabase.com/docs/guides/database)
- Authentication and Authorization. [Docs](https://supabase.com/docs/guides/auth)
- Auto-generated APIs.
- Functions.
- File Storage. [Docs](https://supabase.com/docs/guides/storage)
- AI + Vector/Embeddings Toolkit. [Docs](https://supabase.com/docs/guides/ai)
- Dashboard

[![Supabase Dashboard](https://raw.githubusercontent.com/supabase/supabase/master/apps/www/public/images/github/supabase-dashboard.png)](https://raw.githubusercontent.com/supabase/supabase/master/apps/www/public/images/github/supabase-dashboard.png)

Watch "releases" of this repo to get notified of major updates.

[![Watch this repo](https://raw.githubusercontent.com/supabase/supabase/d5f7f413ab356dc1a92075cb3cee4e40a957d5b1/web/static/watch-repo.gif)](https://raw.githubusercontent.com/supabase/supabase/d5f7f413ab356dc1a92075cb3cee4e40a957d5b1/web/static/watch-repo.gif)

## Documentation

For full documentation, visit [supabase.com/docs](https://supabase.com/docs)

To see how to Contribute, visit [Getting Started](https://github.com/supabase/supabase/blob/master/DEVELOPERS.md)

## Community & Support

- [Community Forum](https://github.com/supabase/supabase/discussions). Best for: help with building, discussion about database best practices.
- [GitHub Issues](https://github.com/supabase/supabase/issues). Best for: bugs and errors you encounter using Supabase.
- [Email Support](https://supabase.com/docs/support#business-support). Best for: problems with your database or infrastructure.
- [Discord](https://discord.supabase.com/). Best for: sharing your applications and hanging out with the community.

## How it works

Supabase is a combination of open source tools. We’re building the features of Firebase using enterprise-grade, open source products. If the tools and communities exist, with an MIT, Apache 2, or equivalent open license, we will use and support that tool. If the tool doesn't exist, we build and open source it ourselves. Supabase is not a 1-to-1 mapping of Firebase. Our aim is to give developers a Firebase-like developer experience using open source tools.

**Architecture**

Supabase is a [hosted platform](https://supabase.com/dashboard). You can sign up and start using Supabase without installing anything. You can also [self-host](https://supabase.com/docs/guides/hosting/overview) and [develop locally](https://supabase.com/docs/guides/local-development).

[![Architecture](https://github.com/supabase/supabase/raw/master/apps/docs/public/img/supabase-architecture.svg)](https://github.com/supabase/supabase/blob/master/apps/docs/public/img/supabase-architecture.svg)

- [Postgres](https://www.postgresql.org/) is an object-relational database system with over 30 years of active development that has earned it a strong reputation for reliability, feature robustness, and performance.
- [Realtime](https://github.com/supabase/realtime) is an Elixir server that allows you to listen to PostgreSQL inserts, updates, and deletes using websockets. Realtime polls Postgres' built-in replication functionality for database changes, converts changes to JSON, then broadcasts the JSON over websockets to authorized clients.
- [PostgREST](http://postgrest.org/) is a web server that turns your PostgreSQL database directly into a RESTful API.
- [GoTrue](https://github.com/supabase/gotrue) is a JWT-based authentication API that simplifies user sign-ups, logins, and session management in your applications.
- [Storage](https://github.com/supabase/storage-api) a RESTful API for managing files in S3, with Postgres handling permissions.
- [pg\_graphql](http://github.com/supabase/pg_graphql/) a PostgreSQL extension that exposes a GraphQL API.
- [postgres-meta](https://github.com/supabase/postgres-meta) is a RESTful API for managing your Postgres, allowing you to fetch tables, add roles, and run queries, etc.
- [Kong](https://github.com/Kong/kong) is a cloud-native API gateway.

#### Client libraries

Our approach for client libraries is modular. Each sub-library is a standalone implementation for a single external system. This is one of the ways we support existing tools.

<table><tbody><tr><th>Language</th><th>Client</th><th colspan="5">Feature-Clients (bundled in Supabase client)</th></tr><tr><th></th><th>Supabase</th><th><a href="https://github.com/postgrest/postgrest">PostgREST</a></th><th><a href="https://github.com/supabase/gotrue">GoTrue</a></th><th><a href="https://github.com/supabase/realtime">Realtime</a></th><th><a href="https://github.com/supabase/storage-api">Storage</a></th><th>Functions</th></tr><tr><th colspan="7">⚡️ Official ⚡️</th></tr><tr><td>JavaScript (TypeScript)</td><td><a href="https://github.com/supabase/supabase-js">supabase-js</a></td><td><a href="https://github.com/supabase/supabase-js/tree/master/packages/core/postgrest-js">postgrest-js</a></td><td><a href="https://github.com/supabase/supabase-js/tree/master/packages/core/auth-js">auth-js</a></td><td><a href="https://github.com/supabase/supabase-js/tree/master/packages/core/realtime-js">realtime-js</a></td><td><a href="https://github.com/supabase/supabase-js/tree/master/packages/core/storage-js">storage-js</a></td><td><a href="https://github.com/supabase/supabase-js/tree/master/packages/core/functions-js">functions-js</a></td></tr><tr><td>Flutter</td><td><a href="https://github.com/supabase/supabase-flutter">supabase-flutter</a></td><td><a href="https://github.com/supabase/postgrest-dart">postgrest-dart</a></td><td><a href="https://github.com/supabase/gotrue-dart">gotrue-dart</a></td><td><a href="https://github.com/supabase/realtime-dart">realtime-dart</a></td><td><a href="https://github.com/supabase/storage-dart">storage-dart</a></td><td><a href="https://github.com/supabase/functions-dart">functions-dart</a></td></tr><tr><td>Swift</td><td><a href="https://github.com/supabase/supabase-swift">supabase-swift</a></td><td><a href="https://github.com/supabase/supabase-swift/tree/main/Sources/PostgREST">postgrest-swift</a></td><td><a href="https://github.com/supabase/supabase-swift/tree/main/Sources/Auth">auth-swift</a></td><td><a href="https://github.com/supabase/supabase-swift/tree/main/Sources/Realtime">realtime-swift</a></td><td><a href="https://github.com/supabase/supabase-swift/tree/main/Sources/Storage">storage-swift</a></td><td><a href="https://github.com/supabase/supabase-swift/tree/main/Sources/Functions">functions-swift</a></td></tr><tr><td>Python</td><td><a href="https://github.com/supabase/supabase-py">supabase-py</a></td><td><a href="https://github.com/supabase/postgrest-py">postgrest-py</a></td><td><a href="https://github.com/supabase/gotrue-py">gotrue-py</a></td><td><a href="https://github.com/supabase/realtime-py">realtime-py</a></td><td><a href="https://github.com/supabase/storage-py">storage-py</a></td><td><a href="https://github.com/supabase/functions-py">functions-py</a></td></tr><tr><th colspan="7">💚 Community 💚</th></tr><tr><td>C#</td><td><a href="https://github.com/supabase-community/supabase-csharp">supabase-csharp</a></td><td><a href="https://github.com/supabase-community/postgrest-csharp">postgrest-csharp</a></td><td><a href="https://github.com/supabase-community/gotrue-csharp">gotrue-csharp</a></td><td><a href="https://github.com/supabase-community/realtime-csharp">realtime-csharp</a></td><td><a href="https://github.com/supabase-community/storage-csharp">storage-csharp</a></td><td><a href="https://github.com/supabase-community/functions-csharp">functions-csharp</a></td></tr><tr><td>Go</td><td>-</td><td><a href="https://github.com/supabase-community/postgrest-go">postgrest-go</a></td><td><a href="https://github.com/supabase-community/gotrue-go">gotrue-go</a></td><td>-</td><td><a href="https://github.com/supabase-community/storage-go">storage-go</a></td><td><a href="https://github.com/supabase-community/functions-go">functions-go</a></td></tr><tr><td>Java</td><td>-</td><td>-</td><td><a href="https://github.com/supabase-community/gotrue-java">gotrue-java</a></td><td>-</td><td><a href="https://github.com/supabase-community/storage-java">storage-java</a></td><td>-</td></tr><tr><td>Kotlin</td><td><a href="https://github.com/supabase-community/supabase-kt">supabase-kt</a></td><td><a href="https://github.com/supabase-community/supabase-kt/tree/master/Postgrest">postgrest-kt</a></td><td><a href="https://github.com/supabase-community/supabase-kt/tree/master/Auth">auth-kt</a></td><td><a href="https://github.com/supabase-community/supabase-kt/tree/master/Realtime">realtime-kt</a></td><td><a href="https://github.com/supabase-community/supabase-kt/tree/master/Storage">storage-kt</a></td><td><a href="https://github.com/supabase-community/supabase-kt/tree/master/Functions">functions-kt</a></td></tr><tr><td>Ruby</td><td><a href="https://github.com/supabase-community/supabase-rb">supabase-rb</a></td><td><a href="https://github.com/supabase-community/postgrest-rb">postgrest-rb</a></td><td>-</td><td>-</td><td>-</td><td>-</td></tr><tr><td>Rust</td><td>-</td><td><a href="https://github.com/supabase-community/postgrest-rs">postgrest-rs</a></td><td>-</td><td>-</td><td>-</td><td>-</td></tr><tr><td>Godot Engine (GDScript)</td><td><a href="https://github.com/supabase-community/godot-engine.supabase">supabase-gdscript</a></td><td>-</td><td>-</td><td>-</td><td>-</td><td>-</td></tr></tbody></table>

## Badges

```
[![Made with Supabase](https://supabase.com/badge-made-with-supabase.svg)](https://supabase.com)
```
```
<a href="https://supabase.com">
  <img
    width="168"
    height="30"
    src="https://supabase.com/badge-made-with-supabase.svg"
    alt="Made with Supabase"
  />
</a>
```
```
[![Made with Supabase](https://supabase.com/badge-made-with-supabase-dark.svg)](https://supabase.com)
```
```
<a href="https://supabase.com">
  <img
    width="168"
    height="30"
    src="https://supabase.com/badge-made-with-supabase-dark.svg"
    alt="Made with Supabase"
  />
</a>
```

## Translations

- [Arabic | العربية](https://github.com/supabase/supabase/blob/master/i18n/README.ar.md)
- [Albanian / Shqip](https://github.com/supabase/supabase/blob/master/i18n/README.sq.md)
- [Bangla / বাংলা](https://github.com/supabase/supabase/blob/master/i18n/README.bn.md)
- [Bulgarian / Български](https://github.com/supabase/supabase/blob/master/i18n/README.bg.md)
- [Catalan / Català](https://github.com/supabase/supabase/blob/master/i18n/README.ca.md)
- [Croatian / Hrvatski](https://github.com/supabase/supabase/blob/master/i18n/README.hr.md)
- [Czech / čeština](https://github.com/supabase/supabase/blob/master/i18n/README.cs.md)
- [Danish / Dansk](https://github.com/supabase/supabase/blob/master/i18n/README.da.md)
- [Dutch / Nederlands](https://github.com/supabase/supabase/blob/master/i18n/README.nl.md)
- [English](https://github.com/supabase/supabase)
- [Estonian / eesti keel](https://github.com/supabase/supabase/blob/master/i18n/README.et.md)
- [Finnish / Suomalainen](https://github.com/supabase/supabase/blob/master/i18n/README.fi.md)
- [French / Français](https://github.com/supabase/supabase/blob/master/i18n/README.fr.md)
- [German / Deutsch](https://github.com/supabase/supabase/blob/master/i18n/README.de.md)
- [Greek / Ελληνικά](https://github.com/supabase/supabase/blob/master/i18n/README.el.md)
- [Gujarati / ગુજરાતી](https://github.com/supabase/supabase/blob/master/i18n/README.gu.md)
- [Hebrew / עברית](https://github.com/supabase/supabase/blob/master/i18n/README.he.md)
- [Hindi / हिंदी](https://github.com/supabase/supabase/blob/master/i18n/README.hi.md)
- [Hungarian / Magyar](https://github.com/supabase/supabase/blob/master/i18n/README.hu.md)
- [Nepali / नेपाली](https://github.com/supabase/supabase/blob/master/i18n/README.ne.md)
- [Indonesian / Bahasa Indonesia](https://github.com/supabase/supabase/blob/master/i18n/README.id.md)
- [Italiano / Italian](https://github.com/supabase/supabase/blob/master/i18n/README.it.md)
- [Japanese / 日本語](https://github.com/supabase/supabase/blob/master/i18n/README.jp.md)
- [Korean / 한국어](https://github.com/supabase/supabase/blob/master/i18n/README.ko.md)
- [Lithuanian / lietuvių](https://github.com/supabase/supabase/blob/master/i18n/README.lt.md)
- [Latvian / latviski](https://github.com/supabase/supabase/blob/master/i18n/README.lv.md)
- [Malay / Bahasa Malaysia](https://github.com/supabase/supabase/blob/master/i18n/README.ms.md)
- [Norwegian (Bokmål) / Norsk (Bokmål)](https://github.com/supabase/supabase/blob/master/i18n/README.nb.md)
- [Persian / فارسی](https://github.com/supabase/supabase/blob/master/i18n/README.fa.md)
- [Polish / Polski](https://github.com/supabase/supabase/blob/master/i18n/README.pl.md)
- [Portuguese / Português](https://github.com/supabase/supabase/blob/master/i18n/README.pt.md)
- [Portuguese (Brazilian) / Português Brasileiro](https://github.com/supabase/supabase/blob/master/i18n/README.pt-br.md)
- [Romanian / Română](https://github.com/supabase/supabase/blob/master/i18n/README.ro.md)
- [Russian / Pусский](https://github.com/supabase/supabase/blob/master/i18n/README.ru.md)
- [Serbian / Srpski](https://github.com/supabase/supabase/blob/master/i18n/README.sr.md)
- [Sinhala / සිංහල](https://github.com/supabase/supabase/blob/master/i18n/README.si.md)
- [Slovak / slovenský](https://github.com/supabase/supabase/blob/master/i18n/README.sk.md)
- [Slovenian / Slovenščina](https://github.com/supabase/supabase/blob/master/i18n/README.sl.md)
- [Spanish / Español](https://github.com/supabase/supabase/blob/master/i18n/README.es.md)
- [Simplified Chinese / 简体中文](https://github.com/supabase/supabase/blob/master/i18n/README.zh-cn.md)
- [Swedish / Svenska](https://github.com/supabase/supabase/blob/master/i18n/README.sv.md)
- [Thai / ไทย](https://github.com/supabase/supabase/blob/master/i18n/README.th.md)
- [Traditional Chinese / 繁體中文](https://github.com/supabase/supabase/blob/master/i18n/README.zh-tw.md)
- [Turkish / Türkçe](https://github.com/supabase/supabase/blob/master/i18n/README.tr.md)
- [Ukrainian / Українська](https://github.com/supabase/supabase/blob/master/i18n/README.uk.md)
- [Vietnamese / Tiếng Việt](https://github.com/supabase/supabase/blob/master/i18n/README.vi-vn.md)
- [List of translations](https://github.com/supabase/supabase/blob/master/i18n/languages.md)
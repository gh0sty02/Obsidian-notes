---
title: "lukevella/rallly: Rallly is an open-source scheduling and collaboration tool designed to make organizing events and meetings easier."
source: "https://github.com/lukevella/rallly"
author:
published:
created: 2026-06-16
description: "Rallly is an open-source scheduling and collaboration tool designed to make organizing events and meetings easier. - lukevella/rallly"
tags:
  - "clippings"
---
[![Rallly](https://github.com/lukevella/rallly/raw/main/assets/images/logo-color.svg)](https://github.com/lukevella/rallly/blob/main/assets/images/logo-color.svg)

[![Screenshot](https://private-user-images.githubusercontent.com/676849/446288112-baafea52-c4da-43bb-96ef-50840f1c0c03.jpeg?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3ODE1NTY5MzcsIm5iZiI6MTc4MTU1NjYzNywicGF0aCI6Ii82NzY4NDkvNDQ2Mjg4MTEyLWJhYWZlYTUyLWM0ZGEtNDNiYi05NmVmLTUwODQwZjFjMGMwMy5qcGVnP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI2MDYxNSUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNjA2MTVUMjA1MDM3WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9OTg5MjE4MmFmODhlZDA4Njc2YjlkYzBiMzY0NDAyYzk1NDA4NzYzNmEwN2NmYjc2MWNmMGNlYWYzZmQ5MmRkZiZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmcmVzcG9uc2UtY29udGVudC10eXBlPWltYWdlJTJGanBlZyJ9.RL30RDlc3D9LvJ1nu7SHoS1gW3ObPaDdZr2QUGjl-lQ)](https://private-user-images.githubusercontent.com/676849/446288112-baafea52-c4da-43bb-96ef-50840f1c0c03.jpeg?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3ODE1NTY5MzcsIm5iZiI6MTc4MTU1NjYzNywicGF0aCI6Ii82NzY4NDkvNDQ2Mjg4MTEyLWJhYWZlYTUyLWM0ZGEtNDNiYi05NmVmLTUwODQwZjFjMGMwMy5qcGVnP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI2MDYxNSUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNjA2MTVUMjA1MDM3WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9OTg5MjE4MmFmODhlZDA4Njc2YjlkYzBiMzY0NDAyYzk1NDA4NzYzNmEwN2NmYjc2MWNmMGNlYWYzZmQ5MmRkZiZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmcmVzcG9uc2UtY29udGVudC10eXBlPWltYWdlJTJGanBlZyJ9.RL30RDlc3D9LvJ1nu7SHoS1gW3ObPaDdZr2QUGjl-lQ)

Schedule group meetings with friends, colleagues and teams. Create meeting polls to find the best date and time to organize an event based on your participants' availability. Save time and avoid back-and-forth emails.

Built with [Next.js](https://github.com/vercel/next.js/), [Prisma](https://github.com/prisma/prisma), [tRPC](https://github.com/trpc/trpc) & [TailwindCSS](https://github.com/tailwindlabs/tailwindcss)

## Self-hosting

Check out the [self-hosting docs](https://support.rallly.co/self-hosting) for more information on running your own instance of Rallly.

## Local Installation

The following instructions are for running the project locally for development.

1. Clone the repository and switch to the project directory
	```
	git clone https://github.com/lukevella/rallly.git
	cd rallly
	```
2. Install dependencies
	```
	pnpm install
	```
3. Setup environment variables
	Copy the sample environment file and fill in the required values:
	```
	cp apps/web/.env.sample apps/web/.env
	cp packages/database/.env.sample packages/database/.env
	```
	See [configuration options](https://support.rallly.co/self-hosting/configuration-options) for a full list of available options.
4. Generate Prisma client
	```
	pnpm db:generate
	```
5. Setup database
	You will need to have [Docker](https://docs.docker.com/get-docker/) installed and running to run the database using the provided docker-compose file.
	To start the database, run:
	```
	pnpm docker:up
	```
	Next run the following command to setup the database:
	```
	pnpm db:reset && pnpm db:seed
	```
	This will:
	- delete the existing database (if it exists)
		- run migrations to create a new database schema
		- seed the database with test users and random data
6. Start the portless proxy
	The dev scripts route the apps through [portless](https://portless.sh/), which exposes them at stable HTTPS URLs (e.g. `https://web.local.rallly.co`) instead of `localhost:<port>`.
	Start the proxy:
	```
	pnpm proxy:start
	```
7. Start the Next.js server
	```
	pnpm dev
	```

## Contributors

Please read our [contributing guide](https://github.com/lukevella/rallly/blob/main/CONTRIBUTING.md) to learn about how to contribute to this project.

### Translators 🌐

You can help translate Rallly to another language by following our [guide for translators](https://support.rallly.co/contribute/translations).

## License

Rallly is open-source under the GNU Affero General Public License Version 3 (AGPLv3) or any later version. See [LICENSE](https://github.com/lukevella/rallly/blob/main/LICENSE) for more detail.

## Sponsors

Thank you to our sponsors for making this project possible.

[Become a sponsor →](https://github.com/sponsors/lukevella)

And thank you to these companies for sponsoring and showing support for this project.
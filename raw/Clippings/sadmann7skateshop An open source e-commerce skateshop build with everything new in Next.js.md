---
title: "sadmann7/skateshop: An open source e-commerce skateshop build with everything new in Next.js."
source: "https://github.com/sadmann7/skateshop"
author:
published:
created: 2026-06-16
description: "An open source e-commerce skateshop build with everything new in Next.js. - sadmann7/skateshop"
tags:
  - "clippings"
---
## Skateshop

This is an open source e-commerce skateshop build with everything new in Next.js 14. It is bootstrapped with `create-t3-app`.

[![Skateshop](https://github.com/sadmann7/skateshop/raw/main/public/images/screenshot.png)](https://skateshop.sadmn.com/)

> **Warning** This project is still in development and is not ready for production use.
> 
> It uses new technologies (drizzle ORM) which are subject to change and may break your application.

## Tech Stack

- **Framework:** [Next.js](https://nextjs.org/)
- **Styling:** [Tailwind CSS](https://tailwindcss.com/)
- **User Management:** [Clerk](https://clerk.com/)
- **ORM:** [Drizzle ORM](https://orm.drizzle.team/)
- **UI Components:** [shadcn/ui](https://ui.shadcn.com/)
- **Email:** [React Email](https://react.email/)
- **Content Management:** [Contentlayer](https://www.contentlayer.dev/)
- **File Uploads:** [uploadthing](https://uploadthing.com/)
- **Payments infrastructure:** [Stripe](https://stripe.com/)

## Features to be implemented

- Authentication with **Clerk**
- File uploads with **uploadthing**
- Newsletter subscription with **React Email** and **Resend**
- Blog using **MDX** and **Contentlayer**
- ORM using **Drizzle ORM**
- Database on **PlanetScale**
- Validation with **Zod**
- Storefront with products, categories, and subcategories
- Seller and customer workflows
- User subscriptions with **Stripe**
- Checkout with **Stripe Checkout**
- Admin dashboard with stores, products, orders, subscriptions, and payments

## Running Locally

1. Clone the repository
	```
	git clone https://github.com/sadmann7/skateshop.git
	```
2. Install dependencies using pnpm
	```
	pnpm install
	```
3. Copy the `.env.example` to `.env` and update the variables.
	```
	cp .env.example .env
	```
4. Start the development server
	```
	pnpm run dev
	```
5. Push the database schema
	```
	pnpm run db:push
	```
6. Start the Stripe webhook listener
	```
	pnpm run stripe:listen
	```

## How do I deploy this?

Follow the deployment guides for [Vercel](https://create.t3.gg/en/deployment/vercel), [Netlify](https://create.t3.gg/en/deployment/netlify) and [Docker](https://create.t3.gg/en/deployment/docker) for more information.

## Contributing

Contributions are welcome! Please open an issue if you have any questions or suggestions. Your contributions will be acknowledged. See the [contributing guide](https://github.com/sadmann7/skateshop/blob/main/CONTRIBUTING.md) for more information.

## Contributors

Thanks goes to these wonderful people for their contributions:

[![](https://camo.githubusercontent.com/da9657e38516b31ff8ba32c6a2828bcbcd4a4ce46498fb24fe1ea6f530f5d08e/68747470733a2f2f636f6e747269622e726f636b732f696d6167653f7265706f3d7361646d616e6e372f736b61746573686f70)](https://github.com/sadmann7/skateshop/graphs/contributors)

Made with [contrib.rocks](https://contrib.rocks/)

## License

Licensed under the MIT License. Check the [LICENSE](https://github.com/sadmann7/skateshop/blob/main/LICENSE.md) file for details.
---
title: "turborepo/examples/with-tailwind/apps/web/package.json at main"
source: "https://github.com/vercel/turborepo/blob/main/examples/with-tailwind/apps/web/package.json"
author:
published:
created: 2026-06-16
description: "Build system optimized for JavaScript and TypeScript, written in Rust - turborepo/examples/with-tailwind/apps/web/package.json at main · vercel/turborepo"
tags:
  - "clippings"
---
1

2

3

4

5

6

7

8

9

10

11

12

13

14

15

16

17

18

19

20

21

22

23

24

25

26

27

28

29

30

31

32

33

34

35

{

"name": "web",

"version": "1.0.0",

"type": "module",

"private": true,

"scripts": {

"dev": "next dev --port 3001",

"build": "next build",

"start": "next start",

"lint": "eslint --max-warnings 0",

"check-types": "next typegen && tsc --noEmit"

},

"dependencies": {

"@repo/ui": "workspace:\*",

"geist": "^1.7.1",

"next": "16.2.0",

"react": "^19.2.0",

"react-dom": "^19.1.0"

},

"devDependencies": {

"@next/eslint-plugin-next": "^16.2.0",

"@repo/eslint-config": "workspace:\*",

"@repo/tailwind-config": "workspace:\*",

"@repo/typescript-config": "workspace:\*",

"@tailwindcss/postcss": "^4.1.5",

"@types/node": "^22.15.30",

"@types/react": "^19.1.0",

"@types/react-dom": "^19.1.1",

"autoprefixer": "^10.4.20",

"eslint": "^9.39.1",

"postcss": "^8.5.3",

"tailwindcss": "^4.1.5",

"typescript": "5.9.2"

}

}
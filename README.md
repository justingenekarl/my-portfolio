# Justin Gene Karl — Photography Portfolio

A minimalist, text-first photography portfolio built with SvelteKit and Tailwind CSS. High-contrast, black-on-white (with an auto dark mode), editorial layout inspired by danglasser.com.

## Pages

- **Home** — full-bleed hero image
- **Work** — gallery grid of street photography
- **About** — bio
- **Contact** — email and Instagram links

## Developing

Install dependencies, then start the dev server:

```sh
npm install
npm run dev

# or start the server and open the app in a new browser tab
npm run dev -- --open
```

## Building

To create a production version of the site:

```sh
npm run build
```

You can preview the production build with `npm run preview`.

> To deploy, you may need to install an [adapter](https://svelte.dev/docs/kit/adapters) for your target environment.

## Adding photos

Drop image files into `static/images/` — they're served at `/images/<filename>`. Filenames referenced in the code:

- `hero.jpg` (homepage)
- `tokyo-alley.jpg`, `tokyo-crossing.jpg`, `amsterdam-canal.jpg`, `amsterdam-morning.jpg`, `coffee-window-seat.jpg`, `coffee-counter.jpg` (Work gallery)

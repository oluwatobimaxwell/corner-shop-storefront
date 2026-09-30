# Corner shop storefront

A small shop sells online. The shelf shows what is genuinely there, a
basket survives the tab it was made in, the shop never takes money for
a tin it does not have, and the buyer can follow their order until
they pick it up.

## What finished looks like

A stranger can open the URL, browse a shelf the shopkeeper filled from
their phone, make an account, fill a basket that is still there
tomorrow and follows them in when they sign in, buy the last tin on
the shelf and be the only one who did, pay for it without somebody
taking it out of their hands while they type, and follow the order
until they collect it. They hear when it is ready, and stock nobody
came back for goes back on the shelf, with nobody at the shop pressing
anything.

Finished does not mean running on a laptop. It means somebody who has
never met the person who built it can buy something on the internet.

## The road map

Eight sprints. Each one makes a different part of that sentence true,
and the order is not arbitrary: take any sprint out and the sentence
stops being true.

| # | Sprint | What it makes possible |
|---|---|---|
| 1 | Behind the counter | A shopper has an account; the till is not something anybody can issue themselves |
| 2 | Stocking the shelves | The shopkeeper puts stock online from their phone, and the shelf says what is left |
| 3 | The basket | A basket outlives the tab, follows a shopper into their account, and prices itself today |
| 4 | The last one on the shelf | Money is taken, the count goes down, and two buyers of the last tin produce one order |
| 5 | Held, not sold | Checkout holds the stock while somebody pays, and gives it back when they walk away |
| 6 | Where the order is | An order moves forward only, the buyer watches it, and cancelling returns the stock |
| 7 | When the shop is shut | Lapsed holds and the ready-to-collect message happen with nobody there |
| 8 | Open for business | It is on the internet, at its own address, with mail that leaves |

## The hard part

Stock is a count, not a slot. A booking system hands one appointment
to one person and a unique constraint settles it. A shop has six tins
and four baskets with claims on them, one shopper halfway through
paying, and a shopkeeper doing a stock take at the same moment.
Sprints four and five are where that is solved, and they are the
reason this product is worth building rather than reading about.

## Working on it

The sprints, their briefs and the tickets under them are in
Blacksmith. Start with sprint one; each sprint opens as the one before
it closes.

The reading attached to a sprint is worth opening before its first
ticket rather than after. It carries the parts a ticket deliberately
does not: what the alternatives were, and which one you are choosing
between.

## What this is built with

The whole product: an API and the screens that use it.

- **Django** with **Django REST Framework** for the API.
- **SimpleJWT** for authentication, against a custom user model in `apps/users`.
- **drf-spectacular** for the OpenAPI schema, served at `/api/schema/` and browsable at `/api/docs/`.
- **React** with **Vite** for the dev server and the build.
- **Chakra UI** for components, and **React Router** for routes.
- **TanStack Query** for every call to the API, so caching and refetching are decided in one place.

## Setting it up

You need the CLI once: `npm install -g blacksmith-cli`.

```bash
blacksmith setup     # dependencies, database, migrations
blacksmith dev       # start it
```

The API answers on `http://localhost:8000`, and the app on `http://localhost:5173`.

Copy `backend/.env.example` to `backend/.env` before the first run. It is ignored by git and holds the secret key, the database URL and anything else this project should not carry in its history.

## Where the code lives

```
backend/
├── config/
│   ├── settings/        # base, development, production
│   └── urls.py          # where routes are mounted
├── apps/
│   └── users/           # the custom user model, and auth
├── utils/               # shared helpers, base model
├── manage.py
└── requirements.txt

frontend/
└── src/
    ├── api/
    │   ├── generated/   # written by `blacksmith sync` — do not edit
    │   └── hooks/       # your queries and mutations
    ├── pages/           # one folder per page
    ├── features/        # auth, and anything else that spans pages
    ├── router/          # routes and layouts
    ├── shared/          # components and hooks used across pages
    └── styles/
```

## Day to day

| Command | What it does |
| --- | --- |
| `blacksmith dev` | Run it locally. |
| `blacksmith sync` | Regenerate the frontend API types and hooks from the backend schema. Run it after changing a serializer or a route. |
| `blacksmith make:resource Post` | Scaffold a model, serializer, viewset and routes, plus the hooks and pages that use them. |
| `blacksmith backend <command>` | Run a Django management command, e.g. `blacksmith backend createsuperuser`. |
| `blacksmith frontend <command>` | Run an npm command in the frontend, e.g. `blacksmith frontend install axios`. |
| `blacksmith eject` | Remove Blacksmith and keep a plain Django and React project. Nothing here is a dependency on us. |

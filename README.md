# Commerce Layer Demo Store

This Demo Store is a completely static ecommerce solution (with SSR capability) powered by Commerce Layer. The store is [full-featured](#features) and fully operational, with no third-party services required. You can easily tailor your own with different levels of customization. Keep reading to learn more.

> The Demo Store project consists of [two repositories](#how-it-works), this one is the GitHub template.

## What is Commerce Layer?

[Commerce Layer](https://commercelayer.io/) is a multi-market commerce API and order management system that lets you add global shopping capabilities to any website, mobile app, chatbot, wearable, voice, or IoT device, with ease. Compose your stack with the best-of-breed tools you already mastered and love. Make any experience shoppable, anywhere, through a blazing-fast, enterprise-grade, and secure API.

## Table of contents

- [Features](#features)
- [How it works](#how-it-works)
- [Getting started](#getting-started)
- [Customization](#customization)
- [Deploy](#deploy)
- [Need help?](#need-help)
- [Troubleshooting](#troubleshooting)
- [License](#license)

## Features

We decided to build the Demo Store removing all third-party services that are usually used to create a comprehensive ecommerce website experience (CMS, search, PIM, etc.).

Everything related to [content](https://commercelayer.io/docs/core-concepts/content-vs-commerce) is stored as JSON files. To build your own Demo Store you will need to create these files manually or via scripts.

The Demo Store comes with:

- [x] a built-in search engine with facet search powered by [fuse.js](https://github.com/krisk/Fuse)
- [x] a full product catalog management with taxonomies and taxons
- [x] single product variants management
- [x] multi-language capabilities to make selling internationally easier
- [x] the whole extensive set of features provided out of the box by Commerce Layer APIs (multiple currency price lists, inventory models that support multiple stock locations and fulfillment strategies, market-specific payment gateways, delivery options and carrier accounts, etc.)

The integration with Commerce Layer is achieved by leveraging some of our open-source [developer tools](https://commercelayer.io/developers), specifically:

- the [React components](https://github.com/commercelayer/commercelayer-react-components)
- the embedded version of our [Hosted cart](https://github.com/commercelayer/commercelayer-cart#embedding-the-cart)
- the [Hosted checkout](https://github.com/commercelayer/commercelayer-react-checkout)

## How it works

The Demo Store project consists of two repositories that you can leverage to build your own store, based on the amount of customization you need to add:

- [`demo-store`](https://github.com/commercelayer/demo-store)

  This is a **GitHub template** that uses the below-mentioned `demo-store-core` as a [git submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules). If you're happy with the features and the look and feel of the Commerce Layer Demo Store we suggest you follow this path. You won't have to care about the whole source code and you'll be free to focus just on [your data and content](#customization). On top of that, you'll get free updates with [almost no risk](#managing-updates) just by running:

  ```sh
  git submodule update --remote
  npm install
  ```

- [`demo-store-core`](https://github.com/commercelayer/demo-store-core)

  This repository contains the source code. If you need to fully customize your store (behavior, UI, UX, etc.) you just have to fork this repo and create your own. **This is also the way to contribute.**

  > :warning: Please note that if you follow this path and start diverging too much from the original source code the risk is to lose all future updates or not be able to replicate them.

## Getting started

If you have no experience with Commerce Layer, you can start by creating an account (it's free!) and following the [onboarding tutorial](https://docs.commercelayer.io/developers/welcome/onboarding-tutorial).

> :information_source: Please note that to set up a Demo Store you need a properly configured organization with at least a few products and one market.

If you prefer to start from scratch, you can create a new organization and use the following commands to configure a Commerce Layer's Demo Store-like project.

### Setting up your organization

Once the organization is created, you need to create two [API clients](https://docs.commercelayer.io/developers/api-clients): a **sales channel** and an **integration**.

If you haven't yet, install the [Commerce Layer CLI](https://github.com/commercelayer/commercelayer-cli) and two of its plugins: the [seeder plugin](https://github.com/commercelayer/commercelayer-cli-plugin-seeder) and the [imports plugin](https://github.com/commercelayer/commercelayer-cli-plugin-imports):

```sh
npm install -g @commercelayer/cli

commercelayer plugins:install seeder
commercelayer plugins:install imports
```

Now you can log in to your integration API client from the CLI:

```sh
commercelayer applications:login \
  --clientId Oy5F2TbPYhOZsxy1tQd9ZVZ... \
  --clientSecret 1ZHNJUgn_1lh1mel06gGDqa... \
  --organization my-awesome-organization \
  --alias cli-admin
```

### Building your store

Once your organization is set up, to build you store you need to follow some simple steps... let's get started!

#### 1. Create a new repository

Whichever [path](#how-it-works) you choose, first of all, you need to create a new repository for your store:

- If you decided to keep the `demo-store` template you just need to click on the _"Use this template"_ from the [repository homepage](https://github.com/commercelayer/demo-store) on GitHub and then run:

  ```sh
  git clone <your-repository-url> my-new-project
  cd my-new-project
  git submodule update --init
  npm install

  cp ./demo-store-core/packages/website/.env.sample.submodule .env.local

  cp -r ./demo-store-core/packages/website/data/json ./data/json
  ```

- If you decided to fork the `demo-store-core` [repository](https://github.com/commercelayer/demo-store-core) you can run this instead:

  ```sh
  git clone <your-repository-url> my-new-project
  cd my-new-project
  npm install

  cp ./packages/website/.env.sample ./packages/website/.env.local
  ```

#### 2. Set the environment variables

Edit `.env.local` and fill in all the missing information:

```properties
# this is the 'sales channel' client id
NEXT_PUBLIC_CL_CLIENT_ID=er34TWFcd24RFI8KJ52Ws6q...

# this is the 'base endpoint'
NEXT_PUBLIC_CL_ENDPOINT=https://my-awesome-organization.commercelayer.io
```

#### 3. Seed the data

> :information_source: This step is optional. If you already have a properly configured organization on your Commerce Layer account, you can skip it.

The following script will populate your organization with all the resources you need to build a multi-market ecommerce with Commerce Layer (the ones we are using for our [Demo Store](https://commercelayer.github.io/demo-store-core)).

```sh
npm run seeder:seed -ws --if-present
```

If the command above executes with no errors, please [skip to step 4](#4-choose-the-countries-where-youre-going-to-sell). Otherwise, you can still seed your organization with sample data using the CLI by running the following:

```sh
commercelayer seeder:seed -b custom -n demo_store_full -u ./demo-store-core/packages/setup/
```

#### 4. Choose the countries where you're going to sell

The `json/countries.json` file contains a list of available countries for your ecommerce. You can change it with your preferred editor. Just make sure to replace all findings of `"market": xxx` with the related markets of your organization. You can get the list of your markets from the Commerce Layer admin dashboard or by running this command:

```sh
npm run markets -ws --if-present
```

#### 5. See it in action :rocket:

```sh
npm run dev

# http://localhost:3000/
```


## Managing updates

You can always find the latest code of the `demo-store-core` in its `main` branch.

To update your Demo Store to the latest changes you can simply run:

```sh
git submodule update --remote
npm install
```

Sometimes can happen that a new major version contains breaking changes. In this case, by updating to the latest, your Demo Store could stop working and you'll need to manually solve all the issues by following the [release notes](https://github.com/commercelayer/demo-store-core/releases).

If you like, you can receive a [GitHub notification](https://docs.github.com/en/account-and-profile/managing-subscriptions-and-notifications-on-github/setting-up-notifications/configuring-notifications#configuring-your-watch-settings-for-an-individual-repository) as soon a new version is released.


## Customization

When you're using our Demo Store template you can customize three main elements: content data, locales, and configuration files.

> :warning:  We're going to continuously improve our Demo Store to add new features and optimize the existing ones. When you update to the latest release, the build could fail. Take a look at the release notes to understand how to fix it by updating your customized files.

### JSON data files

As mentioned earlier, our Demo Store is built around a set of data that are stored as JSON files, to decouple it from any third-party services. To build your store you'll have to create and manage these files.

JSON files are located at `data/json/`, but you can choose a different position by changing the environment variable `NEXT_PUBLIC_JSON_DATA_FOLDER`.

Type-definition files are located at `packages/types/src/json/`. We are using [zod](https://github.com/colinhacks/zod) for schema validation. Take a look at these files to understand which structure you have to follow.

When you are done with all the changes you can check if everything is correct by running:

```
npm run test:data
```

### Locale data files

Our Demo Store is a multi-language website. When you built your data in the previous step, you probably noticed that some fields were localized. You can add new languages or change existing translations.

Locale JSON files are located at `data/locales/`, but you can choose a different position by changing the environment variable `NEXT_PUBLIC_LOCALES_DATA_FOLDER`.

Do as follows to start customizing the locales:

```sh
cp -r ./demo-store-core/packages/website/data/locales ./data/locales
```

```properties
# .env.local
NEXT_PUBLIC_LOCALES_DATA_FOLDER=../../../data/locales/
```

### Configuration files

Configuration files are located at `config/`, but you can choose a different position by changing the environment variable `NEXT_PUBLIC_CONFIG_FOLDER`.

There are three configuration files that you can manage:

- `general.config.js`  
  This file contains the general configuration.

- `facets.config.js`  
  This is the facets configuration file. You can choose the order in which they are displayed in the filter panel, their appearance, and the sorting rules of their values.  
  <img width="400" alt="demo-store-facets" src="https://user-images.githubusercontent.com/1681269/184152000-2163e484-d4bd-441a-b3cb-20c3b03b875a.png">

- `variants.config.js`  
  This is the variants configuration file. You can choose the order in which they are displayed on the product detail page and their appearance.  
  <img width="180" alt="demo-store-product-variants" src="https://user-images.githubusercontent.com/1681269/184152670-bdd5ea2b-d30f-42e8-b5a7-6c541396cd90.png">

Do as follows to start customizing the configuration:

```sh
cp -r ./demo-store-core/packages/website/config ./config
```

```properties
# .env.local
NEXT_PUBLIC_CONFIG_FOLDER=../../../config/
```

### Additional environment variables

There are some environment variables that you can use to customize your store. For an exhaustive list and description, you can take a look at [additional-env.d.ts](https://github.com/commercelayer/demo-store-core/blob/main/packages/website/additional-env.d.ts) file.

## Deploy

You can deploy the Demo Store wherever you prefer. You just need to:

1. Set all the environment variables in the system that you'll use to run the build, according to your needs.
2. Decide if you want to go with [static site generation](#static-site-generation) or [server-side rendering](#server-side-rendering).

> :information_source: The Demo Store is designed to be SSG first and foremost, but you can switch to SSR in a snap. We tested some ways to deploy it (e.g. using GitHub Workflow, Netlify, Vercel, etc.) and you can find more information about it [here](DEPLOY.md). If you did it differently or used other services and you want to share the steps with the community, please [join the discussion](https://github.com/commercelayer/demo-store-core/discussions/new?category=show-and-tell), and thanks in advance!

### Static site generation

To build and deploy the Demo Store:

1. Set the following environment variable accordingly:

   ```properties
   NEXT_PUBLIC_DATA_FETCHING=ssg
   ```

2. Run `npm run export` to create a statically optimized production build of your application.
3. Copy the folder `demo-store-core/packages/website/out` to your preferred static hosting.

### Server-side rendering

The Demo Store can be deployed to any hosting provider that supports Node.js. To do that:

1. Set the following environment variable accordingly:

   ```properties
   NEXT_PUBLIC_DATA_FETCHING=ssr
   ```

2. Run `npm run build` to create an optimized production build of your application.
3. Run `npm start` to start the Node.js server in production mode.

## Need help?

- Join [Commerce Layer's Slack community](https://slack.commercelayer.app).
- Open a new [Q&A discussion](https://github.com/commercelayer/demo-store-core/discussions/categories/q-a)
- Ping us [on Twitter](https://twitter.com/commercelayer).
- Is there a bug? Create an [issue](https://github.com/commercelayer/demo-store-core/issues) on this repository.

## Troubleshooting

1. **Q.** Even if I changed `NEXT_PUBLIC_JSON_DATA_FOLDER`, `NEXT_PUBLIC_LOCALE_DATA_FOLDER` or `NEXT_PUBLIC_CONFIG_FOLDER`, the website is still referring to previous files.

   **A.** These environment variables are used as `alias` in Webpack. Starting from Webpack 5, caching for faster builds has been introduced. Changing these environment variables will not invalidate the Webpack cache. You have to remove `.next` folder manually or by running:

   ```sh
   # update the path if needed
   rm -rf demo-store-core/packages/website/.next/
   ```

## License

This repository is published under the [MIT](LICENSE) license.

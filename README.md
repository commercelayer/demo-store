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

  This is a **GitHub template** that uses the below-mentioned `demo-store-core` as a [git submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules). If you're happy with the features and the look and feel of the Commerce Layer Demo Store we suggest you follow this path. You won't have to care about the whole source code and you'll be free to focus just on [your data and content](#customization). On top of that, you'll get free updates with almost no risk just by running:

  ```sh
  git submodule update --remote
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

- If you decided to keep the `demo-store` template you just need to click on the "Use this template" from the [repository homepage](https://github.com/commercelayer/demo-store) on GitHub and then run:

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

  cp ./packages/website/.env.sample .env.local
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

The following script will populate your organization with all the resources you need to build a multi-market ecommerce with Commerce Layer (the ones we are using for our [Demo Store](https://commercelayer.github.io/demo-store-core)).

```sh
npm run seeder:seed -ws --if-present
```

> :information_source: This step is optional. If you already have a properly configured organization on your Commerce Layer account, you can skip it.

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

Configuration files are located at `config/`, but you can choose a different position by changing the environment variable `NEXT_PUBLIC_LOCALES_DATA_FOLDER`.

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

There are some environment variables that you can use to customize your store. For an exhaustive list and description, you can take a look at [additional-env.d.ts](https://github.com/commercelayer/demo-store-core/blob/master/packages/website/additional-env.d.ts) file.

## Deploy

### GitHub Pages

Configuring GitHub Pages is straightforward. We already created a workflow that can be triggered manually.

You just have to review [`gh-pages.yml`](.github/workflows/gh-pages.yml) by updating the environment variables as you need and create two [secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets#creating-encrypted-secrets-for-a-repository) in your repository.

<img width="640" alt="Repository secrets" src="https://user-images.githubusercontent.com/1681269/185638408-f5c68563-1197-436c-a2b0-9aaf0dfbc16a.png">

Now you should be able to run the workflow. This will build your Demo Store statically and deploy the artifact to a `gh-pages` branch.

![Run workflow](https://user-images.githubusercontent.com/1681269/185639837-5b81186b-f5e7-43cd-bf7a-1c00f3b71b58.png)

Lastly you have to [configure a publishing source](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-from-a-branch) for your GitHub Pages site.

Feel free to update the workflow according to your needs. You can trigger the deploy with every git-push or change the `deploy` step, e.g. by publishing the artifact somewhere else (AWS S3, Netlify, etc.).

> :warning:
> _GitHub Pages is not intended for or allowed to be used as a free web-hosting service to run your online business, e-commerce site, or any other website that is primarily directed at either facilitating commercial transactions [...]_ ( [prohibited-uses](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages#prohibited-uses) )


### Netlify

The Demo Store is SSG first and foremost, but you can switch to SSR very quickly.

Inside the project root you can find two files [netlify.ssg.toml](netlify.ssg.toml) and [netlify.ssr.toml](netlify.ssr.toml).
Rename one of these into `netlify.toml` and update the `[build.environment]` section with the environment variables as you need.

From you Netlify Dashboard, click on `Add new site -> Import an existing project` and then select the repository.

At this point you should have some field already filled-in. Click on `Show advanced` and add the missing environment variables `SITE_URL`, `NEXT_PUBLIC_CL_CLIENT_ID` and `NEXT_PUBLIC_CL_ENDPOINT`.

> :information_source:
> Regarding the `SITE_URL` you cannot know yet which will be the url since Netlify will create a random site name after the first deploy.

<img width="500" alt="image" src="https://user-images.githubusercontent.com/1681269/185694384-235c2ead-49cf-46ea-a19c-8266ce74e366.png">

Click on `Deploy site`.

We suggest to stop the first auto-deploy by clicking on `Cancel deploy` because you can update the `SITE_URL` with the correct information first.

You can also speed up the deploy by [disabling `form detection`](https://docs.netlify.com/site-deploys/post-processing/form-detection/). We don't have form managed by Netlify, so you can securely disable it.

Now you can trigger a new deploy and .. voilà, site will be up and running :rocket:


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

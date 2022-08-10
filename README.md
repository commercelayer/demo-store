# Demo Store by Commerce Layer

Demo Store is a fully static e-commerce solution (with SSR capability) that uses Commerce Layer. Keep reading to tailor your own.

## What is Commerce Layer?

[Commerce Layer](https://commercelayer.io/) is a multi-market commerce API and order management system that lets you add global shopping capabilities to any website, mobile app, chatbot, wearable, voice, or IoT device, with ease. Compose your stack with the best-of-breed tools you already mastered and love. Make any experience shoppable, anywhere, through a blazing-fast, enterprise-grade, and secure API.

## :battery: Batteries included

We decided to build the Demo Store, removing all 3rd party services that are usually used to create a full experience of an e-commerce website (cms, search, pim, etc.).

Everything related to `content` is stored as JSON files and, building you own Demo Store, you will need to create these files, manually or via scripts.

Demo Store comes with:

- [x] built-in search engine with facet search powered by [fuse.js](https://github.com/krisk/Fuse)
- [x] product catalog management with taxonomies and taxons
- [x] product variants
- [x] integration with Commerce Layer (of course) using:
  - [x] [react-component](https://github.com/commercelayer/commercelayer-react-components)
  - [x] [hosted cart](https://github.com/commercelayer/commercelayer-cart)
  - [x] [hosted checkout](https://github.com/commercelayer/commercelayer-react-checkout)

## Getting started

If you haven't had experience before with Commerce Layer, go [here](https://docs.commercelayer.io/developers/) and start the tutorial. Configuring a Demo Store expects that you already have a configured Organization with at least few products and one market.

If you prefer to start from scratch you can create a new Organization and use the following commands to configure a `Commerce Layer's Demo Store` like project.

### Organization

Once the Organization is created, you need to create two [API clients](https://docs.commercelayer.io/developers/api-clients): one `Sales channel` and one `Integration`.

If you haven't done yet, install our [Commerce Layer CLI](https://www.npmjs.com/package/@commercelayer/cli), the [seeder plugin](https://www.npmjs.com/package/@commercelayer/cli-plugin-seeder) and the [imports plugin](https://www.npmjs.com/package/@commercelayer/cli-plugin-imports):

```sh
npm install -g @commercelayer/cli

commercelayer plugins:install seeder
commercelayer plugins:install imports
```

Now you can login to your `Integration` API client from the CLI:

```sh
commercelayer applications:login \
  --clientId Oy5F2TbPYhOZsxy1tQd9ZVZ... \
  --clientSecret 1ZHNJUgn_1lh1mel06gGDqa... \
  --organization my-awesome-organization \
  --alias cli-admin
```

### Demo Store

We have two repositories:

* **[`demo-store-core`](https://github.com/commercelayer/demo-store-core)** contains the source code.

  You just have to fork this repository and create your own starting from here. In this way you can fully customize all the aspects (behaviour, ui, ux), but *you will possibly loose all future updates if you start diverging to much.*

* **[`demo-store`](https://github.com/commercelayer/demo-store) GitHub template**.

    This template is using the `demo-store-core` as a [git submodule](https://git-scm.com/book/en/v2/Git-Tools-Submodules).  
    Using this way you don't have to care about the whole source code, *you can focus on you data*. You'll have free updates with almost no risk just by running:

    ```sh
    git submodule update --remote
    ```

Let's get started!

First of all you'll need to create a new repository starting from the `demo-store` template. Click on `Use this template` from the repository homepage on GitHub and the run:

```sh
git clone <your-repository> my-new-project
cd my-new-project
git submodule update --init
npm install

cp ./demo-store-core/packages/website/.env.sample.submodule .env.local

cp -r ./demo-store-core/packages/website/data/json ./data/json
```

### Environment Variables

Edit `.env.local` and fill all the missing information:

```properties
# this is the 'sales channel' client id
NEXT_PUBLIC_CL_CLIENT_ID=er34TWFcd24RFI8KJ52Ws6q...

# this is the 'base endpoint'
NEXT_PUBLIC_CL_ENDPOINT=https://my-awesome-organization.commercelayer.io
```

### Seed

The following script will populate your organizzation with all resources for a multi-market e-commerce. These are the same we are using for our [Demo Store](https://commercelayer.github.io/demo-store-core).

This step is *optional*. If you already have a well configured organization you can skip it.

```sh
npm run seeder:seed -ws --if-present
```

### countries.json

Edit `json/countries.json` with your preferred editor.

Here you have a list of available countries for your e-commerce.

You have to replace all instances of `"market": xxx` with the related markets of your organization.  
To get the list of your markets you can connect to the Commerce Layer Dashboard or by running this command:

```sh
npm run markets -ws --if-present
```

### Enjoy :rocket:

```sh
npm run dev

# http://localhost:3000/
```

## Customization

You can customize three aspects of the Demo Store: content data, locales and configuration files.

> :warning:  _Demo Store is continuously improving to provide new capabilities.  
> When you update to the latest release, it can happen that the build is no more working. Take a look at the release notes to understand how to fix it by updating your customized files._

### JSON data files

As mentioned earlier, the Demo Store is built around a set of data that are stored as json files. This decision has been taken to decouple the Demo Store from any 3rd party services.

To build your own Demo Store you'll have to create and manage these json data files.

Json files are located at `data/json/`, but you can choose a different position by changing the environment variable `NEXT_PUBLIC_JSON_DATA_FOLDER`.

We also have type-definition files located at `packages/types/src/json/`. We are uring [zod](https://github.com/colinhacks/zod) for the schema validation. Take a look at these files to uderstand which is the structure that you have to strictly follow.

Last but not least, we have a script `npm-prebuild.js` that auto-generates the `organization.json` file by reading all the information directly from your Organization, so you don't need to manually write that json file.

When you have done with all the changes you can run `npm run test:data` to check if everything is correct.


### Locale data files

Demo Store is a multi-language website. When you built your data in the previous step, probably you noticed that some fields were localized.

You can add new languages or change existing translations.

Locale json files are located at `data/locales/`, but you can choose a different position by changing the environment variable `NEXT_PUBLIC_LOCALES_DATA_FOLDER`.

Do the following to start customizing the locales:

```sh
cp -r ./demo-store-core/packages/website/data/locales ./data/locales
```

```properties
# .env.local
NEXT_PUBLIC_LOCALES_DATA_FOLDER=../../../data/locales/
```


### Config

Configuration files are located at `config/`, but you can choose a different position by changing the environment variable `NEXT_PUBLIC_LOCALES_DATA_FOLDER`.

There are 3 configuration files that you can manage:

* `general.config.js`
* `facets.config.js`
* `variants.config.js`

Do the following to start customizing the configuration:

```sh
cp -r ./demo-store-core/packages/website/config ./config
```

```properties
# .env.local
NEXT_PUBLIC_CONFIG_FOLDER=../../../config/
```

### Environment Variables

There are some enviroment variables that you can use to customize the Demo Store. For an exaustive description you can take a look at [additional-env.d.ts](https://github.com/commercelayer/demo-store-core/blob/master/packages/website/additional-env.d.ts) file.


## Troubleshooting

1. **Q.** Even if I changed `NEXT_PUBLIC_JSON_DATA_FOLDER`, `NEXT_PUBLIC_LOCALE_DATA_FOLDER` or `NEXT_PUBLIC_CONFIG_FOLDER`, the website is still refering to previous files.

    **A.** These environment variables reflect as `alias` for Webpack. Starting from Webpack 5, it introduced caching for faster builds. Changing these environment variables will not invalidate the Webpack cache. You have to remove `.next` folder manually or by running:

    ```sh
    # update the path if needed
    rm -rf demo-store-core/packages/website/.next/
    ```

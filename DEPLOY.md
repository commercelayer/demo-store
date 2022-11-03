# Deploying the Demo Store

The Demo Store can be deployed wherever you prefer. Please find here below the details about how to do it using some of the most common platforms. If you tested different ways to deploy it or used other services, please [join the discussion](https://github.com/commercelayer/demo-store-core/discussions/new?category=show-and-tell) and share the steps with the community!

## GitHub workflows

Configuring GitHub workflows is straightforward. We already created one that can be triggered manually. To do that:

1. Review the [`gh-pages.yml`](.github/workflows/gh-pages.yml) file by updating the environment variables. You need and create two [secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets#creating-encrypted-secrets-for-a-repository) in your repository.

    <img width="640" alt="Repository secrets" src="https://user-images.githubusercontent.com/1681269/185638408-f5c68563-1197-436c-a2b0-9aaf0dfbc16a.png">

2. Now you should be able to run the workflow. This will build your Demo Store statically and deploy the artifact to GitHub Pages.

   ![Run workflow](https://user-images.githubusercontent.com/1681269/185639837-5b81186b-f5e7-43cd-bf7a-1c00f3b71b58.png)

> :information_source: Feel free to update the workflow according to your needs. You can trigger the deploy with every `git push` or change the "Deploy" step (e.g. by publishing the artifact somewhere else such as AWS S3, Netlify, etc.).

> :warning: Make sure to stick to GitHub's guideline about the [prohibited-uses](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages#prohibited-uses): _«GitHub Pages is not intended for or allowed to be used as a free web-hosting service to run your online business, ecommerce site, or any other website that is primarily directed at either facilitating commercial transactions [...]»_ ( )

## Netlify

Inside the project root you can find two files: `netlify.ssg.toml` (for static site generation) and `netlify.ssr.toml` (for server-side rendering)

1. Rename one of these into `netlify.toml` and update the `[build.environment]` section with the environment variables as you need.
2. From your Netlify dashboard, click on _"Add new site -> Import an existing project"_ and then select the correct repository.
3. At this point, you should have some fields already filled in. Click on _"Show advanced"_ and add the missing environment variables: `SITE_URL`, `NEXT_PUBLIC_CL_CLIENT_ID`, and `NEXT_PUBLIC_CL_ENDPOINT`. Please note that you cannot know yet which will be the `SITE_URL` until Netlify creates a random site name after the first deployment.

    <img width="500" alt="image" src="https://user-images.githubusercontent.com/1681269/186125308-c9f73c0c-29d2-4b92-a314-7b65e55ed7c1.png">

4. Click on _"Deploy site"_. We suggest stopping the first auto-deploy by clicking on _"Cancel deploy"_ because this way you can update the `SITE_URL` with the correct information first. You can also speed up the deployment by [disabling the form detection](https://docs.netlify.com/site-deploys/post-processing/form-detection/) (the Demo Store project doesn't include forms managed by Netlify).

5. Trigger a new deploy and enjoy your store up and running. :rocket:

## Vercel

### Server-side rendering

From your Vercel dashboard:

1. Click on _"Add New... -> Project"_
2. Select the correct repository.
3. Fill in all the required information as per the image below:

    <img width="722" alt="Deploy the Demo Store SSR to Vercel" src="https://user-images.githubusercontent.com/1681269/186161145-5c9b8ebc-6fc2-4642-9fcc-f64adcd2e55e.png">

4. Click on _"Deploy"_ and in less than 2 minutes your store will be ready to go.

> :information_source: You need to add `ENABLE_EXPERIMENTAL_COREPACK=1` as an environment variable since the installed npm version used by Vercel does not run `postinstall` due to a [bug](https://github.com/orgs/vercel/discussions/789).

### Static site generation

If you prefer to go the SSG way you just need to apply the following changes to the information above:

- Framework preset: `Other`
- Build command: `npm run export`
- Output directory: `demo-store-core/packages/website/out`
- Environment variable: `NEXT_PUBLIC_DATA_FETCHING=ssg`

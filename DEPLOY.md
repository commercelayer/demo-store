# Deploying the Demo Store

## GitHub workflows

Configuring GitHub workflows is straightforward. We already created one that can be triggered manually.

You just have to review [`gh-pages.yml`](.github/workflows/gh-pages.yml) by updating the environment variables as you need and create two [secrets](https://docs.github.com/en/actions/security-guides/encrypted-secrets#creating-encrypted-secrets-for-a-repository) in your repository.

<img width="640" alt="Repository secrets" src="https://user-images.githubusercontent.com/1681269/185638408-f5c68563-1197-436c-a2b0-9aaf0dfbc16a.png">

Now you should be able to run the workflow. This will build your Demo Store statically and deploy the artifact to a `gh-pages` branch.

![Run workflow](https://user-images.githubusercontent.com/1681269/185639837-5b81186b-f5e7-43cd-bf7a-1c00f3b71b58.png)

If you want to release your website to GitHub Pages, you just have to [configure a publishing source](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site#publishing-from-a-branch).

Feel free to update the workflow according to your needs. You can trigger the deploy with every git-push or change the `deploy` step, e.g. by publishing the artifact somewhere else (AWS S3, Netlify, etc.).

> :warning:
> _GitHub Pages is not intended for or allowed to be used as a free web-hosting service to run your online business, e-commerce site, or any other website that is primarily directed at either facilitating commercial transactions [...]_ ( [prohibited-uses](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages#prohibited-uses) )


## Netlify

Inside the project root you can find two files [netlify.ssg.toml](netlify.ssg.toml) (for static site generation) and [netlify.ssr.toml](netlify.ssr.toml) (for server-side rendering).
Rename one of these into `netlify.toml` and update the `[build.environment]` section with the environment variables as you need.

From you Netlify Dashboard, click on `Add new site -> Import an existing project` and then select the repository.

At this point you should have some field already filled-in. Click on `Show advanced` and add the missing environment variables `SITE_URL`, `NEXT_PUBLIC_CL_CLIENT_ID` and `NEXT_PUBLIC_CL_ENDPOINT`.

> :information_source:
> Regarding the `SITE_URL` you cannot know yet which will be the url since Netlify will create a random site name after the first deploy.

<img width="500" alt="image" src="https://user-images.githubusercontent.com/1681269/186125308-c9f73c0c-29d2-4b92-a314-7b65e55ed7c1.png">

Click on `Deploy site`.

We suggest to stop the first auto-deploy by clicking on `Cancel deploy` because you can update the `SITE_URL` with the correct information first.

You can also speed up the deploy by [disabling `form detection`](https://docs.netlify.com/site-deploys/post-processing/form-detection/). We don't have form managed by Netlify, so you can securely disable it.

Now you can trigger a new deploy and .. voilà, site will be up and running :rocket:

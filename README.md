# Suri × Deploy to DigitalOcean

You're viewing a template repository tailored for deploying Suri to
[DigitalOcean](https://www.digitalocean.com/).

What's Suri? Suri is your own link shortener that's easily deployed as a static
site. No server-side hosting, serverless cloud functions, or database necessary.
Head over to the main [`jstayton/suri`](https://github.com/jstayton/suri)
repository to learn more, including additional deployment methods.

## Setup

_**Note:** This is going to seem more complicated than it should be, but it's the
only way to launch with a pre-configured setup that's also connected to your
GitHub repository for auto-deployment. If you know of a more straight-forward
way, please reach out!_

1. Click the "Use this template" button above and then "Create a new
   repository". Fill in the required details to create a new repository based on
   this one. **You must set the visibility to "Public"!**
2. Change line 8 of [.do/deploy.template.yaml](./.do/deploy.template.yaml),
   replacing `{{owner}}` and `{{repo}}` in `repo` with the values for your new
   repository.
3. On the [DigitalOcean Control Panel](https://cloud.digitalocean.com/), head to
   [create a new app](https://cloud.digitalocean.com/apps/new).
4. Connect your GitHub account or edit existing permissions to grant access to
   your new repository. **Don't hit "Next" after arriving back at
   DigitalOcean!**
5. Open the following URL, again replacing `{{owner}}` and `{{repo}}` with the
   values for your new repository:
   [https://cloud.digitalocean.com/apps/new?repo=https://github.com/{{owner}}/{{repo}}/tree/main](https://cloud.digitalocean.com/apps/new?repo=https://github.com/{{owner}}/{{repo}}/tree/main)
6. Hit "Skip to Review", as everything should already be set appropriately. Then
   hit "Create Resources" to begin the build and deploy process.
7. Any commits to the `main` branch of your new repository will trigger a new
   deploy. You can change this by going to the "Settings" of the `suri`
   component and editing the "Branch" and "Autodeploy" options.
8. If you want to use a custom domain, follow DigitalOcean's guide:
   [How to Manage Domains in App Platform](https://docs.digitalocean.com/products/app-platform/how-to/manage-domains/).

## Manage Links

Links are managed through [`./src/links.json`](./src/links.json), which is
seeded with a few examples to start:

```json
{
  "/": "https://github.com/jstayton/suri",
  "1": "https://fee.org/articles/the-use-of-knowledge-in-society/",
  "tw": "https://twitter.com"
}
```

It couldn't be simpler: the key is the "shortlink" path that gets redirected,
and the value is the target URL. Keys can be as short or as long as you want,
using whatever mixture of characters you want. `/` is a special entry for
redirecting the root path.

Go ahead and make an edit, then commit and push to your repository. DigitalOcean
should automatically build and deploy your change. That's it!

## Config

Config options are set in [`suri.config.json`](suri.config.json). There is only
one at this point:

| Option | Description                                                        | Type    | Default |
| ------ | ------------------------------------------------------------------ | ------- | ------- |
| `js`   | Whether to redirect with JavaScript instead of a `<meta>` refresh. | Boolean | `false` |

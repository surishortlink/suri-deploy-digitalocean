<h1 align="center" width="100%">
  <img src="https://raw.githubusercontent.com/surishortlink/suri/HEAD/logo.png" width="200" alt="Suri" />
</h1>

<h3 align="center" width="100%">
  <i>Your own short links as an easily deployed static site on DigitalOcean</i>
</h3>

You're viewing a template repository tailored for deploying Suri to
[DigitalOcean](https://www.digitalocean.com/). Head over to
[the main repository](https://github.com/surishortlink/suri) to learn more about
Suri, including additional deployment options.

## Setup: Step By Step

_**Note:** This is going to seem more complicated than it should be, but it's
the only way to launch with a pre-configured setup that's also connected to your
GitHub repository for auto-deployment. If you know of a more straight-forward
way, please reach out!_

1. Hit the "Use this template" button above and then "Create a new repository".
   Fill in the required details to create a new repository based on this one.
   **You must set the visibility to "Public"!**
2. Change line 8 of [.do/deploy.template.yaml](.do/deploy.template.yaml),
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
   hit "Create Resources" to begin the initial build and deploy.

### Auto-Deploy

Any commits to the `main` branch of your new repository will trigger a new build
and deploy. You can change this by going to the "Settings" of the `suri`
component and editing the "Branch" and "Autodeploy" options.

### Custom Domain

To use a custom domain, follow DigitalOcean's guide:
[How to Manage Domains in App Platform](https://docs.digitalocean.com/products/app-platform/how-to/manage-domains/).

## How It Works

### Manage Links

At the heart of Suri is the [`links.json`](src/links.json) file, located in the
`src` directory, where you manage your links. All of the template repositories
include this file seeded with a few examples:

```json
{
  "/": "https://www.youtube.com/watch?v=CsHiG-43Fzg",
  "1": "https://fee.org/articles/the-use-of-knowledge-in-society/",
  "gh": "https://github.com/surishortlink/suri"
}
```

It couldn't be simpler: the key is the "short link" path that gets redirected,
and the value is the target URL. Keys can be as short or as long as you want,
using whatever mixture of characters you want. `/` is a special entry for
redirecting the root path.

### Build Static Site

Suri ships with a `suri` executable file that generates the static site from the
`links.json` file. The static site is output to a directory named `build`.

All of the template repositories are configured with a `build` script that
invokes this executable, making the command you run simple:

```bash
npm run build
```

When you make a change to the `links.json` file, simply re-run this command to
re-generate the static site, which can then be re-deployed. This template
repository is configured to do this automatically.

### Config

Configuration is handled through the [`suri.config.json`](suri.config.json) file
in the root directory. There is only one option at this point:

| Option | Description                                                        | Type    | Default |
| ------ | ------------------------------------------------------------------ | ------- | ------- |
| `js`   | Whether to redirect with JavaScript instead of a `<meta>` refresh. | Boolean | `false` |

### Public Directory

Finally, any files in the `public` directory will be copied over to the `build`
directory without modification when the static site is built. This can be useful
for files like `favicon.ico` or `robots.txt` (that said, Suri provides sensible
defaults for both).

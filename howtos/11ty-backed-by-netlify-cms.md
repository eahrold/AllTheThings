---
layout: layout.njk
title: Connect existing 11ty to NetlifyCMS
date: 2020-03-31T17:15:32.261Z
---
![]()

### 1. Create Necessary Files / Folders

* Add a folder at the root of the project called `admin`
* Add file `admin/index.html` with the following contents

```
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />

    <script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>
    <title>Netlify CMS</title>
  </head>
  <body>
    <!-- Include the script that builds the page and powers Netlify CMS -->
    <script src="https://unpkg.com/netlify-cms@^2.0.0/dist/netlify-cms.js"></script>
    <!-- Include Netlify Identity for authentication -->
    <script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>
    <script type="module" src="/admin/preview-templates/index.js"></script>
  </body>
</html>
```

* Add a file `admin/config.yml`

```yaml
backend:
  name: git-gateway
  branch: master # Branch to update (optional; defaults to master)

# Uncomment below to enable drafts
# publish_mode: editorial_workflow

media_folder: "static/images" # If you have an existing image asset dir, this can be changed
```

* If you don't have an existing image folder create a folder and intermediaries `static/images`
* Update your `.eleventy.js` file with passthrough copy for the new `admin` and `static/images` folder

```js
eleventyConfig.addPassthroughCopy("admin");
eleventyConfig.addPassthroughCopy("static/images");
```

### 2. Add Your Collections to the configuration file

* This part is a bit custom, I for example have a folder at the root of my 11ty project `reviews`. The reviews are written in markdown, and have the .njx template properties defined

```markdown
---
layout: layout.njk
title: Some Review of Something
date: 2020-03-26T17:15:32.261Z
---
```

so the collection definition in the `admin/config.yam` looks like this

```yaml
collections:
  - name: "reviews" # Used in routes, e.g., /admin/collections/blog
    label: "Reviews" # Used in the UI
    folder: "reviews" # The path to the folder where the documents are stored
    create: true # Allow users to create new documents in this collection
    slug: "{{slug}}" # Filename template, e.g., YYYY-MM-DD-title.md
    fields: # The fields for each document, usually in front matter
      - {label: "Layout", name: "layout", widget: "hidden", default: "layout.njk"}
      - {label: "Title", name: "title", widget: "string"}
      - {label: "Publish Date", name: "date", widget: "datetime"}
      - {label: "Body", name: "body", widget: "markdown"}
```

* This should be repeated for each collection, (Posts, Pages, etc...)

### 3. Enable Identity in the Netlify

* Navigate to your site's identity settings portal `https://app.netlify.com/sites/_your_instance_name_/settings/identity`
* Enable identity (there is a free tier) w/ < 1000 users
* In the Registration section Set the proper registration preferences. I prefere the invite only option.
* Add the following snippets to your site's base template. Mine is at `_includes/layout.njk` for example, and other templates inherit for this.

In the head

```
<head>
  ...
  <script crossorigin="anonymous" src="https://polyfill.io/v3/polyfill.min.js?version=3.52.1&features=default"></script>
  <script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>
</head>
```

*note: there are other way to include the default browser polyfills, https://polyfill.io was just the easiest. If you don't care about Safari or ie, then you don't need to include this.*

Then just before the closing body

```
<body>
  ...
  <script>
    if (window.netlifyIdentity) {
      window.netlifyIdentity.on("init", user => {
        if (!user) {
          window.netlifyIdentity.on("login", () => {
            document.location.href = "/admin/";
          });
        }
      });
    }
  </script>

</body>
```

### 4. Setup Media Management using Netlify Large Media with Netlify CMS

* Install netlify-cli and the credential helper, if you don't have it, and login

```bash
brew tap netlify/git-credential-netlify
brew install git-credential-netlify
npm install netlify-cli -g
netlify login
```

*\* this assumes you have homebrew installed*

* Setup GitLFS support using the `netlify-lm-plugin`

```bash
netlify link
netlify plugins:install netlify-lm-plugin
netlify lm:install
netlify lm:setup
git lfs track "static/images/**"
```

### 5. Deploy

```bash
git push
```

### 6. Login

* Invite yourself as a user. https://app.netlify.com/sites/*your_instance_name*/identity
* You should get an email that will redirect you through mandrillapp, and land you on the front end of your site. If you've correctly included the `netlify-identity-widget.js` you'll see a modal that looks like this 

  ![](/static/images/screen-shot-2020-03-31-at-3.01.25-pm.png)

  prompting you to finalize account creation.



*Note as of 3/31/2020 the identity plugin did not work well in Safari 13.0.5 and most likely not in ie either. The accept_invite did not work at all (wouldn't even present modal to dispay).*
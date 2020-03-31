---
layout: layout.njk
title: Connect existing 11ty to NetlifyCMS
date: 2020-03-31T17:15:32.261Z
---

# Connect existing 11ty to NetlifyCMS

### 1. Create Necissary Files / Folders

- Add a folder at the root of the project called `admin`
- Add file `admin/index.html` with the following contents

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

- Add a file `admin/config.yml`

```yaml
backend:
  name: git-gateway
  branch: master # Branch to update (optional; defaults to master)

# Uncomment below to enable drafts
# publish_mode: editorial_workflow

media_folder: "static/images" # If you have an existing image asset dir, this can be changed
```

- If you don't have an existing image folder create a folder and intermediarys `static/images`


Update your `.eleventy.js` file with passthrough copy for the new `admin` and `static/images` folder

```js
eleventyConfig.addPassthroughCopy("admin");
eleventyConfig.addPassthroughCopy("static/images");
```

### 2. Add Your Collections to the configuration file
- This part is a bit custom, I for example have a folder at the root of my 11ty project `reviews`. The reviews are written in markdown, and have the .njx template properties defined

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

- This should be repeated for each collection, (Posts, Pages, etc...)

### 3. Enable Identity in the Netifly

- Navigate to your site's identity settings portal `https://app.netlify.com/sites/_your_instance_name_/settings/identity`
- Enable identity (there is a free tier) w/ < 1000 users
- In the Registration section Set the proper registration preferences. I prefere the invite only option.

- Add the following snippets to your site's base template. Mine is at `_includes/layout.njk` for example, and other templates inherit for this.

In the head
```
<head>
  ...
  <script src="https://identity.netlify.com/v1/netlify-identity-widget.js"></script>
</head>
```

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

### 4. Setup Media Managment

### 5. Deploy

### 6. Login
- Invite yourself as a user. https://app.netlify.com/sites/_your_instance_name_/identity

### 7. Create a new entry.
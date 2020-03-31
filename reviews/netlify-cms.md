---
layout: layout.njk
title: Netlify CMS
date: 2020-03-26T17:15:32.261Z
---
### pros

- git based backend version control of data is nice.
- You can actually hook it up to any site.
- There's a local repo feature in beta
    - added in netlify-cms@2.10.6 /netlify-cms-app@2.11.3
- The DEMO site looked real promising and checked off the most basic use case boxes. [https://cms-demo.netlify.com/](https://cms-demo.netlify.com/). Plus there was actually a workflow feature that tells me it's well thought out.

### cons

- With millions of records repos could become too big to manage.
- There doesn't really seem to be a clear way to define relations between collections (authors -> posts), it's not an RMDB, and while there's a "relation widget" for this feature, I'd be a bit weary of it's performance at scale, especially if you're looking to get any depth ( user -> posts -> comments -> user )

- The media library feature has some 3rd party dependencies.


### All in all.
Could work nicly for mid sized sites that just need static content. Though Media files might be a little tricky.
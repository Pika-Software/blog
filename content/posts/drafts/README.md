+++
draft = true
title = 'README'
+++

## How to create a draft
To start, run 
```bash
hugo new content content/posts/drafts/my-blog.md
```

If you need to add assets like images, create a folder with file name, 
and move draft file inside folder and rename to `index.md`

```bash
# before
drafts/
  | my-blog.md

# after
drafts/
  | my-blog/
    | index.md
    | asset.png
```

## Publishing the draft
After you are ready to publish your masterpiece, move your post to `content/posts/year/month/day` folder. 
You can also update `date` property to `date = 'year-month-day'`

And don't forget to remove `draft = true` from the post!

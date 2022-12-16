Making changes to the blog appearance 
------

Unfortunately, hugo does not document how to change a theme appearance very well. [This](https://bwaycer.github.io/hugo_tutorial.hugo/themes/customizing/) is the best resource I have found for how to edit a theme. 

>The following are key concepts for Hugo site customization. Hugo permits you to supplement or override any theme template or static file, with files in your working directory. When you use a theme cloned from its git repository, you do not edit the theme’s files directly. 

If you want to change the blog appearance, you need to create folders and files in the **root directory** that match folders and files in the **public directory**. Hugo will copy and replace files in the root directory into the public directory. 

### General blog settings 

Other settings in the blog can be set in the `./config.toml`. 

### css

Changes to theme css can be done using. 

```
.
├───static
    └───dist 
        └───even.min.css
```

**NOTE!** editing the `./public/dist/even.min.css` will have no effect on the blog appearance because it is replaced by `./static/dist/even.min.css` when hugo builds the blog. 

### flavicon and logo image

To replace the flavicon change the files in the root `./static/`. 

```
.
├───static
    ├───favicon-16x16.png
    ├───favicon-32x32.png
    └───GBIF-analytics-blog.png
```

### How I added the logo to the blog

To add the logo by editing `./layouts/partials/header.html`. 

```
<div class="logo-wrapper">
  <a href="{{ "/" | relLangURL }}" class="logo">
	<img src= "{{ .Site.Params.logoSrc }}" alt="GBIF-analytics-blog" style ="width:20%;">
  </a>
</div>

```
I set the following variable in `config.toml`

```
logoSrc = "/logo.png"
```

### How I added author names 

I added author names by editting 

* `./layouts/post/single.html` 
* `./layouts/post/summary.html`

I added the following lines. 
```
<div class="post-author">{{ .Params.author }}</div>
```

### How to add discoure.org comments 

I connected the gbif community forum (https://discourse.gbif.org/) to the blog using the instructions found [here](https://meta.discourse.org/t/embedding-discourse-comments-via-javascript/31963). 

Following the instructions I added code below to `./layouts/partials/footer.html`: 

```
{{if eq .Section "post"}} 
<div id='discourse-comments'></div>
<script type="text/javascript">
  DiscourseEmbed = { discourseUrl: 'https://discourse.gbif.org/',
                     discourseEmbedUrl: 'https://data-blog.gbif.org{{ .URL }}' };

  (function() {
    var d = document.createElement('script'); d.type = 'text/javascript'; d.async = true;
    d.src = DiscourseEmbed.discourseUrl + 'javascripts/embed.js';
    (document.getElementsByTagName('head')[0] || document.getElementsByTagName('body')[0]).appendChild(d);
  })();
</script>  
{{end}}
```
**NOTE!**
* I had to add `{{ .URL }}` to the **discourseEmbedUrl** to make this work. 
* I also added `{{if eq .Section "post"}}` so that comments would only appear at the bottom of post pages. 

Steps to take using discourse.gbif.org UI: 

1. I went to **Admin > Customize > Embedding**
2. Created New embeddable host
3. Set **Path Whitelist** to `/post/.*` 
4. Created new category **data-blog-preview** (staff-only)
5. Created new category **data-blog** (everyone)

How to make comments viewable to the general public: 
1. Comments at the bottom of the page will initially show "Embedding Failed"
2. To make comments viewable. In **discourse.gbif.org**, set category **data-blog-preview** to **data-blog**
3. You should now see something like "loading..."
4. Then if you re-fresh the page the comments should appear. 


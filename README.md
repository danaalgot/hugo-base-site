# Starter theme for development in hugo

I'm creating a base theme in hugo so I can do development! Learning as I go. 
## Setup
**Install Hugo and create the site:**
- brew install hugo
- hugo version (to make sure it’s installed)
- hugo new site site-name

**Start up the server:**
- hugo server (View published content only)
- hugo server -D (View draft and published content)

Then you can grab the starter-theme from the themes folder and place it within your newly created site.

## Content
**Two types of content - Single pages and list pages**
- List page: Page that lists other content.
- Single page: Page that displays information

**Create a content file: (markdown)**
- hugo new name.md
- hugo new directory/filename.md

_index.md creates a listing page for directories that are not within the root of the content folder. Any directories that are direct children of the content folder will have listing pages created automatically. But you can use this to override the file that Hugo creates automatically to include your own content. 

## Front matter
Front matter is metadata that provides information about the content file. It's displayed at the top of each markdown file within the content directory. 

```
---
title: "Title"
date: 2023-08-10T13:56:02-06:00
draft: true
---
```

These can be written in three languages - YAML, TOML, JSON. The default in Hugo is YAML. You can create any front matter variables that you want and can access these variables within the templates. 

## Archetypes
Archetypes allow you to define the front matter that is automatically added to each new content file created. By default the content will use the default.md file.

You can create archetypes for each directory within the content folder. For example, if I make a file in the archetypes folder called dir1.md it will use this file for any content files created within the dir1 content folder.

<img width="448" alt="FIRST SITE" src="https://github.com/danaalgot/hugo-base-site/assets/36510178/ad342987-2ad5-4a62-828d-f64523643655">

Any new files created within the dir1 folder will have the new front matter instead of the default. 

## Shortcodes

Shortcodes are predefined chunks of HTML that can be added into your markdown content. Hugo comes equipped with a bunch of default shortcodes, but you can also create your own. 
```
{{< shortcode-name param1 param2 >}}
```
<img width="469" alt="date 2023-08-10T135602-0600" src="https://github.com/danaalgot/hugo-base-site/assets/36510178/fb57ea17-3fbf-42b5-b744-5902b28fb5f7">

Above is an example of embedding a YouTube video within a markdown file. Using the predefined YouTube shortcode and the ID of the YouTube video. I used this video for an example: [https://www.youtube.com/watch?v=N_Z_baf5kuc](https://www.youtube.com/watch?v=N_Z_baf5kuc). Take the ID from the end of the URL and inset it as the first parameter. 

### Shortcode templates
We can create our own custom shortcode templates. These are like partials but for code that we display within the content files. 

Create a folder within layouts called shortcodes. Then create any HTML file within here for your shortcode. Include your custom shortcode in a content file with ```{{< myshortcode>}}```. The name of your shortcode will be the name of your file. I’ve named my file myshortcode.html. 

We can also pass in parameters for the shortcode to use. There are two ways to do this. Variable and positional. 

#### Variable parameter
To include a variable parameter, add the following code to the markdown file. 
```
{{<myshortcode color="blue"}}
```
You can then output that variable within the myshortcode.html file.
```
<p style="color: {{ .Get `color`}}">This is my custom shortcode</p>
```
When you use the ```.Get``` variable, you would usually add double quotes around the value like ```{{ .Get “color” }}```. Sometimes this doesn’t work with inline CSS. To fix this we’ve added in backticks.

#### Positional parameter
To include a positional marameter, add the following code to the markdown file. 
```
{{<myshortcode blue red}}
```
Notice that the parameters don't have names. We only added in values. We now access this in our template with a position. 
```
<p style="color: {{ .Get 0}}">This is my custom shortcode</p>
```
This changes my text to blue. If I wanted to access the red value I’d change it to be ```{{ .Get 1 }}```

#### Double tag shortcodes
All the examples above are single tag shortcodes. We can also do double tag shortcodes, meaning they have a start and end tag. 
```
Some text that I want {{<highlight color="yellow">}}hightlighted{{</highlight>}} in yellow.
```
We then include the following code in our hightlighted.html file.
```
<span style="background: {{ .Get `color` }}">{{ .Inner }}</span>
```
{{ .Inner }} is a variable that will access any raw text that is within the tags. 

#### Including markdown
If you want to include markdown within the shortcodes we need to update the shortcode call. You need to replace the less than and greater than characters to be percentage characters. 
```
Some text that I want {{%highlight%}}**hightlighted**{{%/highlight%}} in yellow.
```
Just be aware that any HTML within the template doesn’t render anymore. It will just be markdown.

## Taxonomy
Taxonomy is how you logically group content together more efficiently. All taxonomies are defined in the front matter on a content page. 

**Two taxonomy types in Hugo:**
- Tags (think of a blog site, tagging posts with keywords, look at all the content tagged with a keyword)
- Categories (group peices of content into categories)

```
---
title: "Title"
date: 2023-08-14T14:34:44-06:00
draft: true
tags: ["tag1", "tag2", "tag3"] #tags go here!
categories: ["category1"] #categories go here!
---
```

Hugo automatically generates a listing page for taxonomies. To list out all the content that is tagged with the same tags. 

### How to create custom taxonomies:

Go into your front matter and include a custom taxonomy with your name and values.
```
---
title: "Title"
date: 2023-08-14T14:34:44-06:00
draft: true
tags: ["tag1", "tag2", "tag3"]
categories: ["category1"]
moods: ["Happy", "Upbeat"] #This is a custom taxonomy
---
```
Then go into your hugo.toml file and define the taxonomy you are using.
```
baseURL = 'http://example.org/'
languageCode = 'en-us'
title = 'My New Hugo Site'
theme = "hugo-theme"
[taxonomies]
  tag = "tag"
  category = "categories"
  mood = "moods"
```
Hugo doesn’t automatically create listing pages for custom taxonomy. We need to configure Hugo to recognize our custom taxonomy within the Hugo.toml file. Add a taxonomies array with all the taxonomies you are using. You must always include the default taxonomies as well if you define any custom values. 

## Templates
The HTML of the site which we can display the content within.

**Types of templates:**
- List templates: Page that lists other content.
- Single templates: Page that displays information
- Home templates
- Section templates
- Base templates and blocks

For example, all single pages share a template to display the content, and all list pages share the same list template. 
All templates go into the layouts folder. _default folder is what templates the site will use by default. 

### List page templates (list.html)
We can create a custom list template in layouts/_default/

**Functions and variables to display content:**
- {{ .Content }} Displays the content from the markdown file of that listing page.
- {{ range .Pages }}{{ end }} Loops through all the pages in this section
    - {{ .Title }} Prints out the title
    - {{ .Permalink }} Prints out the URL
```
{{ range .Pages }}
  <a href="{{ .Permalink }}">{{ .Title }}</a><br>
{{ end }}
```
[Read more on list templates](https://gohugo.io/templates/lists/)

### Home page template
By default the home page will display as a listing, but we can create a custom home page template within layouts called index.html. Create this page as you would for the homepage of the site. 

### Section templates
Section templates allow you to override other default templates for each directory (single and list) and customize them. When you create the file it will match the directory name you want to override. For example, if we have a folder in content called dir1, we would create a folder in the layouts directory called dir1. Within that folder you would create your list.html or single.html file you want to override. 

<img width="469" alt="5 single html" src="https://github.com/danaalgot/hugo-base-site/assets/36510178/a2293dcb-a4c7-462a-ad58-fce89ac8a9dd">

In the image above, I’ve created a single.html file that I want to override the default template for the dir1 directory. Now all single pieces of content will display as the new file. Instead of the default file within /layouts/_default.

[Read more on section templates](https://gohugo.io/templates/section-templates/)

### Base templates and blocks
Base templates are a advanced way to organize the content on your site. Base files must be named ```based.html```. This acts as the over arching template of our Hugo site. Instead of copying and pasting repeated code for the document structure in every template, we can add it here. The baseof template is the highest level template, meaning any piece of content is going to inherit it’s values. In here we are creating the document structure that will be used in every page of the site. This file lives within layouts/_default/baseof.html

Define a block within the base template like so: {{ block "main" . }}{{ end }}. You can change main to whatever name you want. You can create many blocks within the base template. Another example is a footer. Then in your single or list templates include the block like so: {{ define "main" }}{{ end }}. Any code you place within this block will display inside the main section of the base template.   baseof.html
```
<!DOCTYPE html>
<html lang="en">
{{ partial "head" . }}
  <body>
    {{ partial "header" . }}
    <main>
      {{ block "main" . }}

      {{ end }}
    </main>
    {{ partial "footer" . }}
  </body>
</html>
```

 single.html
```
{{ define "main" }}
  <h1>{{ .Title }}</h1>
  {{ .Content }}
{{ end }}
```

[Read more on base templates and blocks](https://gohugo.io/templates/base/)

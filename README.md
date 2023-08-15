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

## Variables
Can only use variables in templates. Variables always start with a period and a capital letter. 

**Common variables:**
- {{ .Title }}
- {{ .Permalink }}
- {{ .Date }}

### Custom variables
We can custom variables from within the content front matter. If I added author to the front matter, I could display that like ```{{ .Params.author }}```. 

You can also create variables within templates. ```{{ $myVariable := "string" }}``` Then you can print this out on the same template with ```{{ $myVariable }}```.

Variables from front matter can also be used within HTML elements for styles. 
If I create a custom variable within my content called color. 
```
---
title: "Title"
date: 2023-08-14T14:34:44-06:00
draft: true
color: "blue"
---
```
I can then go into the template and use the variable like so.
```
<h1 style="color: {{ .Params.color }}">{{ .Title }}</h1>
```
￼
The dot before variable represents the entire collection of variables that we have access to. 

[Read more on variables here](https://gohugo.io/variables/)

## Functions
Used within the templates. Looks like {{ functionName param1 param 2 }}

**Common functions**
- {{ truncate 10 “This is a really long string” }}
- {{ add 1 5 }}
- {{ sub 1 5 }}
- {{ singularize “dogs”}}
- {{ range .Pages}}{{ end }} - Loop through a collection of pages. Each loop we can access variables about that page. Used in listing pages. 

[Read more on functions](https://gohugo.io/functions/)

## Conditionals
If/else statements look a bit different in Hugo than in other languages. You put the operator before the items you are comparing.

**Operators:**
- eq (equal to)
- lt (less than)
- le (less than equal to)
- gt (greater than)
- ge (greater than equal to)
- Isset
- and
- or

If $var1 is equal to $var2:
```
{{ $var1 := "dog" }}
{{ $var2 := "cat" }}

{{ if eq $var1 $var2 }}
  <p>True</p>
{{ else }}
  <p>False</p>
{{ end }}
```

If $var1 is not equal to $var2:
```
{{ $var1 := "dog" }}
{{ $var2 := "cat" }}

{{ if not $var1 $var2 }}
  <p>True</p>
{{ else }}
  <p>False</p>
{{ end }}
```

If $var1 is less than $var2 AND $var1 is less than $var2:
```
{{ $var1 := 1 }}
{{ $var2 := 2 }}
{{ $var3 := 3 }}

{{ if and (lt $var1 $var2) (lt $var1 $var3) }}
  <p>True</p>
{{ else }}
  <p>False</p>
{{ end }}
```

Using conditionals inline in HTML elements.
```
{{/*  Grab the current page title  */}}
{{ $title := .Title }}

{{ range .Pages }}
  <a href="{{ .Permalink }}" style="{{ if eq .Title $title }} background: orange; {{ end }}">{{ .Title }}</a><br>
{{ end }}
```

[Read more on conditionals](https://gohugo.io/templates/introduction/#conditionals)

## Data Files
The data folder within a project is a place to put any data that you will be using for the site. Mini database for your website. 
Types of files that can be added: YAML, TOML, JSON

As an example I found a [JSON file online with Canadian provinces](https://raw.githubusercontent.com/Clavicus/Testing-Requests/master/canadian-provinces.json). I added this file into the data folder in my project.
￼
We can grab this data with ```.Sites.Data.name```. In my case it would be ```.Sites.Data.provinces```. Then I can display values from the JSON file within my template. 
```
{{ range .Site.Data.provinces }}
  <p>{{ .capital }} is the capital of {{ .name }} ({{ .short }})</p>
{{ end }}
```
It displays like this on the site. 
![Edmonton is the capital of Alberta (AB)](https://github.com/danaalgot/hugo-base-site/assets/36510178/39cf4b15-3d1d-4b75-89bb-5122140fbbb8)

[Read more on data files](https://gohugo.io/templates/data-templates/)

## Partial Templates
Partial templates make your website more modular. Repeating elements that you can include into templates. Inside the layouts folder we will create a new folder called partials. All partial files are HTML files.
￼
To include a partial within a template file, use ``{{ partial “name” variable }}``.

We can also include variables from the page content within the partials. Anything you pass into the partial will be available within the file. If you want to include all global variables within the partial, you include a dot. The dot represents the entire collection of variables that we have access to. So the header file will have access to all the variables available to us for the set content. Like title and date within the front matter. 

### Passing custom values
Here is an example passing in a custom dictionary with values.
```
<h1>{{ .myTitle }}</h1>
<p>{{ .myDate }}</p>
```
We have set these variables within the template file.
```
{{ partial "header" (dict "myTitle" "My custom title" "myDate" "July 3rd 2022") }}
```

## Images
There are couple ways that we can include images in our Hugo site. Within both of these directories (static and assets), you can create as many directories as you’d like. I’m placing all my images in a directory called images. 

### Static folder:
Hugo will allow anything in the static folder to be accessed easily inside markdown posts or in layouts, just by using a slash /. This displays the raw file. 
```
![Dog](/images/dog.jpg 'Dog')
```

### Assets folder:
Assets provides us some functions in our templates to optimize images. We can resize and crop our images so they are not so large. 
```
{{ $asset := resources.Get "/images/horses.jpg" }}
{{ $img := $asset.Fit "600x400" }}
<img alt="Dog" src="{{ $img.RelPermalink }}" />
```
In the example above, we are taking an image from the assets folder, resizing it (which also optimizes the size of the file to be not so large), and then displays it. 
We can also utilize these functions within shortcodes. 
```
{{ $alt := .Get "alt" }}
{{ $image := resources.GetMatch (.Get "src") }}
{{ $url := ($image.Resize "600x400").RelPermalink | safeURL }}

<img src="{{ $url }}" alt="{{ $alt }}">
```

[Read more on Image processing](https://gohugo.io/content-management/image-processing/#image-processing-methods)

## SCSS (Compiled styles)
Hugo extended comes with compiler features for SCSS. Brew automatically installs the extended version of Hugo. 

The tutorial I followed for this is [here](https://www.youtube.com/watch?v=wgB7kMEVOg0). 

Create new folder within the assets directory called SCSS, then I organized my SCSS files as I like.
We are going to create a new partial to hold the compiler code called libsass.html. 
```
{{/* assign the path to the $targetPath variable */}}
{{ $targetPath := . }}
{{/* check for "scss/" or "sass/" at the start and ".scss" or ".sass" at the of of the targetPath */}}
{{ if and (hasPrefix $targetPath (or "scss/" "sass/")) (strings.HasSuffix $targetPath (or ".scss"  ".sass" )) }}
    {{/* return the string, but starting at the 6th character and removing the last 5 characters */}}
    {{ $targetPath = substr $targetPath 5 -5 }}
    {{/* print a new target path */}}
    {{ $targetPath = printf "css/%s.css" $targetPath}}
{{ end }}

{{/* sass options common to dev and production */}}
{{ $commonOpts := (dict "transpiler" "libsass" "targetPath" $targetPath "includePaths" (slice "node_modules")) }}

{{/* production options */}}
{{ $opts := merge $commonOpts (dict "outputStyle" "compressed") }}
{{/* development options */}}
{{ if eq hugo.Environment "development" }}
  {{ $opts = merge $commonOpts (dict "enableSourceMap" true) }}
{{ end }}

{{/* get source file and compile */}}
{{ $css := resources.Get . | toCSS $opts }}
<link rel="stylesheet" href="{{ $css.RelPermalink }}">
```
Then in the head.html partial we will include the libsass partial.  
```
<head>
  <meta charset="UTF-8">
  <meta http-equiv="X-UA-Compatible" content="IE=edge">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  {{ partial "libsass" "scss/styles.scss" }}
  <title>
    {{ if .IsHome }}
      {{ print "Home | " .Site.Title }}
    {{ else }}
      {{ print .Title " | " .Site.Title }}
    {{ end }}
  </title>
</head>
```

## Preparing for production
Once you are ready to upload your site to the server, you can run one command that will bundle the site into a public folder.
All you need to run is ```hugo``` in the terminal. This will bundle your site into a public folder which will show up within your project. You can then copy the contents of the site and place it on the server. 


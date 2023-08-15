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
title: "A"
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

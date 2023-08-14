# Starter theme for development in hugo

I'm creating a base theme in hugo so I can do development! Learning as I go. 
## Setup
Install Hugo and create the site:
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
Front matter is metadata that provides information about the content file. It's displayed at the top of each file within the content directory. 

Can be written in three languages - YAML, TOML, JSON. The default in Hugo is YAML. 
Front matter can be used in Hugo to display the information better. 
You can create any front matter variables you want for the page and you can access these variables within the templates. 
# Locomotive CMS kickass

This book is incomplete and should evolve in the future. Any contribution is very welcomed ! 

For any questions or advices about this book, ask [mail@geraudmathe.com](mailto:mail@geraudmathe.com), and if you need support from Locomotive team, ask [support@locomotivecms.com](mailto:support@locomotivecms.com).


## Summary

1. __[Foreword](#foreword)__
  *  __[Why this guide ?](#foreword_1)__
  *  __[Why you should use Locomotive ?](#foreword_2)__
  *  __[Philosophy behind the CMS](#foreword_3)__
  *  __[Assumptions](#foreword_4)__
  *  __[Organization of this book](#foreword_5)__
2. __[Overview](#overview)__
  *  __[Anatomy of a Locomotive app](#overview_1)__
  *  __[Key features](#overview_2)__
3. __[Getting something running in 5 minutes](#getting_something_running)__
4. __[Templating](#templating)__ 
  *  __[Templating Logic](#templating_1)__
  *  __[Liquid syntax](#templating_2)__
  *  __[Creating a page](#templating_3)__
  *  __[Recipe : Create a RSS feed](#rss_feed)__
5. __[Models](#models)__
  *  __[Basics](#models_basics)__
  *  __[Models mapping](#models_mapping)__
  *  __[Templatize a Model](#models_templatize)__
  *  __[Recipe: Public Submission](#public_submission)__
6. __[Locomotive Editor](#locomotive_editor)__
7. __[Using Locomotive in an existing Rails app](#locomotive_rails_app)__
7. __[Tips](#tips)__
  *  __[Using multi-sites](#multi_sites)__
  *  __[Export site](#export_site)__
  *  __[Internationalization](#internationalization)__
  *  __[Customise TinyMCE](#customise_tiny_mce)__
  *  __[Writing custom Liquid tags](#custom_liquid_tag)__
8. __[Appendix](#appendix)__

##Foreword <a name="foreword"></a>

### Why this guide ? <a name="foreword_1"></a>

There is already an official documentation reference, which lists almost everything. Still, a pragmatic guide to Locomotive is missing, especially for beginners.

What's more, since there is a lot of goodness in the Locomotive's <a href="http://groups.google.com/forum/?fromgroups#!forum/locomotivecms">Google Group</a>, it seems relevant to gather good practices & hacks in one place.

This guide isn't the official one, even if some members of the Locomotive core team have reviewed some parts of it. 

### Why should you use locomotive ? <a name="foreword_2"></a>

Locomotive is a CMS that has been created with a main guideline: keep it simple !

- Keep it simple, for the lambda user who doesn't write code.
- Keep it simple, for the developer who shouldn't have to go deep in architecture, and should be able to edit a website quickly.
- Keep it simple, for the author who needs to be focused on content, and shouldn't have to go through several pages to edit.

If you recognize yourself in on of the case listed above, you should use Locomotive.

///

From a "business" point of view, Locomotive have several benefits to sell :

- Front-end editing of static texts, using Aloha editor
- Hosting on Heroku / AWS very cheap, almost free
- Finally, a great looking backoffice !


### Philosophy behind the CMS <a name="foreword_3"></a>

TODO: demander à Didier de l'expliquer ?

### Assumptions <a name="foreword_4"></a>

During this reading, it is assumed that:

- You know what is Ruby and Rails and you've a good feeling with terms like Gem, Bundle, deployment
- You know what is a data model, and ideally what is a document-oriented storage like Mongo
- You have basic knowledge about shell and command-line interface

### Organization of this book <a name="foreword_5"></a>

This guide is structured as follow :

First, an *Overview* of the CMS aims to introduce the environment and the main things to know about.

*Getting something running in 5 minutes* may help Locomotive's beginners walking through the 

## Overview <a name="overview"></a>

### Anatomy of a Locomotive app <a name="overview_1"></a>

Locomotive CMS is crafted as an engine.

<i>
A Rails engine is an application packaged in a rubygem that is able to be run or mounted within another Rails application. An engine can have its own models, views, controllers, generators and publicly served static files.
</i>
([more about engines](http://guides.rubyonrails.org/engines.html))

What's inside ?

- Rails 3
- Mongoid
- Devise
- Liquid
- Haml
- Formtastic
- Carrierwave

TODO: complete this


### Key features <a name="overview_2"></a>

You have out of the box :

- Multi sites : manage multiple websites with one application instance
- Flexible content types
- Front-end inline editing (Aloha editor)
- Content localization
- Restful API
- Haml / Sass support
- Liquid templating langage
- A very nice User Interface


## Getting something running in 5 minutes <a name="getting_something_running"></a>

TODO: définir avec géraud et didier ce que l'on fait dans cette app, liste des choses à voir : editable texts, models, templates (héritage), tags liquid de base, … ?

il n'y a pas de template de base, sauf si on achète loco editor là il y en a mais sinon non, pas dans la version 2.0


## Templating <a name="templating"></a>

### Templating Logic <a name="templating_1"></a>

#### Basics of inheritance

The logic in Locomotive is differs a bit from what you are certainly used to, it may be weird a first, but it's actually very simple.

In the classic, Rails way, you have the following architecture, with page's content integrated in the application layout using the ``` yield ``` statement :

    +- Views
        +- layout
            +- application
        +- mysite
            +- index
            +- first page
            +- second page

In Locomotive, it's a bit different :

    +- Pages
        +- index
            +- first page
            +- second page

All pages inherit from index. This way, the index contains the application's layout and the index page content. How do you re-use the layout without re-using the index page content ? By introducing ```{% block 'block_name' %} ... {% endblock %}``` : since all pages inherit from index, you declare blocks of content inside the layout (index), which will be overwritten in child pages. Here is a the simpliest example :

Index page :

```html
<html>
  <head>
    <title>My index page</title>
  </head>
  <body>
    <header>
      layout header
    </header>
    <div id="content">
      {% block content %}
        the content of the index page
      {% endblock %}
    </div>
    <footer>
      layout footer
    </footer>
  </body>
</html>
```

A page, which inherits from index :
```html
{% extends parent %}

{% block content %}
  the content of this page
{% endblock %}
```

By extending index, 'a-page' re-use all of its content, except the content inside the ``` {% block %} ``` tag which is overwritten. This tag is written with the Liquid syntax which will is explained later.

You can have as many ``` {% block %} ``` tags as you want, everywhere in the layout, as long as the name of each block is unique. For a basic application which only have one layout, that's all you need to know.

src: http://doc.locomotivecms.com/templates/tags#block-section

#### Going further

**Several levels of inheritance**

The principle of page's inheritance can be applied to every page. 
When you create a page, it automatically inherits from index, but you can also make it inherits from another page, by specifying it's parent :

![Specifying parent](Locomotive-fundamentals/raw/master/images/specifying_parent.png)

By doing so, you can define as many levels as you want :

    +- Pages
        +- index
            +- first page
                +- child of first page
                    +- child of first page's child
            +- second page

**Inherit from an other page than the parent one**

When you extend the parent's layout, you use the tag ``` {% extends parent %} ```, but what if you would like to extends a page which isn't a direct parent ?

For example, how would you make "first page" extends "second page" ?

    +- Pages
        +- index
            +- first page
            +- second page

It's simple : ``` {% extends first_page %} ``` ! 
You specify the page you want to extend with its *slug*.

src: http://doc.locomotivecms.com/templates/tags#extends-section

**What about several layouts ?**

Let's say your website needs two layouts, how do it without putting the entire index in ``` {% block %} ``` tags ? It's actually fairly simple : you are not forced to make a page inherits its content from another.

Remember the previous page we created which inherited from index : 
```html
{% extends parent %}

{% block content %}
  the content of this page
{% endblock %}
```

Well, actually the tag ``` {% extends parent %} ``` can be removed, so the page doesn't extends any other page, letting you define a brand new layout if needed.

Let's illustrate this with an example: 

- the main layout of the site will be define in index
- a second layout will be defined in a page called "alternate_layout"
- we will have a page called "normal" which will use the main layout
- and finally an other page called "alternate_page" which will use the alternate layout

The skeleton will look like that :

    +- Pages
        +- index
            +- normal
            +- alternate_layout
                +- alternate_page
            
 Here we go :
 
 First, the index page : 
 ```html
 <html>
  <head>
    <title>The Main Layout</title>
  </head>
  <body>
    <header>
      Main header
    </header>
    <div id="content">
      {% block main_content %}
        the content of the index page
      {% endblock %}
    </div>
    <footer>
      Main footer
    </footer>
  </body>
</html>
 ```
 
 The "normal" page, which inherits from it index :
 
 ```html
{% extends parent %}

{% block main_content %}
  the content of the normal page
{% endblock %}
 ```
 
 Then the "alternate layout" page, which doesn't extends its parent, index :
 ```html
<html>
  <head>
    <title>The Alternate Layout</title>
  </head>
  <body>
    <header>
      Alternate header
    </header>
    <div id="content">
      {% block alternate_content %}
        the content of the alternate layout page, it can be empty if you just want to define an empty layout
      {% endblock %}
    </div>
    <footer>
      Alternate footer
    </footer>
  </body>
</html>
```
 
And finally, the "alternate page", which inherits from "alternate layout". You may notice the ``` {% extends alternate_layout %} ``` instead of ``` {% extends parent %} ```, as explained in the previous part.
```html
{% extends alternate_layout %}

{% block alternate_content %}
  the content of alternate page, using the layout defined in alternate_layout.liquid.html
{% endblock %}
```
 

**Snippets**

To conclude with templating basics, it's worth to know that Locomotive gives you the ability to put some blocs of code in a separate file called snippet, in the same way Rails does with partials. 
That's very usefull when you have to build a modular layout without repeating code. 

Like Rails, you can pass a variable to the snippet, or simply include a static bloc of code. The following example will cover both cases, don't bother with the liquid syntax which will be explained in the next part.

    +- Pages
        +- index
    +- Snippets
        +- sidebar
        +- product_information


Here is the index, which includes the sidebar, and also loops on products model and include the snippet "product_information" at each product item :
```html
<html>
  <head>
    <title>Snippet example</title>
  </head>
  <body>
    <header>
    </header>
    <div id="content">
      <!-- Loop on products  -->
      {% for product in contents.products %}
        <!-- Include "product_information" snippet with the current product -->
        {% include 'product_information' with product %}
      {% endfor %}
    </div>
    {% include 'sidebar' %}
    <footer>
    </footer>
  </body>
</html>
```
 
Then the sidebar :
```html
<div id="sidebar">
  the sidebar
</div>
```
 
And finally the product_information snippet which uses the context "product" :
```html
<div class="product">
{{ product.name }} :  {{ product.price }}$
</div>
```
src: http://doc.locomotivecms.com/templates/tags#include-section

### Liquid syntax <a name="templating_2"></a>

Liquid is a templating library extracted from Shopify, the project is hosted at <a href="http://liquidmarkup.org" >http://liquidmarkup.org</a>. Locomotive reuse a lot of the original library.

#### Everything in 2 markups

The liquid syntax is a templating engine based on a set of functions that allow the developer (or the designer, since you don't need strong code skills to write it) to keep focus on the rendering of the data, not on the way it could render it. 
Liquid defines 2 types of markup, pretty close to what you are used to with Erb :


**Ouput markup : matched pairs of curly brackets output the value of an object :**

Erb :  

    <%= @product.name %>

Liquid :
    
    {{ product.name }}

**Tag markup : matched pairs of culry brackets and percent, not resolved to text :**

Erb :
    
    <% name = @product.name %>

Liquid :
    
    {% assign name with product.name %}

Liquid is extracted from http://www.shopify.com, but Locomotive extends it.
To cover all, we will distinguish 3 cases :
- Objects
- Filters
- Tags
which are all in the Locomotive doc : http://doc.locomotivecms.com/templates/basics
In the next part, we give some examples.

original Liquid doc: https://github.com/Shopify/liquid/wiki/Liquid-for-Designers
    
#### Objects

When writing a liquid template, you will access to a couple of objects representing for instance the current site, page, logged in account as well as collections such as your custom content types. They are also called 'drops'.

Available objects and their attributes are listed here : http://doc.locomotivecms.com/templates/objects

**SEO purpose**

You can either use the object ``` site ``` and have the same meta all over your website :

```html
<html>
  <head>
    <title>{{ site.seo_title }}</title>
    <meta name='description' content='{{ site.meta_description }}' />
    <meta name='keyword' content='{{ site.meta_keywords }}' />
  </head>
  <body>
  </body>
</html>
 ```
Or you can define SEO meta for each ``` page ``` :
```html
<html>
  <head>
    <title>{{ page.seo_title }}</title>
    <meta name='description' content='{{ page.meta_description }}' />
    <meta name='keyword' content='{{ page.meta_keywords }}' />
  </head>
  <body>
  </body>
</html>
```
 




#### Filters

#### Tags

editable file => https://groups.google.com/forum/#!topic/locomotivecms/hOaqFUcZCm8 only in backoffice for 2.0

exemple: <img src="{% editable_file 'Promotion_2_image', hint: 'Upload a promotion image (perfect size: 250px by 100px)' %} {{'promo.jpg'| theme_image_url }} {% endeditable_file %}" alt="promotion" />




### Creating a page <a name="templating_3"></a>

TODO: montrer la gestion visuelle des pages dans la home
TODO: lister les différents types de page ?

You have several option when you create a page. Let's take a look.

#### General information

![General Information](Locomotive-fundamentals/raw/master/images/page_general_info.png)

Nothing complex, just specify the name of the page. The slug field will be updated automatically.

Be aware the slug will reference the url linked with the page you are creating, if you change it later, it could break links in the website.

Set the parent page, as explained in __[Templating Logic](#templating_1)__.

#### SEO settings

![SEO settings](Locomotive-fundamentals/raw/master/images/page_seo_settings.png)

Edit the meta ```title```, ```keyword```, ```description``` for the page, or leave it empty if you want use the global meta.

These meta values will then be available for use in the template with Liquid tags, this way :

```html
<title>{{ page.seo_title }}</title>
<meta name="keywords" content="{{ page.keywords }}"/>
<meta name="description" content="{{ page.description }}"/>
```

#### Advanced options

![Advanced options](Locomotive-fundamentals/raw/master/images/page_advanced_options.png)


- Handle : 

TODO: link to integrate Locomotive with a rails app

- Response type :

You can choose between HTML, RSS, XML or JSON. You may this way generate a RSS feed or build an simple API from your Locomotive site.

- Templatized : 

TODO: link to the part of the book about templatized pages

- Published : 

Since only authenticated accounts can view unpublished pages, this allows debugging a page on a deployed site.

- Listed :

The Liquid ``` {% nav %} ``` generates a menu ([doc](http://doc.locomotivecms.com/templates/tags#nav-section)) based on your page. Control here weither this page appears in the generated menu.

- Redirect :

If you check this, you can redirect the page to a url.

It then will be a 301 redirection, which from a SEO point of view, is a permanent redirection. You should use it when you have changed your urls. The search engine will then forget the previous page and update the url in its database.


``` source: [https://groups.google.com/d/topic/locomotivecms/UoNFhChvpOQ/discussion](https://groups.google.com/d/topic/locomotivecms/UoNFhChvpOQ/discussion)``` 

- Cache strategy :

Define here the cache strategy for this page.


### Recipe : Create a RSS feed <a name="rss_feed"></a>


## Models <a name="models"></a>

https://groups.google.com/forum/#!topic/locomotivecms/GGUJfwvzS9k

https://groups.google.com/d/topic/locomotivecms/xV3KR-IlOmI/discussion


### Basics <a name="models_basics"></a>

### Models mapping <a name="models_mapping"></a>

### Templatize a model <a name="models_templatize"></a>

 The idea of a templatized page is that it is a view of one instance of the model you specified in the templatized option. 
 
 The templatized mechanism expects to have "models" under a parent folder which makes more sense for SEO purpose actually (well that's my opinion).

A quick example:

/home
/products (<= used for instance to list of your products, could also be a redirect page)
/products/template (templatized => true, model => Products)

Note: in the 2.0 version, you can not have multiple nested levels of templatized pages. It will be possible with the 2.1 version, if you need this feature now, have a look at [this](https://github.com/locomotivecms/engine/tree/2.1-dev) branch and [this](https://github.com/locomotivecms/engine/pull/125) discussion.

sources :
https://groups.google.com/d/topic/locomotivecms/EQ-leEOgU2U/discussion

note : 

nested pas possible pour l'instant, voir la branche 2.1 wildcards








### Rendering models

https://groups.google.com/forum/#!topic/locomotivecms/talQGR12CQ8



### Recipe : Public Submission <a name="public_submission"></a>

https://github.com/locomotivecms/engine/blob/master/features/public/contact_form.feature



////
Very big form, session problem hack : https://github.com/locomotivecms/engine/issues/418



## Locomotive Editor <a name="locomotive_editor"></a>

lien: Carrier Wave seems to be locking down ThemeAsset to only allow images

https://groups.google.com/forum/#!topic/locomotivecms/r7f-54gSg0U

## Using Locomotive in an existing Rails app <a name="locomotive_rails_app"></a>

sources :

https://groups.google.com/d/topic/locomotivecms/suBZggHJ0OI/discussion


https://groups.google.com/d/topic/locomotivecms/ZMhKPe78pZM/discussion




## Tips <a name="tips"></a>

### Using multi-sites <a name="multi_sites"></a>

notes : 

Well, each site is fully independent form the others: they have different pages, domains, content types, ...etc. The ONLY part they have in common is that they have a "administration" access point based on a common domain name as explained in the guide (http://doc.locomotivecms.com/guides/multisites) but since we can use domain aliases, it's not a problem at all.


dev locally : https://groups.google.com/d/topic/locomotivecms/nmgDaCdb7Ts/discussion

### Export site <a name="export_site"></a>

``` source: [https://groups.google.com/d/topic/locomotivecms/odfojqSPeC4/discussion](https://groups.google.com/d/topic/locomotivecms/odfojqSPeC4/discussion) ```

In Locomotive 1.0, there was an export feature, which allowed to export the site (template, models, entries) in a zip. 

This is no longer the case in Locomotive 2.0, since it now uses a REST API for push & pull commands.

Pushing a site (a Locomotive Editor one) is described in [this](#locomotive_editor) section and [here](http://doc.locomotivecms.com/editor/deployment).

For now, there isn't any script for pulling a Locomotive app, but it's possible though, following these steps :

1. Get an auth token

```
curl -d 'email=me@mysite.com&password=secret' 'http://mysite.com/locomotive/api/tokens.json'
```

Obviously change the email and password to be valid credientials.

Response will be something like 
```
{"token":"dtsjkqs1TJrWiSiJt2gg"}
```

2. Use the token to get the information you need. 

Pages :

```curl 'http://mysite.com/locomotive/api/pages.json?auth_token=dtsjkqs1TJrWiSiJt2gg'```

Content Types :

```curl 'http://test.engine.dev:1234/locomotive/api/content_types.json?auth_token=dtsjkqs1TJrWiSiJt2gg'```

Content Entries ( assume we have a "Projects" model ) : 

```curl 'http://test.engine.dev:1234/locomotive/api/content_types/projects/entries.json?auth_token=dtsjkqs1TJrWiSiJt2gg'```

### Internationalization <a name="internationalization"></a>


localised snippets / pages

### Customise TinyMCE <a name="customise_tiny_mce"></a>

``` source: [https://groups.google.com/d/topic/locomotivecms/u5XwjR5zP8M/discussion](https://groups.google.com/d/topic/locomotivecms/u5XwjR5zP8M/discussion)``` 

TODO: test this and finish it !

Within your main app,

1. create a partial at the "app/views/locomotive/shared/_main_app_head.html.haml" location 
note: create the folders if they don't exist

2. edit the _main_app_head.html.haml file and insert this:

= javascript_include_tag  'locomotive_misc'

3. create a javascript file at the "app/assets/javascripts/locomotive_misc.js.coffee" and fill in with

window.Locomotive.tinyMCE.defaultSettings.valid_elements = "<your option>"

Note: basically, you can modify all the default tinymce settings for Locomotive. Take a look at this file if you need to check them: https://github.com/locomotivecms/engine/blob/master/app/assets/javascripts/locomotive/utils/tinymce_settings.js.coffee





### Writing custom Liquid tags <a name="custom_liquid_tag"></a>

https://groups.google.com/d/topic/locomotivecms/vfxun5pOvEY/discussion

## Appendix <a name="appendix"></a>

### Locomotive API <a name="locomotive_api"></a>

notes récupérées du google group :

I looked at the api source and it appears currently you can only query the entire model using it's slug which returns all entries in the model

https://groups.google.com/d/topic/locomotivecms/_sPLTseOnJs/discussion



# LocomotiveCMS Guide

This book is incomplete and should evolve in the future. Any contribution is very welcomed !

For any questions or advices about this book, ask [mail@geraudmathe.com](mailto:mail@geraudmathe.com), and if you need support from LocomotiveCMS team, ask [support@locomotivecms.com](mailto:support@locomotivecms.com).


## Summary

1. __[Foreword](#foreword)__
  *  __[Why this guide ?](#foreword_1)__
  *  __[Philosophy](#foreword_2)__
  *  __[Why you should use LocomotiveCMS ?](#foreword_3)__
  *  __[Assumptions](#foreword_4)__
  *  __[Organization of this book](#foreword_5)__
2. __[Overview](#overview)__
  *  __[Anatomy of a LocomotiveCMS app](#overview_1)__
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
  *  __[Rendering models](#models_rendering)__
  *  __[Templatize a model](#models_templatize)__
  *  __[Recipe: Public Submission](#public_submission)__
6. __[LocomotiveCMS Editor](#locomotive_editor)__
7. __[Using LocomotiveCMS in an existing Rails app](#locomotive_rails_app)__
7. __[Tips](#tips)__
  *  __[Using multi-sites](#multi_sites)__
  *  __[Export site](#export_site)__
  *  __[Internationalization](#internationalization)__
  *  __[Customize TinyMCE](#customize_tiny_mce)__
  *  __[Writing custom Liquid tags](#custom_liquid_tag)__
8. __[Appendix](#appendix)__

<a name="foreword"></a>
##Foreword

<a name="foreword_1"></a>
### Why this guide ?

There is already an official documentation reference, which lists almost everything. Still, a pragmatic guide to LocomotiveCMS is missing, especially for beginners.

What's more, since there is a lot of goodness in the LocomotiveCMS's <a href="http://groups.google.com/forum/?fromgroups#!forum/locomotivecms">Google Group</a>, it seems relevant to gather good practices & hacks in one place.

TBR: This guide isn't the official one, even if some members of the LocomotiveCMS core team have reviewed some parts of it.

<a name="foreword_2"></a>
### Philosophy

Let's start first with a little bit of history.
When I was a developer in a Chicago web agency, I built numerous custom and unique content management system applications for each of our clients. Even if I enjoyed crafting unique back-offices for the clients, it was clear to me that I should spend less time on that kind of projects and more on bigger ones. Besides, at this time, there were no open source Rails CMS fitting both my needs and my vision of my dream CMS. Then, I left the web agency and went back to France.

But I kept thinking about what should be the perfect CMS. My thoughts were also enhanced with my experiences as a Rails developer in other companies. Then, in my spare time, I began to code a first prototype which I constantly improved since. I also spent a lot of time trying out many concepts, and in the process learned a lot about what the perfect CMS should look like.

However, I did not question the basic requirements of my dream CMS and never moved away from them. What were they ?

- A single instance of LocomotiveCMS should host many sites. Once LocomotiveCMS is installed, setting up a new site has to be quick and does not have to require the help of an admin system guy.
- It should be super easy for the content editor people to edit the site without messing up with the layout or even breaking the site.
- Developing a LocomotiveCMS site should not require Ruby on Rails knowledge.
- It should be possible and easy to extend or customize LocomotiveCMS in an elegant way.
- The back-office should be sexy as hell.
- The code of LocomotiveCMS should not "smell". Thus, refactoring the code is a continuous process during all the development. That also means using standard components like for instance "Devise" for the authentication part.

<a name="foreword_3"></a>
### Why should you use LocomotiveCMS ?

LocomotiveCMS is a CMS that has been created with a main guideline: keep it simple !

- Keep it simple, for the lambda user who doesn't write code.
- Keep it simple, for the developer who shouldn't have to go deep in architecture, and should be able to edit a website quickly.
- Keep it simple, for the author who needs to be focused on content, and shouldn't have to go through several pages to edit.

If you recognize yourself in on of the case listed above, you should use LocomotiveCMS.

///

From a "business" point of view, LocomotiveCMS have several benefits to sell :

- Front-end editing of static texts, using Aloha editor
- Hosting on Heroku / AWS very cheap, almost free
- Finally, a great looking back-office !

<a name="foreword_4"></a>
### Assumptions

During this reading, it is assumed that:

- You know what is Ruby and Rails and you've a good feeling with terms like Gem, Bundler, deployment
- You know what is a data model, and ideally what is a document-oriented storage like Mongo
- You have basic knowledge about shell and command-line interface

<a name="foreword_5"></a>
### Organization of this book

This guide is structured as follow :

First, an *Overview* of the CMS aims to introduce the environment and the main things to know about.

*Getting something running in 5 minutes* may help LocomotiveCMS's beginners walking through the

<a name="overview"></a>
## Overview

<a name="overview_1"></a>
### Anatomy of a LocomotiveCMS app

LocomotiveCMS is crafted as an engine.

<i>
A Rails engine is an application packaged as a ruby gem that is able to be run or mounted within another Rails application. An engine can have its own models, views, controllers, generators and publicly served static files.
</i>
([more about engines](http://guides.rubyonrails.org/engines.html))

What's inside ?

- [Rails 3](http://www.rubyonrails.org)
- [Mongoid](http://www.mongoid.org)
- [Devise](http://blog.plataformatec.com.br/tag/devise)
- [Liquid](http://www.liquidmarkup.org)
- [Haml](http://haml-lang.com)
- [Formtastic](https://github.com/justinfrench/formtastic)
- [Carrierwave](http://carrierwave.rubyforge.org)

<a name="overview_2"></a>
### Key features

You have out of the box :

- Multi sites : manage multiple websites with one application instance
- Flexible content types
- Front-end inline editing (Aloha editor)
- Content localization
- Restful API
- Haml / Sass support
- Liquid templating langage
- A very nice User Interface

<a name="getting_something_running"></a>
## Getting something running in 5 minutes

TODO: définir avec géraud et didier ce que l'on fait dans cette app, liste des choses à voir : editable texts, models, templates (héritage), tags liquid de base, … ?

il n'y a pas de template de base, sauf si on achète loco editor là il y en a mais sinon non, pas dans la version 2.0


<a name="templating"></a>
## Templating

<a name="templating_1"></a>
### Templating Logic

#### Basics of inheritance

The logic in LocomotiveCMS is differs a bit from what you are certainly used to, it may be weird a first, but it's actually very simple.

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

All pages inherit from index. This way, the index contains the application's layout and the index page content. How do you re-use the layout without re-using the index page content ? By introducing ```{% block 'block_name' %} ... {% endblock %}``` : since all pages inherit from index, you declare blocks of content inside the layout (index), which will be overwritten in child pages. Here is a the simplest example :

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

To conclude with templating basics, it's worth to know that LocomotiveCMS gives you the ability to put some blocks of code in a separate file called snippet, in the same way Rails does with partials.
That's very useful when you have to build a modular layout without repeating code.

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

<a name="templating_2"></a>
### Liquid syntax

Liquid is a templating library extracted from Shopify, the project is hosted at <a href="http://liquidmarkup.org" >http://liquidmarkup.org</a>. LocomotiveCMS reuses a lot of the original library.

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

Liquid is extracted from http://www.shopify.com, but LocomotiveCMS extends it.
To cover all, we will distinguish 3 cases :
- Objects
- Filters
- Tags
which are all in the LocomotiveCMS doc : http://doc.locomotivecms.com/templates/basics
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


 img magick http://markevans.github.com/dragonfly/file.ImageMagick.html

#### Tags

editable file => https://groups.google.com/forum/#!topic/locomotivecms/hOaqFUcZCm8 only in backoffice for 2.0

example: <img src="{% editable_file 'Promotion_2_image', hint: 'Upload a promotion image (perfect size: 250px by 100px)' %} {{'promo.jpg'| theme_image_url }} {% endeditable_file %}" alt="promotion" />


<a name="templating_3"></a>
### Creating a page

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
<title>{{ page.seo_title }}</title>
<meta name="keywords" content="{{ page.keywords }}"/>
<meta name="description" content="{{ page.description }}"/>
```

#### Advanced options

![Advanced options](Locomotive-fundamentals/raw/master/images/page_advanced_options.png)

- Handle :

  Used when you integrate LocomotiveCMS with a Rails app, see [this chapter](#locomotive_rails_app).

- Response type :

  You can choose between HTML, RSS, XML or JSON. You may this way generate a RSS feed or build an simple API from your LocomotiveCMS site.

- Templatized :

  Defines wether this page should be a template for a model instance, see [this chapter](#models_templatize).


- Published :

  Since only authenticated accounts can view unpublished pages, this allows debugging a page on a deployed site.

- Listed :

  The Liquid ``` {% nav %} ``` generates a menu ([doc](http://doc.locomotivecms.com/templates/tags#nav-section)) based on your page. Control here wether this page appears in the generated menu.

- Redirect :

  If you check this, you can redirect the page to a url.

  It then will be a 301 redirection, which from a SEO point of view, is a permanent redirection. You should use it when you have changed your urls. The search engine will then forget the previous page and update the url in its database.


  ``` source: [https://groups.google.com/d/topic/locomotivecms/UoNFhChvpOQ/discussion](https://groups.google.com/d/topic/locomotivecms/UoNFhChvpOQ/discussion)```

- Cache strategy :

  Define here the cache strategy for this page.


<a name="rss_feed"></a>
### Recipe : Create a RSS feed

You can create a page which will be used as a RSS feed for your blog. In the list of pages, it is tagged with the "RSS" label.

![RSS page](Locomotive-fundamentals/raw/master/images/page_rss.png)

Assumptions:

- a "articles" page was created as well as a templatized page for an article.
- an "article" model was also created.

In order to create that kind of page, follow these steps:

- click on the "new page" button.
- select "RSS" as the response type in the "Advanced options".
- fill the template

```xml
<?xml version="1.0" encoding="UTF-8"?>
<rss version="2.0"
  xmlns:content="http://purl.org/rss/1.0/modules/content/"
  xmlns:wfw="http://wellformedweb.org/CommentAPI/"
  xmlns:dc="http://purl.org/dc/elements/1.1/"
  xmlns:atom="http://www.w3.org/2005/Atom"
  xmlns:sy="http://purl.org/rss/1.0/modules/syndication/"
  xmlns:slash="http://purl.org/rss/1.0/modules/slash/"
  >
  <channel>
    <title>{{ site.seo_title }}</title>
    <description>{{ site.meta_description }}</description>
    <link>http://www.example.com</link>
    <language>en</language>
    <copyright>Example</copyright>
    <ttl>30</ttl>
    <atom:link href="http://www.example.com/articles/rss.xml" rel="self" type="application/rss+xml" />
    {% for article in contents.articles %}
      <item>
        <title>{{ article.title }}</title>
        <description><![CDATA[{{ article.excerpt }}]]></description>
        <content:encoded><![CDATA[{{ article.body }}
        ]]></content:encoded>
        <link>http://www.example.com/articles/{{ article._permalink }}</link>
        <guid isPermaLink="true">http://www.example.com/articles/{{ article._permalink }}</guid>
        <pubDate>{{ article.posted_at | localized_date: '%a, %d %b %Y %H:%M:%S %z' }}</pubDate>
        <source url="http://www.example.com/">example.com</source>
      </item>
    {% endfor %}
  </channel>
</rss>
```

- click on the "create" button at the bottom of the screen

Note: do not forget to set the "Published" flag to true.

If you want the browsers and news readers to auto-detect your RSS feed, add the following statement within the "head" tag of your template.

``` {{ '/articles/rss.xml' | auto_discovery_link_tag }} ```

<a name="models"></a>
## Models

This chapter covers models, or the custom content, LocomotiveCMS lets you build in the UI. Here we use the word ```model``` as it's what we are used to, but in the LocomotiveCMS [reference](http://doc.locomotivecms.com/) you will see ```content type``` for ```model```, and ```content entry``` for an instance of a ```model```.

The [first](#models_basics) subchapter aims to introduce the very basic creation and usage of models, the [second one](#models_mapping) is about building relationships between your models.
We cover [then](#models_rendering) the rendering of models using Liquid, where we try to show the common use cases, and after that a [subchapter](#models_templating) is dedicated to the functionality of templating a model.
Finally, we will cover the [public submission](#models_public_submission) of models, which allows frontend users to create instances.

<a name="models_basics"></a>
### Basics

First step of model creation, specify the name of the model :

![Create model](Locomotive-fundamentals/raw/master/images/models_basics_creation.png)

As mentioned in the hint, you will reference your model in Liquid logic by its *slug*.
Then, define the fields (attributes) of your model in the following section :

![Create fields](Locomotive-fundamentals/raw/master/images/models_basics_fields.png)


#### Fields types

The following types of attributes (fields) are available :

![Types list](Locomotive-fundamentals/raw/master/images/models_basics_types_list.png)

Let's detail each one of them. The rendering of these types will be detailed [later](#models_rendering).

*Nota bene : you will encounter some properties not explained, it's because they are common to all field types and will be detailed later.*

- Simple input :

  A string, max 255 chars (?)

- Text :

  Text field, but you can choose the format.
  When you add a field :

  ![type text 1](Locomotive-fundamentals/raw/master/images/models_basics_text_1.png)

  you then have a properties panel which appears by clicking on the arrow on the right part of the line :

  ![type text 2](Locomotive-fundamentals/raw/master/images/models_basics_text_2.png)

  If you choose ```Text formatting : HTML```, you will have a WYSIWYG editor, TinyMCE :

  ![type text tinymce](Locomotive-fundamentals/raw/master/images/models_basics_text_tinymce.png)

   And if you choose ```Text formatting : none```, you will have a simple textarea :

   ![type text textarea](Locomotive-fundamentals/raw/master/images/models_basics_text_textarea.png)

- Select :

  Displays a select list of options for the field.

  ![type select](Locomotive-fundamentals/raw/master/images/models_basics_select.png)

  You have to put the options of the list in the property of the field, here we have "frontend" "backend" and "api" for the example. This type of attribute is handy, since it may avoids you the creation of a third-party model for simple lists like this one.

  The options are both editable from the model editing page and the model instance creating/editing pages.

- Checkbox :

  A simple boolean field. The "Required" attribute will not applied to that kind of field.

- Date :

  A date field which is editable by this :

  ![type date](Locomotive-fundamentals/raw/master/images/models_basics_date.png)

  But if you need a field for "updated at" or "created at", be aware this attributes already exist by default, they are rendered with ```entry.created_at``` and ```entry.updated_at```.


- File :

  A field of this type supports the upload of any kind of file.


The other fields specifying a relationship with an other model (```belongs_to```, ```has_many``` and ```many_to_many```) will be explained in the next section, [Models mapping](#models_mapping).

**Common fields properties :**

When you define an attribute (or a field) for your model, you have some properties which are specific for each kind of attribute (detailed previously), and some which are common to every one.

![type properties](Locomotive-fundamentals/raw/master/images/models_basics_properties.png)

- Required / Optional :

  Defines wether this field is required or not for the validation.

  A model must have at least one field, that's obvious, and the first field you will define will be considered as the mandatory one, and will be automatically saved as ```Required```. There is one exception though: you can't have a mandatory field whose type defines a relationship with an other model.

- Name :

  Make sure the name of the field *highlighten in yellow here* match the "Name" property bellow. As the tip explains "Name of the property for liquid templates", it will be this value you will have to use in the liquid template, and not the value highlighten in yellow.

  It seems obvious, but if you change the name of the field (the one highlighten in yellow) and forgot the update accordingly the value of the field bellow, you will not be able to retrieve the object in Liquid and may wonder why...

- Hint :

  Hint for the end user of the back-office, displayed in the model form just below the field.

- Localized :

  Used for internationalization, detailed [here](#internationalization).

<a name="presentation_and_advanced_options"></a>
#### Presentation and Advanced options

When the attributes of the model are defined, click on "Create" to edit advanced options of the model :

![models advanced props](Locomotive-fundamentals/raw/master/images/models_basics_advanced.png)

For the purpose of the example, the following model will be used in what's follow :


![models advanced model](Locomotive-fundamentals/raw/master/images/models_basics_advanced_model.png)

So let's say we have a model of posts, with a title (string), some text (text), a category (select with options "frontend" and "backend") and a publishing_date (date).

**Presentation**

Theses options let you customize how entries of your model are displayed in the back-office page.

- Label field :

  Choose the field of the model displayed for each entry.

  If you choose the field ```title```, you have :

  ![models advanced label 1](Locomotive-fundamentals/raw/master/images/models_basics_advanced_label1.png)


  And if you choose ```publishing_date```, you end up with :

  ![models advanced label 2](Locomotive-fundamentals/raw/master/images/models_basics_advanced_label2.png)

- Group by field :

  Group entries by a common field value. This is available only for fields which have the type ```select```. So here we can group by category :

  ![models advanced group by](Locomotive-fundamentals/raw/master/images/models_basics_advanced_groupby.png)

- Item template :

  Let you really customize the string displayed for each entry in the list of model's entries.
  For example, let's say I don't display my posts grouped by category, but still I would like the category appears aside the post title, I would do so :

  ![models advanced item template](Locomotive-fundamentals/raw/master/images/models_basics_advanced_itemtemplate.png)

  And my entries would display this way :

  ![models advanced item template1](Locomotive-fundamentals/raw/master/images/models_basics_advanced_itemtemplate1.png)


**Advanced options**

And finally, last properties of your model:

- Order by, Order direction :
  The ordering of items in your model, both in frontend and in backend.

- Public Submission :
  Let frontend users create entries for the model, so typically you would use that option in a model "messages" for a contact form. This is detailed [here](#models_public_submission).


<a name="models_mapping"></a>
### Models mapping

LocomotiveCMS lets you define the relationships between models you are used to in Rails, but the creation and usage of these mapped models can sometimes be uneasy, so let's see for each one of them of to deal with.

This part is dedicated to the models creation and mapping in the admin UI, and the template code shown here will be very concise and simple. For more about displaying models, read the next part.

We will see the ```belongs to```, ```has many``` and ```many to many``` relationships and finally look at more complex mapping


#### Belongs to

We have the model ```books``` belongs_to ```authors```.

First, we create the model ```authors``` in its simplest form :

![authors](Locomotive-fundamentals/raw/master/images/belongsto_authors.png)

The mapping with the model ```books``` will be defined in ```books```.
Let's create ```books```:

![books step 1](Locomotive-fundamentals/raw/master/images/belongsto_books_1.png)

We give him a ```title``` field, and a ```writer``` field which defines the ```belongs_to``` relationship.

But wait, we haven't mapped ```books``` to ```authors``` yet, so click on the "add" button and then on the down arrow to specify more options concerning this field :

![books arrow](Locomotive-fundamentals/raw/master/images/belongsto_arrow.png)

You then have the option panel where you choose the ```Class name``` of the model targeted by the belongs_to relationship :

![books step 2](Locomotive-fundamentals/raw/master/images/belongsto_books_2.png)

We are done, so click on the "Create" button to save the model.

*Nota Bene : the name of the field defining the belongs_to relationship (here 'writer') can be named as you want, you don't have to name it the singular of the targeted model (here it would be 'author'), even if it may be a good practice. (???)*

Now we have our models defined, let's add some dummy entries. We will create a new book entry :

![books entrie empty](Locomotive-fundamentals/raw/master/images/belongsto_book_entrie_empty.png)

We can give him a name, but the ```writer``` list is empty, right, because ```authors``` model hasn't any entries yet.

So create an author :

![author add](Locomotive-fundamentals/raw/master/images/belongsto_author_entrie.png)

And then go back in the book creation page, the author appears in the ```writer``` list :

![book entrie valid](Locomotive-fundamentals/raw/master/images/belongsto_book_entrie_valid.png)

Great, save the entry and we will check if it works.

In a dummy page, we loop on ```books``` entries, and for each one (here the only one), we display the title of the book and its writer :

```
{% for book in contents.books %}
  {{ book.title }} written by {{ book.writer.name }} is a great lecture.
{% endfor %}
```
and it displays : ```Responsive Web design written by Ethan Marcotte is a great lecture.```.

#### Has many

Let's keep the previous models, and let's say ```books``` have ```reviews```.
A review is basically a piece of text, and it is often published in a media. What's more, a book has many reviews, but a review refers to one and only one book. So we are indeed in the relationship ```books``` has_many ```reviews```.

First, we create the ```reviews``` model, which has a string field ```journal``` (in which the review is published) and a text field ```content```, and also a belongs_to field ```book``` :

![book reviews](Locomotive-fundamentals/raw/master/images/hasmany_reviews.png)

**The ```belongs_to``` field targeting the parent model class name, ```books```, is required !**

![book reviews belongs to field](Locomotive-fundamentals/raw/master/images/hasmany_reviews_belongsfield.png)

Save the model, and now go editing ```books```. We will add a has_many field named ```reviews```, targeting the ```Class name``` ```reviews```, and ```Inverse of``` itself, so ```books``` :

![book editing](Locomotive-fundamentals/raw/master/images/hasmany_books.png)

*Nota Bene : here again, the name of the field defining the has_many relationship (here 'reviews') can be named as you want.*

Save the updated ```books``` model, and let's edit the previous ```books``` entry :

![book entrie](Locomotive-fundamentals/raw/master/images/hasmany_bookentrie_1.png)

Let's add a review to this book, click on "Add a new entry" and fill it with dummy text :

![book entrie reviewed](Locomotive-fundamentals/raw/master/images/hasmany_bookentrie_2.png)

Click on "Save" to close the modal window and create the ```reviews``` entry related to this ```books``` entry. Click again on "Save" to update the ```book```.

In the previous dummy page where we tested the belongs_to relationship, we add a loop on the reviews of a book :

```
{% for book in contents.books %}
  {{ book.title }} written by {{ book.writer.name }} is a great lecture.
  <br>
  Reviews :
  <br>
  {% for review in book.reviews %}
    Published in {{ review.journal }} : {{ review.content }}
  {% endfor %}
{% endfor %}
```

and it displays :

```
Responsive Web design written by Ethan Marcotte is a great lecture.
Published in Web design monthly :
Awesome book, blablabla ...
```

##### UI enabled


When you added ```review``` to your ```book``` writer, it was possible because of the property "Ui enabled" of your ```has many``` field. This property is set to ```true``` by default :

![tips ui enable](Locomotive-fundamentals/raw/master/images/models_tips_uienable.png)

This property sets either you can edit and create a child model entry from a parent model entry, or not.

#### Many to many

Finally, we will add the ability to associate tags to a book. Here, the model ```tags``` have many ```books``` and ```books``` have many ```tags```.

Let's create the ```tags``` model.

![tags creation 1](Locomotive-fundamentals/raw/master/images/manytomany_tags_1.png)
![tags creation 2](Locomotive-fundamentals/raw/master/images/manytomany_tags_2.png)

We have the ```books``` field referring to books via a ```many_to_many``` relationship. Like previously, we specify the ```Class name``` of the targeted model, which is ```books```. We also have to specify ```Inverse of``` (itself) ```tags``` here, but we can't, the select list is empty.

For now, save the  ```tags``` model, we will get back here soon.
Go edit the  ```books``` model and add, as you may guessed, the ```tags``` field referring to tags via a ```many_to_many``` relationship. The ```Class name``` of the targeted model is ```tags```. Yes I'm repeating myself a little, just in case.
But here you can define the ```Inverse of``` (itself) which is ```books```, so do it please :

![books m2m update](Locomotive-fundamentals/raw/master/images/manytomany_books.png)

Then save the ```books``` model and go back editing ```tags``` model : magic, the ```Inverse of``` attribute of the many_to_many field ```books``` is field with the appropriate value ```tags``` :


![tags inverse of update](Locomotive-fundamentals/raw/master/images/manytomany_tags_inverseof.png)

Here it is, your many to many is settled.

Now we will add some tags to our book, but unlike the previous cases, you can't create a tag entry in the book entry page :

![m2m enable ui problem](Locomotive-fundamentals/raw/master/images/manytomany_enableui.png)

We have to create a new tag entry separately, and then add it when editing the book entry. So we do :

![m2m tag available in book](Locomotive-fundamentals/raw/master/images/manytomany_tag_entrie.png)


You don't have to select here the book entry you want to connect the tag.
And when we go back to the book entry we were editing, the tag is available in the select list :

![m2m tag available in book](Locomotive-fundamentals/raw/master/images/manytomany_books_tags_available.png)

So let's (finally !) add our tag to our book, save, and check if everything is okay back in frontend :

```
{% for book in contents.books %}
  {{ book.title }} written by {{ book.writer.name }} is a great lecture.
  <br>
  Tags :
  <br>
  {% for tag in book.tags %}
    "{{ tag.text }}"
  {% endfor %}
{% endfor %}
```

displays :

```
Responsive Web design written by Ethan Marcotte is a great lecture.
Tags :
"responsive"
```

And if we do the opposite, it's okay too (as expected after so much pain) :

```
{% for tag in contents.tags %}
  Tag : "{{ tag.text }}" is related to the following books :
  <br>
  {% for book in tag.books %}
    {{ book.title }} written by {{ book.writer.name }}
  {% endfor %}
{% endfor %}
```

displays :

```
Tag : "responsive" is related to the following books :
Responsive Web design written by Ethan Marcotte
```

#### More complex mapping

TODO: considerations about nested relationships and performance of associated mongo queries


<a name="models_rendering"></a>
### Rendering models

In this subchapter, we will try to show the most common cases of rendering a model entries. It would be tedious to list every possible cases, the aim is only to give an overview of what's possible.

First we will see the very basics of iterating over a collection of entries and the available logic you can add, then the pagination of results and finally the scoping the query of results.

#### Basics


The simplest loop is an iteration over your model entries. We loop here on the model ```posts```, the one from the previous [Models basics](#models_basics) subchapter:


  {% for item in contents.posts %}
  <p>{{ item.title }}</p>
  {% endfor %}

Loop over ```contents.slug_of_your_model```, and for each entry you have access to the custom fields of your model, and also to a list of attributes :

![entries attributes](Locomotive-fundamentals/raw/master/images/entries_attributes.png)

[Reference](http://doc.locomotivecms.com/templates/tags#for-section)


**Adding logic**

Logic liquid tags let you put some logic inside your loops.

- if / else / unless : [Reference](http://doc.locomotivecms.com/templates/tags#if-else-unless-section)
- case : [Reference](http://doc.locomotivecms.com/templates/tags#case-section)

What about empty fields ?

You will certainly have some ```required``` fields, and some not, so how to not display an attribute if it's empty ?
An empty field will actually return ```nil```, so you can test it in a ```if``` statement :

```html
{% for item in contents.posts %}
  {% if item.category %}
    <div class="category">{{ itme.category }}</div>
  {% endif %}
{% endfor %}
```


**Rendering model's attributes**

We will see here how to render each type of attribute.

- Simple input :

  Nothing tricky here, ```title``` is our simple input field :

  ```html
  {% for item in contents.posts %}
  <p>Title of the post : {{ item.title }}</p>
  {% endfor %}
  ```

- Text :

  This field has an property ```Text formatting``` HTML or none.
  In the first case, you can edit the content of it with a WYSIWYG editor, in the other case it's just a textarea.
  It doesn't change anything for the rendering, in one case the output will be HTML formatted and in the other it will be a simple long string.

  ```main_text``` is our text field :

  ```html
  {% for item in contents.posts %}
  <p>Content of the post : {{ item.main_text }}</p>
  {% endfor %}
  ```

- Select :

  Display the current value of an entry's select field. You will not be able to list all the select options of a model, just the current value.

  ```category``` is our select field :

  ```html
  {% for item in contents.posts %}
  <ul>
    <li>Category of the post : {{ item.category }}</li>
  </ul>
  {% endfor %
   ```

- Checkbox :

  This field is a simple boolean one. So you may use it mainly for templating logic, but you can also display it's raw values ('true' or 'false').

  Here the checkbox field is ```is_a_report``, let's first use it for logic :


  ```html
  {% for item in contents.posts %}
    {% if item.is_a_report %}
      This item is a report.
    {% else %}
      This item is not a report
    {% endif %}
  {% endfor %}
   ```

  And let's display it's raw value :

  ```html
  {% for item in contents.posts %}
    Is it a report ? {{  item.is_a_report }}
  {% endfor %}
   ```

- Date :

  Every entry has by default the properties ```created_at``` and ```updated_at```. But if you need an additional date field, like ```publishing``` in the above example, here is how to render it :


  ```html
  {% for item in contents.posts %}
    Publishing date : {{  item.publishing }}
  {% endfor %}
   ```

   It will render a raw ISO date, if you need to format it, have a look at the next part "Don't forget Liquid filters".


- File :

  The only thing you can do with a file field is display its url. You must specify the property ```url``` when you display it, like in the above example with an ```attached``` field :

  ```html
  {% for item in contents.posts %}
    Attached file url : {{  item.attached.url }}
  {% endfor %}
   ```

  One of the usual case is displaying images. In this case, you would simply have to encapsulate the url in an image HTML tag, and if you need to resize it have a look at the next section.

  ```html
  {% for item in contents.posts %}
    <img src="{{  item.attached.url }}">
  {% endfor %}
   ```

**Usage of capture and assigns**

- Capture :

  Combine a number of strings into a single string and save it to a variable.

  May be useful to clean your Liquid template if you have a lot of logic and formatting, or simply to keep it DRY.


  ```html
  {% for item in contents.posts %}
    {% capture full_titile %}{{ item.title }} - {{ item.category }}{% endcapture %}
    {{ full_title }}
  {% endfor %}
   ```

  [Reference](http://doc.locomotivecms.com/templates/tags#capture-section)

- Assigns :

  Can be used to assign a value to a variable.

  [Reference](http://doc.locomotivecms.com/templates/tags#assigns-section)



**Don't forget Liquid filters**

Some Liquid filters will allow you to format your entries attributes.

- Resize image :

  An image rendered from a file's field is done this way :
  ```html
  <img src="{{  item.attached.url }}">
  ```

  But the the DragonFly gem can resize any image on the fly behind the scene.
  The url to the dynamically resized image is returned. The processing relies on ImageMagick. Here is an example :

  ```html
  <img src="{{  item.attached.url | resize: '100x100' }}">
  ```

  Just add the "resize" filter, which takes the ImageMagick geometry string for argument. Here is a list of arguments examples taken from the [Dragonfly doc](http://markevans.github.com/dragonfly/file.ImageMagick.html):


  ```
  '400x300'            # resize, maintain aspect ratio
  '400x300!'           # force resize, don't maintain aspect ratio
  '400x'               # resize width, maintain aspect ratio
  'x300'               # resize height, maintain aspect ratio
  '400x300>'           # resize only if the image is larger than this
  '400x300<'           # resize only if the image is smaller than this
  '50x50%'             # resize width and height to 50%
  '400x300^'           # resize width, height to minimum 400,300, maintain aspect ratio
  '2000@'              # resize so max area in pixels is 2000
  '400x300#'           # resize, crop if necessary to maintain aspect ratio (centre gravity)
  '400x300#ne'         # as above, north-east gravity
  '400x300se'          # crop, with south-east gravity
  '400x300+50+100'     # crop from the point 50,100 with width, height 400,300
  ```

  Resized images are cached by default by the Rack::Cache middleware. If you host your LocomotiveCMS on Heroku, Varnish is used instead. For more information, please visit [this](http://markevans.github.com/dragonfly/file.Caching.html) page.

  [Reference](http://doc.locomotivecms.com/templates/filters#resize-section)

- Format / Localize a date :

  If you have a ```date``` field, or if you simply use the properties ```created_at, updated_at``` of an entry, you may wanna format it.
  Here it is :

  ```{{ item.created_at | localized_date: '%d %B' }}```

  The ```localized_date``` takes as argument the format string which depends on the current locale. There is also an optional argument 'locale', if you need to force an other locale than the current one :

  ```{{ item.created_at | localized_date: '%d %B', 'fr' }}```

  [Reference](http://doc.locomotivecms.com/templates/filters#localized-date-section)

- Format text :

  There is several filters allowing text formatting,

  - Underscore : Makes an underscored, lowercase form from the expression in the string. [Reference](http://doc.locomotivecms.com/templates/filters#underscore-section)
  - Dasherize : Replaces underscores with dashes in the string. [Reference](http://doc.locomotivecms.com/templates/filters#dasherize-section)
  - Multi_line : Inserts a <br /> tag in front of every \n linebreak character. [Reference](http://doc.locomotivecms.com/templates/filters#multi-line-section)
  - Concat : Append strings passed in args to the input [Reference](http://doc.locomotivecms.com/templates/filters#concat-section)
  - Textile : Convert a Markdown-formatted string into HTML. [Reference](http://doc.locomotivecms.com/templates/filters#textile-section)

- Embed Flash tag : Embed a flash movie into a page given an url [Reference](http://doc.locomotivecms.com/templates/filters#flash-section)


**Displaying Grouped By**

In the [Presentation and Advanced options](#presentation_and_advanced_options) of the Basics subchapter, we saw how customize the displaying of a model's entries.
One of the options is to group entries by a field, at the condition the field you want group_by entries is a ```select``` type.

In frontend, you also have this feature. Here is an example, using the ```posts``` model, the one from the previous [Basics](#models_basics) subchapter.


  {% for cat in contents.posts.group_by_category %}
    <br>
    {{ cat.name }} :
    {% for entrie in cat.entries %}
      <br>
      {{ entrie.title }}
    {% endfor %}
  {% endfor %}


The syntax is the following : ```contents.slug_of_your_model.group_by_field_name```, with the field name corresponding to the ```select``` type field you group by entries.

As explained in the documentation : *The method returns an ordered Array of Hash. Each Hash stores 2 keys, name which is the name of the option and entries which is the list of the ordered entries for the option. The Array is ordered based on the order of the options set in the back-office.*

So we first loop on each value of the ```select``` type field, which is "cat", and then for each "cat" we loop on each entries of this category.


#### Paginate entries

LocomotiveCMS comes with a paginate tag.

  {% paginate contents.posts by 3 %}
      {% for post in paginate.collection %}
        <p>{{ post.title }}</p>
      {% endfor %}
  {% endpaginate %}

It creates a ```paginate``` objects with the following attributes :

![paginate attributes](Locomotive-fundamentals/raw/master/images/paginate_object.png)

There are all pretty straightforward, but let's have a look at the ```parts``` attributes, it seems there is everything here to build the navigation of your paginated pages.

Here is an example of how we would to it :

  {% paginate contents.posts by 1 %}
    {% for post in paginate.collection %}
      <p>{{ post.title }}</p>
      {% endfor %}

    <!-- pagination links -->
    {% for page in paginate.parts %}
      {% if page.is_link %}
        <a href="{{ page.url }}" >{{ page.title }}</a>
      {% else %}
        {{ page.title }}
      {% endif %}
    {% endfor %}
  {% endpaginate %}


Well, that's fine if you want have the control of your markup, but if you don't, there is the filter ```default_pagination``` for rendering a clean pagination navigation without pain :

  {% paginate contents.posts by 1 %}
    {% for post in paginate.collection %}
      <p>{{ post.title }}</p>
      {% endfor %}
    <!-- pagination links -->
    {{ paginate | default_pagination, next: 'Next', previous: 'Previous' }}
  {% endpaginate %}

Which takes 2 arguments :

![paginate filter](Locomotive-fundamentals/raw/master/images/paginate_filter.png)

And which renders this kind of markup :

```html
<div class="pagination ">
  <span class="disabled prev_page">« Previous</span>
    <span class="current">1</span>
    <a href="/posts?page=2">2</a>
    <a href="/posts?page=3">3</a>
  <a href="/posts?page=2" class="next_page">Next »</a>
</div>
```

[Reference : paginate](http://doc.locomotivecms.com/templates/tags#paginate-section)

[Reference : default paginate](http://doc.locomotivecms.com/templates/filters#default-pagination-section)


#### Scope results


http://doc.locomotivecms.com/templates/tags#with-scope-section

{% with_scope _slug: params.section %}
{% assigns section = contents.sections.first %}
{% endwith_scope %}

{% for article in section.articles %}
...
{% endfor %}



with_scope: replace _permalink by _slug in the query
https://github.com/locomotivecms/engine/issues/449


<a name="models_templatize"></a>
### Templatize a model

The idea of a templatized page is that's a view of one instance of a model you specify in the ```templatized``` option of a page.

Here is how it works : let's say you have the model ```posts```, the one from the previous [Basics](#models_basics) subchapter. You need to have the following pages structure :

![templatized page archi](Locomotive-fundamentals/raw/master/images/templatize_archi.png)

With :

- **posts** page :

  The templatized mechanism expects to have "models" under a parent folder which makes more sense for SEO purpose. This page can be used for instance to list the products, or could be a redirect page.

  So for the example, here are the parameters of the page, but there isn't anything specific here :

  ![templatized page posts](Locomotive-fundamentals/raw/master/images/templatize_posts1.png)

  ![templatized page posts 2](Locomotive-fundamentals/raw/master/images/templatize_posts2.png)

  Again for the example, we could list in this page the entries of ```posts``` model, and display the link of each entry. You have to build the relative url according this page's slug, and using the ```_permalink``` attribute of a model instance.

  ![templatized page posts liquid](Locomotive-fundamentals/raw/master/images/templatize_posts_liquid.png)


- **template** page :

  The template of the templatized model is defined as follow :

  ![templatized page 1](Locomotive-fundamentals/raw/master/images/templatize_template1.png)

  You set the page **posts** as the *parent* of this one. A templatized page **must** have a *parent*, other than index.

  ![templatized page 2](Locomotive-fundamentals/raw/master/images/templatize_template2.png)

  Set the parameter ```Templatized``` as true, and select bellow the model you want to templatize.

  To follow the example, we will display a full post, using directly the ```posts``` instance (notice the singular) :

  ![templatized page template liquid](Locomotive-fundamentals/raw/master/images/templatize_template_liquid.png)



**Note: in LocomotiveCMS 2.0, you can't have multiple nested levels of templatized pages. It will be possible with the 2.1 version, if you need this feature now, have a look at [this](https://github.com/locomotivecms/engine/tree/2.1-dev) branch and [this](https://github.com/locomotivecms/engine/pull/125) discussion.**


<a name="models_public_submission"></a>
### Recipe : Public Submission

https://github.com/locomotivecms/engine/blob/master/features/public/contact_form.feature



////
Very big form, session problem hack : https://github.com/locomotivecms/engine/issues/418


<a name="locomotive_editor"></a>
## LocomotiveCMS Editor

lien: Carrier Wave seems to be locking down ThemeAsset to only allow images

https://groups.google.com/forum/#!topic/locomotivecms/r7f-54gSg0U

<a name="locomotive_rails_app"></a>
## Using LocomotiveCMS in an existing Rails app

sources :

https://groups.google.com/d/topic/locomotivecms/suBZggHJ0OI/discussion


https://groups.google.com/d/topic/locomotivecms/ZMhKPe78pZM/discussion




<a name="tips"></a>
## Tips

<a name="multi_sites"></a>
### Using multi-sites

notes :

Well, each site is fully independent form the others: they have different pages, domains, content types, ...etc. The ONLY part they have in common is that they have a "administration" access point based on a common domain name as explained in the guide (http://doc.locomotivecms.com/guides/multisites) but since we can use domain aliases, it's not a problem at all.


dev locally : https://groups.google.com/d/topic/locomotivecms/nmgDaCdb7Ts/discussion

<a name="export_site"></a>
### Export site

*This tip was given [here](https://groups.google.com/d/topic/locomotivecms/odfojqSPeC4/discussion)*

In LocomotiveCMS 1.0, there was an export feature, which allowed to export the site (template, models, entries) in a zip. This is no longer the case in LocomotiveCMS 2.0, since it now uses a REST API for push & pull commands.

Pushing a site (build with LocomotiveCMS Editor) is described in [this](#locomotive_editor) section and [here](http://doc.locomotivecms.com/editor/deployment).

For now, there isn't any pulling script, but still it's possible, following these steps :

- Get an auth token

  ```
  curl -d 'email=me@mysite.com&password=secret' 'http://mysite.com/locomotive/api/tokens.json'
  ```

  Obviously change the email and password to be valid credientials.

  Response will be something like
  ```
  {"token":"dtsjkqs1TJrWiSiJt2gg"}
  ```

- Use the token to retrieve needed data.

  Pages :

  ```
  curl 'http://mysite.com/locomotive/api/pages.json?auth_token=dtsjkqs1TJrWiSiJt2gg'
  ```

  Content Types :

  ```
  curl 'http://mysite.com/locomotive/api/content_types.json?auth_token=dtsjkqs1TJrWiSiJt2gg'
  ```

  Content Entries ( assume we have a "Projects" model ) :

  ```
  curl 'http://mysite.com/locomotive/api/content_types/projects/entries.json?auth_token=dtsjkqs1TJrWiSiJt2gg'
  ```


<a name="internationalization"></a>
### Internationalization

LocomotiveCMS handles internationalization easily. We will see how set up several languages, deal with localized pages and models, and add a custom language to an app.


#### Setup

First thing first, choose the languages you want support in the *Settings* panel :

![internationalization panel](Locomotive-fundamentals/raw/master/images/internationalization_1.png)

Select your locales and save.
If you go back in the 'Contents' tab, you will see a locale switcher at the right part of the menu :

![internationalization locale switcher](Locomotive-fundamentals/raw/master/images/internationalization_2.png)

There is several kind of content you may wanna translate :


- HTML template of pages
- editable contents in pages
- content entries (your models entries)

#### Page translation

A page has as many HTML templates as locales in the app. You can

Let's try it : create a page named ```lambda``` with the following template :

```html
That's it, a page in english
The editable custom text in english.
```

At ```test.herokuapp.com/lambda``` we have :

```html
That's it, a page in english
The editable custom text in english.
```

Go back to the admin, and switch to the French locale. You will see that LocomotiveCMS indicates pages which are not yet translated :

![internationalizationeng page](Locomotive-fundamentals/raw/master/images/internationalization_untranslated.png)

**Notice :
Pages which are not translated are not available in front, they generates a 404.**

Edit the ```lambda``` page in French this time, we will edit both the template and the custom content (editable_short_text) :

```html
Et voila, une page en francais.
{% editable_short_text 'custom-content' %}
Le contenu editable en francais.
{% endeditable_short_text %}
```

And now at ```test.herokuapp.com/fr/lambda``` (notice the locale prefix) we have the translated page :

```html
Et voila, une page en francais.
Le contenu editable en francais.
```


Since the default locale of this app is English, the lambda page url is ```test.herokuapp.com/lambda``` but is also ```test.herokuapp.com/en/lambda```. It's your choice to redirect all url to the prefixed one, which may make sense for SEO purpose.


#### Models translation

Let's create a dummy model with a string field :

![internationalization model](Locomotive-fundamentals/raw/master/images/internationalization_model.png)

See the ```localized``` checkbox ? It's all it takes to localize a model field ! You can localize the fields you want, but not necessarily all custom fields.

Let's create an entry in english, save it, and switch to French, and translate this entry.

**Notice :
LocomotiveCMS indicates which entries are not translated, as it does with pages. But unlike pages, a model entry which isn't translated yet will still appears in front.**

Then we go back to our lambda page and display the dummy models entries. Don't forget to update the template in both locales :

```html
{% for item in contents.dummy %}
{{ item.title }}
{% endfor %}
```
As expected, ```test.herokuapp.com/en/lambda``` displays

```
My dummy entry in english.
```

and  ```test.herokuapp.com/fr/lambda``` displays

```
Mon entree en francais.
```

#### Internationalize the template

**Locale switcher in front**

The first thing you may need is a locale switcher, allowing front users to choose their language. There is a Liquid tag for that : ```{% locale_switcher %}```. The params of the tag are explained [here](http://doc.locomotivecms.com/templates/tags#locale-switcher-section).

**Variables**

There is also global variables relating to locales ([reference](http://doc.locomotivecms.com/templates/objects)) :

- ```locale``` gives is the current locale, which you may use in logical statements :

  ```html
  {% if locale == 'en' %}
  English content
  {% else %}
  Contenu francais
  {% endif %}
  ```
- ```locales``` is an array of the site's locales :

  ```html
  {% for loc in locales %}
  {{ loc }}
  {% endfor %}
  ```

- ```default_locale``` is the default site's locale.


**Duplicated templates**

LocomotiveCMS's behavior allows a different page's template for each locale. So let's say you have developed your site in English only, everything is developed, and then you add a locale : pages will be duplicated in the new locale.

It's not possible to specify one template for every locales, so if you don't want to duplicate every page template at each code update, it could be easier to validate the site in one locale, and then at the end, adding the other locales.

**Localized links**

Your template will have relative links to the app page, but if you don't localize them, they will point to the default localized page, and not the current localized one.

Making them localized is pretty simple, just add the locale prefix :
```html
<a href="/{{ locale }}/target-page">click here</a>
```
This way, each link will point to the current localized page.


#### Add a language

LocomotiveCMS comes with the following available locales :

![internationalization locales](Locomotive-fundamentals/raw/master/images/internationalization_locales.png)

What if you need for example to have a Chinese locale ? There is 2 things to consider with locales :

- back-office language
- url prefixes (for SEO purpose)

There is a way to add custom locale without pain.

1. Install a LocomotiveCMS engine as described in the [reference](http://doc.locomotivecms.com/installation/getting_started)
2. Once you have run ```bundle exec rails g locomotive:install```, go in ```config/initializers/locomotive.rb```


  ```
  # default locale (for now, only en, de, fr, pt-BR, it and nb are supported)
  #config.default_locale = :en

  # available locales suggested to "localize" a site. You will have to pick up at least one among that list.
  #config.site_locales = %w{en tw}
  ```

  The first line specifies the default locale, but as mentioned, you can't specify a locale which doesn't exists in LocomotiveCMS defaults locales.

  The second one specifies the list of available locales for the site. That's where you will add your custom locale. You can also remove all unwanted locales, so that they don't appear in the admin panel.


3. When your custom locale is added in the LocomotiveCMS config, you need to add the .png image of the locale flag in the application assets, which will appears in the admin panel (if you don't, it will raises an error) :

  ![internationalization locales](Locomotive-fundamentals/raw/master/images/internationalization_locales.png)

  The .png image is 24x24px. Put this file in the main Rails app, in the folder ```/app/assets/images/locomotive/icons/flags/```. The name of the image must be your locale's name, for example Taiwanese flag must be named ```tw.png```.

4. Precompile your assets : ```bundle exec rake assets:precompile```
5. Add translations.

  LocomotiveCMS backoffice have the following translation files (with LOCALE being the shortname of each locale) :

  ```
  admin_ui.LOCALE.yml
  carrierwave.LOCALE.yml
  default.LOCALE.yml
  devise.LOCALE.yml
  flash.LOCALE.yml
  formtastic.LOCALE.yml
  ```

  The first thing to know is i18n will look for ```default_locale``` translations if there isn't any translation for the current locale. In the case you want for example a backoffice in English for every locale, you don't need to add any custom translation, since it will fallback to the ```default_locale```.

  The second thing is, in every case, you need to add the translation of your custom locale name in the ```admin_ui.LOCALE.yml``` above the others :

  ```
  locales:
      en: English
      de: German
      fr: French
      pt-BR: "Brazilian Portuguese"
      it: Italian
      nl: Dutch
      nb: Norwegian
      es: Spanish
      ru: Russian
  ```

  Add here your custom locale, in order to display it in the backoffice panel.

  *To summarize :*

  If your custom locales doesn't have to be translated in the admin, define your ```default_locale``` and just add your custom locale name translation in ```admin_ui.DEFAULT_LOCALE.yml```.

  If you want translate the backoffice in your custom locale, copy these files :

  ```
  admin_ui.LOCALE.yml
  carrierwave.LOCALE.yml
  default.LOCALE.yml
  devise.LOCALE.yml
  flash.LOCALE.yml
  formtastic.LOCALE.yml
  ```
  and rename them with your custom locale shortname. Translate their content, and don't forget to add your custom locale name above in ```admin_ui.LOCALE.yml``` :

  ```
  locales:
      en: English
      de: German
      fr: French
      pt-BR: "Brazilian Portuguese"
      it: Italian
      nl: Dutch
      nb: Norwegian
      es: Spanish
      ru: Russian
  ```


<a name="customize_tiny_mce"></a>
### Customize TinyMCE

The ```<head>``` section of the admin layout is structured as follow (full code [here](https://github.com/locomotivecms/engine/blob/master/app/views/locomotive/shared/_head.html.haml
)) :

```haml
%meta
  …
%script
  …
= yield :head
= render 'locomotive/shared/main_app_head'
```

There is an empty partial meant to be overridden for additional css or js : ```_main_app_head.html.haml``` located in ```app/views/locomotive/shared/``` of the engine directory. Since it's the last one called in the ```<head>```, every css or js added here will override the other ones.

How override this partial ? Well, LocomotiveCMS is build as a Rails engine, so you just need to create it in your main Rails app, because when a view is called, Rails first look for it in the main app, and then in the matching engine. We will add here a javascript tag referring to our customized TinyMCE configuration.

Let's proceed :

1. Within your main Rails app, create a partial at the ```app/views/locomotive/shared/_main_app_head.html.haml``` location (create the folders if they don't exist).

2. Edit the ```_main_app_head.html.haml``` file and insert this : ```= javascript_include_tag  'locomotive_misc'```

3. Create a javascript file at the ```app/assets/javascripts/locomotive_misc.js.coffee``` and fill in with ```window.LocomotiveCMS.tinyMCE.defaultSettings.valid_elements = "<your option>"```

  You can modify all the default TinyMCE settings for LocomotiveCMS.
Take a look at [this](https://github.com/locomotivecms/engine/blob/master/app/assets/javascripts/locomotive/utils/tinymce_settings.js.coffee) file.

4. Tells Rails this asset have to be precompiled, add ```config.assets.precompile += %w( locomotive_misc.js )``` in ```config/application.rb```


<a name="custom_liquid_tag"></a>
### Writing custom Liquid tags

https://groups.google.com/d/topic/locomotivecms/vfxun5pOvEY/discussion

<a name="appendix"></a>
## Appendix

<a name="locomotive_api"></a>
### LocomotiveCMS API

notes récupérées du google group :

I looked at the api source and it appears currently you can only query the entire model using it's slug which returns all entries in the model

https://groups.google.com/d/topic/locomotivecms/_sPLTseOnJs/discussion



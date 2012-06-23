# Locomotive CMS kickass

This book is incomplete and should evolve in the future. Any contribution is very welcomed ! 

For any questions or advices about this book, ask [mail@geraudmathe.com](mailto:mail@geraudmathe.com), and if you need support from Locomotive team, ask [support@locomotivecms.com](mailto:support@locomotivecms.com).

## Summary

1. __[Foreword](#foreword)__
  *  __[Why this guide ?](#foreword_1)__
  *  __[Why you should use Locomotive ?](#foreword_2)__
  *  __[Philosophy behind the CMS](#foreword_3)__
  *  __[Assumptions](#foreword_4)__
2. __[Overview](#overview)__
  *  __[Anatomy of a Locomotive app](#overview_1)__
  *  __[Key features](#overview_2)__
3. __[Getting something running in 5 minutes](#getting_something_running)__
4. __[Templating](#templating)__ 
  *  __[Templating Logic](#templating_1)__
  *  __[Liquid syntax](#templating_2)__
  *  __[Page options](#templating_3)__
  *  __[Use case : templatize a model](#templating_4)__
  *  __[Use case : create a RSS feed](#templating_5)__
5. __[Models](#models)__
6. __[Locomotive Editor](#locomotive_editor)__
7. __[Using Locomotive in an existing Rails app](#locomotive_rails_app)__
8. __[Using multi-sites](#multi_sites)__
9. __[Customizing Locomotive](#customizing_locomotive)__
10. __[Appendix](#appendix)__
  *  __[List of examples](#appendix_1)__
  * Deployment
  * Schemas
  * Links

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

TODO: demander à Didier une idée d'app exemple


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



### Page options <a name="templating_3"></a>

### Use case : templatize a model <a name="templating_4"></a>

### Use case : create a RSS feed <a name="templating_5"></a>

## Models <a name="models"></a>
## Locomotive Editor <a name="locomotive_editor"></a>
## Using Locomotive in an existing Rails app <a name="locomotive_rails_app"></a>
## Using multi-sites <a name="multi_sites"></a>
## Customizing Locomotive <a name="customizing_locomotive"></a>
## Appendix <a name="appendix"></a>

### List of examples <a name="appendix_1"></a>


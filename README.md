# Locomotive CMS Guide

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
  *  __[Rendering models](#models_rendering)__
  *  __[Templatize a model](#models_templatize)__
  *  __[Recipe : Editing has_many in parent model](#editing_has_many)__
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

<a name="foreword"></a>
##Foreword

<a name="foreword_1"></a>
### Why this guide ?

There is already an official documentation reference, which lists almost everything. Still, a pragmatic guide to Locomotive is missing, especially for beginners.

What's more, since there is a lot of goodness in the Locomotive's <a href="http://groups.google.com/forum/?fromgroups#!forum/locomotivecms">Google Group</a>, it seems relevant to gather good practices & hacks in one place.

This guide isn't the official one, even if some members of the Locomotive core team have reviewed some parts of it. 

<a name="foreword_2"></a>
### Why should you use locomotive ?

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


<a name="foreword_3"></a>
### Philosophy behind the CMS

TODO: demander à Didier de l'expliquer ?

<a name="foreword_4"></a>
### Assumptions

During this reading, it is assumed that:

- You know what is Ruby and Rails and you've a good feeling with terms like Gem, Bundle, deployment
- You know what is a data model, and ideally what is a document-oriented storage like Mongo
- You have basic knowledge about shell and command-line interface

<a name="foreword_5"></a>
### Organization of this book

This guide is structured as follow :

First, an *Overview* of the CMS aims to introduce the environment and the main things to know about.

*Getting something running in 5 minutes* may help Locomotive's beginners walking through the 

<a name="overview"></a>
## Overview

<a name="overview_1"></a>
### Anatomy of a Locomotive app

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

<a name="templating_2"></a>
### Liquid syntax

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



<a name="templating_3"></a>
### Creating a page

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


<a name="rss_feed"></a>
### Recipe : Create a RSS feed


<a name="models"></a>
## Models

TODO: it's a draft, rewritte it


https://groups.google.com/forum/#!topic/locomotivecms/GGUJfwvzS9k

https://groups.google.com/d/topic/locomotivecms/xV3KR-IlOmI/discussion

<a name="models_basics"></a>
### Basics

introduction blqblq




First step, specifying the name of the model and so on :

![Create model](Locomotive-fundamentals/raw/master/images/models_basics_creation.png)

As mentioned in the hint, it's the slug of the model's name you will then refence to in your liquid logic.

Then, you define all the fields (attributes) of your model in this section :

![Create fields](Locomotive-fundamentals/raw/master/images/models_basics_fields.png)

You have the following available types for a field :

![Types list](Locomotive-fundamentals/raw/master/images/models_basics_types_list.png)

Let's see each one of them :

- Simple input 

	Simple string, max 255 chars

- Text
- Select
- Checkbox
- Date
- File

The other fields specifying a relationship with an other model (belongs_to, has_many, many_to_many) will be explained in the next section. 


tips : 

verifier que on a bien le meme nom de champ et de slug dans option panel, sinon on peut ne pas comprendre pourquoi on n qccede pas a un champ

il faut au moins un champs texte je crois obligatoire dans un model, le premier champs sera rendu obligatoire a lenregistremenet sil ny en a aucun de defini

UI ENABLED

<a name="models_mapping"></a>
### Models mapping

Locomotive let you define the relationships between models you are used to in Rails, but the creation and usage of these mapped models can sometimes be disturbing in the UI, so let's see for each one of them of to deal with.

This part is dedicated to the models creation and mapping in the admin UI, and the template code shown here will be very concise and simple. For more about displaying models, read the next part.


#### Belongs to

We have the model ```books``` belongs_to ```authors```.

First, we create the model ```authors``` in its simpliest form :

![authors](Locomotive-fundamentals/raw/master/images/belongsto_authors.png)

The mapping with the model ```books``` will be defined in ```books```.
Let's create ```books```:

![books step 1](Locomotive-fundamentals/raw/master/images/belongsto_books_1.png)

We give him a ```title``` field, and a ```writter``` field which defines the ```belongs_to``` relationship.

But wait, we haven't mapped ```books``` to ```authors``` yet, so click on the "add" button and then on the down arrow to specify more options concerning this field :

![books arrow](Locomotive-fundamentals/raw/master/images/belongsto_arrow.png)

You then have the option panel where you choose the ```Class name``` of the model targeted by the belongs_to relationship :

![books step 2](Locomotive-fundamentals/raw/master/images/belongsto_books_2.png)

We are done, so click on the "Create" button to save the model.

*Nota Bene : the name of the field defining the belongs_to relationship (here 'writter') can be named as you want, you don't have to name it the singular of the targeted model (here it would be 'author'), even if it may be a good practice. (???)*


Now we have our models defined, let's add some dummy entries. We will create a new book entrie :

![books entrie empty](Locomotive-fundamentals/raw/master/images/belongsto_book_entrie_empty.png)

We can give him a name, but the ```writter``` list is empty, right, because ```authors``` model hasn't any entries yet.

So create an author :

![author add](Locomotive-fundamentals/raw/master/images/belongsto_author_entrie.png)

And then go back in the book creation page, the author appears in the ```writter``` list :

![book entrie valid](Locomotive-fundamentals/raw/master/images/belongsto_book_entrie_valid.png)

Great, save the entrie and we will check if it works.

In a dummy page, we loop on ```books``` entries, and for each one (here the only one), we display the title of the book and it's writter :

```
{% for book in contents.books %}
	{{ book.title }} written by {{ book.writter.name }} is a great lecture.
{% endfor %}
```
and it displays : ```Responsive Web design written by Ethan Marcotte is a great lecture.```.

#### Has many

Let's keep the previous models, and let's say ```books``` have ```reviews```.
A review is basically a piece of text, and it is often published in a media. What's more, a book has many reviews, but a review refers to one and only one book.  So we are indeed in the relationship ```books``` has_many ```reviews```.

First, we create the ```reviews``` model, which has a string field ```journal``` (in which the review is published) and a text field ```content```, and also a belongs_to field ```book``` :

![book reviews](Locomotive-fundamentals/raw/master/images/hasmany_reviews.png)

**The ```belongs_to``` field targeting the parent model class name, ```books```, is required !**

![book reviews belongs to field](Locomotive-fundamentals/raw/master/images/hasmany_reviews_belongsfield.png)

Save the model, and now go editing ```books```. We will add a has_many field named ```reviews```, targeting the ```Class name``` ```reviews```, and ```Inverse of``` itself, so ```books``` :

![book editing](Locomotive-fundamentals/raw/master/images/hasmany_books.png)

*Nota Bene : here again, the name of the field defining the has_many relationship (here 'reviews') can be named as you want.*

Save the updated ```books``` model, and let's edit the previous ```books``` entrie :

![book entrie](Locomotive-fundamentals/raw/master/images/hasmany_bookentrie_1.png)

Let's add a review to this book, click on "Add a new entry" and fill it with dummy text :

![book entrie reviewed](Locomotive-fundamentals/raw/master/images/hasmany_bookentrie_2.png)

Click on "Save" to close the modal window and create the ```reviews``` entrie related to this ```books``` entrie. Click again on "Save" to update the ```book```.

In the previous dummy page where we tested the belongs_to relationship, we add a loop on the reviews of a book :

```
{% for book in contents.books %}
	{{ book.title }} written by {{ book.writter.name }} is a great lecture.
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

#### Many to many

Finally, we will add the ability to associate tags to a book. Here, the model ```tags``` have many ```books``` and ```books``` have many ```tags```.

Let's create the ```tags``` model.

![tags creation 1](Locomotive-fundamentals/raw/master/images/manytomany_tags_1.png)
![tags creation 2](Locomotive-fundamentals/raw/master/images/manytomany_tags_2.png)

We have the ```books``` field refering to books via a ```many_to_many``` relationship. Like previously, we specify the ```Class name``` of the targeted model, which is ```books```. We also have to specify ```Inverse of``` (itself) ```tags``` here, but we can't, the select list is empty. 

For now, save the  ```tags``` model, we will get back here soon.
Go edit the  ```books``` model and add, as you may guessed, the ```tags``` field refering to tags via a ```many_to_many``` relationship. The ```Class name``` of the targeted model is ```tags```. Yes I'm repeating myself a little, just in case. 
But here you can define the ```Inverse of``` (itself) which is ```books```, so do it please :

![books m2m update](Locomotive-fundamentals/raw/master/images/manytomany_books.png)

Then save the ```books``` model and go back editing ```tags``` model : magic, the ```Inverse of``` attribute of the many_to_many field ```books``` is field with the appropriate value ```tags``` :


![tags inverse of update](Locomotive-fundamentals/raw/master/images/manytomany_tags_inverseof.png)

Here it is, your many to many is settled.

Now we will add some tags to our book, but unlike the previous cases, you can't create a tag entrie in the book entrie page :

![m2m enable ui problem](Locomotive-fundamentals/raw/master/images/manytomany_enableui.png)

We have to create a new tag entrie separatelly, and then add it when editing the book entrie. So we do :


![m2m tag available in book](Locomotive-fundamentals/raw/master/images/manytomany_tag_entrie.png)


You don't have to select here the book entrie you want to connect the tag.
And when we go back to the book entrie we were editing, the tag is available in the select list :

manytomany_books_tags_available

So let's (finally !) add our tag to our book, save, and check if everything is okay back in frontend :

```
{% for book in contents.books %}
	{{ book.title }} written by {{ book.writter.name }} is a great lecture.
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
		{{ book.title }} written by {{ book.writter.name }}
	{% endfor %}
{% endfor %}
```

displays :

```
Tag : "responsive" is related to the following books : 
Responsive Web design written by Ethan Marcotte
```



<a name="models_rendering"></a>
### Rendering models

https://groups.google.com/forum/#!topic/locomotivecms/talQGR12CQ8

ds la loop for, pas besoin de mettre le model au singulier c pas obligatoire


<a name="models_templatize"></a>
### Templatize a model

The idea of a templatized page is that it is a view of one instance of the model you specified in the templatized option. 
 
 The templatized mechanism expects to have "models" under a parent folder which makes more sense for SEO purpose actually (well that's my opinion).

A quick example:

/home
/products (<= used for instance to list of your products, could also be a redirect page)
/products/template (templatized => true, model => Products)

Note: in the 2.0 version, you can not have multiple nested levels of templatized pages. It will be possible with the 2.1 version, if you need this feature now, have a look at [this](https://github.com/locomotivecms/engine/tree/2.1-dev) branch and [this](https://github.com/locomotivecms/engine/pull/125) discussion.

sources :
https://groups.google.com/d/topic/locomotivecms/EQ-leEOgU2U/discussion


<a name="editing_has_many"></a>
### Recipe : Editing has_many in parent model

homes has many pictures 

homes : adress, has_many(pictures)
pictures : item file

save a homes object and then you can edit it and upload several pictures

<a name="public_submission"></a>
### Recipe : Public Submission

https://github.com/locomotivecms/engine/blob/master/features/public/contact_form.feature



////
Very big form, session problem hack : https://github.com/locomotivecms/engine/issues/418


<a name="locomotive_editor"></a>
## Locomotive Editor

lien: Carrier Wave seems to be locking down ThemeAsset to only allow images

https://groups.google.com/forum/#!topic/locomotivecms/r7f-54gSg0U

<a name="locomotive_rails_app"></a>
## Using Locomotive in an existing Rails app

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

<a name="internationalization"></a>
### Internationalization


localised snippets / pages

<a name="customise_tiny_mce"></a>
### Customise TinyMCE 

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




<a name="custom_liquid_tag"></a>
### Writing custom Liquid tags

https://groups.google.com/d/topic/locomotivecms/vfxun5pOvEY/discussion

<a name="appendix"></a>
## Appendix

<a name="locomotive_api"></a>
### Locomotive API

notes récupérées du google group :

I looked at the api source and it appears currently you can only query the entire model using it's slug which returns all entries in the model

https://groups.google.com/d/topic/locomotivecms/_sPLTseOnJs/discussion



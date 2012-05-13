# Locomotive CMS kickass

[geraud@lewattman.com](mailto:geraud@lewattman.com), if you need support from locomotive team, ask to [nocoffee](mailto:didier@nocoffe.fr).

## Summary

1. __[Foreword](#foreword)__
  *  __[Why this guide ?](#foreword_1)__
  *  __[Why you should use Locomotive ?](#foreword_2)__
  *  __[Assumptions](#foreword_3)__
  *  __[Ressources](#foreword_4)__
2. __[Overview](#overview)__
  *  __[What is Locomotive CMS ?](#overview_1)__
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
### Why should you use locomotive ? <a name="foreword_2"></a>
### Assumptions <a name="foreword_3"></a>
### Ressources <a name="foreword_4"></a>

## Overview <a name="overview"></a>

### What is Locomotive CMS ? <a name="overview_1"></a>
### Key features <a name="overview_2"></a>
### Anatomy of a Locomotive app

## Getting something running in 5 minutes <a name="getting_something_running"></a>

## Templating <a name="templating"></a>

### Templating Logic <a name="templating_1"></a>

#### Basics

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
<script src="https://gist.github.com/2687937.js?file=index.liquid.html"></script>

A page, which inherits from index :
<script src="https://gist.github.com/2687937.js?file=a-page.liquid.html"></script>

By extending index, 'a-page' re-use all of its content, except the content inside the ``` {% block %} ``` tag which is overwritten. This tag is written with the Liquid syntax which will is explained later.

You can have as many ``` {% block %} ``` tags as you want, everywhere in the layout, as long as the name of each block is unique. For a basic application which only have one layout, that's all you need to know.

    src: http://doc.locomotivecms.com/templates/tags#block-section

#### Going further

**Several levels of inheritance**

The principle of page's inheritance can be applied to every page. 
When you create a page, it automatically inherits from index, but you can also make it inherits from another page, by specifying it's parent :

<img src="specifying_parent.png" >

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

<script src="https://gist.github.com/2687937.js?file=a-page.liquid.html"></script>

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
 <script src="https://gist.github.com/2688818.js?file=index.liquid.html"></script>
 
 The "normal" page, which inherits from it index :
 <script src="https://gist.github.com/2688818.js?file=normal.liquid.html"></script>
 
 Then the "alternate layout" page, which doesn't extends its parent, index :
 <script src="https://gist.github.com/2688818.js?file=alternate_layout.liquid.html"></script>
 
 And finally, the "alternate page", which inherits from "alternate layout". You may notice the ``` {% extends alternate_layout %} ``` instead of ``` {% extends parent %} ```, as explained in the previous part.
 <script src="https://gist.github.com/2688818.js?file=alternate_page.liquid.html"></script>

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
<script src="https://gist.github.com/2688941.js?file=index.liquid.html"></script>

Then the sidebar :
<script src="https://gist.github.com/2688941.js?file=sidebar.liquid.html"></script>

And finally the product_information snippet which uses the context "product" :
<script src="https://gist.github.com/2688941.js?file=product_information.liquid.html"></script>

    src: http://doc.locomotivecms.com/templates/tags#include-section

### Liquid syntax <a name="templating_2"></a>

    project: http://liquidmarkup.org/

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

    original Liquid doc: 
    https://github.com/Shopify/liquid/wiki/Liquid-for-Designers
    
#### Objects

When writing a liquid template, you will access to a couple of objects representing for instance the current site, page, logged in account as well as collections such as your custom content types. They are also called 'drops'.

Available objects and their attributes are listed here :

    http://doc.locomotivecms.com/templates/objects

**SEO purpose**

You can either use the object ``` site ``` and have the same meta all over your website :
<script src="https://gist.github.com/2689208.js?file=index.liquid.html"></script>
Or you can define SEO meta for each ``` site ``` :
<script src="https://gist.github.com/2689208.js?file=page.liquid.html"></script>





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


The first MetaModel
-------------------

Install with composer
=====================

You’ll need the MetaModels core and some attributes / filter to get MetaModels running. In you composer search
``metamodels/core`` an ``metamodels/bundle_all`` to install the core and all bundles and filters. 
Don’t forget to run composer install through „Update packages“.
When installed, run the database update and your MetaModels installation is done.

.. note:: If you know that you don’t need all attributes and/or filter you can install every single package by it’s own.

Your first MetaModel
====================

Create MetaModels
-----------------
To get started with MetaModels we need at least one MetaModel, jai! We will build a small MetaModel, non translated,
MetaModel for real estate references.

In our example we need two MetaModels:

:reference:
    (the MetaModel which contain the real estate objects)
:category:
    (the MetaModel to define categories for references)

Create reference and category metamodels.

Create attributes
-----------------

An (empty) MetaModel is just a container for your data objects. But before you can store data in your MetaModel, you
need to define some types of data which you like to store.

In MetaModels there are several „attributes“ to store different kind of data. Most of the time you need at least a
text attribute (e.g. to store a name).

mm_reference
^^^^^^^^^^^^
Our reference will contain these attributes:

* Name (text)
* Alias (alias)
* Published (checkbox)
* Description (longtext)
* Keyfacts (tabletext)
* Category (multiple select)
* Highlight-Picture (file)
* Picture Gallery (file, multiselect)

**Name**

:Attribute Type: text
:Column Name: name
:Name: Name
:Description: Name of reference

**Alias**

The alias is an (optional) unique Name / identifier for the data record.

:Attribute Type: alias
:Column Name: alias
:Name: Alias
:Unique: Yes
:Description: Alias of reference
:Alias-Fields: Name [text]

**Published**

:Attribute Type: checkbox
:Column Name: published
:Name: Published
:Published: yes

**Description**

:Attribute Type: longtext
:Column Name: description
:Name: Description
:Description: Description of reference

**Keyfacts**

:Attribute Type: tabletext
:Column Name: keyfacts
:Name: Keyfacts
:Label: Entry
:Width: 500

**Category**

:Attribute Type: multi select
:Column Name: category
:Name: Category
:Description: Select a category for the reference
:Database table: mm_category

Currently, we haven’t added attributes to our ``mm_category`` MetaModel. So for the moment leave the other selects
blank, we’ll get back later.

**Highlight picture**

:Attribute type: file
:column name: picture_highlight
:Name: Highlight picture

Customize filetree (optional): select a „content“ folder where the reference pictures are stored

**Gallery**

:Attribute type: file
:column name: picture_gallery
:Name: Gallery
:Customize filetree (optional): select a „content“ folder where the reference gallery pictures are stored
:multiselect: yes

mm_category
^^^^^^^^^^^

For our category MetaModel we just need four attributes:

* name (text; „name“)
* alias (alias; „alias“)
* published (checkbox; „published“)
* description (longtext; „description“)

Create the attributes as you have just learned in the reference MetaModel.

Select configuration
^^^^^^^^^^^^^^^^^^^^

Early, we introduced in our „reference“ MetaModel a select attribute but leaved it’s configuration nearly blank.

The real power of MetaModel now gets obvious here. With a simple select attribute you can easily connect MetaModels
(or any other sql-table) and optional filter the objects. Filter...? We'll talk about this later.

Edit the „multi select“ attribute in your „References“. 

Choose: 

:table: mm_category
:Name: name - text
:Alias: alias - alias
:Sorting: sorting

Create Rendersettings
=====================

For now, we have two MetaModel with some attributes and a link between booth. But we didn’t want just to store some
data, we also like to display them.

A render setting contains some global settings, attributes you like to display and there settings.
No matter if you like to display data in the backend or fronted you need at least one render setting. But we recommend
to create at least one setting for the backend and one for the frontend.

.. note:: Prefix your render setting name with BE / FE for easy relocating*.

.. info:: It’s necessary to define one render setting as default one*

**Basic-settings**

.. note:: MetaModels provides a set of well organized, solid templates. There are templates for each render setting
          (e.g. metamodel_prerendered). You can create your own templates the contao why (Backend > Templates > Create >
          select the template you like to overwrite > Save (maybe with a new / name addition) > Edit > Choose)

-metamodel_prerendered All attributes are rendered with there template and settings (if available)
-metamodel_unrendered  All attributes are rendered in „raw“ to the frontend (the settings of the child attributes are
                       ignored)

*Output Format:*

-HTML 5     Renders as HTML5 content (This is the default format in Contao and therefore suggested).
-XHTML      Renders as xhtml (this format is deprecated in Contao and therefore not suggested).
-Text       Renders the „content“ as plaintext.

**Jump-to-Page**

The jump-to-page comes into the game when we like to display our references as list with a detail link to one item.
So you need to define a jump-to-page where the user gets redirected if he clicks on a „detail“ link of one of our
reference objects.

The filter setting define the rules for the target, your detail page. 

.. info:: In list views, you need to set a filter (which includes the conditions of your detail page)

**Expert-settings**

:hide empty entries: yes
:hide labels: yes

Create a rendersetting (backend)
--------------------------------

Go to the „render settings“ of „reference“.

* Create a render setting called „BE: references“
* Add „all attributes“ 
* After adding, activate „name“ and „category“

.. note:: When you (later) add attributes to your MetaModel you need to add them also in your render setting.*

Create a rendersetting (frontend list)
---------------------------------------

Go to the „render settings“ of „reference“.

* Create a render setting called „FE: references list“
* Add „all attributes“ 
* After adding, activate „name“, „category“, „picture_highlight“

Create a rendersetting (frontend detail)
----------------------------------------

Go to the „render settings“ of „reference“.

* Create a render setting called „FE: reference detail“
* Add „all attributes“ 
* After adding, activate „name“, „description“, „category“, „picture_highlight“, „picture_gallery“

Input Screens
=============

For now there are two MetaModels with some Attributes and Rendersetting. But how do we get data in our MetaModels?
With input screens!

Input Screens could hold a collection of these attributes which are necessary to grep some data.
Most times you just add all attributes in one Input Screen, but with the power of different input screen you can create
different edit masks for different kind of user(groups).

But in our tutorial we just need one input screen for our users.

**Basic-settings**

So create a Input Screen with the following settings:

:Name: BE: References
:Standard: yes
:Panel-Layout: -leave this empty-
:Integration: standalone
:Backend-Section: Content
:Render mode: Flat
:Data manipulation permission: We want to allow editing, creating and deleting items - so choose all three.

Select configuration
--------------------

Okay. Now we got the empty Input Screen container with a few settings. But to get things working, we need (remember
the render setting!) some attributes in it.

Switch to the „settings“ of your currently created Input Screen and choose „add all“.

Define Attribute settings
-------------------------

Our input screen is ready. But we need tweak the attributes a little bit. For example we always want a name, description
and Highlight Picture.

To get this done, we choose in these attribute settings the „mandatory“. 

.. info:: Input Screens are very powerful. Take a coffee and explore the visibility conditions and attribute settings.

Grouping and sorting settings
-----------------------------

In the grouping & sorting section you need to create at least one object to sort & maybe group your entries.

For example: "Enable manual sorting" without grouping.

View conditions
===============

View conditions are the easy part in MetaModels. But, you might guess that you also need here at least one to get things
work.

The view conditions define who could see and use which render setting and input screen.

.. info:: In most cases you like to show your metamodel data to all of your visitors. So you can leave the „member
          group“ blank.

Define a view condition
-----------------------
Define one view condition with following settings:

:member-group: -leave this empty-
:user-group: administrator
:input screen: BE: Referenz
:Rendersetting: BE: Referenz

.. info:: Wasn’t it a good Idea to prefix our input screens and render setting? ;-)

We are ready to enter Data
--------------------------
Some time ago, we started with just a MetaModels package and already arrived to create data. Easy, hm?

Continue to the new „Referenz“ entry in your „content“ navigation and add a first item.

Filter Setting
==============
(Todo)

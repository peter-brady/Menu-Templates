# Menu Templates for Drupal (sandbox project)

For complex menu's IE. mega menus,  menu's with many nested lists etc. you may be tempted to resort to using such contrib modules as [Megamenu](https://www.drupal.org/project/megamenu) and [Menu Minipanels](https://drupal.org/project/menu_minipanels). While these fulfill a need, the logic and system resources required to render the menu's can be substantial.

On one site I worked on, I saw well over one hundred database calls being made just to render a relatively simple mega-menu.

For the purists out there, you could instead use this module.

The Menu Templates for Drupal module allows developers to render Drupal menus via a template file - **menu-template--[menu-template-block-name].tpl.php** - in the default theme.

![menu-templates-1](https://cloud.githubusercontent.com/assets/2240510/6551685/7984724a-c634-11e4-9f55-18d869633a0c.png)

You can also create multiple template files for the same menu. This can be used to render the same menu to be through a different **menu-template--[menu-template-block-name].tpl.php** file in different sections on your site.

Switching these menu's could be achieved using the **core Drupal blocks UI**, or the [Context](https://www.drupal.org/project/context) contrib module**, for example.

## Caveat

Using this module will be a lot lighter in terms of system resource but be aware that if a menu link changes, or you want to add a new link, the menu's **menu-template--[menu-template-block-name].tpl.php** file will require updating. So a code release would be required.

To be realistic, this module should only really be considered if the menu's and menu links on your site aren't going to change often **and** if you're concerned about performance (which you really should be) :-)

## Dependencies

- [Bean](https://www.drupal.org/project/bean) contrib module
- Block core module
- Menu core module

## Installation

- Clone this project into your modules directory
- Enable the module

### Note

After enabling the feature, you may need to set the correct Bean permissions.

- visit admin/people/permissions
 - ensure Administrator has access to:
 - Menu template: Add Bean
 - Menu template: Edit Bean
 - Menu template: View Bean
 - Menu template: Bean Bean

##  Usage

There are two steps involved in creating menu templates

1. Creating the bean
2. Creating the template file to render your menu

Follow the steps below. You'll be menu templating ninja before you know it.

###1. Create the Bean

  1) Visit Content > Blocks (admin/content/blocks) Click 'Add Block'

  2) Give your menu block a name.

  3) You'll see the exact name for the .tpl file you need appear as you type.

  4) Copy this file name and create that file in the menu-templates folder somewhere in the default theme.

  4) From the drop-down, select the menu you want to template.

  5) Save.

### 2. Create the Template file

Note, it's slightly better to create the file before you save this form, as the form submit rebuilds the theme registry. No problem if you don't though. Just **Flush all caches > Theme registry** after creating the .tpl.pghp file in the theme.

### The Template file

Menu links are available in your menu template file in the $menu array.

The menu link path is used as the key name so a menu link with the path **about** is available to your **menu-template--[menu-template-block-name].tpl.php** file as:

```
$menu['about']['#link'];
```

A menu link with a path of **about/contact-us** is available to your **menu-template--[menu-template-block-name].tpl.php** file as:

```
$menu['about_contact_us']['#link'];
```
  
To add .active, .active trail, .expanded and .menu-[mlid] classes to a list item, you can do this:

```
  <li<?php print drupal_attributes($menu['home']['#li_attributes']) ?>><?php print $menu['home']['#link'] ?></li>
 ```
 
Note that the correct .active, .active-trail and .expanded classes have already been added to #link, based on current URL.

## Troubleshooting

If the menu doesn't display, check the following:

- Edit the bean. Ensure the name of the template file exists in the theme. Also, visiting Reports > Recent log messages will tell you if Drupal can't find the menu template file it needs.
- Check that the region you've added this menu to is actually available to and being rendered in the theme.

## A Final Note

I was never convinced that this solution elegantly addresses the problem of rendering complex menu structures. Personally, I don't think it's intelligent enough because the **menu-template--[menu-template-block-name].tpl.php** always needs to match the links available in your actual menu.

Also,

```
  <li<?php print drupal_attributes($menu['home']['#li_attributes']) ?>><?php print $menu['home']['#link'] ?></li>
```

...is kind of ugly.

I hope to revisit this at some point as I think a lightweight, intelligent mega-menu solution in the Drupal world is sorely needed.

Anyhow, I'm putting it out there. In it's current state, somebody may find this module useful :-) 

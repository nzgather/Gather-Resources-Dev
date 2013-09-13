# Building with Wordpress

This is the documentation for the workshop: "Building with Wordpress", written by Ludwig Wendzich.

## Tools of the Trade

### Software
* __Text Editor__ Most files you will work with will be some kind of text file, and thus finding the right Text Editor is an important part of the process. Here are some popular ones:
	* [Sublime Text 2](http://www.sublimetext.com/) (Free, Open Source)
	* [Text Wrangler](http://www.barebones.com/products/textwrangler/) (Free)
	* [Textmate](http://macromates.com/) ($50)
	* [Coda](http://www.panic.com/coda/) ($75, includes FTP, Documentation, MySQL Editor and more)
	* [BBEdit](http://www.barebones.com/products/bbedit/) ($129, $49 Educational)
	
	I have used Textmate and Coda but find that I prefer Sublime Text 2. I would recommend Coda for those who aren't professional developers or don't prefer bare-bones text editors. For Windows user, [Notepad++](http://notepad-plus-plus.org/) looks like an alternative to Sublime Text 2 but I have never used it. Don't even think about Dreamweaver.
	
	Sublime Text 2 also works on Windows and Linux
* __FTP Client__ Once you've created files and you want to publish them to a server, you need to get them up on a server somehow. The easiest way to do this is with FTP.
	* Built-in Coda
	* [Transmit](http://www.panic.com/transmit/) ($34)
	* [Cyberduck](http://cyberduck.ch/) (Free)
	* [Filezilla](https://filezilla-project.org/download.php?type=client) (Free)
	
	I use Transmit. I used to use Filezilla which is also available on Windows and Linux.
* __Content Management System__ Updating a website manually is difficult. CMSes make this easier, that's what we will be learning to implement. We're going to be using Wordpress, the web's most popular CMS:
	* [Wordpress](http://wordpress.org/)
	
* __Graphic Editor__ You will probable need a graphical editor. Adobe is still where it's at.
	* Photoshop
	* Illustrator
	
### Other
* __Local Server__ Building a website locally is faster when you don't have to upload every change with FTP. Just hit Save.
	* [MAMP](http://www.mamp.info/en/index.html)
	* [WAMP](http://www.wampserver.com/en/)
* __Remote Server__ If you want your website accessible from the web, then your files have to live somewhere that's always on and always connected.

## Installing Wordpress

### Getting the Local Server Ready

Installing Wordpress is very simple ([Codex: Installing Wordpress](http://codex.wordpress.org/Installing_WordPress)). The basic process is:

1. Setup local web server
2. Create database
3. Install Wordpress

Platform-specific instructions:

* [Installing Wordpress on MAMP](http://codex.wordpress.org/Installing_WordPress_Locally_on_Your_Mac_With_MAMP)
* [Installing Wordpress on WAMP](http://wp.tutsplus.com/tutorials/hosting/instant-wordpress-development-with-wamp/) or [here](http://www.wpwebhost.com/install-wordpress-on-wampserver/)


Note: [Wordpress on USB Key (Windows)](http://www.instantwp.com/) and [1-click Install on Mac OS X](https://itunes.apple.com/us/app/bitnami-stack-for-wordpress/id588981362?mt=12)

### Development Server with Dropbox

If one has access to a development server (which has the added benefits of say allowing external access to websites) then one could setup students with auto-magic syncing between their local machine and the dev machine using Dropbox.

One simply creates a master Dropbox account, creates shared folders with each student's Dropbox account and then create a symlink from the /htdocs/ or /www/ folder on the server to students' shared Dropbox folder on the dev server.

How to symlink website project folders to `/www` (`/htdocs`) on a Mac (using Terminal):

	cd ~/Path/to/Dropbox/_Path_to_www_folder_
	ln -s ~/Path/to/Dropbox/_path_to_project_www project_name

Or [create symlinks in Windows](http://www.howtogeek.com/howto/16226/complete-guide-to-symbolic-links-symlinks-on-windows-or-linux/).

This way, students save locally, Dropbox syncs and their changes are seen on the development server. Easy development (no FTP) and easy hand-in.

It may be necessary to turn on `Follow Symlinks` for Apache. [Here's how](http://superuser.com/a/244250).

## Pages and Posts

Wordpress started off as a blogging engine. This means the most important object in Wordpress is a post. Pages are tacked on, and Custom Post Types are an abstraction of Posts and Pages, but it's clear that Posts are still king.

###Posts

Posts are chronological entries. They belong to categories and can belong to many categories.

### Pages

Pages are hierarchical entries. They don't belong to categories, they belong to other pages and they can only belong to one other page.

### Custom Post Types

Posts and Pages are to default "post types". We can create their own. CPTs can have many different flags. For example, they can be made hierarchical (like Pages) which means they can belong to one another.

Categories are the default form of something called a taxonomy. You can create many taxonomies. Another default is "tags". Taxonomies are ways to organise posts into groups.

### Permalinks (URL Rerouting)

__Chronological__ posts can be accessed by their timestamps. eg. `domain.com/year/month/day` will show all the post with timestamp correlating to that day/month/year. Similarly `/year/month` and `/year/`. A single entry may be `/year/month/day/entry-title-goes-here/`.

__Heirarchical__ posts are accessed through their parents. eg. `/parent-page-title/child-page-title`. It shows who belongs to who.

__Taxonified__ posts can be accessed via their taxonomy. eg. `/category/category-name/` will show all posts in `category-name`.

The way we organise our information in Wordpress and setup our Custom Post Types and Taxonomies should take in account the way we want people to access our pages and posts.

>__Pro-tip__

>Write our the URLs you want your website to have. How you want the different pieces of content to be accessed and relate to each other. Then start creating custom post types and taxonomies on paper before jumping into Wordpress.

## Themes

Themes are where we control the output of our Wordpress website. Here's where we sprinkle PHP throughout HTML templates and style them up with CSS to look just the way we want them.

### Essential Files

There are a couple of default files almost every theme will need

* `header.php` — Called at the top of every page
* `footer.php` — Called at the bottom of every page.
* `index.php` — This is the default theme. It will be used whenever a more specific theme can't be found.
* `style.css` — Default CSS file.
* `functions.php` — Used to over-ride default Wordpress functionality and turn on and off features.

Then we get specific:

* `front-page.php` — The homepage template
* `single-post.php` or `single-custom-post-type-slug.php` — The template used to render that post type.
* `page.php` or `page-slug.php` — The template used to render all pages, or a specific page, e.g. `page-about.php`.

Read about [Template Hierarchy in the Codex](http://codex.wordpress.org/Template_Hierarchy).

[Download Basic Custom Theme](/wp-content/uploads/2013/03/custom_theme.zip)

### The Loop

The loop is what Wordpress uses to access all the posts that should be shown on that page. It goes through the loop once for every post. This means on a single post page, it will go through the loop once and print out that one post. Same for a page, it will go through the loop once to print out the page. On an archive page, or search result page, it will go through the loop multiple times printing out each post.

Read more about [The Loop in the Codex](http://codex.wordpress.org/The_Loop)

### Sidestepping the loop

It may be necessary to pull in posts onto a page which Wordpress doesn't automatically thinks belongs there. Maybe it's a "Recent Posts" in a sidebar, or we're doing "Related Posts".

To do that, we can query [get_posts](http://codex.wordpress.org/Template_Tags/get_posts) and run through the array with our own PHP foreach loop. In order to retain default functionality that rely on globals (like `the_title()` we need to override the global `$post` for each loop and then reset it once we're done.)

	<?php 
	
	// Query for posts
	$posts_array = get_posts( $args );
	
	// Save $post for later
	$op = $post;
	
	// Run through each post in $posts_array and assign it to $post
	foreach($posts_array as $key=>$post) {
	
		//Output post
		
	}
	
	// Reset $post
	$post = $op;
	
	?>
	

### DRY Principle

Don't-repeat-yourself (or DRY) is a well-known principle in the programming world. It's why we have things like CSS. It's why we abstract our code into re-usable pieces called functions. It's why we split out `header.php` and `footer.php` from our other templates.

Other parts of the website can also be repeated. But that repetition can be done programmatically so that we only need to manually make a change once.

With `get_template_part()` we can import various templates into our templates. eg. `get_template_part('subnav')` will import `subnav.php` into our page.

The function takes a second (optional) argument that allows us to request a specific file (much like `page-about.php`) that will fallback if the specific version doesn't exist.

`get_template_part("subnav","about");` will include `subnav-about.php` if it exists, else it will import `subnav.php`.

Read more about [Get Template Part in the Codex](http://codex.wordpress.org/Function_Reference/get_template_part).

## Menus

There are a number of patterns recorded here to create the navigation on their Wordpress-based websites. You may not always find the pattern you need, but these are tried and tested.

### Overview

In Wordpress we make use of the `Appearance > Menus` feature to create most of our navigation items. These include:

* __Main Menu (L1s):__ This should include the *entire* website IA. Use the `depth` parameter of `wp_nav_menu` to limit the output of this menu. This means we can re-use this menu object in other place, eg. a site map, a large L1+L2 menu in the footer etc.
* __Unique Menus:__ These include special "Supporter" logos in the footer.

_Note: Wordpress Menu's are used for the organisation of content; unless that content is literally a list of links, then it's better to create a Custom Post Type and use those within the Menu._

There are other types of navigational items found on the page, these include:

* __Sub Nav:__ We use `wp_list_pages()` to output these for a number of reasons. [Learn how]
* __Related Items:__ Sometimes it may look like something should be a menu. For example, we have a group of Team Members on the People page. Each team member is a Custom Post Type because they have a special template. It may look like we create a menu to display these on the People page but we don't. [Find out about relational posts].

### Setting up Wordpress Menus

Menus are just an easy way to group of a lot of different types of content (Posts, Pages, Custom Post Types, Tags and Taxonomies, anything really) together in one list/menu.

It's important to understand that the Menus feature of Wordpress does _just_ that, organises content into menus. It doesn't affect the hierarchy of the actual content (that's done by assigning parents to content), nor does it change the order of the content (that's done with plugins, to create a manual order, or in the templates during the query.)

The first step is registering our Menu in our functions.php page:

	// ~theme/functions.php
	register_nav_menu( 'primary', 'Main Menu');

* Read more about [register_nav_menu()](http://codex.wordpress.org/Function_Reference/register_nav_menu)

We give our nav a `theme_location` (a unique ID, like a slug) of `primary`  and a `default_name` of `Main Menu`. Each _unique_ menu we want to use needs to be registered, so if you have a "Sponsors" list you'd like to handle with a menu then we need to register it also.

>*__Tip:__ We recommend creating a Main Menu that includes the entire site structure in it (L1s down). We can always limit the depth of the menu in the templates.*

Next, we setup the menu under `Appearance > Menus` in the Wordpress Admin. __Remember to save__.

Once we have a menu to call, we need to call it from the template:

	<nav id="access" role="navigation">
		<? wp_nav_menu( array( 'theme_location' => 'primary', 'container' => false, 'depth'=>'1') ); ?>
	</nav>

You'll notice we call it using the `theme_location`. We also use a number of parameters to dictate the output. `container` is set to `false` because we are providing our own container. `depth` is set to `1` because we only want L1s to be outputted here.

We can exclude the `depth` parameter to output a site map, limit it to `2` to get an extended menu in the footer of L1s and L2s, etc. Menus can be very powerful.

* Read more about [wp_nav_menu()](http://codex.wordpress.org/Function_Reference/wp_nav_menu). 

#### Hooks for styling Menus

The following style hooks are provided by Wordpress to style menus:

* `current-menu-item` for the currently active item.
* `current-menu-ancestor` for each ancestor of the currently active item

These are usually grouped when styling:

	#access .current-menu-item a,
	#access .current-menu-ancestor a,
	#access a:hover {
		// style here
		}

* [Read more about CSS Patterns for styling navigation](CSS%20Patterns.html#Navigation)


### Sub navs in Wordpress

Menus are used to create menus, but don't create real hierarchy of posts. When hierarchy is require, we need to use the "pages" functionality of Wordpress. This can be added to any **custom post type** and basically means, _"treat this post-type as heirarchical like regular WP pages and not chronological like regular WP posts"_.

Once our pages (or custom post types with pages functionality enabled) are created, and our child pages are given the correct pages, then we can call the sub nav using `wp_list_pages()`.

In the following snippet we check if the current page is a grandchild (L3), or child (L2) and then grab the closest L2 ancestor (or itself). If the closest L2 has children, we output those children in a sub nav.

	<? if($post->post_parent) {	//if grandchild (L3)
		$children = wp_list_pages("title_li=&child_of=".$post->post_parent."&echo=0"); 
	  } else {	//if child (L2)
	  	$children = wp_list_pages("title_li=&child_of=".$post->ID."&echo=0");
	  }
	  if ($children) { ?>
		<nav class="sub-nav">
			<h2><a href="<? echo get_post($post->post_parent)->permalink; ?>"><? echo get_post($post->post_parent)->post_title; ?></a></h2>
			  <ul>
			  	<? echo $children; ?>
			  </ul>
		</nav>
	<? } ?>

This snippet will ensure that child pages of an active L2 are shown in the sub nav, and that, that _same_ sub-nav is visible when an L3 is active.

#### Hooks for styling sub-navs

The following style hooks are provided by Wordpress to style `wp_list_pages()`

* `current_page_item` for the currently active page.


## Custom Post Types

We've already talked about Post, Pages and how they relate to Custom Post Types. There are a number of patterns recorded here to allow us to create re-usable content formats using Custom Post Types in Wordpress and relate them to each other. You may not always find the pattern you need, but these are tried and tested.

### When should I use Custom Post Types?
Generally, whenever you feel like you start using HTML inside the Visual Editor in Wordpress that can easily be broken by someone using the Visual Editor without being proficient in writing HTML/CSS then you should be creating a custom post type. In other news, when you are creating a page with special layout, custom post types comes into play.

The general feel is to chunk content, not "blob" it. Separating content into discrete chunks gives us flexibility in the future and allows us to reuse content.

Common custom post types are:

* Call to actions/Sidebar items
* Profiles
* Reviews
* Projects

## Plugins
There are two plugins that are almost a necessity for each project because they allow custom post types with many-to-many relationships

* [Types](http://wp-types.com) — this plugin allows you to setup custom post types and taxonomies and assign custom fields to custom post types.
* [Microkid's Related Posts](http://www.microkid.net/wordpress/related-posts/) — this plugin allows editors to easily relate specific posts of type with other posts. eg. Associate CTAs with pages, or people profiles with pages.

Other useful plugins include:

* [Simple Page Ordering](http://10up.com/plugins/simple-page-ordering-wordpress/) — drag and drop ordering added to Pages section of Wordpress Admin.
* [Intuitive Custom Post Order]() – drag and drop ordering added to Custom Post Type Sections of Wordpress Admin.

#### Using Types

Once activated a new option will appear on the left-hand menu of the Wordpress Admin, "Types".

We create custom field groups that we assign to custom post types once they are created. Types will also allow us to create and assign custom taxonomies.

To access a custom field using Types outside the default loop (explained in [Themes](#Themes)), we need to we need to use the technique to replace the global `$post` for each of the posts in our array. This is explained in "Sidestepping the Loop" under [Themes](#Themes).

Setting up custom post types, taxonomies and custom field groups are explained [here](http://wp-types.com/documentation/user-guides/). Using Types to access custom fields are explained [here](http://wp-types.com/documentation/user-guides/displaying-wordpress-custom-fields/) and the documentation of the options for each type can be found [here](http://wp-types.com/documentation/functions/).

#### Using Related Posts
Every time we related posts to one another, we normally want to output these related posts from within our templates. By default, Related Posts will output these posts after `the_content()`, __almost every time we want to turn this off and manually control the output.__ We do this under `Settings > Related Posts`.

An common usage of related custom post types is the sidebar CTA. Once a `cta` custom post type has been created (usually with Types), one can use the following code to pull out related posts.

	$ctas = MRP_get_related_posts( $post->ID, true, true, 'cta' );
	
One can also grab specific CTAs based on taxonomy (not using Related Posts).

	$taxonomy_cta_args = array(
							"post_type" => 'cta',
							"tag" => "home",
							"exclude" => '31'
							);

	$taxonomy_ctas = get_posts($taxonomy_cta_args);


Then, once we have an array of CTAs, we can output them by iterating over the array.

	if($ctas){
		foreach($ctas as $key => $cta) {
		
			if(!get_post_custom_values("html", $cta->ID)){ ?>
				<div class="promo"><? echo wpautop($cta->post_content); ?></div>
			<? } else { 
				echo $cta->post_content;
				}
				
			} //end foreach
		} //end if ctas
		
You can see in the above code we check if we even have `$ctas` (if the array is empty, that will return false). If we do, then we loop through them. To not, for each custom post in our array, we check it's custom field value (in this case, `html`) and then modify the output based on the result. Here, if `html` is equal to `true` then the author of that custom post type has indicated we shouldn't wrap the custom post type in a `.promo` `div` on output, e.g. a search box.

##Diving into Docs

The hardest part about finding help is knowing what to look for. Hopefully these Gather "Building the Web" docs will give you enough of a start to know how to find what you're looking for. Here are some resources you can trust:

* [StackOverflow](http://stackoverflow) — General programming help
* [CSS Tricks](http://css-tricks.com/) — Front-end development techniques
* [Wordpress Codex](http://codex.wordpress.org/) — Wordpress Documentation

The best way to find what you're looking for on those websites though, is through using a search engineer (read: Google) and looking for results that come from those trusted-domains.
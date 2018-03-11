# The Loop

## Description

In this lesson you will learn what The Loop is and when to use it? This step-by-step walkthrough will give you an understanding of how to use The Loop to display lists of post titles, links, and other content.

## Objectives

After completing this lesson, you will be able to:

*   Define The Loop.
*   Explain why The Loop is used.
*   Show how The Loop works.

## Prerequisite Skills

You will be better equipped to work through this lesson if you have experience in and familiarity with:

*   [WordPress Templates](http://make.wordpress.org/training/modules-in-progress/page-template/ "Templates") lesson

## Assets

*   [Twenty Thirteen Theme](http://wordpress.org/themes/thirteen "Twenty Thirteen Theme")

## Screening Questions

*   Do you understand how WordPress theme template files are organized?
*   Do you have at least a basic knowledge of HTML and PHP?
*   Do you feel comfortable using a text editor to edit code?
*   Will you have a locally or remotely hosted sandbox WordPress site to use during class?

## Teacher Notes

*   Performing a live demo while teaching the steps to use The Loop is crucial to having the material "click" for students.
*   It is easiest for students to work on a locally installed copy of WordPress. Set some time aside before class to assist students with installing WordPress locally if they need it. For more information on how to install WordPress locally, please visit our [Teacher Resources page](http://make.wordpress.org/training/teacher-resources/).
*   The preferred answers to the screening questions is "yes." Participants who reply "no" to all 4 questions may not be ready for this lesson.

## Hands-on Walkthrough

### Introduction: What is The Loop

This is The WordPress Loop: <?php if ( have_posts() ) : while ( have_posts() ) : the_post(); ?> It is a recipe for displaying repeating blocks of content on your site. When your site shows a template that displays multiple posts in a similar format, the loop sets up the structure that each post will follow, then repeats that structure until all posts have been displayed. It is a very crucial part of WordPress.

* * *

### Why use The Loop?

Many places on a web site display lists of content excerpts, instead of full content for a single post. Some examples include:

*   Recent blog posts on the home page
*   Lists of articles in a category on a category archive
*   Alternate content styles, such as thumbnails for a Gallery Custom Post Type

The [Template Hierarchy](http://make.wordpress.org/training/modules-in-progress/the-wordpress-template-hierarchy/ "Template Hierarchy"), The Loop, and Template Tags work together to display a page much like a meal type, recipe, and ingredients work together to create a meal:

<table>

<thead>

<tr>

<th> Question</th>

<th>Meal</th>

<th>Page</th>

</tr>

</thead>

<tbody>

<tr>

<td>What is our context?</td>

<td>Lunch</td>

<td>Home page</td>

</tr>

<tr>

<td>What should we have in this context?</td>

<td>10 sandwiches!</td>

<td>10 recent posts</td>

</tr>

<tr>

<td>How will we make each one?</td>

<td>By repeating a recipe</td>

<td>By repeating a template</td>

</tr>

<tr>

<td>What makes this different than others?</td>

<td>Which ingredients we use</td>

<td>Which template tags we use</td>

</tr>

</tbody>

</table>

### Where should The Loop be used?

In its default form, The Loop is used on Archive Pages, which display lists of posts. See [Template Hierarchy](http://make.wordpress.org/training/modules-in-progress/the-wordpress-template-hierarchy/ "Template Hierarchy") or the [Template Hierarchy chart](https://make.wordpress.org/training/files/2014/09/template-hierarchy-retina-light.jpg) for more information. Some of these templates include:

*   Home Page, if displaying recent posts
    *   index.php, front-page.php, home.php
*   Category Archives
*   Tag Archives
*   Custom Taxonomy Archives
*   Search Results
*   Author Archives
*   Date Archives

Custom loops can also be used anywhere you would like to display a group of posts that come from a custom query.

* * *

### How The Loop works

When you visit an Archive Page on your site, WordPress creates and populates a default query with the posts that should display on that page. Functions for The Loop allow you to repeat information for each of those default posts, or for a custom query of your choice.

* * *

### Setting up a default loop

The simplest loop you can use is the default loop, because it will automatically use the posts WordPress has already retrieved for the current page request. For example, on the home page, this might be 10 recent posts. On a category archive, this might be 10 recent posts for a specific category, like "Puppies". For example, see The Loop in `[wp-content/themes/twentythirteen/index.php](https://github.com/WordPress/WordPress/blob/master/wp-content/themes/twentythirteen/index.php#L21)`. The code below is a copy of that loop, with comments explaining each step. [html] <!-- Check if we have any posts from the default query. If we do have some posts, go to line 9. If we don't have any posts, go to line 45. --> <?php if ( have_posts() ) : ?> <!-- We have some posts! Start the loop. Everything between here and endwhile; will repeat for each post. the_post() is placed immediately after ":" simply because it should always run first in a loop. It sets up all the template tags, like the_title(), to display the correct information for each item. --> <?php while ( have_posts() ) : the_post() ?> <!-- This code will be repeated for each item in the loop. This second argument, get_post_format(), is optional. Its use will cause content.php to be used only if the post is not using a Post Format. If the post does use a Post Format, it might switch to a variation, like content-image.php or content-link.php --> <?php get_template_part( 'content', get_post_format() ); ?> <?php endwhile; ?> <!-- End of the loop --> <!-- This is a custom function specific to the TwentyThirteen theme. It will display paging navigation after The Loop if it's needed. --> <?php twentythirteen_paging_nav(); ?> <?php else : ?> <!-- We do not have posts! This happens in cases like a category that has no posts assigned, or a search that returns no results. This specific line will use the code found in content-none.php --> <?php get_template_part( 'content', 'none' ); ?> <?php endif; ?> <!-- End of checking whether we have posts or not --> [/html]

* * *

### Choosing which template to repeat in The Loop

Once inside The Loop, we can include a section of repeating code using [get_template_part()](https://developer.wordpress.org/reference/functions/get_template_part/). In its most basic form, we might always include `[content.php](https://github.com/WordPress/WordPress/blob/master/wp-content/themes/twentythirteen/content.php)`. [html] <?php get_template_part( 'content' ); ?> [/html] A second argument can be added to get_template_part() to allow switching between different templates based on some changing information. For example: Display `content-alternate.php` if exists in this theme or a child theme. Otherwise, display `content.php`. [html] <?php get_template_part( 'content', 'alternate' ); ?> [/html] If the current post uses a [Post Format](http://codex.wordpress.org/Post_Formats), such as "Gallery", and `content-gallery.php` exists, display it. Otherwise, display `content.php`. [html] <?php get_template_part( 'content', get_post_format() ); ?> [/html] If the current post uses a [Custom Post Type](http://codex.wordpress.org/Post_Types), such as "Event", and `content-event.php` exists, display it. Otherwise, display `content.php`. [html] <?php get_template_part( 'content', get_post_type() ); ?> [/html]

* * *

### Displaying information with Template Tags

While in The Loop, template tags will display information about the current post. For more information, see [Codex: Template Tags > Post tags](http://codex.wordpress.org/Template_Tags#Post_tags). For example, see TwentyThirteen's `[content.php](https://github.com/WordPress/WordPress/blob/master/wp-content/themes/twentythirteen/content.php)`.

* * *

### Using additional loops

Loops don't only have to display content from the default query. The structure we looked at earlier to can be combined with a custom query to display any list of posts. For detailed information, see [Codex: WP Query](http://codex.wordpress.org/Class_Reference/WP_Query). [html] <!-- Run a custom query. Get the last 3 posts from the category with slug "puppies". --> <?php $recent_puppies = new WP_Query( array( 'posts_per_page' => 3, 'category_name' => 'puppies', ) ); ?> <!-- Does the "$recent_puppies" query has some posts? --> <?php if ( $recent_puppies->have_posts() ) : ?> <!-- Run a custom loop just on the "$recent_puppies" posts. Important! $recent_puppies->the_post() will change template tags like the_title() and the_content() to be used for this custom loop. We need to make sure to switch them back later using wp_reset_postdata(); --> <?php while ( $recent_puppies->have_posts() ) : $recent_puppies->the_post() ?> <!-- Display a content.php for each post. --> <?php get_template_part( 'content' ); ?> <?php endwhile; ?> <!-- End of the loop --> <!-- Important for custom loops: Make template tags work for default loops again. --> wp_reset_postdata(); <?php else : ?> <!-- No posts were found for the "$recent_puppies" query! Display content-none.php if it exists. --> <?php get_template_part( 'content', 'none' ); ?> <?php endif; ?> <!-- End of checking whether we have posts or not --> [/html]

* * *

### Summary

Loops allow a repeating structure to be applied to content appropriate for the current page. WordPress' The Loop structure shows content that WordPress gets automatically based on the requested page, while custom loops are used to display content of your choice.

## Exercises

**Try adding some text on a line [before get_template_part()](https://github.com/WordPress/WordPress/blob/master/wp-content/themes/twentythirteen/index.php#L25) inside The Loop in TwentyThirteen's [index.php](https://github.com/WordPress/WordPress/blob/master/wp-content/themes/twentythirteen/index.php).**

*   Do you see your text show up before each post on your site's home page?
*   What happens if you add HTML?
*   What happens if you move your change into [content.php](https://github.com/WordPress/WordPress/blob/master/wp-content/themes/twentythirteen/content.php)?

## Quiz

**In what case might there be no posts to display in The Loop?**

1.  No posts have been assigned to the category on a Category Archive.
2.  No posts have been assigned to the tag on a Tag Archive.
3.  No posts are found for a user search on the search results page.
4.  All of the above.

**Answer:** 4. All of the above.

* * *

**What's the difference between `if ( have_posts() )` and `while ( have_posts() ) : the_post()` ?** **Answer:** `if` checks if any posts exist. `while` starts a loop over each individual post.

* * *

**What would happen if `the_post()` is not included after `while ( have_posts() ) :` ?** **Answer:** Template Tags, such as `the_title()` and `the_content()` would not work correctly.

## Additional Resources

[The Loop](https://developer.wordpress.org/themes/basics/the-loop/) @ developer.wordpress.org

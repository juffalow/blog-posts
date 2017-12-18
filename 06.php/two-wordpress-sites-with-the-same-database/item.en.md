---
title: 'Two WordPress sites with the same database'
media_order: wordpress.png
date: '30-09-2016 14:12'
taxonomy:
    tag:
        - php
        - WordPress
header_image_file: wordpress.png
header_image_width: 128
header_image_height: 128
---

When you try to use two ( or more ) WordPress sites with one shared database, you will have problem to access the pages except the original one. WordPress knows what should be its URL and when it does not match with the request, it redirects you. Fortunately, there is an easy “hack” for this to make it work.

===

Let’s say we have these two domains ( each with its own wordpress ) *site-one.dev* and *site-two.dev* and they share the same database. If you log into the first site – *site-one.dev* – and display general settings, there is WordPress Address (URL). This should point on curernt site, i. e. *http://site-one.dev*. Now, when you try to access admin interface on the second page, it redirects you back on the first page.

## Solution

For the second wordpress site, you have to override that setting. It is saved in `wp_options` table under `siteurl` key. So you gonna need to duplicate this table and tell wordpress to use it.

#### Duplicate *wp_options* table

Create a copy of original table :

```
CREATE TABLE wp_second_options LIKE wp_options;

INSERT INTO wp_second_options SELECT * FROM wp_options;
```

Then change `siteurl` to point on the second wordpress site :

```
UPDATE wp_second_options SET option_value='http://second-site.dev' WHERE option_name='siteurl';
```

#### Change table name in settings

Open file */wp-includes/load.php* and find `wp_set_wpdb_vars()` function. In this function, wordpress sets table names and prefixes. Here, you can “force” it to use the table clone, by adding one line :

```
function wp_set_wpdb_vars() {
    ...
    $prefix = $wpdb->set_prefix( $table_prefix );

    $wpdb->options = 'wp_second_options'; // add this line
    ...
}
```

## Conclusion

Now, you can try to access admin again and it should work. This is realy fast “fix” and I don’t know how it will behave in case of update, so be careful. If you know some other way, please let me know.

#### References

[This comment](https://wordpress.org/support/topic/two-sites-two-themes-one-database-same-content#post-398505) suggests solution, but it is *N* years old.

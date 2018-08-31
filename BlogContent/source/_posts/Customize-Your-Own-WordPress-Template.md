---
title: Customize Your Own WordPress Template
tags:
  - php
  - wordpress
photos:
  - >-
    https://raw.githubusercontent.com/Ealon/ealon.github.io/master/images/ealon/default_post_image.png
description: description about this page
date: 2018-08-31 15:16:51
---

> [Repo is here](https://github.sydney.edu.au/yhua4993/wordpress-customize-autora)

## 1. Download or write your own pure html/css/js template
  * Here we downloaded the ***[Autora](https://elements.envato.com/autora-construction-business-html-template-924SLW)*** template, including all files and docs.

## 2. Install a clean WordPress framework
  1. Go to https://wordpress.org/download/ and download the latest wordpress version, and put the source code in your server root directory.
  2. Create a new MySQL database.
    * Here we name the database `wordpress-customize-autora`
    * Make sure the user has priviledge, keep the username and password
  3. Modify the `wp-config-sample.php` files
    * Change the name of `wp-config-sample.php` to `wp-config.php`
    * Modify the config parameters in `wp-config.php`, e.g.:
      ```php
      /** The name of the database for WordPress */
      define('DB_NAME', 'wordpress-customize-autora');

      /** MySQL database username */
      define('DB_USER', 'ealon');
      ...
      ```
    * Use the link https://api.wordpress.org/secret-key/1.1/salt/ in `wp-config.php`, copy the content and replace 
      ```php
      define('AUTH_KEY',         'put your unique phrase here');
      define('SECURE_AUTH_KEY',  'put your unique phrase here');
      define('LOGGED_IN_KEY',    'put your unique phrase here');
      define('NONCE_KEY',        'put your unique phrase here');
      define('AUTH_SALT',        'put your unique phrase here');
      define('SECURE_AUTH_SALT', 'put your unique phrase here');
      define('LOGGED_IN_SALT',   'put your unique phrase here');
      define('NONCE_SALT',       'put your unique phrase here');
      ```
  4. Run your Apache server, go to http://localhost/, fill in the information needed, then install wordpress.

## 3. Download and install theme template
  * Go to http://underscores.me/ and download a clean wordpress theme template.
  ![](https://raw.githubusercontent.com/Ealon/ealon.github.io/master/images/2018/Aug/WX20180831-161808.png)
  * Install the theme(by putting the extracted files into /wp-content/themes/, or by uploading).
  * Activate the theme, and delete other pre-installed themes.

## 4. Theme development
  1. Delete all other pages.
  2. Create a new menu called "Main Menu".
  3. Create a homepage
    * Copy `/theme/autora_wordpress/page.php` and rename the new file to `page-home.php`
    * Replace the top part as below 
      ```php
        <?php
        /**
        * Template Name: Home Page
        */
        get_header();
        ?>
      ```
    * Go to `Pages->Add New`, name it as **Homepage**, select template **Home Page**
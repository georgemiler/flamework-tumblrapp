flamework-tumblrapp
===

This repository includes the basic additions and changes you need to create a Flamework app which authenticates using the Tumblr API.

Setup
---

You will need to create an API Key at http://www.tumblr.com/docs/en/api/v2. This will result in a Consumer Key and a Consumer Secret. These should be placed in your config.php file like so:

<pre>$GLOBALS['cfg']['tumblr_api_key'] = 'YOUR-TUMBLR-API-CONSUMER-KEY';
$GLOBALS['cfg']['tumblr_api_secret'] = 'YOU-TUMBLR-API-CONSUMER-SECRET';
</pre>

To get started, copy in to your existing Flamework app the following files.

<ul><li>signin_tumblr_tumblrauth.php</li>
<li>auth_callback_tumblr_oauth.php</li>
<li>include/lib_tumblr_api.php</li>
<li>include/lib_tumblr_users.php</li>
<li>include/tumblroauth -- This API library can be found here http://github...</li>
<li>templates/page_auth_callback_tumblr_tumblrauth.txt</li></ul>

You will also need to modify your .htaccess file so that the /signin page is redirected as follows

<pre>RewriteRule  ^signup$			signin_tumblr_tumblrauth.php?%{QUERY_STRING}		[L]</pre>

Lastly, you will want to add the following table to your database

<pre>CREATE TABLE `TumblrUsers` (
  `user_id` bigint(64) UNSIGNED NOT NULL,
  `tumblr_username` varchar(20) NOT NULL,
  `following` int(10) UNSIGNED NOT NULL,
  `likes` int(10) UNSIGNED NOT NULL,
  `default_post_format` varchar(10) NOT NULL,
  `oauth_token` char(64) NOT NULL,
  `oauth_secret` char(64) NOT NULL,  
  PRIMARY KEY (`user_id`),
  UNIQUE KEY `by_tumblr_username` (`tumblr_username`),
  KEY `by_token` (`oauth_token`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;</pre>

That's it! You should now be able to authenticate your Flamework app using oAuth and the Tumblr API.

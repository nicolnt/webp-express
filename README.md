# WebP Express

Serve autogenerated WebP images instead of jpeg/png to browsers that supports WebP. Works on anything (media library, galleries, theme images etc).

## Description

This plugin let's you take advantage of the WebP image format without you doing anything else than installing the plugin. Just. Install and forget - And enjoy the increased performance of your website.

The plugin works by .htaccess magic coupled with an image converter. Basically, jpegs and pngs are routed to the image converter, unless the image converter has already converted the image. In that case, it is routed directly to the converted image. The images are saved in a subfolder to the "uploads" folder, preserving the same structure as the originals. In order to allow caching on CDN, the .htaccess rules add a "Vary" HTTP header when serving the WebP images.

The approach has the benefit that is works regardless of how an image found its way into your site. The plugin does not need to hook into Media Library events, Gallery events etc, because it does not need to maintain a complete collection of converted images. It makes it so much simpler -- and lighter.

Note that the plugin does not change any HTML. In the HTML the image src is still set to ie "example.jpg". To verify that the plugin is working, do the following:

- Open the page in Google Chrome
- Right-click the page and choose "Inspect"
- Click the "Network" tab
- Reload the page
- Find a jpeg or png image in the list. In the "type" column, it should say "webp"

The plugin is based on a general solution, which I also have put on github: [webp-on-demand](https://github.com/rosell-dk/webp-on-demand)

Note:
The rules created in .htaccess are sensitive to the location of your image folder and the location of wordpress. If you at some point change one of these, the rules will have to be updated. You do this by deactivating and reactivating the plugin.

Note:
Do not simply remove the plugin without deactivating it first. Deactivation takes care of removing the rules in the .htaccess file. With the rules there, but converter gone, your Google Chrome visitors will not see any jpeg images.

Note:
The plugin does not support multisite configurations. A donation may help fixing that.


## Installation

1. Upload the plugin files to the `/wp-content/plugins/webp-express` directory, or install the plugin through the WordPress plugins screen directly.
2. Activate the plugin through the 'Plugins' screen in WordPress
3. Verify that it works (see description above)

## Limitations

* The plugin does not work on Microsoft IIS server
* The plugin does not work with multisite. 
* The plugin requires PHP > 5.5.0 compiled with webp support
* The plugin has only been tested in Wordpress 4.7.5, but I expect it to work in other versions.
* The plugin has not been tested in all possible Wordpress configurations. It has been tested in the following configurations: root install, subdir install, and subdir install with redirect from root (described as method 1 (here)[https://codex.wordpress.org/Giving_WordPress_Its_Own_Directory]).
* There might be compatability issues with other plugins. For example .htaccess rules from other plugins might interfere.

## Known compatability issues
* W3TotalCache: When CDN is enabled, W3TotalCache creates some .htaccess rules which interferes when they appear before the rules created by this plugin. You can move them by manually editing the .htaccess file

## Frequently Asked Questions

### How do I donate?
Putting this question in the "frequently" asked questions section is of course some mixture of humour, sarcasm and wishful thinking. In case there really is someone out there wanting to donate, you can simply write to me, and we can arrange. My contact information is available here https://www.bitwise-it.dk/contact. I have paypal and of course an ordinary bank account.


### How do I set the WebP quality?
I will provide a setting for this in a future update. Until then, you can set it manually in the generated .htaccess. Look for the text saying "quality=80". Change the number to any number between 0-100. The setting only applies to jpegs, not png's

### I'm only interested in converting JPG's, not PNG's...
I will provide a setting for this in a future update. Until then, you can manually remove the text saying "|png" in the generated .htaccess (its there two times, remove both of them).

### How do I make this work with a CDN?
Chances are that the default setting of your CDN is not to forward any headers to your origin server. But the plugin needs the "Accept" header, because this is where the information is whether the browser accepts webp images or not. You will therefore have to make sure to configure your CDN to forward the "Accept" header.

The plugin takes care of setting the "Vary" HTTP header to "Accept" when routing WebP images. When the CDN sees this, it knows that the response varies, depending on the "Accept" header. The CDN is thus instructed not to cache the response on URL only, but also on the "Accept" header. This means that it will store an image for every accept header it meets. Luckily, there are (not that many variants for images)[https://developer.mozilla.org/en-US/docs/Web/HTTP/Content_negotiation/List_of_default_Accept_values#Values_for_an_image], so it is not an issue.

### The plugin refuses to install, because it says that the imagewebp function is not available
The image converter is using the [imagewebp()](http://php.net/manual/en/function.imagewebp.php) function in PHP to create WebP images. The function is is available from PHP 5.5.0. However, it requires that PHP is compiled with WebP support, which unfortunately aren't the case on many webhosts (according to [this link](https://stackoverflow.com/questions/25248382/how-to-create-a-webp-image-in-php)). WebP generation in PHP 5.5 requires that php is configured with the "--with-vpx-dir" flag and in PHP 7.0, php has to be configured --with-webp-dir flag [source](http://il1.php.net/manual/en/image.installation.php).


# Roadmap

* Make conversion of png optional (right now its hardcoded in the htaccess - you can change the htaccess manually)
* Add quality option (right now its hardcoded in the htaccess - you can change the htaccess manually)
* Use cweb converter (when imagewebp isn't available)
* Use imagemagic webp converter (when imagewebp isn't available)
* Add option to strip metadata




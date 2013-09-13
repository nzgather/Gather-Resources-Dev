# Retinafy

There are a number of patterns recorded here to make sure that our websites are retina-ready. You may not always find the pattern you need, but these are tried and tested.

## Overview

HiDPI screens are no longer are fringe use-case. Most phones are HiDPI, as are most tablets and more and more HiDPI computers are making it to market. An even stronger argument is that it seems like there's no going back to a pre-Retina age. So, if making sure our sites are retina-ready are about following a couple of patterns then why not.

* Use of CSS3, instead of bitmap images to create assets.
* Use of SVG for graphics, with PNG fallback (both background and inline images.)
* Always make logos SVGs.
* Providing @2x graphics for bitmap images.
* Using icon fonts.

Apart from @2x graphics, all of these techniques have an added benefit of making our pages lighter or decreasing the number of HTTP requests our pages have to make, increasing our page performance!

## CSS3 Assets

There are many features in CSS that can be used to create assets which before were only possible with a bitmap image. Not all of them are CSS3 either! Remember that [Can I Use](http://caniuse.com) is a great resource.

### CSS 2.1
* ::before and ::after pseudo-elements, combined with generated content. This allows us to throw in icons before an element, to create an arrow for the end of a button or add extra ornamentation without adding markup.

### CSS 3
* __Gradients.__ Use the [CSS3 Gradient Generator](http://www.colorzilla.com/gradient-editor/) to create the gradient code, but don't just blindly copy and paste. We're going to check to enable using SVGs for IE9+. Remove the `filter:` line and have IE8 and less fallback to a plain colour.
* __Drop Shadows.__ Use box-shadow and text-shadow (prefix-less) in your designs. Remember: a box-shadow can also be an outer glow, you can have multiple drop-shadows/text-shadows by comma delimiting the values and you can have _inset_ box-shadows allowing for inner-glows/shadows.
* __Border Radius.__ Add rounded corners to your elements. Use prefix-less.
* __CSS Arrows.__ [CSS Arrow Please](http://cssarrowplease.com) will make CSS arrows for you.
* __Feathered Strokes.__ A feathered stroke, or double stroke (1px light, 1 px dark) can be created using the `::before` and `::after` elements, gradient backgrounds and a 1px height.
* __rgba.__ Use `rgba` colours with `rgb` fallback, always. Hex colours obscure meaning, except for common colours such as greys (`#FFFFFF`, `#EEEEEE` or `#000000`) or primary colours such as red `#FF0000` which ultimately require an understanding of RGB to understand anyway.


Normally, if there's reason for something to be a PNG, then it's a great candidate to be an SVG. Common examples includes logos and diagrams.

## CSS Background Fallback

### SVG Graphics with PNG Fallback

The PNG fallback for SVG CSS backgrounds is simple. We use [Modernizr](http://modernizr.com) to check support for SVG in the browser, and if that exists, we override the PNG `background-image`. Not the use of the very specific `background-image` instead of `background`, it means we don't override values we defined for `background-size` or `background-repeat` etc.

	.logo a {
		background-size: cover;
		background-image: url(../img/logo.png);
		}
	
		.svg .logo a {
			background-image: url(../img/logo.svg);
			}

### @2x Fallback

Use [Slicy](http://slicyapp.com) to help you generate @2x graphics. Then, relying on Modernizr detecting `background-size` support (which will be needed to resize the image to fit), we apply the new background image.

	.page {
		background-image: url(../img/bg.jpg);
		}

		.backgroundsize .page {
			background-image: url(../img/bg@2x.jpg);
			background-size: cover;
			}
			
There's another method which doesn't rely on Modernizr to work, but relies on a very big assumption. That every client which supports multiple backgrounds, also supports `background-size`. Currently this is true, but it is an assumption and there's nothing in the spec preventing someone from implementing one without the other.

	.rule {
			background: url("background@1x.png");
			background: url("background@2x.png"), transparent;
			background-size: Npx;
			}
`Background-size` and the second `background` line (the one containing the transparent layer) will both work at the same time. If one isn't supported, chances are, neither is the other and the fallback is the `@1x` image. (`Npx` is the size of the background image at 1x.)

## Inline Images Fallback

### SVG Graphics with PNG Fallback

The simplest fallback for inline images relies on a piece of Javascript and a naming convention. There are a number of downsides to switching out the image post-load (the biggest being we download an image we ultimately will be replacing) but until a native solution arises, there's not much we can do.

Again, we rely on [Modernizr](http://modernizr.com) to check for SVG support in the browser. Then we switch out the `src` attribute replacing the `.png` file extension with a `.svg` file extension for all `img` elements where we've explicitly added the `class` of `.has-svg-fallback`. We need to add this explicitly because we may not provide a fallback to _every_ `.png` image.

	//PNG fallback for SVGs
	if(!Modernizr.svg) {
		$("img.has-svg-fallback").each(function(){
			$(this).attr("src",$(this).attr("src").replace(".svg",".png"));
		});
	}

	
## Icon Fonts

It is possible to [create your own icon fonts](http://www.webdesignerdepot.com/2012/01/how-to-make-your-own-icon-webfont/) and for many projects that will be necessary. For most, something like [Font Awesome](http://fortawesome.github.com/Font-Awesome/) would do just fine. Using icon fonts work the same way, no matter where you get the font file from.

The first step is do define your `@font-face`.

	@font-face {
	  font-family: 'icons';
	  src: url('../fonts/fontawesome-webfont.eot');
	  src: url('../fonts/fontawesome-webfont.eot?#iefix') format('embedded-opentype'),
		   url('../fonts/fontawesome-webfont.woff') format('woff'),
		   url('../fonts/fontawesome-webfont.ttf') format('truetype');
	  font-weight: normal;
	  font-style: normal;
	}
	
There are a number of ways we can use our icons.

### Inline Icons from HTML Entities

The following CSS will turn your HTML entities for your icons, into icons:

	font-family: "icons", "Helvetica", sans-serif;
	
The `"Helvetica", sans-serif` is your regular type's font-families.

### Generated Icons

We can also use the `::before` and `::after` pseudo-elements to insert icons in special places, denoted with the `data-icon` attribute.

Before text in a button:

	<button type="submit"><span data-icon="search">Search</span></button>
	
Before an anchor element in a list:

	<li><a href="/search/" data-icon="search">Search</a></li>
	
The CSS:

	.generatedcontent.fontface [data-icon]:before {
		display: inline-block;
		content: "";
		font-family: "icons";
		font-size: 1em;
		color: inherit;
		}

	.generatedcontent.fontface [data-icon="search"]:before { content: "\f002"; }

### Icons to replace content

Sometimes we want to replace a word, with an icon. We use the above generated icons declarations to define the icons (re-using CSS and making our code easier to maintain.) Then the following CSS to handle the replacement.

	.generatedcontent.fontface [data-icon].replace {
			display: inline-block;
			width: 1.4em; //icon width
			position: relative; //for :before to position to
			
			/* height 0, overflow hidden and padding-top is our preferred image replacement technique */
			height:0;
			overflow:hidden;
			padding-top: 1.2em;
			}
	
			.generatedcontent.fontface [data-icon].replace:before {
				position: absolute;
				top: 0;
				left: 0;
				}
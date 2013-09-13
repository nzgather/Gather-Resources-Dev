# CSS Patterns

There are a number of patterns recorded here to solve common CSS problems. You may not always find the pattern you need, but these are tried and tested.

## Clearfix
When we float elements within a container that is itself not floated, then it will collapse as we have removed all it's children from the "flow", and thus there's nothing left inside the container to pull it all the way down.

The way to fix this is with a clear fix, so named because we used the `clear` property in CSS to add "content" to the container element that isn't float, but clears all the floated elements, pulling the container down as it clears.

	.container:after {
		content:"";
		display: block;
		clear:both;
		}


## Image Replacement
There are numerous image replacement techniques around. One of the most common uses text-indent to get rid of the text (`text-indent:-9999999999px`). You should never use this technique. This technique forces the browser to render a canvas that extends 9999999999px off the left-side of the viewport. All of that space is invisible; yet all of that space is rendered.

We use a combination of `height: 0`, `padding-top: N` and `overflow: hidden`.

	.image-replacement-example {
		width:24px;
		background: url(img-24x24.jpg);
		
		/* Image replacement magic*/
		height: 0;
		padding-top: 24px;
		overflow: hidden;
		}

## Images/Object Max-width
When building websites with media-queries, often we find that internal static elements (like images, videos objects or embeds) break out of the container. They can't wrap like text. The solution is simple:
	
	img, video, embed, object { max-width: 100%; }

## Centering layout
Sometimes we want to center our entire layout, or center a container element within it's context (eg. navigation items in a nav-bar).

To do this our containing element must have a defined width (or min-width or max-width) and must not be floated.

	.centered-container-element {
		width: 960px;
		float: none;
		
		/* Centering magic*/
		margin: 0 auto;
		}
		
This sets the top and bottom margins of the element to 0, and tells the browser to figure out the other two.

	>|----auto----[defined width]----auto----|<


## Navigation

We've collected a number of patterns to use when developing responsive websites that must work across a range of viewport sizes *and* both `touch` and `no-touch` devices.

### Menu Collapse

This pattern is used for the point where a menu breaks down into a collapsed one line menu until it's interacted with. At this point, the menu expands to reveal the full list of options.

This pattern is useful because in its collapsed form this menu will display the currently active page with an indication that more is available (by default, the hamburger icon, but another icon would suffice.) If no page in the list is active (generally occurs on the homepage for the Main Menu, or L1 for sub nav) then explanatory text is displayed (by default, "Menu").

This pattern is normally wrapped in a media query and invoked when the viewport becomes to small for a horizontal navigation to fit or when screen real estate becomes increasingly valuable and it would be useful to collapse a list menu down into a collapsed menu. _Mobile._

####JS
	
	// ~theme/assets/js/main.js

	$("#access, .sub-nav").bind("touchend", function(e){
		$(this).bind("touchmove", function(e){
			$(this).addClass("moved");
		}).bind("touchend", function(e){
			$(this).unbind("touchmove");
		});

		if($(this).hasClass("moved")){
			$(this).removeClass("moved");
			e.preventDefault;
			return false;
		}else if(e.target.nodeName == "A" && $(this).hasClass("open")) {
			//Follow Link
		} else {
			$(this).toggleClass("open");
			e.preventDefault;
			return false;
		}
	});

####CSS

	/* ~theme/assets/css/main.css */

	@media screen and (max-width: Npx) {
		#access {
			width: 100%;
			position: relative;
			}

				#access:before {
					font-family: "icons";
					content: "\f0c9"; /* icon - Hamburger here */
					display: block;
					width: 2rem;
					font-size: 2rem;
					text-align: left;
					position: absolute;
					top:1rem;
					left:0;
					z-index: 3;
					}

				#access:after {
					content: "Menu"; /* Default text if no active child */
					position: absolute;
					top:0;
					left:0;
					z-index: 0; /* z-index hides it if active child exists */
					display: block;
					width: 100%;
					box-sizing: border-box;
					}

			#access a {
				width: 100%;
				padding: 1rem 0;
				display: none;
				z-index: 1; /* z-index hides default text if active child exists */
				position: relative;
				}
				
			/* this is where the magical expand occurs, on :hover, .open and specifically: always for active pages */
			
			#access:hover a,
			#access.open a,
			#access .current-menu-item a,
			#access .current-menu-ancestor a {
				display: block;
				}
		}
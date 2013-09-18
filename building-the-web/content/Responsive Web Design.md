# Responsive Web Design

Responsive web design is more than just using `@media` queries, flexible images and fluid grids. Technically, that’s how [Ethan Marcotte defined it](http://alistapart.com/article/responsive-web-design), but it’s a design approach that’s Content First.

## Overview

In this guide we will talk about implementing the three technologies required for a “responsive web design” but also how the design process is different to what you may have done before. An approach that Jeremy Keith refers to as being “[Content First](http://adactio.com/journal/4523/)”.

## Content First

Before we even talk about technology, we need to talk about how one’s approach to designing a responsive website is different to how you may have designed websites before.

Much like in programming, designers are becoming aware of the value of modular design. “Object-oriented design”:

* [Style Tiles](http://styletil.es/) by Samantha Warren
* [Object-orientated CSS](http://oocss.org/) by Nicole Sullivan
* [Twitter Bootstrap](http://twitter.github.io/bootstrap/)
* [BBC GEL](http://www.bbc.co.uk/gel/web/foundations/universal-grid/the-grid) (Global Experience Language)
* [SMACSS](http://smacss.com/) by Jonathon Snook
* [Pattern Libraries](http://www.uie.com/brainsparks/2009/01/09/tools-for-creating-pattern-libraries/) by Jared Spool

The idea is to design systems instead of mockups. Systems of re-usable types of content that are modular and can rearrange themselves, work in different contexts and feel right at home.

Starting from the content, we figure out what we’re dealing with. We figure out how to present each piece of content. Then we design a system where these pieces of content can fit together. Starting from the smallest unit —a content block— we move out, to the viewport. Instead of starting from the viewport and boxing everything in.

### Breakpoints

We’re going to talk about the technical implementation of breakpoints in a moment, but first let’s talk about what they are for.

Considering that we have design blocks of content that fit together, and not mockups that fit to a specific viewport, we can start laying out our flexible content blocks. There will come a point though, where the layout will break. Something will look wrong, or bad. Something won’t fit, or maybe it’s too stretched. The line length has exceeded the optimum length. We need to insert a new column of content, because our layout has broken.

That’s what breakpoints are for. Not for sectioning of parts of the design that is reserved for iPhone, or even for “mobile” but for managing the scaling of our website across a continuum of sizes, not a discrete set of a designs. Fixing the little things that *break* along the way.

## Media Queries

The technology most connected to “responsive web design” is media queries. Now technically, when we just use Media Queries without the fluid grid and flexible images, we’re actually only talking about “adaptive web design” where we jump between discrete designs.

We mentioned breakpoints earlier, and now we’re going to learn how to create one.

	@media screen and (condition) {
		//Normal CSS here
		
		.class {
			property: value;
			}
		}
		
Our first media query. Sort of. That `condition` needs to be defined. We normally use the following values:

* __width/height__ (including min/max)
* __device-width/device-height__ (including min/max)

But can also use:

* orientation
* aspect-ratio
* device-aspect-ratio
* color
* color-index
* monochrome
* resolution
* scan
* grid

(For more information on how to use these conditions: [CSS Media Queries](http://www.w3.org/TR/css3-mediaqueries/))

	@media screen and (max-width: 472px) {
		//Normal CSS here
		
		.class {
			property: value;
			}
		}


You’ll notice that the max-width is `472px`. That’s odd. Which device are we targeting? None! Our layout happened to break at `472px`, that’s all. We needed to fix it.

### and () and ()

Remember you can combine conditions:

	@media screen and (max-width: 472px) and (max-width: 486px){
		//Normal CSS here
		
		.class {
			property: value;
			}
		}

### Do I need to group my styles?

When people started building adaptive website, they grouped their styles. All the `320px` (read: iPhone) styles were in one media query. All their `768px` styles (read: iPad Portrait) were in another block. This made sense because they were siloing the different designs for various devices.

“Responsive web design” throws all that out the window. We just don't know what viewport size will be requesting our website, so we design for all of them, not a few. That means it makes much more sense to use media queries as they are needed, inline with your other CSS. You’re changing the `#menu` at a certain width because your layout broke, then keep that change in context. Right after your `#menu` declaration.

This may seem odd at first because you’re not used to seeing `@media` queries throughout your file, but you will quickly become used to it.

## Fluid Grids and Flexible Images

These two ideas are very much the same idea, applied to two different elements. (When we talk about *flexible images*, then remember that we mean all embedded content including `video`, `frame`, `object` etc).

When designing for a continuous screen width, we need to make sure our grids are flexible, so we make the most of the space available to us at all times. Fluid grids means using percentages for `width`s.

Flexible images means applying `max-width: 100%;` (and possibly `height: auto`) to embedded content so that they break out of their containing element.

	img, figure, video, iframe, object {
		max-width: 100%;
		height: auto;
		} 
BackstopJS
==========

**Catch CSS curve balls.**


BackstopJS tests your responsive web UI by visually comparing DOM screenshots at various viewport sizes.

**Heres the process:**

1. Set up a test config file: specify screen sizes and DOM selectors.
2. Create reference screenshots – these will be used to test future changes.
3. Make some changes to your CSS or add new DOM components.
4. Run a test. Any changes effecting your layout it will show up in the report!

    
**Backstory:** BackstopJS is basically a wrapper around the very fabulous [Resemble.js](https://github.com/Huddle/Resemble.js) component written by [James Cryer](https://github.com/jamescryer). Other implementations of Resemble.js, namely [PhantomCSS](https://github.com/Huddle/PhantomCSS) require writing long form [CasperJS](http://casperjs.org) tests. This is of course great for testing complex UI interactions – but kind of cumbersome for more simple applications like static CMS templates or other higher level sanity testing. 

BackstopJS may be just the thing if you develop custom Wordpress, Drupal or other CMS templates.  Or not, [Let me know what you think!](https://twitter.com/garris)

<strong><a href="https://twitter.com/garris" class="twitter-follow-button" data-show-count="false">Follow @garris</a></strong>
<script>!function(d,s,id){var js,fjs=d.getElementsByTagName(s)[0],p=/^http:/.test(d.location)?'http':'https';if(!d.getElementById(id)){js=d.createElement(s);js.id=id;js.src=p+'://platform.twitter.com/widgets.js';fjs.parentNode.insertBefore(js,fjs);}}(document, 'script', 'twitter-wjs');</script>



...


---



##Installation

**BackstopJS package.**  see... http://backstopjs.org/
    
    $ bower install backstopjs

**Install reporting host.**

    $ cd bower_components/backstopjs
    $ npm install

**Initalize BackstopJS.**

    $ gulp init

**If you don't already have a global PhantomJS install...** http://phantomjs.org/download.html

    $ npm install phantomjs

**If you don't already have a global CasperJS install...** http://docs.casperjs.org/en/latest/installation.html
    
    $ npm install -g casperjs




##Configuration

see `capture/config.json`

	{
		"viewports" : [
			{
			 "name": "phone",
			 "viewport": {"width": 320, "height": 480}
			}
			,{
			 "name": "tablet_v",
			 "viewport": {"width": 568, "height": 1024}
			}
			,{
			 "name": "tablet_h",
			 "viewport": {"width": 1024, "height": 768}
			}
		]
		,"grabConfigs" : [
			{
				"testName":"http://getbootstrap.com"
				,"url":"http://getbootstrap.com"
				,"ignoreSelectors": [
					"#carbonads-container"
				]
				,"selectors":[
					"header"
					,"main"
					,"body .bs-docs-featurette:nth-of-type(1)"
					,"body .bs-docs-featurette:nth-of-type(2)"
					,"footer"
					,"body"
				]
			}
		]
	}
    


##Usage

### generating (or updating) reference bitmaps

    $ gulp reference

This task will create a (or update an existing) `bitmaps_reference` directory with screen captures from the current project build.


### generating test bitmaps

    $ gulp test

This task will create a new set of bitmaps in `bitmaps_test/<timestamp>/`.  

Once the test bitmaps are generated, a report comparing the most recent test bitmaps against the current reference bitmaps will run. Significant differences will be detected and shown. 


## Usage Notes

### dynamic content

For obvious reasons, this screenshot approach is not optimal for testing live dynamic content. The best way to test a dynamic app would be to use a known static content data stub – or ideally many content stubs of varying lengths which, regardless of input length, should produce certain specific bitmap output.

That said, for a use case where you are testing a DOM with say an ad banner or a block of content which retains static dimensions, we have the `ignoreSelectors` property in `capture/config.json`...

    "ignoreSelectors": [
    	"#someAdSpaceSelector"
    ]

Any DOM selectors found in this property list will be set to `visibility:hidden`. This will hide the content from our Resemble.js analysis but still allow you to test for any changes in the overall box-model orientation or flow.


### running the report server

The test comparison report was written in Angular.js and requires a running HTTP server instance.  This instance is auto-started after a test is run.  The server is also auto-stopped after 15 minutes so you don't have to go worrying about bloaty node processes running all over the place.

To manually start the server...

    $ gulp start
    
...and to manually stop theres...

    $ gulp stop
    
    






**fin.**
# VideoJS for Rails Asset Pipeline

This gem has been modified to Rails 4.x. Video.js' language files are bundle inside this gem also.

It is used the stable version of video.js in GitHub (branch stable) to compile video.js.

The video.js' version used is 4.12.15.

## Installation

Add to your Gemfile

```ruby
gem 'videojs_rails', git: "https://github.com/josefsj3m/videojs_rails.git", branch: 'master'
```

And run bundle to install the library.

```ruby
bundle
```

Add the resources to your application.js file

```coffeescript
# app/assets/javascripts/application.js
//= require video
```

And that resource to application.css file

```sass
/*
*= require_self
*= require video-js
*/
```

And to production.rb add this line

```ruby
config.assets.precompile += %w( video-js.swf vjs.eot vjs.svg vjs.ttf vjs.woff )
```

In Rails > 4.1
Add this line to config/initializers/assets.rb

```ruby
Rails.application.config.assets.precompile += %w( video-js.swf vjs.eot vjs.svg vjs.ttf vjs.woff )
```

## Usage

```erb
<%= videojs_rails sources: { mp4: "http://domain.com/path/to/video.mp4", webm: "http://another.com/path/to/video.webm", ogg: "http://another.com/path/to/video.ogv"}, setup: "{}", controls: false, width:"400" %>
```

If you want to specify a codec for the video type modify it to include the codec, you must use double quotes for the codec. For instance, you can use this syntax for the webm:

```erb
<%= videojs_rails sources: { mp4: "http://domain.com/path/to/video.mp4", 'webm;codecs="vp8,vorbis"': "http://another.com/path/to/video.webm"}, setup: "{}", controls: false, width:"400" %>
```

Or for ogg video:

```erb
<%= videojs_rails sources: { mp4: "http://domain.com/path/to/video.mp4", 'ogg;codecs="theora,vorbis"': "http://another.com/path/to/video.ogv"}, setup: "{}", controls: false, width:"400" %>
```

If you want add a callback if user don't support JavaScript use block with displayed html code:

```erb
<%= videojs_rails sources: { mp4: "http://domain.com/path/to/video.mp4", webm: "http://another.com/path/to/video.webm" }, width:"400" do %>
	Please enable <b>JavaScript</b> to see this content.
<%- end %>
```

If using haml use this:

```haml
= videojs_rails(sources: { mp4: "http://domain.com/path/to/video.mp4", webm: "http://another.com/path/to/video.webm" }, width:"400"){Please enable <b>JavaScript</b> to see this content.}
```

## Html output

The helper method videojs_rails generates a html video tag surrounded by a div tag:

```haml
= videojs_rails sources: { mp4: video_path('intro1.mp4'),
                          'webm;codecs="vp8,vorbis"': video_path('intro3.webm')},
                         setup: "{}", controls: true, autoplay: true, loop:true, width: '100%', height: '600px', style: 'height:auto;overflow:hidden' do
  'Turn on javascript to see this video.'                           
```

The above code generates this html, and it is important to note that all tags specified in style: will be inserted into video tag:

```html
<div id="vjs_video_3" class=" video-js vjs-default-skin vjs-controls-enabled vjs-playing vjs-has-started vjs-user-inactive" data-setup="{}" style="height: 600px; overflow: hidden; width: 100%;" loop="true" autoplay="true" preload="auto">
    <video preload="auto" autoplay="" loop="" style="height:auto;overflow:hidden" data-setup="{}" class="vjs-tech" id="vjs_video_3_html5_api" src="/assets/webex-639e5225ff2f30315507fdabc5faf4db.mp4">
      <source src="/assets/intro1-639e5225ff2f30315507fdabc5faf4db.mp4" type="video/mp4"/>
      <source src="/assets/intro3-21dbbfe2f72850402d7ddbddcfc07f30.webm" type="video/webm;codecs=&quot;vp8,vorbis&quot;"/>
    </video>
</div>                              
```

## Captions

This is currently an experimental function.

```erb
<%= videojs_rails sources: { mp4: "http://domain.com/path/to/video.mp4" }, width:"400", captions: { en: { src: "http://domain.com/path/to/captions.vvt", label: "English" }, default_caption_language: :en } %>
```

## Turbolinks

Some of you might want to use VideoJS with Turbolinks. [andrkrn](https://github.com/andrkrn) provided CoffeeScript that he use:

```coffeescript
change = ->
    for player in document.getElementsByClassName 'video-js'
        video = videojs('example_video')

before_change = ->
    for player in document.getElementsByClassName 'video-js'
        video = videojs('example_video')
        video.dispose()

$(document).on('page:before-change', before_change)
$(document).on('page:change', change)
```

## Resources
http://videojs.com/
http://videojs.com/#getting-started


## Updating this gem to the latest video.js release

### Clone this repository

    git clone https://github.com/seanbehan/videojs_rails.git

### Clone video.js repository

    git clone https://github.com/videojs/video.js.git


### Run the rake videojs:update task with the tag

    TAG=v4.12.5
    rake videojs:update

Note: The build will fail if you don't have `grunt` installed.  To install it:

    cd ../video.js
    npm install -g grunt

### Make sure everything is added to git

    git add .
    git ci -m "Update to $TAG"

### Push to rubygems

* $VIDEO_JS_RAILS_HOME/vendor/assets/javascripts/video.js.erb
* $VIDEO_JS_RAILS_HOME/vendor/assets/stylesheets/video-js.css.erb

Alternatively, you can set the Flash player SWF file in your web view with the `videojs.options.flash.swf` command:
```
<script>
  videojs.options.flash.swf = "http://example.com/path/to/video-js.swf"
</script>
```
As the instructions here suggests: https://github.com/videojs/video.js/blob/stable/docs/guides/setup.md#self-hosted

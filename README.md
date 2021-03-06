# IMGKit

Create JPGs using plain old HTML+CSS. Uses [wkhtmltoimage](http://github.com/antialize/wkhtmltopdf) on the backend which renders HTML using Webkit.

Heavily based on [PDFKit](http://github.com/jdpace/pdfkit/).

## Install

### IMGKit

    gem install imgkit

### wkhtmltoimage
 * **Automatic**: `sudo imgkit --install-wkhtmltoimage`  
 install latest version into /usr/local/bin  
 (overwrite defaults with e.g. ARCHITECTURE=amd64 TO=/home/foo/bin)
 * By hand: http://code.google.com/p/wkhtmltopdf/downloads/list

## Usage
    
    # IMGKit.new takes the HTML and any options for wkhtmltoimage
    # run `wkhtmltoimage --extended-help` for a full list of options
    kit = IMGKit.new(html, :quality => 50)
    kit.stylesheets << '/path/to/css/file'
    
    # Get the image BLOB
    img = kit.to_img
    
    # Save the JPG to a file
    file = kit.to_file('/path/to/save/file.jpg')
    
    # IMGKit.new can optionally accept a URL or a File.
    # Stylesheets can not be added when source is provided as a URL of File.
    kit = IMGKit.new('http://google.com')
    kit = IMGKit.new(File.new('/path/to/html'))

    # Add any kind of option through meta tags
    IMGKit.new('<html><head><meta name="imgkit-quality" content="75")
    
## Configuration

If you're on Windows or you installed wkhtmltoimage by hand to a location other than /usr/local/bin you will need to tell PDFKit where the binary is. You can configure PDFKit like so:

    # config/initializers/imgkit.rb
    IMGKit.configure do |config|
      config.wkhtmltoimage = '/path/to/wkhtmltoimage'
      config.default_options = {
        :quality => 60
      }
    end


## Rails 

### Mime Types
register a .jpg mime type in: 

    #config/initializers/mime_type.rb
    Mime::Type.register       "image/jpeg", :jpg

### Controller Actions
You can then send JPGs with

    format.jpg do
      send_data(@kit.to_img, :type => "image/jpeg", :disposition => 'inline')
    end

This allows you to take advantage of rails page caching so you only generate the
image when you need to.

## Note on Patches/Pull Requests
 
* Fork the project.
* Setup your development environment with: gem install bundler; bundle install
* Make your feature addition or bug fix.
* Add tests for it. This is important so I don't break it in a
  future version unintentionally.
* Commit, do not mess with rakefile, version, or history.
  (if you want to have your own version, that is fine but bump version in a commit by itself I can ignore when I pull)
* Send me a pull request. Bonus points for topic branches.

## Copyright

Copyright (c) 2010 <a href="mailto:christopher.continanza@gmail.com">Chris Continanza</a>
Based on work by Jared Pace  
See LICENSE for details.

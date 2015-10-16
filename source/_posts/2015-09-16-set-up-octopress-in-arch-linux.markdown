---
layout: post
title: "Set Up Octopress in Arch linux"
date: 2015-09-16 20:22:31 +0300
comments: true
categories:
---
Disclaimer: Hack and slash

Well I started used an ancient arch install I had - and I needed to use
my blog. Given I know nothing of ruby and octopress had left me with
open questions I doubted I'd succed. Well I did - here is how:


    [root@mrsd-arch utumno]# pacman -S ruby
    Packages (2) libyaml-0.1.6-1  ruby-2.2.3-1
    ##: hmmm - warning below I missed, see below
    The default location of gem installs is $HOME/.gem/ruby
    Add the following line to your PATH if you plan to install using gem
    $(ruby -rubygems -e "puts Gem.user_dir")/bin
    ##: ....

Then:

    [root@mrsd-arch utumno]# gem install bundler
    Fetching: bundler-1.10.6.gem (100%)
    ##: hmmm - warning below I missed, see below
    WARNING:  You don't have /root/.gem/ruby/2.2.0/bin in your PATH,
    	  gem executables will not run.
    Successfully installed bundler-1.10.6
    ##: ....
    [root@mrsd-arch utumno]# exit

I did not want to _install_ octopress - just keep using my existing blog:

    utumno@mrsd-arch ~ $ cd /mnt/win/Dropbox/Udun/octopress/
    utumno@mrsd-arch /mnt/win/Dropbox/Udun/octopress (source) $ rake preview
    rake aborted!
    LoadError: cannot load such file -- bundler/setup
    /mnt/win/Dropbox/Udun/octopress/Rakefile:2:in `<top (required)>'
    (See full trace by running task with --trace)

Remember the warnings ? [Issue][1]:

    PATH="$(ruby -e 'print Gem.user_dir')/bin:$PATH"

Then - below from a fresh install:

    utumno@mrsd-arch /mnt/win/Dropbox/Udun/arch_octopress/octopress (master) $ bundle install
    Fetching gem metadata from https://rubygems.org/...........
    Fetching version metadata from https://rubygems.org/...
    Fetching dependency metadata from https://rubygems.org/..
    Resolving dependencies...
    Using rake 10.4.2
    ##: ....
    Password:

    SystemExit: exit
    An error occurred while installing RedCloth (4.2.9), and Bundler cannot
    continue.
    Make sure that `gem install RedCloth -v '4.2.9'` succeeds before bundling.

Ooops - back to my own blog:

<--! more -->

    utumno@mrsd-arch /mnt/win/Dropbox/Udun/arch_octopress/octopress (master) $ cd ../../octopress/
    utumno@mrsd-arch /mnt/win/Dropbox/Udun/octopress (source) $ rake preview
    You have requested:
      RedCloth ~> 4.2.9

    The bundle currently has RedCloth locked at 4.2.9.
    Try running `bundle update RedCloth`
    Run `bundle install` to install missing gems.

No clue what all this means:

    utumno@mrsd-arch /mnt/win/Dropbox/Udun/octopress (source) $ bundle update RedCloth
    Fetching gem metadata from https://rubygems.org/...........
    ##: ....
    Password:
    Installing rake 10.3.2
    Installing RedCloth 4.2.9 with native extensions
    Installing blankslate 2.1.2.4
    Installing timers 1.1.0
    Installing celluloid 0.15.2
    Installing chunky_png 1.3.1
    Installing fast-stemmer 1.0.2 with native extensions
    Installing classifier 1.3.4
    Installing coffee-script-source 1.7.1
    Installing execjs 2.2.1
    Installing coffee-script 2.3.0
    Installing colorator 0.1
    Installing fssm 0.2.10
    Installing sass 3.2.19
    Installing compass 0.12.7
    Installing ffi 1.9.3 with native extensions
    Installing tilt 1.4.1
    Installing haml 4.0.5
    Installing jekyll-coffeescript 1.0.0
    Installing jekyll-gist 1.1.0
    Installing jekyll-paginate 1.0.0
    Installing jekyll-sass-converter 1.2.0
    Installing rb-fsevent 0.9.4
    Installing rb-inotify 0.9.5
    Installing listen 2.7.9
    Installing jekyll-watch 1.0.0
    Installing kramdown 1.4.1
    Installing liquid 2.6.1
    Installing mercenary 0.3.4
    Installing posix-spawn 0.3.9 with native extensions
    Installing yajl-ruby 1.1.0 with native extensions
    Installing pygments.rb 0.6.0
    Installing redcarpet 3.1.2 with native extensions
    Installing safe_yaml 1.0.3
    Installing parslet 1.5.0
    Installing toml 0.1.1
    Installing jekyll 2.2.0
    Installing jekyll-date-format 1.0.1
    Installing jekyll-page-hooks 1.3.1
    Installing jekyll-sitemap 0.5.1
    Installing rack 1.5.2
    Installing rack-protection 1.5.3
    Installing rdiscount 2.1.7.1 with native extensions
    Installing rubypants 0.2.0
    Installing sass-globbing 1.0.0
    Installing sinatra 1.4.5
    Installing stringex 1.4.0
    Using bundler 1.10.6
    Bundle updated!

Hmm - let's see:

    utumno@mrsd-arch /mnt/win/Dropbox/Udun/octopress (source) $ rake previewrake aborted!
    Gem::LoadError: You have already activated rake 10.4.2, but your Gemfile requires rake 10.3.2. Prepending `bundle exec` to your command may solve this.
    /home/utumno/.gem/ruby/2.2.0/gems/bundler-1.10.6/lib/bundler/runtime.rb:34:in `block in setup'
    /home/utumno/.gem/ruby/2.2.0/gems/bundler-1.10.6/lib/bundler/runtime.rb:19:in `setup'
    /home/utumno/.gem/ruby/2.2.0/gems/bundler-1.10.6/lib/bundler.rb:127:in `setup'
    /home/utumno/.gem/ruby/2.2.0/gems/bundler-1.10.6/lib/bundler/setup.rb:8:in `<top (required)>'
    /mnt/win/Dropbox/Udun/octopress/Rakefile:2:in `<top (required)>'
    LoadError: cannot load such file -- bundler/setup
    /mnt/win/Dropbox/Udun/octopress/Rakefile:2:in `<top (required)>'
    (See full trace by running task with --trace)

That's ruby traceback - help - cross fingers:

    utumno@mrsd-arch /mnt/win/Dropbox/Udun/octopress (source) $ bundle exec rake preview
    Starting to watch source with Jekyll and Compass. Starting Rack on port 4000

    ######### YEY

Create new post works:

    utumno@mrsd-arch /mnt/win/Dropbox/Udun/octopress (source) $ bundle exec rake new_post["Pacman pycharm"]
    mkdir -p source/_posts
    Creating new post: source/_posts/2015-09-15-pacman-pycharm.markdown

But...

    utumno@mrsd-arch /mnt/win/Dropbox/Udun/octopress (source) $ bundle exec rake generate
    ## Generating Site with Jekyll
    overwrite source/stylesheets/screen.css
    /usr/lib/ruby/gems/2.2.0/gems/safe_yaml-1.0.3/lib/safe_yaml/load.rb:43:in `<module:SafeYAML>': undefined method `tagged_classes' for Psych:Module (NoMethodError)
    	from /usr/lib/ruby/gems/2.2.0/gems/safe_yaml-1.0.3/lib/safe_yaml/load.rb:26:in `<top (required)>'
    	from /usr/lib/ruby/gems/2.2.0/gems/jekyll-2.2.0/lib/jekyll.rb:26:in `require'
    	from /usr/lib/ruby/gems/2.2.0/gems/jekyll-2.2.0/lib/jekyll.rb:26:in `<top (required)>'
    	from /usr/lib/ruby/gems/2.2.0/gems/jekyll-2.2.0/bin/jekyll:6:in `require'
    	from /usr/lib/ruby/gems/2.2.0/gems/jekyll-2.2.0/bin/jekyll:6:in `<top (required)>'
    	from /usr/bin/jekyll:23:in `load'
    	from /usr/bin/jekyll:23:in `<main>'

Opsie-wopsie - another ruby traceback - [happily I wasn't the only one][2] - I had to run update:

    utumno@mrsd-arch /mnt/win/Dropbox/Udun/octopress (source) $ bundle update
    Fetching gem metadata from https://rubygems.org/...........
    Fetching version metadata from https://rubygems.org/...
    Fetching dependency metadata from https://rubygems.org/..
    Resolving dependencies...
    Using rake 10.4.2 (was 10.3.2)
    Using RedCloth 4.2.9
    Using blankslate 2.1.2.4
    ##: ....
    Password:
    Installing hitimes 1.2.3 with native extensions
    Installing timers 4.0.4 (was 1.1.0)
    Installing celluloid 0.16.0 (was 0.15.2)
    Installing chunky_png 1.3.4 (was 1.3.1)
    Using fast-stemmer 1.0.2
    Installing classifier-reborn 2.0.3
    Installing coffee-script-source 1.9.1.1 (was 1.7.1)
    Installing execjs 2.6.0 (was 2.2.1)
    Installing coffee-script 2.4.1 (was 2.3.0)
    Using colorator 0.1
    Using fssm 0.2.10
    Using sass 3.2.19
    Using compass 0.12.7
    Installing ffi 1.9.10 (was 1.9.3) with native extensions
    Installing tilt 2.0.1 (was 1.4.1)
    Installing haml 4.0.7 (was 4.0.5)
    Installing jekyll-coffeescript 1.0.1 (was 1.0.0)
    Installing jekyll-gist 1.3.4 (was 1.1.0)
    Installing jekyll-paginate 1.1.0 (was 1.0.0)
    Installing jekyll-sass-converter 1.3.0 (was 1.2.0)
    Installing rb-fsevent 0.9.6 (was 0.9.4)
    Using rb-inotify 0.9.5
    Installing listen 2.10.1 (was 2.7.9)
    Installing jekyll-watch 1.2.1 (was 1.0.0)
    Installing kramdown 1.8.0 (was 1.4.1)
    Installing liquid 2.6.3 (was 2.6.1)
    Installing mercenary 0.3.5 (was 0.3.4)
    Installing posix-spawn 0.3.11 (was 0.3.9) with native extensions
    Installing yajl-ruby 1.2.1 (was 1.1.0) with native extensions
    Installing pygments.rb 0.6.3 (was 0.6.0)
    Installing redcarpet 3.3.2 (was 3.1.2) with native extensions
    Installing safe_yaml 1.0.4 (was 1.0.3)
    Using parslet 1.5.0
    Installing toml 0.1.2 (was 0.1.1)
    Installing jekyll 2.5.3 (was 2.2.0)
    Using jekyll-date-format 1.0.1
    Using jekyll-page-hooks 1.3.1
    Installing jekyll-sitemap 0.8.1 (was 0.5.1)
    Installing rack 1.6.4 (was 1.5.2)
    Using rack-protection 1.5.3
    Installing rdiscount 2.1.8 (was 2.1.7.1) with native extensions
    Using rubypants 0.2.0
    Using sass-globbing 1.0.0
    Installing sinatra 1.4.6 (was 1.4.5)
    Using stringex 1.4.0
    Using bundler 1.10.6
    Bundle updated!

    utumno@mrsd-arch /mnt/win/Dropbox/Udun/octopress (source) $ bundle exec rake generate
    ## Generating Site with Jekyll
    identical source/stylesheets/screen.css
    /usr/lib/ruby/gems/2.2.0/gems/execjs-2.6.0/lib/execjs/runtimes.rb:48:in `autodetect': Could not find a JavaScript runtime. See https://github.com/rails/execjs for a list of available runtimes. (ExecJS::RuntimeUnavailable)
    	from /usr/lib/ruby/gems/2.2.0/gems/execjs-2.6.0/lib/execjs.rb:5:in `<module:ExecJS>'
    	from /usr/lib/ruby/gems/2.2.0/gems/execjs-2.6.0/lib/execjs.rb:4:in `<top (required)>'
    	from /usr/lib/ruby/gems/2.2.0/gems/coffee-script-2.4.1/lib/coffee_script.rb:1:in `require'
    	from /usr/lib/ruby/gems/2.2.0/gems/coffee-script-2.4.1/lib/coffee_script.rb:1:in `<top (required)>'
    	from /usr/lib/ruby/gems/2.2.0/gems/coffee-script-2.4.1/lib/coffee-script.rb:1:in `require'
    	from /usr/lib/ruby/gems/2.2.0/gems/coffee-script-2.4.1/lib/coffee-script.rb:1:in `<top (required)>'
    	from /usr/lib/ruby/gems/2.2.0/gems/jekyll-coffeescript-1.0.1/lib/jekyll-coffeescript.rb:2:in `require'
    	from /usr/lib/ruby/gems/2.2.0/gems/jekyll-coffeescript-1.0.1/lib/jekyll-coffeescript.rb:2:in `<top (required)>'
    	from /usr/lib/ruby/gems/2.2.0/gems/jekyll-2.5.3/lib/jekyll/deprecator.rb:46:in `require'
    	from /usr/lib/ruby/gems/2.2.0/gems/jekyll-2.5.3/lib/jekyll/deprecator.rb:46:in `block in gracefully_require'
    	from /usr/lib/ruby/gems/2.2.0/gems/jekyll-2.5.3/lib/jekyll/deprecator.rb:44:in `each'
    	from /usr/lib/ruby/gems/2.2.0/gems/jekyll-2.5.3/lib/jekyll/deprecator.rb:44:in `gracefully_require'
    	from /usr/lib/ruby/gems/2.2.0/gems/jekyll-2.5.3/lib/jekyll.rb:166:in `<top (required)>'
    	from /usr/lib/ruby/gems/2.2.0/gems/jekyll-2.5.3/bin/jekyll:6:in `require'
    	from /usr/lib/ruby/gems/2.2.0/gems/jekyll-2.5.3/bin/jekyll:6:in `<top (required)>'
    	from /usr/bin/jekyll:23:in `load'
    	from /usr/bin/jekyll:23:in `<main>'
    utumno@mrsd-arch /mnt/win/Dropbox/Udun/octopress (source) $

Well - clear enough (although I had no idea) - decided to go with nodejs:

    [root@mrsd-arch utumno]# pacman -S nodejs
    resolving dependencies...
    looking for conflicting packages...

    Packages (1) nodejs-0.12.7-1
    ##: ....
    Optional dependencies for nodejs
        npm: nodejs package manager

Then:

    utumno@mrsd-arch /mnt/win/Dropbox/Udun/octopress (source) $ bundle exec rake generate
    ## Generating Site with Jekyll
    identical source/stylesheets/screen.css
    Configuration file: /mnt/win/Dropbox/Udun/octopress/_config.yml
                Source: source
           Destination: public
          Generating...
         Build Warning: Layout 'nil' requested in blog/categories/octopress/atom.xml does not exist.
         Build Warning: Layout 'nil' requested in blog/categories/jekyll/atom.xml does not exist.
         Build Warning: Layout 'nil' requested in blog/categories/python/atom.xml does not exist.
         Build Warning: Layout 'nil' requested in blog/categories/java/atom.xml does not exist.
         Build Warning: Layout 'nil' requested in blog/categories/eclipse/atom.xml does not exist.
         Build Warning: Layout 'nil' requested in blog/categories/maven/atom.xml does not exist.
         Build Warning: Layout 'nil' requested in blog/categories/git/atom.xml does not exist.
    jekyll 2.5.3 | Error:  undefined method `gsub' for #<Array:0x0000000218c8b8>
    utumno@mrsd-arch /mnt/win/Dropbox/Udun/octopress (source) $

AAARGHH moment - googling for this did not really help - made me start hacking
and slashing in [/plugins/octopress_filters.rb][3]. Ruby tracebacks kept
shifting and I quited.

Till next day it dawned me - how about updating octopress itself ? NB: [I am not
sure how to go about this][4], but I do it by

    $ git checkout master # in my blog repository, 'source' branch is checked out
    $ git pull # if octopress is set up correctly pulls the latest octopress master
    $ git checkout source # create a backup branch if rebase below fails
    $ git rebase master

Unfortunately the last part of the update did not run for me (edit: to solve this
delete the sass.old by yourself.):

	utumno@mrsd-arch /mnt/win/Dropbox/Udun/octopress (source) $ bundle exec rake update_style
	removed existing sass.old directory
	rm -r sass.old
	rake aborted!
	ArgumentError: parent directory is world writable, FileUtils#remove_entry_secure does not work; abort: "sass.old" (parent directory mode 40777)
	/mnt/win/Dropbox/Udun/octopress/Rakefile:186:in `block in <top (required)>'
	Tasks: TOP => update_style
	(See full trace by running task with --trace)

Well - no harm I guess (and why being world writable is a problem escapes me) - *cause generate worked*:

    utumno@mrsd-arch /mnt/win/Dropbox/Udun/octopress (source) $ bundle exec rake generate
    ## Generating Site with Jekyll
        write source/stylesheets/screen.css
    Configuration file: /mnt/win/Dropbox/Udun/octopress/_config.yml
                Source: source
           Destination: public
          Generating...
         Build Warning: Layout 'nil' requested in blog/categories/octopress......
     Auto-regeneration: disabled. Use --watch to enable.

YEY!

Edit: to solve the traceback above delete the sass.old by yourself.

  [1]: https://wiki.archlinux.org/index.php/Ruby#Setup
  [2]: http://stackoverflow.com/a/29245615/281545
  [3]: https://github.com/imathis/octopress/issues/1669
  [4]: http://stackoverflow.com/q/23438578/281545

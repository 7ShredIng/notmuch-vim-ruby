## notmuch vim ruby

This is a vim plug-in that provides a fully usable mail client interface,
utilizing the notmuch framework, through it's ruby bindings.

It aims to provide a familiar experience for the people already using the
existing vim plug-in in notmuch, but with more features.

## Features

 * Most of the basic features of the current plug-in
 * Gradual searches; you don't have to wait for the whole search to finish,
   sort of like the 'less' command
 * Proper multi-part handling; finds out if there's text/plain, or if
   text/html, converts it using elinks.
 * Extract all attachments
 * Open message with mutt (or any external application that can open an mbox)
 * More proper UTF-8 handling
 * Configurable key mappings
 * Much simpler, cleaner, beautiful, and extensible code (only 600 lines!)

## Install

### Homebrew

* Install notmuch using cehoffman's homebrew
[formula](https://github.com/cehoffman/homebrew/blob/master/Library/Formula/notmuch.rb).
* Add `$(brew --prefix)/lib/ruby/1.9.1/*` to your `RUBYOPT` env variable for
  notmuch ruby bindings
* Install notmuch-ruby using pathogen, vundler, or plain copying
* Install [bundler](http://www.gembundler.com) for the ruby vim was compiled
  against
* Run `bundle install --path vendor/` from the notmuch-ruby's plugin root directory
* Add something like the following to your vimrc to make original notmuch vim
  plugin accessible

```vim
  set runtimepath=~/.vim,$VIMRUNTIME
  if executable('brew')
    exe "set runtimepath+=" . substitute(system('brew --prefix'), "\\n", '', 'g') . '/share/vim'
  endif
  set runtimepath+=~/.vim/after
```

### Non Homebrew

#### Ruby Bindings

Go to notmuch source code, and then 'bindings/ruby':

    % ruby extconf.rb
    % make
    % sudo make install

If you have installed libnotmuch in a non-standard location, you might want to
add a few flags, so you don't have to export `LD_LIBRARY_PATH` all the time.

    % make LOCAL_LIBS='-Wl,--enable-new-dtags -Wl,-rpath,/opt/notmuch/lib'

If you don't want to mess with you system, you can just copy the .so file to
any place you want, but make sure you export the `RUBYLIB` variable to point
there.

You should now be able to run

    % ruby -e "require 'notmuch'"

cleanly. If you don't see any errors, it means it's working :)

#### Actual Vim Plugin

You first need to install the original vim plugin, or at least have the syntax
files available:

    ~/.vim/syntax/notmuch-{compose,folders,git-diff,search,show}.vim

If you have the notmuch source code available, just go to the 'vim' directory
and type 'make install' (make sure you create the missing directories if the
build complains).

Then just copy install notmuch-ruby using pathogen, vundler, or however
you prefer to install vim plugins.

#### Mail Gem

Since libnotmuch library concentrates on things other than handling mail, we
need a library to do that, and for Ruby the best library for that is called
'mail'. The easiest way to install it is with [bundler](http://www.gembundler.com).

Once you have bundler run the following from where you installed the plugin:

    % bundle install --path vendor/

That's it just, fire it up!

    % gvim -c ':NotMuchR'

Enjoy ;)

## More stuff

As an example to configure a key mapping to add the tag 'to-do' and archive,
this is what I use:

    let g:notmuch_rb_custom_search_maps = {
      \ 't':		'search_tag("+to-do -inbox")',
      \ }

    let g:notmuch_rb_custom_show_maps = {
      \ 't':		'show_tag("+to-do -inbox")',
      \ }

https://github.com/felipec/notmuch-vim-ruby

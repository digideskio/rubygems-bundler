[![Build Status](https://secure.travis-ci.org/mpapis/rubygems-bundler.png?branch=1.0.0)](http://travis-ci.org/mpapis/rubygems-bundler)
[![Dependency Status](https://gemnasium.com/mpapis/rubygems-bundler.png)](https://gemnasium.com/mpapis/rubygems-bundler)

# rubygems-bundler && Noexec

Let's stop using `bundle exec`, kthx.

Introduction of the 1.0.0 release: http://niczsoft.com/2012/05/rubygems-bundler-integration-gem-1-0-0/

## Installation

    gem install rubygems-bundler

Next run (once):

    gem regenerate_binstubs

And you're done!

## Configuration

### ~/.gemrc

It's no more required to modify `~/.gemrc` anymore,
just remove the old entry to be sure it works as expected.
In case you need to use your own `custom_shebang`
you can define it in `~/.gemrc` to override the default:

    custom_shebang: $env <your_custom_shebang_program>

### ./.noexec.yaml

Though you can let noexec do it's own thing and rely on looking up your binary via your Gemfile, 
you can also specify which binaries you want included or excluded. 
Create a .noexec.yaml file along side any Gemfiles you want to use. 
Then, to enable (or disable) the usage of your particular binary into your bundle, 
add an include or exclude section. For example:

```yml
exclude: [rake]
```
Or, 

```yml
include: [haml]
```

### NOEXEC=skip

In case you need explicitly skip loading Bundler.setup, prefix your command with `NOEXEC=0`:

    NOEXEC=0 rails new app
    NOEXEC=skip gist

both `0` and `skip` will work, choose the one that sounds better for you.

## Problems?

Things not going the way you'd like? Try your command again with 
`NOEXEC_DEBUG=1` set and create a ticket. I'll fix it right away!

### IRC support:

[#rubygems-bundler on irc.freenode.net](http://webchat.freenode.net/?channels=#rubygems-bundler)


## How does this work (ruby_noexec_wrapper)

It modifies gem wrappers shebang to load `ruby_noexec_wrapper`.
Then, when you run gem binaries, it takes a look at your working directory,
and every directory above it until it can find a `Gemfile`. 
If the executable you're running is present in your Gemfile, 
it switches to using that `Gemfile` instead (via `Bundle.setup`).

Rubygems and Bundler integration, makes executable wrappers
generated by rubygems aware of bundler.

rubygems-bundler was merged with [noexec gem](https://github.com/joshbuddy/noexec) in version **0.9.0**.

# Uninstallation

    rubygems-bundler-uninstaller
    gem uninstall rubygems-bundler

this will set all gems to `/usr/bin/env ruby` which is one of the safest choices (especially when using rvm).

# Authors

 - Joshua Hull <joshbuddy@gmail.com>
 - Michal Papis <mpapis@gmail.com>

# Thanks

 - Carl Lerche     : help with the noexec code
 - Evan Phoenix    : support on rubygems internalls
 - Yehuda Katz     : the initial patch code, helping on making it even more usable
 - Loren Segal     : shebang customization idea and explanations
 - Wayne E. Seguin : support in writing good code
 - André Arko      : clarifications how rubygems/bundler works

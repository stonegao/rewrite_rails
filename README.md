The RewriteRails Plug-In
========================

RewriteRails adds syntactic abstractions like [Andand](http://github.com/raganwald/rewrite_rails/tree/master/doc/andand.textile "") and [String to Block](http://github.com/raganwald/rewrite_rails/tree/master/doc/string_to_block.md "") to Rails projects without monkey-patching. All of the power and convenience, none of the compatibility woes and head-aches.

Should You Care?
---

If you're already using gems like `Object#andand` or `String#to_proc`, RewriteRails is absolutely for you. You can continue to do what you're already doing, but your rails code will be faster and you will never have to worry about some gems conflicting with each other and with ActiveSupport as it grows.

If you have considered using `Object#andand` or `String#to_proc`, but hesitated because you are worried about encumbering classes like `Object` and `String` with even more methods, RewriteRails is for you. You get to use these powerful constructs without monkey-patching. You read that right. **RewriteRails is a No Monkey-Patching Zone**.

*If you want the power and convenience without the head-aches. RewriteRails is for you.*

Q & A
-----

**How does it work?**

Install the `RewriteRails` plugin in your Rails project and the gems ParseTree and Ruby2Ruby (in your system or frozen into your project). You can write ruby files as usual (e.g. `foo_bar.rb`), and things will work as usual. You can also have `RewriteRails` rewrite Ruby files for you. Any file with the suffix `.rr` will be "rewritten."

RewriteRails takes your `.rr` files and scans them with *rewriters*. Each rewriter looks for a certain kind of Ruby code and rewrites it into another kind of Ruby code. This produces the same effect as a C Preprocessor, a C++ template, or a Lisp Macro.

Currently, the rewriters are things that could be implemented by opening core classes and performing metaprogramming wizardry, but implementing them as rewriters means that you have higher performance and fewer conflicts with existing code.

By default, the rewritten files are stored in the `rewritten` directory of your project. So if you create a file called `foo.rr` in `lib` directory, you will find a file called `foo.rb` in `rewritten/lib`. This means you can always see what RewriteRails is doing, and if you want to stop using it you have 100% working Ruby files.

**How do I know what will be rewritten?**

Consult [the doc folder](http://github.com/raganwald/rewrite_rails/tree/master/doc). Every rewriter gets its own page. At the moment, those are [Andand](http://github.com/raganwald/rewrite_rails/tree/master/doc/andand.textile "doc/andand.textile"), [Into](ttp://github.com/raganwald/rewrite_rails/tree/master/doc/into.md) and [String to Block](http://github.com/raganwald/rewrite_rails/tree/master/doc/string_to_block.md "doc/string_to_block.md"). More will be added as I write them or port them from the old rewrite gem.

**I like this for development, but I don't want to install all those gems on my server**

1. Run `rake rewrite:prepare`. This will recursively rewrite all of the `.rr` files in your project so that it is not necessary to run them in production.
2. Do not install the RewriteRails plugin on your server.
3. Open up `config/environments/production.rb` and add the following lines
  * `config.load_paths += %W( #{RAILS_ROOT}/rewritten/app/controllers )`
  * `config.load_paths += %W( #{RAILS_ROOT}/rewritten/app/helpers )`
  * `config.load_paths += %W( #{RAILS_ROOT}/rewritten/app/models )`
  * `config.load_paths += %W( #{RAILS_ROOT}/rewritten/app/lib )`
  * ...and any other directories where you might place `.rr` files

Now in production files will not be rewritten but Rails will automatically load the rewritten files from the `rewritten` directory. (TODO: Automate this.) 

**How does this differ from the rewrite gem?**

Where the rewrite gem allows you to rewrite specific blocks of code and to treat rewriters as first-class entities for meta-meta-programming, `RewriteRails` simply rewrites entire files with a known set of rewriters.

**That was fun, but we hired a new architect who has decided make his mark by criticizing all of our decisions and insists we stop all new development while we migrate it out of our million line project. Are we fuxxored?**

Your new smartest guy in the room might be fuxxored, but your code is safe. Simply run `rake rewrite:all rewritten=.` This does the `prepare` task that rewrites all of your code in place, but instead of placing the resulting `.rb` files in a hierarchy in the `rewritten` folder, it places them all in a hierarchy in the root of your project. Which means, they go right next to the .rr files. You can now remove the rewrite plugin and carry on. Removing the out-dated `.rr` files from the command line shouldn't be a problem for your new smart fellow.

The summary is that you can experiment with `RewriteRails` as much as you like, but you are not painting yourself into a corner. You can escape to standard Ruby at any time.

Installation and Dependencies
------------

1. `sudo gem install ParseTree`
2. `sudo gem install ruby2ruby`
3. Clone this project into the `vendor/plugins` directory of your project.

Legal
-----

Copyright (c) 2008 Reginald Braithwaite, released under the [MIT license](http:MIT-LICENSE).

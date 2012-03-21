# Shoestrap

Shoestrap is a simple framework to bootstrap *nix machines.

It speaks Bash so there's virtually no learning curve. More importantly, you
won't have to learn yet another DSL. Shoestrap aims to get out of your way.

You should be able to get up and running in minutes, not hours.


## What about Chef, Puppet and co.?

Chef and Puppet are great tools, but they are too complex for most use cases.
The learning curve for these tools is quite steep as they each have their own
DSL. On the other end, Shoestrap is just Bash. It does not require any
'Bash to config files' translation.

I believe Shoestrap is a great simple alternative to Chef or Puppet that will
fulfill the needs of most people.


## Terminology

Shoestrap uses some of the Chef terminology since I couldn't come up with
better names or analogies.

### Cookbook

A cookbook is a Bash script that executes different actions. For example,
it may install packages, run 'recipes'. Think of it as a dispatcher.

Cookbooks live at the root of your Shoestrap project. You can have multiple
cookbooks per project.

### Recipes

Recipes are snippets of Bash code that can be executed from a Cookbook. For
example, you may have a recipe to install `memcached`, or a recipe to setup
SSH keys on the target machine. Remember, it's just Bash, so anything goes.

### Assets

An asset is a file that will be needed by the target machine. For example,
a configuration file or an init script.


## Helpers

Shoestrap ships with many Bash helpers functions. They can be found in
`helpers/default`. You do NOT need to use the built-in helper functions,
but they will simplify many of the most common tasks you'll need to perform.

Helper functions can be used from cookbooks or recipes. You may also pass
arguments to these functions.

You may add your own helper functions in `helpers/custom`.

Here are some of the most commonly used helpers:

#### `add_line`
Concatenate a line to a text file if it's not already there.

#### `add_user`
Add a user to the system.

#### `copy`
Copy an asset file. It first looks in the assets/{cookbook} directory and falls back to assets/default if file doesn't exist.

#### `error`
Write an error to the screen and halt execution.

#### `is_installed`
Check if an element has already been installed. Useful to prevent code from running more than once. Also see `set_installed`.

#### `log`
Write a line to the screen.

#### `package`
Install a package (ie: apt-get install {package-name}).

#### `package_update`
Update packages in package manager (ie: apt-get update).

#### `recipe`
Run a recipe. It first looks in the recipes/{cookbook} directory and falls back to recipes/default if file doesn't exist.

#### `set_installed`
Sets an element as 'installed'.


## Getting Started

1.  Clone the `shoestrap` repo to your local machine.
    `git clone https://github.com/cmer/shoestrap.git`

2.  Rename `./my-cookbook` to something a little bit more meaningful. For example,
    you might want to call your cookbook `web` if it bootstraps a web server. Make
    sure it is executable (`chmod +x {my-cookbook}`).

3.  Specify actions to take in the cookbook. For example, which recipes to run, which
    packages to install or which user(s) to add. For example: `recipe 'nginx'`.

4.  Create a recipe file under `recipes/default`. For example: `recipes/default/nginx`. The recipe
    is the code to execute. In our example, it would be the code to run to install `nginx`.

5.  Add assets (if needed) under `assets/default/{recipe}`. For example: `assets/default/nginx/nginx.conf`.

6.  Upload your project to the target machine. You can use `scp`, Capistrano, Git or whatever you feel
    comfortable with.

7.  Run your cookbook from the target machine. For example: `sudo ./web`.


## Example

You can see a sample project at http://github.com/cmer/shoestrap-example

Browse the source code, it's the best way to familiarize yourself with Shoestrap. It's also a great starting
point for your own Shoestrap project.


## Example: Directory Structure of a Shoestrap Project

    [assets]
      [default]            # Assets to be used by default
        [recipe1]          # Assets for 'recipe1'.
          foo.conf
          bar.conf
      [cookbook1]          # Assets for 'cookbook1'. If asset cannot be found here, fallback is 'default'
        [recipe1]          # Assets for 'recipe1' when executed from 'cookbook1'. Overrides anything in [default].
          foo.conf
    [helpers]
      custom               # Your custom Bash functions and helpers
      default              # Shoestrap's default helpers
      initialize           # Initialize script.
    [recipes]
      [default]            # Recipes to be used by default
        recipe1
        recipe2
        recipe3
      [cookbook1]          # Recipes for 'cookbook1'. Overrides anything in [default].
        recipe1
    cookbook1              # The cookbook script itself. This is your point of entry to Shoestrap


## Compatibility

Shoestrap has only been tested with Ubuntu Oneiric 11.10 but should work with any/most Unix-like
operating systems. My goal is to support Ubuntu/Debian, CentOS/Red Hat and Mac OS X. I will need
help from the community to achieve this, however.

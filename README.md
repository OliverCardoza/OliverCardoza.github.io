# Oh dear

You've done it again...

## Development

Execute below to set up the dev env. Full honesty, this is cobbled together
with duct tape and hope. It doesn't use fancy dev VMs, Docker, or whatever the
kids be using nowadays. I personally find the requirement for `zlib1g-dev` to
be kind of ridiculous (required by github-pages gem dependency).

    sudo apt-get install ruby-full build-essential zlib1g-dev
    sudo gem install jekyll bundler
    bundler config set path 'vendor/bundle'

Next up is updating dependencies:

    bundler update

Finally to start up a local server:

    bundler exec jekyll serve

Some sources

-   [Jekyll Quickstart](https://jekyllrb.com/docs/)

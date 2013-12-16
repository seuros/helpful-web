# [Helpful](http://helpful.io)
Support that makes you better at support.

[![Build Status](https://travis-ci.org/asm-helpful/helpful-web.png?branch=master)](https://travis-ci.org/asm-helpful/helpful-web)
[![Code Climate](https://codeclimate.com/github/support-foo/web.png)](https://codeclimate.com/github/support-foo/web)

Helpful is an open product that's being build by a fantastic group of people on [Assembly](https://assemblymade.com/support-foo). Anybody can join in building this product and earn a stake of the profit.

## Getting Started with Vagrant
[Vagrant](http://vagrantup.com) is a great way to quickly get started on Helpful.

If you don't already have Vagrant installed download it here: http://www.vagrantup.com/downloads before continuing.

Once you have Vagrant installed you can run:

		vagrant up
		
This will take a while to run so you may want to grab some coffee. Once Vagrant finishes follow the onscreen instructions to finish the install process. That's it.

Welcome to Helpful.

## Getting started
You need these installed locally to run Helpful:

* [Ruby 2.0.0](https://www.ruby-lang.org)
* [Postgres](http://www.postgresql.org)
* [Redis](http://redis.io)
* [ElasticSearch]()
* [Heroku Toolbelt](https://toolbelt.heroku.com)

    # Install dependent gems
    bundle install

    # Setup the database
    cp config/database.yml.example config/database.yml
    # edit config/database.
    rake db:setup

    # Install & Setup dependencies (for Mac)
    brew install elasticsearch
    cp -s /usr/local/Cellar/elasticsearch/X.XX.X /usr/local/Cellar/elasticsearch/latest
    brew install redis
    cp -s /usr/local/Cellar/redis/X.XX.X /usr/local/Cellar/elasticsearch/redis

    # Configure the environment
    cp .env.example .env
    # Edit the .env file to customize the options in there (the defaults are pretty sane if you followed this guide, but you should check)

### If you are running redis and elasticsearch already:

    $ foreman start

### If you are not running redis and elasticsearch seperately and would like to run them in the same session:

    $ cp Procfile.dev.example Procfile.dev
    # edit Procfile.dev

    # Start the server
    $ foreman start -f Procfile.dev
    # open localhost:5000 in your browser

### Send a test message to the app

Once you've created an account you can send it test messages using the API:

    curl -X POST http://helpful.io/api/messages \
             --data "account=helpful" \
             --data "email=user@example.com" \
             --data "content=I need help please."

### Configuring Search (Elastic Search)

On OS X:

    brew install elasticsearch
    elasticsearch -f
    rake search:reindex
    foreman start

### Configuring Analytics (Segment.io)

You can get some analytics from your app by configuring a [Segment.io](https://segment.io/) secret key in .env:

    SEGMENT_SECRET=XXXXXXXXXXXX

### Configuring Email

#### Sending with Gmail

In development.rb, add:

    config.action_mailer.delivery_method = :smtp
    config.action_mailer.smtp_settings = {
        address:              'smtp.gmail.com',
        port:                 587,
        domain:               'example.com',
        user_name:            'your_gmail_username',
        password:             'your_gmail_password',
        authentication:       'plain',
        enable_starttls_auto: true
    }

#### Recieving with Mailgun (optional)

Setting up Mailgun in development takes a little work but allows you to use the
actual email workflow used in production.

1. Register for a free account at https://mailgun.com.
2. Get your Mailgun API key from https://mailgun.com/cp it starts with "key-"
and add it to your .env file as MAILGUN_API_KEY.
4. Get your Mailgun test subdomain from the same page and add it to your .env
file as INCOMING_EMAIL_DOMAIN.
5. In order to recieve webhooks from Mailgun we need to expose our development
instance to the outside world. We can use a tool called
[Ngrok](http://ngrok.com) for this. Download and setup Ngrok by following the
instructions on the [Ngrok](http://ngrok.com) site.
6. Run rake mailgun to make sure everything is setup right. It should prompt you
to create a route using `rake mailgun:create_route`.
7. Run `rake mailgun:create_route` and when prompted enter your Ngrok address
as the domain name.
8. Send a test email to helpful@INCOMING_EMAIL_DOMAIN and you should see it
appear in the helpful account.

## Contributing

There are a couple of steps you need to take before contributing:

1. Go to https://assemblymade.com and sign up.
2. Link your GitHub account to your Assembly account
3. Create a new WIP at https://assemblymade.com/support-foo/wips. Think of WIPs as GitHub issues.

Then just go ahead, fork the repo & issue a pull request. You're on your way to having a stake in Helpful.

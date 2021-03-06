Getting Started with ruby 2.4 rails 5.1 with neo4j graphdb

#Run In Bash Terminal/CLI
rails new appname -m http://neo4jrb.io/neo4j/neo4j.rb -O
cd appname
rake neo4j:install[community-latest]
rake neo4j:start

#now that you have neo4j installed go to localhost:7474 in a browser and create a new user for the app. 
#the default credentials are formatted as user:password = neo4j:neo4j 
#to securely use neo4j you must set up your own. (use in application.rb with a private repo or as ENV variable)


###########################################################################################################
#application.rb -- replace NEO4jUsername and PASSWORD with the values you created in neo4j web admin:
###########################################################################################################
require_relative 'boot'

require "rails"
require "active_model/railtie"
require "active_job/railtie"
require 'neo4j/railtie'
require "action_controller/railtie"
require "action_mailer/railtie"
require "action_view/railtie"
require "action_cable/engine"
require "sprockets/railtie"
require "rails/test_unit/railtie"

Bundler.require(*Rails.groups)

module Appname
  class Application < Rails::Application
    
    config.generators do |g|
      g.orm             :neo4j
    end

    config.neo4j.session.type = :http
    config.neo4j.session.url = 'http://NEO4jUSERNAME:PASSWORD@127.0.0.1:7474'
    config.neo4j.session.options = {faraday_options: { ssl: { verify: true }}}
    
    config.load_defaults 5.1
  end
end
##########################################################################################################


#now it's back to your Terminal/CLI, run the following to create a user for your app:
rails generate scaffold User name:string email:string

#run the following to create additional app models/scaffolds. (rails g is short syntax for rails generate, rails s for rails server)
rails g scaffold (insert any additional model scaffold here)
rails neo4j:migrate RAILS_ENV=development
rails s
open localhost:3000

#create a git repository for your appname at github.com with your github_username
git init
git commit -m "first commit"
git remote add origin https://github.com/github_username/appname.git
git push -u origin master

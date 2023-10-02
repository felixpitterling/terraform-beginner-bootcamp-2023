# Week 2 - Custom Terraform Provider

- [Working with Ruby](#working-with-ruby)
- [Terratowns Mock Server](#terratowns-mock-server)

#### **[Journal Overview ←](./../README.md#weekly-journals)**

## Working with Ruby

### Bundler
- Bundler is a package manager for Ruby

### Install Gems
- You need to create a Gemfile and define your gems.

```rb
source "https://rubygems.org"

gem 'sinatra'
gem 'rake'
gem 'pry'
gem 'puma'
gem 'activerecord'
```

- Then run `bundle install` command
- This will install the gems on the system globally
  - Unlike NodeJS which installs in node_modules
- Gemfile.lock will be created to lock down the gem versions

### Executing ruby scripts in the context of bundler
- Use `bundle exec` to tell ruby scripts to use the gems already installed
- It sets the "context"

### Sinatra
- Sinatra is a micro web-framework for Ruby
- [Documentation](https://sinatrarb.com/)

## Terratowns Mock Server
- All of the code for the server is stored in the [server.rb](./../terratowns_mock_server/server.rb) file.

### Running the web server

```rb
bundle install
bundle exec ruby server.rb
```

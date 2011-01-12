require 'rubygems'
require 'bundler/setup'

Bundler.require(:default, :test)


require 'nesta/config'
require 'nesta/models'

namespace :heroku do
  desc "Set Heroku config vars from config.yml"
  task :config do
    Nesta::App.environment = ENV['RACK_ENV'] || 'production'
    settings = {}
    Nesta::Config.settings.map do |variable|
      value = Nesta::Config.send(variable)
      value && settings["NESTA_#{variable.upcase}"] = value
    end
    if Nesta::Config.author
      Nesta::Config.author_settings.map do |author_var|
        value = Nesta::Config.author[author_var]
        if value
          value && settings["NESTA_AUTHOR__#{author_var.upcase}"] = value
        end
    end
    params = settings.map { |k, v| %Q{#{k}="#{v}"} }.join(" ")
    system("heroku config:add #{params}")
  end
end

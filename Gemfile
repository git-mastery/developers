source 'https://rubygems.org'
# Hello! This is where you manage which Jekyll version is used to run.
# When you want to use a different version, change it below, save the
# file and run `bundle install`. Run Jekyll with `bundle exec`, like so:
#
#     bundle exec jekyll serve
#
# This will help ensure the proper Jekyll version is running.
# Happy Jekylling!
# gem 'jekyll', '~> 4.4.1'
gem 'github-pages', group: :jekyll_plugins
gem 'just-the-docs'
group :jekyll_plugins do
  gem 'jekyll-feed', '~> 0.12'
end

platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem 'tzinfo', '>= 1', '< 3'
  gem 'tzinfo-data'
end

gem 'wdm', '~> 0.1', platforms: %i[mingw x64_mingw mswin]

gem 'http_parser.rb', '~> 0.6.0', platforms: [:jruby]

gem 'webrick'

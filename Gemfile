# frozen_string_literal: true

source "https://rubygems.org"

gem "jekyll-theme-chirpy", "~> 5.2", ">= 5.2.1"

group :test do
  gem "html-proofer", "~> 3.18"
end

# Windows and JRuby does not include zoneinfo files, so bundle the tzinfo-data gem
# and associated library.
install_if -> { RUBY_PLATFORM =~ %r!mingw|mswin|java! } do
  gem "tzinfo", "~> 1.2"
  gem "tzinfo-data"
end

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.1.1", :install_if => Gem.win_platform?

# Jekyll <= 4.2.0 compatibility with Ruby 3.0
gem "webrick", "~> 1.7"

# https://github.com/jekyll/jekyll-compose
# help: bundle exec jekyll help
# Create new page: bundle exec jekyll page "My New Page"
# Create new post: bundle exec jekyll post "My New Post"
#  or specify a custom format for the date attribute in the yaml front matter
#  bundle exec jekyll post "My New Post" --timestamp-format "%Y-%m-%d %H:%M:%S %z"
gem 'jekyll-compose', group: [:jekyll_plugins]

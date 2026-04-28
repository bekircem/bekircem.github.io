source "https://rubygems.org"

gem "jekyll", "~> 4.3"

# Default theme — compatible with Jekyll 4
gem "minima", "~> 2.5.1"

# Required for Jekyll 4: kramdown 2.x needs this for GitHub Flavored Markdown
gem "kramdown-parser-gfm"

# Required for Jekyll 4 local server (WEBrick removed from Ruby stdlib in Ruby 3.0)
gem "webrick"

group :jekyll_plugins do
  gem "jekyll-feed"
  gem "jekyll-seo-tag"
  gem "jekyll-avatar"
  gem "jekyll-redirect-from"
end

# Windows does not include zoneinfo files, so bundle the tzinfo-data gem
gem "tzinfo-data", platforms: [:mingw, :mswin, :x64_mingw, :jruby]

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.1.0" if Gem.win_platform?

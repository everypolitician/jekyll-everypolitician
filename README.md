# Jekyll::Everypolitician [![Build Status](https://travis-ci.org/theyworkforyou/jekyll-everypolitician.svg?branch=master)](https://travis-ci.org/theyworkforyou/jekyll-everypolitician) [![Gem Version](https://badge.fury.io/rb/jekyll-everypolitician.svg)](https://badge.fury.io/rb/jekyll-everypolitician)

Use [EveryPolitician](http://everypolitician.org) data in your Jekyll generated site.

## Installation

Add this to your application's Gemfile:

```ruby
group :jekyll_plugins do
  gem 'jekyll-everypolitician'
end
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install jekyll-everypolitician

And then add the following to `_config.yml`:

```yaml
gems:
  - jekyll-everypolitician
```

## Usage

You need to configure Jekyll so it knows which data to use. In your `_config.yml` add the following to tell jekyll-everypolitician where to get the data from:

```yaml
everypolitician:
  sources:
    - https://github.com/everypolitician/everypolitician-data/raw/master/data/Australia/Representatives/ep-popolo-v1.0.json
```

This tells it to use the data for the Australian Representatives, and specifies which version of the data to use, in this case `master`.

When sources is an array, as in the example above, the collections that are created are `people`, `organizations`, `areas` and `events`.

Sometimes you'll want more than one source, for example you might want the upper house as well. For this case you can specify sources as a hash:

```yaml
everypolitician:
  sources:
    assembly: https://github.com/everypolitician/everypolitician-data/raw/master/data/Australia/Representatives/ep-popolo-v1.0.json
    senate: https://github.com/everypolitician/everypolitician-data/raw/master/data/Australia/Senate/ep-popolo-v1.0.json
```

When `sources` is a hash like this the collections that are created are prefixed with the key of the hash, e.g. `assembly_people`, `senate_people`, `assembly_areas` etc.

### Outputting collections

Once you've completed the steps above you will be able to access the collections on the `site` object in templates. So for example to show a list of people on a template you might use something like this:

```liquid
{% for person in site.assembly_people %}
  <p>{{ person.name }}</p>
{% endfor %}
```

If you want to create a separate page for each item in the collection then you'll need to add some extra bits to `_config.yml` to tell Jekyll to output your collection.

```yaml
collections:
  assembly_people:
    output: true
    permalink: '/assembly/person/:path/'
```

This tells Jekyll to output a file for each document in the collection. the `:path` part of the permalink will be replaced with the document's `id`.

It's important to remember that this plugin always creates collections from the EveryPolitician datasources. You can find more details about Jekyll's collections in the [official collections documentation](https://jekyllrb.com/docs/collections/).

### Layouts

This plugin will try and use layouts that are named after the generated collections. So in the examples above if you'd specified `sources` as an array then it would try to use `_layouts/people.html` as the layout for the `people` collection. Similarly if you specify `sources` as a hash then it would try to use something like `_layouts/assembly_people.html` for the `assembly_people` collection.

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake test` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/theyworkforyou/jekyll-everypolitician.


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

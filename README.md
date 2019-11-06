# Forwardable

The Forwardable module provides delegation of specified methods to a designated object, using the methods #def_delegator and #def_delegators.

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'forwardable'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install forwardable

## Usage

For example, say you have a class RecordCollection which contains an array <tt>@records</tt>.  You could provide the lookup method `#record_number()`, which simply calls #[] on the <tt>@records</tt> array, like this:

```
  require 'forwardable'

  class RecordCollection
    attr_accessor :records
    extend Forwardable
    def_delegator :@records, :[], :record_number
  end
```

We can use the lookup method like so:

```
  r = RecordCollection.new
  r.records = [4,5,6]
  r.record_number(0)  # => 4
```

Further, if you wish to provide the methods #size, #<<, and #map, all of which delegate to @records, this is how you can do it:

```
  class RecordCollection # re-open RecordCollection class
    def_delegators :@records, :size, :<<, :map
  end

  r = RecordCollection.new
  r.records = [1,2,3]
  r.record_number(0)   # => 1
  r.size               # => 3
  r << 4               # => [1, 2, 3, 4]
  r.map { |x| x * 2 }  # => [2, 4, 6, 8]
```

You can even extend regular objects with Forwardable.

```
  my_hash = Hash.new
  my_hash.extend Forwardable              # prepare object for delegation
  my_hash.def_delegator "STDOUT", "puts"  # add delegation for STDOUT.puts()
  my_hash.puts "Howdy!"
```

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake test` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/ruby/forwardable.

## License

The gem is available as open source under the terms of the [2-Clause BSD License](https://opensource.org/licenses/BSD-2-Clause).

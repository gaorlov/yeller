# Loudmouth

Simple in-process pub sub notifier for ruby. Everything runs inside the process with no queue. All the listeners get notified about events in real time with no repeats. 

## Installation

Add this line to your application's Gemfile:

```ruby
gem 'loudmouth'
```

And then execute:

    $ bundle

Or install it yourself as:

    $ gem install loudmouth

## Usage

There's 2 actions, as you would expect:

### Broadcast

To broadcast a message, just specify the message and the keys (they will be joined into one; this is for complex keys, so that you don't have to build them yourself)

```ruby
class MyLoudClass
  def yell!
    Loudmouth.broadcast "HELLO, EVERYONE!", "my", "key", "fragments"
  end
end
```

and it will publish `"HELLO, EVERYONE!"` to everyone who is subscribed to that combination of key fragments (order doesn't matter; the broadcaster will order them for you)

### Subscribe

The subscribers define what they listen to and how to respond. 

```ruby
class MyListenerClass
  include Loudmouth::Subscribable

  # the subscribable module provides this method
  subscribe :react, "fragments", "key", "my"

  def self.react( message )
    puts "received #{message}"
  end
end
```

which will print "received HELLO, EVERYONE!" when `MyLoudClass.yell!` is called.

You can also subscribe on the insance level with 

```ruby
Loudmouth.subscribe( MyListenerClass.new, :instance_level_react, "key", "fragments" )
```

## Development

After checking out the repo, run `bin/setup` to install dependencies. Then, run `rake test` to run the tests. You can also run `bin/console` for an interactive prompt that will allow you to experiment.

To install this gem onto your local machine, run `bundle exec rake install`. To release a new version, update the version number in `version.rb`, and then run `bundle exec rake release`, which will create a git tag for the version, push git commits and tags, and push the `.gem` file to [rubygems.org](https://rubygems.org).

## Contributing

Bug reports and pull requests are welcome on GitHub at https://github.com/gaorlov/loudmouth.


## License

The gem is available as open source under the terms of the [MIT License](http://opensource.org/licenses/MIT).

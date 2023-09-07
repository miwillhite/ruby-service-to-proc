# ruby-service-to-proc
PAAS (proc as a service)

```ruby
class Max
  def self.to_proc
    -> (acc, element) { new(acc, element).run }
  end

  def initialize(acc, element)
    @result = acc > element ? acc : element
  end

  def run
    @result
  end
end

# Find the max element
puts [1000, 100, 10].reduce(0, &Max)

class SumForKey
  def self.with(key)
    @@key = key
    self
  end

  def initialize(acc, item)
    @acc = acc
    @item = item
  end

  def run
    @acc + @item
  end

  def self.to_proc
    -> (acc, item) { new(acc, item[@@key].to_i).run }
  end
end

breakdown = [
  {
    Key: '1'
  },
  {
    Key: '1'
  },
  {
    OtherKey: '1'
  }
]

# I wonder if we can use an approach with current to make this nicer?
puts breakdown.inject(0, &SumForKey.with(:Key))
```

Source: https://replit.com/@miwillhite/prockit#main.rb

# My "each\_slice"

Need to make alternative version of `each_slice` meth in `Range`

## Smoke

```bash
git clone https://github.com/dmitryck/each_slice

cd each_slice

ruby each_slice.rb

# origin
# [1, 2, 3]
# [4, 5, 6]
# [7, 8, 9]
# [10]
# my
# [1, 2, 3]
# [4, 5, 6]
# [7, 8, 9]
# [10]
```

## Code

```ruby
class Range

  # via recursion
  def my_slice n, a=nil, x=nil, y=nil, i=0, c=nil, &block
    yield (a||=to_a)[(x||=0)..(y||=n-1)]
    my_slice(n, a, x+n, y+n, i, c, &block) if (i+=1)<=(c||=(count-1)/n)
  end

  # logic readable version (of what's going on in 'my_slice')
  def my_slice_logic n, a=nil, x=nil, y=nil, i=0, c=nil, &block
    a = a || to_a
    x = x || 0
    y = y || n-1

    sliced_array = a[x..y]

    yield sliced_array

    iterations_done = i + 1
    iterations_need = c || (count - 1)/n

    not_fully_sliced = (iterations_done <= iterations_need)

    if not_fully_sliced
      x_next = x + n
      y_next = y + n

      new_args = [n, a, x_next, y_next, iterations_done, iterations_need]

      send __method__, *new_args, &block
    end
  end
end

p 'origin'
(1..10).each_slice(3){|part| p part}
# [1,2,3]
# [4,5,6]
# [7,8,9]

p 'my'
(1..10).my_slice(3){|part| p part}
# [1,2,3]
# [4,5,6]
# [7,8,9]
# [10]
```

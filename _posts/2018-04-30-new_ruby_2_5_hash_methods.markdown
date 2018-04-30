---
layout: post
title:      "New Ruby 2.5 Hash Methods"
date:       2018-04-30 04:28:32 +0000
permalink:  new_ruby_2_5_hash_methods
---


Ruby continues to up its game in ease of use with a couple new hash methods introduced in Ruby 2.5. If it weren't enough that Ruby has methods like Hash#select available, here’s a couple new ones: Hash:#transform_keys and Hash#slice.

## Hash#transform_keys

Ruby already had Hash#transform_values and has released the complimentary method for keys. Manipulating a hash (or say an Object in JavaScript) is something that seems like it should always be easy, but sometimes takes more work than you think. With Hash#transform keys Ruby has given us another easy way to manipulate an existing hash. As usual, the bang operator, as in Hash#transform_keys! transforms the hash in place. Here's an example of converting a hash with strings as keys to integers.

```
h = {'1': 'bat', '2': 'cat', '3': 'hat', '4': 'rat', '5': 'sat'}
h_by_int = h.transform_keys { |k| k.to_s.to_i }
=> {1=>"bat", 2=>"cat", 3=>"hat", 4=>"rat", 5=>"sat"} 


h_by_int.key('hat') == 3
 => true 
```

## Hash#slice

And... now we can slice a hash! You simply pass the keys as symbols to the slice method called on a hash and you get back the hash sliced or 'filtered' with just those keys.

```
address = {
      street_address: '660 Palo Alto Ave',
      city: 'Palo Alto',
      state: 'CA',
      country:'U.S.',
      zip:'94301'
    }
		
		address.slice(:city, :state)
		=> {:city=>"Palo Alto", :state=>"CA"} 
```

And once again Ruby continues to make life easier on it's developers.!

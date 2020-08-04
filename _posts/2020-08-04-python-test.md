---
layout: post
title: python 練習題 dic list str
date: 2020-08-04
tags: python dic list str
---


```
def squares(start, end):
    return [i* i for i in range(start, end+1)]

print(squares(2, 3)) # Should be [4, 9]
print(squares(1, 5)) # Should be [1, 4, 9, 16, 25]
print(squares(0, 10)) # Should be [0, 1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

```
def groups_per_user(group_dictionary):
    user_groups = {}
    # Go through group_dictionary
    for group,users in group_dictionary.items():
        # Now go through the users in the group
        for user in users:
            # Now add the group to the the list of
            user_groups[user] = user_groups.get(user,[]) + [group]
# groups for this user, creating the entry
# in the dictionary if necessary

    return(user_groups)

print(groups_per_user({"local": ["admin", "userA"],
		"public":  ["admin", "userB"],
		"administrator": ["admin"] }))
```

```
def count_letters(text):
  result = {}
  # Go through each letter in the text
  for letter in text:
    # Check if the letter needs to be counted or not
    if letter.isalpha():
    # Add or increment the value in the dictionary
        result[letter.lower()] = int(result.get(letter.lower()) or 0) + 1
  return result

print(count_letters("AaBbCc"))
# Should be {'a': 2, 'b': 2, 'c': 2}

print(count_letters("Math is fun! 2+2=4"))
# Should be {'m': 1, 'a': 1, 't': 1, 'h': 1, 'i': 1, 's': 1, 'f': 1, 'u': 1, 'n': 1}

print(count_letters("This is a sentence."))
# Should be {'t': 2, 'h': 1, 'i': 2, 's': 3, 'a': 1, 'e': 3, 'n': 2, 'c': 1}
```

```
def format_address(address_string):
  # Declare variables
  aa =''
  # Separate the address string into parts
  a =  address_string.split(' ')
  # Traverse through the address parts
  for i in a:
    # Determine if the address part is the
    if i.isdigit():
    # house number or part of the street name
        n = i
  # Does anything else need to be done
    else:
  # before returning the result?
        aa = aa +' '+ i
  # Return the formatted string  
  return "house number {} on street named{}".format(n,aa)

print(format_address("123 Main Street"))
# Should print: "house number 123 on street named Main Street"

print(format_address("1001 1st Ave"))
# Should print: "house number 1001 on street named 1st Ave"

print(format_address("55 North Center Drive"))
# Should print "house number 55 on street named North Center Drive"

```

```
def combine_lists(list1, list2):
  # Generate a new list containing the elements of list2
  # Followed by the elements of list1 in reverse order
  list1.reverse()
  list2.extend(list1)
  return (list2)
  
	
Jamies_list = ["Alice", "Cindy", "Bobby", "Jan", "Peter"]
Drews_list = ["Mike", "Carol", "Greg", "Marcia"]

print(combine_lists(Jamies_list, Drews_list))
# Should print: ['Mike', 'Carol', 'Greg', 'Marcia', 'Peter', 'Jan', 'Bobby', 'Cindy', 'Alice']
```

```
def squares(start, end):
    return [i* i for i in range(start, end+1)]

print(squares(2, 3)) # Should be [4, 9]
print(squares(1, 5)) # Should be [1, 4, 9, 16, 25]
print(squares(0, 10)) # Should be [0, 1, 4, 9, 16, 25, 36, 49, 64, 81, 100]
```

```
def car_listing(car_prices):
  result = ""
  for k,v in car_prices.items():
    result += "{} costs {} dollars".format(k,v) + "\n"
  return result

print(car_listing({"Kia Soul":19000, "Lamborghini Diablo":55000, "Ford Fiesta":13000, "Toyota Prius":24000}))
'''
Kia Soul costs 19000 dollars
Lamborghini Diablo costs 55000 dollars
Ford Fiesta costs 13000 dollars
Toyota Prius costs 24000 dollars
'''
```

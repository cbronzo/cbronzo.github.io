---
layout: post
title:      "BOOleans...are they actually scary? (Procedural Ruby)"
date:       2018-07-01 23:29:02 -0400
permalink:  booleans_are_they_actually_scary_procedural_ruby
---


Procedural Ruby has been almost addicting to learn as it plays to the logical side of my brain, whilst reminding me of high school algebra (my favorite subject). Although fun to learn, I truly struggled graspings all the concepts, but now that I've moved on to Object Orientation -- boy does Procedural Ruby seem easy in comparison! I'm learning that coding is super difficult until you start learning an even more difficult concept, and then all of a sudden the past concepts seem so simple and they magically click. Hindsight is 20/20, right?

In Procedural Ruby, we learned about **Booleans**. They may sound scary (cheesy, I know), but I promise you they are not! Everything in ruby is either truthy or falsely, and booleans are true or false data types. The only type of data that is falsey is false and nil.  Everything else is truthy, including "false" and "nil" because these are now data strings.

A boolean statement can be extremely simple or complex, but all they do is evaluate to true or false. It reminds me of equations in math class, but much easier! So there's no reason to be scared of them!

To create boolean statements that return *true* or *false*, we can use the following boolean operators (and more):

**==** => Represents EQUAL TO
If the values are equal, it will return true.

**&&** => Represents AND
If BOTH values are true, it will return true. If one value is false and the other is true, it will return false.

**||** => Represents OR
Just one value needs to be true in order for it to return true.

**!** => Represents NOT
Since this represents NOT, the bang will do the opposite. For example "!=" this now means NOT EQUAL TO, and it will only return true if both values are *not* equal to each other.



Let's look at a few examples and see whether they evaluate to true or false:

`"The Cat Code" == "The Cat Code"`

*This will evaluate to **true** since these strings are equal to each other.*


`"UCLA" != "UCLA" `

*This will evaluate to false since "UCLA" does equal "UCLA" and the bang (!) in front of the equal sign indicates the opposite logic of NOT.*


`" " && ("California" != "New Hampshire")`

*This evaluates to **true** since the string " " is true AND the statement that California does NOT equal NH is also true. And since they are both true on either side of the operator, this statement is true.*


`false && "false"`

*This evaluates to **false** since false on the left evaluates to false, but "false" evaluates to true. Since they BOTH have to be true in order for this to return true, it will return false since only one side of the operator is true.*


`!nil`

*This is **true**. Nil is false, but the bang operator changes the logic, so instead of false, it becomes true.*


`55 > 10 || !("Catherine is going to be a great coder!" && [9,3,5,12])`

*This will evaluate to true. 55 is greater than 10 so this is true. The right side is false since the bang reverses the logic. However, we only need one side of the operator to be true to return true since this is an OR statement. *


![](https://media.giphy.com/media/atejfh29pJPtC/giphy.gif)


In addition to booleans, here's a (super) quick & basic summary about what I've learned in Procedural Ruby so far:

**- RegEx:**

Regular Expressions help you match or find certain characters using the following syntax:

`/your regex/`

If you are looking for the letter's "a", "b" or "c" you would write: `/[a-c]/`.


**- Variables & Methods**

A variable helps us define and store data/information. If a variable is defined, our computer is then able to refer back to it within the scope and see the value it is storing. 

`blog_name = "The Cat Code"`

A method defines something that our program will be able to do. A method has a method signature, a method body and a method closing.

```
def blog_greeting  #Method Signature

   puts "Welcome to my blog!"  #Method Body

end  #Method closing
```

**- Arrays**

An array is a list of values stored in the following way:

`array = [ ]`

If I were to write a list of  tech companies I could make an array (instead of making a new variable for each company):

`tech_company = ["Apple", "Google", "Microsoft"]`

If I want to access "Google" I would write:

`tech_company[1]`
(Index's start at 0 in programming)



**- Logic & Conditionals**

In life, we use conditionals every single day. For example, IF I am tired, I will take go to sleep. IF NOT, I will go workout. The keyword is "if" and we use conditionals in programming to help with control flow which will execute our code conditionally depending on our "if" "elsif" and "else" statements.

Here's an example:

```
today = "Saturday"

if today == "Monday"
  puts "Wake up at 7am."
elsif today == "Saturday"
  puts "Sleep in today."
else
  puts "Wake up by 8am. "
end
```

In this conditional statement, the program would output "Sleep in today." because it is Saturday. If today were set to Tuesday, it would say "Wake up by 8am.".

**- Looping**

Looping is just what it sounds like, it allows us to loop over a part of our code over and over again, without having to write a new line of code each time. 

For example, if I want to output "Flatiron Rocks!" 27 times - I could either write 

```
puts "Flatiron Rocks!" 
puts "Flatiron Rocks!" 
puts "Flatiron Rocks!" 
#(and so on 24 more times)
```

or I could make a loop:

```
27.times do
   puts "Flatiron Rocks"
end
```

Which one is easier, cleaner and less repetitive? The loop.


**- Iterations**

Iterations allow the program to go over data, such as an array and apply its code to each individual element.  I found this to be a harder concept as there are multiple ways of iterating and it is an abstract concept. 

Examples of iterations include: #while, #each, #until, #collect, #select & #find.

If I had the following array:

`tech_company = ["Apple", "Google", "Microsoft"]`

I could iterate over the companies by using #each. Take a look:

`tech_company.each {|company| puts "I love #{company}!`"

or

```
tech_company.each do |company|
   puts "puts "I love #{company}!"
end
```

This would output:

I love Apple!
I love Google!
I love Microsoft!



**- Hashes**

Like arrays, hashes are a collection of data with keys and values. Instead of just a list of values, a hash has a key that actually points to a value.

`hash = {"key" => "value", "key_two" => "value_two"}`

For example:

`july = {"season" => "summer", "weather" => "hot", "occasions => "my birthday"}`

As explained by Flatiron, hashes are like dictionaries and one unit of the hash contains the word and the definition.

We also learned about data structures, specifically, about nested hashes (which is essentially a hash inside of a hash). There can even be an array inside one of the nested hashes. It can start looking complicated fairly quickly, but by applying the logic learned, it all makes sense.


Looking back, I can't believe how much I struggled to learn these concepts, as they seem so rudimentary compared to the more advanced topics. I guess that's how coding works -- you're confused until you're not!









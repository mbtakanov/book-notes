==================================================================
VARIABLES
==================================================================
Global variables begin with $ - $global_variable = 10

Instance variables begin with @

Class variables begin with @@ and must be initialized before they can be used in method definitions.

class Customer
   @@no_of_customers = 0
   def initialize(id, name, addr)
      @cust_id = id
      @cust_name = name
      @cust_addr = addr
   end

   def display_details()
      puts "Customer id #@cust_id"
      puts "Customer name #@cust_name"
      puts "Customer address #@cust_addr"
   end

   def total_no_of_customers()
      @@no_of_customers += 1
      puts "Total number of customers: #@@no_of_customers"
   end

   def self.noc
    @@no_of_customers
   end
end


Local variables begin with a lowercase letter or _. The scope of a local variable ranges from class, module, def, or do to the corresponding end or from a block's opening brace to its close brace {}.

Constants begin with an uppercase letter - VAR1 = 100

==================================================================
LOOPS
==================================================================
if/unless condition
end


while/until condition [do]
end


times/each do [{] |n|
end [}]


returns copy of the array
collect/map do [{] |n|
end [}]

(filter) - returns copy of the array
select do [{] |n|
end [}]

case "condition"
  when "cat"
    puts "cat"
  when "dog"
    pust "dog"
  else
    puts "animal"
end


varialbe = nil
varialbe ||= 1 # varialbe is 1

"L".upto("P") { |l| puts l }


.respond_to? takes a symbol and returns true if an object can receive that method and false otherwise.
[1, 2, 3].respond_to?(:push)    # true
[1, 2, 3].respond_to?(:to_sym)  # false

4.next # 5

==================================================================
ARRAYS
==================================================================
fruits = ["orange", "apple", "banana", "pear", "grapes"]
fruits.sort! { |a, b| b <=> a }

array.sort!
array.sort! { |a, b| a <=> b }
1 <=> 2 # -1
1 <=> 1 # 0
2 <=> 1 # 1

array.reverse!

array.include? "string" # true / false
array.find { |item| item.include?("a") } # locates and returns the first element in the array that matches a condition

[1, 2, 3, 4] << 5 # [1, 2, 3, 4, 5]


array.select { |n| } # filter the elements
array.filter { |n| } # filter the elements

array.collect { |n| } # map the elements
array.map { |n| } # map the elements
==================================================================
HASH
==================================================================
foo = Hash.new / Hash.new(0) #each new key will have 0 as default value
foo['bar'] = 'xyz'
family = { "Homer" => "dad" }


hash.each { |k, v| }
hash.each do |k, v|; end

hash.select { |k, v| } # .map
hash.each_key { |k| }
hash.each_value { |v| }

==================================================================
SYMBOLS
==================================================================
While there can be multiple different strings that all have the same value, there’s only one copy of any particular symbol at a given time.

-----
puts "string".object_id #11964600
puts "string".object_id #11964340
puts :symbol.object_id  #802268
puts :symbol.object_id  #802268
-----
my_first_symbol = :alright
animals = { :foxes => 1, :rabbits => 2 }  # Old syntax
animals = { foxes: 1, rabbits: 2 }        # New syntax
animals = [:foxes, :rabbits]

"sasquatch".to_sym  # ==> :sasquatch
"sasquatch".intern  # ==> :sasquatch
:sasquatch.to_s # ==> "sasquatch"

==================================================================
YIELD
==================================================================
def yield_name(name)
  puts "In the method! Let's yield."
  yield("Kim")
  puts "In between the yields!"
  yield(name)
  puts "Block complete! Back in the method."
end

# yield_name("Eric") { |n| puts "My name is #{n}." }
==================================================================
PROC
==================================================================
Proc.new { puts "Hello!" }

You can think of a proc as a “saved” block: just like you can give a bit of code a name
and turn it into a method, you can name a block and turn it into a proc. 

cube = Proc.new { |x| x ** 3 }
[1, 2, 3].collect!(&cube)
# ==> [1, 8, 27]

Second way of calling procs
test = Proc.new { # does something }
test.call
# does that something!


The & is used to convert the cube proc into a block (since .collect! and .map! normally take a block).

SYMBOLS AND PROCS
==================================================================
["1", "2", "3"].map(&:to_i)

==================================================================
LAMBA
==================================================================
lambda { |param| block }
lambda { puts "Hello!" }
--------------------------
def lambda_demo(a_lambda)
  puts "I'm the method!"
  a_lambda.call
end

l = lambda { puts "I'm the lambda!" }
lambda_demo(l)
# => I'm the method!
# => I'm the lambda!


my_array = ["raindrops", :kettles, "whiskers", :mittens, :packages]
symbol_filter = lambda { |item| item.is_a? Symbol}
symbols = my_array.select(&symbol_filter)
puts symbols

==================================================================
Lambdas vs. Procs
==================================================================
1) A lambda checks the number of arguments passed to it, while a proc does not. This means that a lambda will throw an error if you pass it the wrong number of arguments, whereas a proc will ignore unexpected arguments and assign nil to any that are missing.

2) When a lambda returns, it passes control back to the calling method; when a proc returns, it does so immediately, without going back to the calling method.

==================================================================
kind_of?, is_a?
==================================================================
kind_of? and is_a? are synonymous.

"hello".is_a? Object 
"hello".kind_of? Object
# => true
Because "hello" is a String and String is a subclass of Object.

==================================================================
Inheritance
==================================================================
class SuperBadError < ApplicationError

==================================================================
SETers and GETers
==================================================================
class CapitalizedCamelCasePerson
  attr_reader :name
  attr_writer :name
  attr_reader :job
  attr_writer :job

  OR
  attr_accessor :name, :job
  
  def initialize(name, job)
    @name = name
    @job = job
  end
end

attr_accessor = attr_reader + attr_writer
==================================================================
Modules
==================================================================
module CapitalizedCamelCaseCircle

  PI = 3.141592653589793
  
  def Circle.area(radius)
    PI * radius**2
  end
  
  def Circle.circumference(radius)
    2 * PI * radius
  end
end

==================================================================
Modules, include, extend, mixins

module Bar
  def bar
    puts "OK!"
  end
end

class Foo
  include Bar #Can be used ONLY from the class instances
end

Foo.new.bar

class Xyz
  extend Bar #Can be used ONLY within the class
end


Xyz.bar

==================================================================
Miscellaneous
==================================================================
require 'Math' #at the top of the file
require 'Prime'
require 'Date'
require 'benchmark'

puts Math::PI

string.gsub!(/s/, "th")
string.downcase!
string.upcase!
string.include? "s"
for n in 1..50; end
10.times do |n|; end

(1..5).to_a.each do |n|; end
(1..5).to_a.each { |n| }

"string".to_i # to integer

"string" << " is a string" # "string is a string"

30.to_s #  "30"

puts "string" if 1 < 2
puts "string" unless 2 < 1
puts 1 < 2 ? "true" : "fals"

5**2 # 25


Ruby on Rails
==================================================================
rails generate controller Pages

rails generate model Message

bundle exec rake db:migrate - updates the database with the new messages data model.
bundle exec rake db:seed    - seeds the database with sample data from db/seeds.rb
bundle exec rake db:create  - create the database
bundle exec rake routes     - view all of the new URLs that the resource route created.


Controller Actions

<% @messages.each do |message| %> 
<div class="message"> 
  <p class="content"><%= message.content %></p> 
  <p class="time"><%= message.created_at %></p> 
</div> 
<% end %> 




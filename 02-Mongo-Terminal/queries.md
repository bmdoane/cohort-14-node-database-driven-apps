# MongoDB in the terminal

show dbs -- To show databases
use test -- To use the database listed as test
show collections -- Shows collections in a db

## Requirements

- instructions for how to install shared db

Import the sample dataset from mongoDB's [website](https://docs.mongodb.com/getting-started/shell/import-data/). This will import a database named "test" with a collection of "restaurants."

- instructions for how to record queries 

Create a markdown (.md) file where you can record the queries that return the requested information. You can then push the markdown file to github.

Provide the following:

1. Provide a query showing just the names (and nothing else) of the Italian restaurants.
test> db.restaurants.find({cuisine: "Italian"}, {"name": 1, "_id": false})


2. Provide a query showing how many Bakeries have a name that start with M.
test> db.restaurants.find({cuisine: "Bakery", "name": /^M/}).length()


3. Provide a query showing the zip codes (and _id's) of all restaurants with the word "Ice" in their cuisine.
test> db.restaurants.find({cuisine: { $regex: /^Ice/i }}, {"address.zipcode": 1})


4. Provide a query showing the street and street number of Chinese restaurants ordered by zip code.
test> db.restaurants.find({cuisine: "Chinese"}, {"address.building": 1, "address.street": 1, "address.zipcode": 1}).sort({"address.zipcode": 1})


5. Show only the American restaurants in Manhattan.
test> db.restaurants.find({cuisine: "American ", "borough": "Manhattan"})


6. Provide a query showing the restaurants that have been graded exactly 4 times.
test> db.restaurants.find({$where: "this.grades.length === 4"})


7. Provide a query showing only _id, name and 2 grades from each restaurant on Broadway.
test> db.restaurants.find({"address.street": "Broadway"}, {"name": 1, "grades": {$slice: 2}})


8. Provide a query showing the 5 pizza restaurants in the Bronx with the highest score on an evaluation.
test> db.restaurants.find({"borough": "Bronx"}, {"name": 1, "grades.score": 1}).sort({"grades.score": -1}).limit(5)


9. Provide a query to find all of the restaurants in Brooklyn and list only the 21st-30th results when ordered alphabetically by name.
test> db.restaurants.find({"borough": "Brooklyn", "name": {$ne: ""}}, {"name": 1}).sort({"name": 1}).skip(20).limit(9)


10. Provide a query that returns all pizza and Italian restaurants in reverse alphabetic order.
test> db.restaurants.find({cuisine: "Pizza/Italian"}, {"name": 1}).sort({"name": -1})

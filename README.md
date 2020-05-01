# eq-routing-validator-spike

A repositary for investigating solutions for introducting routing validation into `eq-author-app`.

## Solution 1 - Traversing the JSON schema

This solution involves taking the ID of the object which will be deleted and sifting through every routable object within the questionnaire to identify which ones have a rule that points to it. Essentially, a series of `for` loops and `if` statements (or their simplified loadash counterpart).

### Benefits

- No extra NodeJS packages required
- No models will need to be maintained as schema always reflects questionnaire state
- Can validate before or after an object is deleted

### Downsides

- Will need to re-traverse the schema for every validation rule

## Solution 2 - Directed Graph

> In mathematics, and more specifically in graph theory, a directed graph is a graph that is made up of a set of vertices connected by edges, where the edges have a direction associated with them. - Wikipedia

This solution involves building a directed graph which models all of the routes between routable objects, i.e a question page, confirmation page, etc. In this, each vertex on the graph would be an individual routable object and the edges the routes between these objects. The direction would be necessary to evaluate which question comes before or after another.

For example, consider the following questionnaire and routing rules:

1 - What is your name?

2 - How old are you?

3 - Do you have a driving license?

4 - Why do you not have a driving license?

5 - End of questionnaire

1 -> 2

if 2 < 16: 2 -> 5
else: 2 -> 3

if 3 == yes: 3 -> 5
else: 3 -> 4

4 -> 5

This could be modeled on a directed graph in the following way:

![graph](https://i.gyazo.com/eb78ace44fa71ff83b744da05238ee06.png)

Using this model, you would then be able to programatically traverse the graph or target a particular vertex, or question, and validate the in and out edges, or routing rules. For instance, ensuring that circular routing rules were not applied or identifying when rules lead to a question that has been deleted.

In the latter example, this could be implemented by targeting the vertex belonging to the question that has been deleted and identifying the source of all the edges pointing to it. This would flag the questions which have routing rules pointing to the deleted question, allowing the application to flag a validation warning on those pages.

### Benefits

- Flexible; multiple validation rules can be applied with ease
- NodeJS packages which build and facilitate traversing graphs exist
- Can validate before or after an object is deleted

### Downsides

- Will need to redraw the graph multiple times or maintain a copy in the db

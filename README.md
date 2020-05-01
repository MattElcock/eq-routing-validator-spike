# eq-routing-validator-spike

A repositary for investigating solutions for introducting routing validation into `eq-author-app`.

## Soltuion 1 - Directed Graph

> In mathematics, and more specifically in graph theory, a directed graph is a graph that is made up of a set of vertices connected by edges, where the edges have a direction associated with them. - Wikipedia

This solution involves building a directed graph which models all of the routes between routable objects, i.e a question page, confirmation page, etc. In this, each vertex on the graph would be an individual routable object and the edges the routes between these objects. The direction would be necessary to evaluate which question comes before or after another.

# The Ultimate Redux Course
___

[Introduction to RTK Query basics](https://egghead.io/courses/rtk-query-basics-query-endpoints-data-flow-and-typescript-57ea3c43?af=7pnhj6) as taught by Lenz Weber-Tronic, who invented it.

First step is to create an api with a base query and endpoints
- base query is a function that takes a url and returns a promise
  - when you use the build.query and build.mutation functions, they return a function that takes an object with a url and other options
  - if you call your build.query something like pokemonList, it creates a hook internally called usePokemonListQuery. This comes "for free" with the build.query function
  - the hook takes an object with a url and other options
- the hook (usePokemonListQuery) returns an object with data, error, isLoading, and other properties which you can deconstruct:
  - const { data, error, isLoading } = usePokemonListQuery();
- the data is the response from the server, error is any error that occurred, and isLoading is a boolean that indicates if the request is still loading

Next step is to get the data from the server or api request
- the data is returned as an array of objects, each object representing a pokemon

Pass query arguments to RTK query hooks: 
- this allows the request to be dynamic and take a parameter (like a user id)
- within the build.query, there's the result = await fetch(api)
- if it looks like await fetch(access-api/controller/{userId}), then you would pass it into the query like this:
  - const { data, error, isLoading } = usePokemonListQuery({ userId });

Base query:
- the base query is a function that takes a url and returns a promise
- a base query can combine multiple endpoints and makes it so that we don't have code duplication
- 
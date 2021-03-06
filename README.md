# GraphQL.js

This is a technical preview of the JavaScript reference implementation for
GraphQL, a query language created by Facebook for describing data requirements
on complex application data models.

[![Build Status](https://travis-ci.org/graphql/graphql-js.svg)](https://travis-ci.org/graphql/graphql-js)
[![Coverage Status](https://coveralls.io/repos/graphql/graphql-js/badge.svg?branch=master)](https://coveralls.io/r/graphql/graphql-js?branch=master)
[![Public Slack Discussion](https://graphql-slack.herokuapp.com/badge.svg)](https://graphql-slack.herokuapp.com/)

## Technical Preview Contents

This technical preview contains a [draft specification for GraphQL]
(https://github.com/facebook/graphql) and a reference implementation in
JavaScript that implements that draft, GraphQL.js.

The reference implementation provides base libraries in JavaScript that would
provide the basis for full GraphQL implementations and tools. It is not a fully
standalone GraphQL server that a client developer could use to start
manipulating and querying data. Most importantly, it provides no mapping to a
functioning, production-ready backend. The only “backend” we have targeted for
this early preview are in-memory stubs in test cases.

We are releasing this now because after GraphQL was first discussed publicly,
many engineers used this information to implement the parts of the system that
we discussed publicly. We want to support those engineers by providing both a
formal specification and a reference implementation for the system as a whole.

To that end, the target audience is not the client developer, but those who have
built or are actively interested in building their own GraphQL implementations and
tools. Critically, we also want feedback on the system and to incorporate that
feedback in our final release.

In order to be broadly adopted, GraphQL will have to target a wide
variety of backends, frameworks, and languages, which will necessitate a
collaborative effort across projects and organizations. This technical preview
marks the beginning of that process.

## Getting Started

An overview of GraphQL in general is available in the
[README](https://github.com/facebook/graphql/blob/master/README.md) for the
[Specification for GraphQL](https://github.com/facebook/graphql). That overview
describes a simple set of GraphQL examples that exist as [tests](src/__tests__)
in this repository. A good way to get started with this repository is to walk
through that README and the corresponding tests in parallel.

### Using GraphQL.js

Install GraphQL.js from npm

```sh
npm install --save graphql
```

GraphQL.js provides two important capabilities: building a type schema, and
serving queries against that type schema.

First, build a GraphQL type schema which maps to your code base.

```js
import {
  graphql,
  GraphQLSchema,
  GraphQLObjectType,
  GraphQLString
} from 'graphql';

var schema = new GraphQLSchema({
  query: new GraphQLObjectType({
    name: 'RootQueryType',
    fields: {
      hello: {
        type: GraphQLString,
        resolve() {
          return 'world';
        }
      }
    }
  })
});
```

This defines a simple schema with one type and one field, that resolves
to a fixed value. A more complex example is included in the top level
[tests](src/__tests__) directory.

Then, serve the result of a query against that type schema.

```js
var query = '{ hello }';

graphql(schema, query).then(result => {

  // Prints
  // {
  //   data: { hello: "world" }
  // }
  console.log(result);

});
```

This runs a query fetching the one field defined. The `graphql` function will
first ensure the query is syntactically and semantically valid before executing
it, reporting errors otherwise.

```js
var query = '{ boyhowdy }';

graphql(schema, query).then(result => {

  // Prints
  // {
  //   errors: [
  //     { message: 'Cannot query field boyhowdy on RootQueryType',
  //       locations: [ { line: 1, column: 3 } ] }
  //   ]
  // }
  console.log(result);

});
```

### Contributing

We actively welcome pull requests, learn how to
[contribute](https://github.com/graphql/graphql-js/blob/master/CONTRIBUTING.md).

### Changelog

Changes are tracked as [Github releases](https://github.com/graphql/graphql-js/releases).

### License

GraphQL is [BSD-licensed](https://github.com/graphql/graphql-js/blob/master/LICENSE).
We also provide an additional [patent grant](https://github.com/graphql/graphql-js/blob/master/PATENTS).

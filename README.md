# Parse + React

Seamlessly bringing Parse data into your React applications.

[![Build Status](https://travis-ci.org/ParsePlatform/ParseReact.svg?branch=master)](https://travis-ci.org/ParsePlatform/ParseReact) [![npm version](https://badge.fury.io/js/parse-react.svg)](http://badge.fury.io/js/parse-react)

## Overview

Parse + React is an interface layer on top of the
[Parse JS SDK](https://parse.com/docs/js_guide) that provides simple access to
the Parse API from [React](http://facebook.github.io/react/). It lets React
components subscribe to Parse queries, and allows data mutations to be
dispatched in a Flux-style manner. In the background, these subscriptions are
managed in a way that lets these components automatically update as objects are
created and modified, allowing user interfaces to be snappy and responsive.

### [Full API](/docs/api/)

## Example

To add Parse data to a component, it simply needs to subscribe to a standard
Parse Query. This is done through an implementation of the [newly-proposed
`observe()` API](https://github.com/facebook/react/issues/3398) for React. The
ParseReact Mixin allows a version of this new lifecycle method to be used today
with Parse Queries.

If you're using React with ES6 classes, we also provide a [subclass of
`React.Component`](/docs/api/ES6.md) that allows you to use the
`observe()` and Query-specific APIs.

```js
var CommentBlock = React.createClass({
  mixins: [ParseReact.Mixin], // Enable query subscriptions

  observe: function() {
    // Subscribe to all Comment objects, ordered by creation date
    // The results will be available at this.data.comments
    return {
      comments: (new Parse.Query('Comment')).ascending('createdAt')
    };
  },

  render: function() {
    // Render the text of each comment as a list item
    return (
      <ul>
        {this.data.comments.map(function(c) {
          return <li>{c.text}</li>;
        })}
      </ul>
    );
  }
});
```

Whenever this component mounts, it will issue the query and the results will be
attached to `this.data.comments`. Each time the query is re-issued, or objects
are modified locally that match the query, it will update itself to reflect
these changes.

Mutations are dispatched in the manner of
[Flux](http://facebook.github.io/flux/) Actions, allowing updates to be
synchronized between many different components without requiring views to talk
to each other. All of the standard Parse data mutations are supported, and you
can read more about them in the [Data Mutation](/docs/DataMutations.md) guide.

```js
// Create a new Comment object with some initial data
ParseReact.Mutation.Create('Comment', {
  text: 'Parse <3 React'
}).dispatch();
```

## Getting Started

Parse + React  is available from [our CDN](https://www.parsecdn.com/js/parse-react.js)
([minified](https://www.parsecdn.com/js/parse-react.min.js)), and 
[npm](https://www.npmjs.com/package/parse-react).

If you're not familiar with React, we recommend you first walk through their
[tutorials](http://facebook.github.io/react/docs/tutorial.html) before adding
Parse data to your React applications.

If you're using a tool like Webpack or Browserify to enable Common JS `require`
statements, you need to make sure you also include the `'parse'` npm package
in your dependencies.

```js
var React = require('react');
var Parse = require('parse');
var ParseReact = require('parse-react-next');

Now that you've included all of the necessary libraries, you're ready to start
[subscribing to Parse data](/docs/Subscriptions.md) and
[mutating it](/docs/DataMutations.md).

## Contributing

See the CONTRIBUTING file for information on how to help out.

## License

Copyright (c) 2015, Parse, LLC. All rights reserved.

You are hereby granted a non-exclusive, worldwide, royalty-free license to use, copy, modify, and distribute this software in source code or binary form for use in connection with the web services and APIs provided by Parse.

As with any software that integrates with the Parse platform, your use of this software is subject to the Parse Terms of Service [https://www.parse.com/about/terms]. This copyright notice shall be included in all copies or substantial portions of the software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

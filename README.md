# rss-parser

This is a fork from https://github.com/bobby-brennan/rss-parser

But it has the following modifications:
* It uses the request package to allow for better handling of 301 redirects
* It has an alternative parse function call that allows an options object to be passed.
* Categories will by simplified from objects: {_: "myCategory", $: [myCategoryAttributes]} to "myCategory"

## Installation

### NodeJS
```bash
npm install --save rss-parser
```

### Web
```bash
bower install --save rss-parser
```

## Usage
rss-parser exposes `parseURL()`, `parseString()`, and `parseFile()` functions.

You can either call these functions with or without an options object.

Currently the options object only supports one option:
* includeAllFields [boolean]    This will copy over all the fields from RSS and puts it into the json object. Without this options only: title, link, pubDate, author, content:encoded and enclosure are copied over.

example:

With:
```js
var parser = require('rss-parser');

parser.parseURL('https://www.reddit.com/.rss', {includeAllFields: true}, function(err, parsed) {
  console.log(parsed.feed.title);
  parsed.feed.entries.forEach(function(entry) {
    console.log(entry.title + ':' + entry.link);
  })
})
```

Without:
```js
var parser = require('rss-parser');

parser.parseURL('https://www.reddit.com/.rss', function(err, parsed) {
  console.log(parsed.feed.title);
  parsed.feed.entries.forEach(function(entry) {
    console.log(entry.title + ':' + entry.link);
  })
})
```

Check out the output format in [test/output/reddit.json](test/output/reddit.json)

### NodeJS
```js
var parser = require('rss-parser');

parser.parseURL('https://www.reddit.com/.rss', function(err, parsed) {
  console.log(parsed.feed.title);
  parsed.feed.entries.forEach(function(entry) {
    console.log(entry.title + ':' + entry.link);
  })
})
```

### Web
```html
<script src="/bower_components/rss-parser/dist/rss-parser.js"></script>
<script>
RSSParser.parseURL('https://www.reddit.com/.rss', function(err, parsed) {
  console.log(parsed.feed.title);
  parsed.feed.entries.forEach(function(entry) {
    console.log(entry.title + ':' + entry.link);
  })
})
</script>
```

## Contributing

### Running Tests
The tests run the RSS parser for several sample RSS feeds in `test/input` and outputs the resulting JSON into `test/output`. If there are any changes to the output files the tests will fail.

To check if your changes affect the output of any test cases, run

`npm test`

To update the output files with your changes, run

`WRITE_GOLDEN=true npm test`

### Publishing Releases
```bash
grunt browserify
git commit -a -m "browserify"
npm version minor # or major/patch
npm publish
git push --follow-tags
```


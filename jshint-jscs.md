#Code Checkers: jsHint, jscs, scssLint, html5lint



##jsHint Config

- Check oficial [jsHint github page](https://github.com/jshint/jshint)
- Check [documentation page](http://jshint.com/docs/)
- Check [options page](http://jshint.com/docs/options/)
- Check [API page](http://jshint.com/docs/api/)

.jshintrc
```javascript
{
  "lastsemic": true,  
  "boss":      true,  
  "browser":   true,  
  "devel":     true,  
  "jquery":    true,  
  "globals": {        
    "angular": true,
    "Modernizr": true
  },
  "unused":    true,  
  "nonbsp":    true,  
  "curly":     true,  
  "strict":    true,  
  "forin":     true,  
  "newcap":    false, 
  "eqeqeq":    true,  
  "undef":     true,  
  "freeze":    true,  
  "maxdepth":  3,     
  "maxstatements": 15,
  "maxcomplexity": 6 
}
```

##jscs Config

- Check oficial [jscs web site](http://jscs.info/)
- Check [rules page](http://jscs.info/rules)

.jscsrc
```javascript
{
  "disallowEmptyBlocks": true,
  "disallowKeywords": [
    "with"
  ],
  "disallowKeywordsOnNewLine": [
    "else"
  ],
  "disallowMixedSpacesAndTabs": true,
  "disallowMultipleLineBreaks": true,
  "disallowMultipleLineStrings": true,
  "disallowMultipleSpaces": {
    "allowEOLComments": true
  },
  "disallowNewlineBeforeBlockStatements": true,
  "disallowOperatorBeforeLineBreak": true,
  "disallowQuotedKeysInObjects": true,
  "disallowSpaceAfterObjectKeys": {
    "allExcept": ["aligned"]
  },
  "disallowSpaceAfterPrefixUnaryOperators": true,
  "disallowSpaceBeforeBinaryOperators": [
    ","
  ],
  "disallowSpaceBeforeComma": true,
  "disallowSpaceBeforePostfixUnaryOperators": true,
  "disallowSpaceBeforeSemicolon": true,
  "disallowSpacesInAnonymousFunctionExpression": {
    "beforeOpeningRoundBrace": true
  },
  "disallowSpacesInCallExpression": true,
  "disallowSpacesInFunctionDeclaration": {
    "beforeOpeningRoundBrace": true
  },
  "disallowSpacesInFunctionExpression": {
    "beforeOpeningRoundBrace": true
  },
  "disallowSpacesInNamedFunctionExpression": {
    "beforeOpeningRoundBrace": true
  },
  "disallowTrailingWhitespace": true,
  "disallowYodaConditions": true,
  "requireBlocksOnNewline": 1,
  "requireCamelCaseOrUpperCaseIdentifiers": "ignoreProperties",
  "requireCapitalizedConstructors": true,
  "requireCommaBeforeLineBreak": true,
  "requireDotNotation": true,
  "requireKeywordsOnNewLine": [
    "if",
    "for",
    "while",
    "switch",
    "case",
    "return",
    "try"
  ],
  "requireLineFeedAtFileEnd": true,
  "requireMultipleVarDecl": true,
  "requireOperatorBeforeLineBreak": true,
  "requirePaddingNewLinesAfterBlocks": true,
  "requirePaddingNewLinesBeforeLineComments": {
    "allExcept": "firstAfterCurly"
  },
  "requireParenthesesAroundIIFE": true,
  "requireSemicolons": true,
  "requireSpaceAfterBinaryOperators": true,
  "requireSpaceAfterKeywords": [
    "if",
    "else",
    "for",
    "while",
    "do",
    "switch",
    "case",
    "return",
    "try",
    "catch",
    "typeof"
  ],
  "requireSpaceBeforeBinaryOperators": true,
  "requireSpaceBeforeBlockStatements": true,
  "requireSpaceBetweenArguments": true,
  "requireSpacesInConditionalExpression": true,
  "requireSpacesInForStatement": true,
  "requireSpacesInFunctionDeclaration": {
    "beforeOpeningCurlyBrace": true
  },
  "requireSpacesInFunctionExpression": {
    "beforeOpeningCurlyBrace": true
  },
  "requireSpacesInFunction": {
    "beforeOpeningCurlyBrace": true
  },
  "requireSpacesInNamedFunctionExpression": {
    "beforeOpeningCurlyBrace": true
  },
  "requireSpacesInsideArrayBrackets": "all",
  "requireSpacesInsideBrackets": true,
  "requireSpacesInsideParentheses": "all",
  "requireTrailingComma": {
    "ignoreSingleLine": true
  },
  "requireVarDeclFirst": true,
  "safeContextKeyword": "_this",
  "validateIndentation": 2,
  "validateLineBreaks": "LF",
  "validateQuoteMarks": "'"
}
```

##Html5 Lint Config
Basically the only thing needed is installing the gulp plugin.  No config file is required though there are options you can check in the html5-lint mozilla repository: [https://github.com/mozilla/html5-lint](https://github.com/mozilla/html5-lint)

##How do we configure the jsHint, jscs and html5 reports with Gulp?

We first must install the npm plugins:
```javascript
npm install gulp-jshint gulp-jscs gulp-jscs-stylish gulp-html5-lint --save-dev
```
Then we import the packages to our gulpfile.js
```javascript
var html5Lint = require('gulp-html5-lint');
var jshint = require('gulp-jshint');
var jscs = require('gulp-jscs');
var stylish = require('gulp-jscs-stylish');
```
Then, in the scripts task we do:
```javascript

// Make sure you edit your scripts task like this
gulp.task('scripts', function () {
  gulp
    .src(['source/_scripts/app/**/*.js'])
    .pipe(maps.init())
    .pipe(jshint())
    .pipe(jscs({
      configPath: '.jscsrc'
    }))
    .on('error', noop)
    .pipe(stylish.combineWithHintResults())
    .pipe(jshint.reporter('jshint-stylish'))
    .pipe(concat('app.js'))
    .pipe(maps.write())
    .pipe(gulp.dest('source/scripts/'))
    .pipe(gulp.dest('_site/scripts/'))
    .pipe(browserSync.reload({stream: true}));
});

// Create this new task
gulp.task('html5-lint', function() {
    return gulp.src('./_site/*.html')
        .pipe(html5Lint());
});

// Add the html5 task to the watcher list
gulp.task('watch', function () {
  gulp.watch('source/_sass/**/*', ['sass']);
  gulp.watch('source/_scripts/app/**/*.js', ['scripts']);
  gulp.watch(['source/*.html', 'source/_layouts/**/*', 'source/_includes/**/*', 'source/assets/**/*'], ['jekyll-rebuild']);
  gulp.watch('_site/*.html', ['html5-lint'])
});
```

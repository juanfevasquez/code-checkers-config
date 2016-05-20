#jsHint Config

Check oficial [jsHint github page](https://github.com/jshint/jshint)
Check [documentation page](http://jshint.com/docs/)
Check [options page](http://jshint.com/docs/options/)
Check [API page](http://jshint.com/docs/api/)

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

#jscs Config

Check oficial [jscs web site](http://jscs.info/)
Check [rules page](http://jscs.info/rules)

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

##How do we configure the jsHint and jscs reports with Gulp?

We first must install the npm plugins:
```javascript
npm install gulp-jshint gulp-jscs gulp-jscs-stylish --save-dev
```
Then we import the packages to our gulpfile.js
```javascript
var jshint = require("gulp-jshint");
var jscs = require('gulp-jscs');
var stylish = require("gulp-jscs-stylish");
```
Then, in the scripts task we do:
```javascript
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
```
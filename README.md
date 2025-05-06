# JavaScript Coding Style Guide

## A. Parens, Braces, Linebreaks

Use spaces, braces, and line breaks for readability.

```js
// Bad
if(condition) doSomething();
while(condition) iterating++;
for(var i=0;i<100;i++) someIterativeFn();

// Good
if ( condition ) {
  // statements
}

while ( condition ) {
  // statements
}

for ( var i = 0; i < 100; i++ ) {
  // statements
}

var i, length = 100;
for ( i = 0; i < length; i++ ) {
  // statements
}

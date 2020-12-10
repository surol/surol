### Take a look at my projects

#### [Hatsy]
###### Asynchronous TypeScript-friendly HTTP server

```typescript
import { httpListener, Rendering } from '@hatsy/hatsy';
import { dispatchByPattern, Routing } from '@hatsy/router';
import { createServer } from 'http';

const server = createServer(httpListener(
  Routing
    .and(Rendering)
    .for(
      dispatchByPattern({
        on: '**',
        to({ renderJson }) {
          renderJson({ hello: 'world' });
        },
      }),
    ),
));

server.listen(3000);
```

[Hatsy]: https://github.com/hatsyjs/hatsy


#### [run-z]
###### Run that, then this
###### package.json scripts and deps runner

```shell
# First, run `clean` task.
# After that run `build`, `lint`, and `test` tasks simultaneously.
# Then publish the package.
npx run-z clean build,lint,test --then npm publish
```

[run-z]: https://github.com/run-z/run-z


#### [Wesib]
###### (_In development_) Web components building blocks

An UI framework based on web components.

Features IoC container, loadable features, routing, CSS-in-TS, form validation and processing, etc.

Defines custom elements by decorating a component classes:
```typescript
import { Attribute, Component, ComponentContext, Render } from '@wesib/wesib';

@Component('greet-text') // Custom element name
export class MyComponent {
  
  @Attribute() // Custom attribute
  name: string;

  constructor(private readonly _context: ComponentContext /* IoC context */) {
  }

  @Render() // Render when component state changes
  render() {
    this._context.contentRoot.innerText = `Hello, ${this.name}!`;
  }

}
```
No need to extend `HTMLElement` or any other class. Instead, Wesib creates a custom element accordingly to its
definition built either programmatically or using component decorators.

See [RealWorld application] implemented with [Wesib].

[Wesib]: https://github.com/wesib/wesib
[RealWorld application]: https://github.com/wesib/realworld-app


#### [fun-events]
###### Functional event processor

Handle events in reactive style. Think RxJS specialized on events rather generalized for data streams processing.

```typescript
import { nextArgs, nextSkip } from '@proc7ts/call-thru';
import { EventEmitter, OnEvent } from '@proc7ts/fun-events';
import { thruOn } from '@proc7ts/fun-events/call-thru';
import { Supply } from '@proc7ts/primitives';

// API supports arbitrary event receiver signatures
// An event is its receiver parameters
function printMessage(sender: string, message: string): void { 
  console.info(`Message from ${sender}: ${message}`);
}

const messageEmitter = new EventEmitter<[user: { name: string, email: string }, message: string, urgent?: boolean]>();
const onUrgentMessage: OnEvent<[string, string]> = messageEmitter.on.do(
  thruOn(
    ({ name, email }, message, urgent = false) => urgent
      ? nextArgs(`${name} <${email}>`, message)
      : nextSkip() // Filter out non-urgent messages
  ),
);

// Call the `OnEvent` function to start receiving events
const supply: Supply = onUrgentMessage(printMessage);

// Emit some events
messageEmitter.send({ name: 'Tester', email: 'tester@example.com' }, 'Hello', true);
// Prints: "Message from Tester <tester@example.com>: Hello"
messageEmitter.send({ name: 'Tester', email: 'tester@example.com' }, 'Not so urgent');
// Prints nothing

// Cutting off message supply
supply.off();

// Messages won't be received any more
messageEmitter.send({ name: 'Tester', email: 'tester@example.com' }, 'Too late', true);
// Prints nothing
```

[fun-events]: https://github.com/proc7ts/fun-events


#### Other Tools And Libraries

- [context-values] - IoC context values provider
- [input-aspects] - Framework-agnostic library controlling various aspects of user input, such as form validation
- [log-z] - Logging library for browsers and Node.js
- [optionz] - Advanced command line options parser
- [push-iterator] - Push iteration protocol. A faster, two-way compatible, alernative to [JavaScript iteration protocol]
- [rollup-plugin-flat-dts] - `.d.ts` files flattener and Rollup plugin
- [style-producer] - Produces and dynamically updates stylesheets (CSS-in-TS)

[context-values]: https://github.com/proc7ts/context-values
[input-aspects]: https://github.com/frontmeans/input-aspects
[log-z]: https://github.com/run-z/log-z
[optionz]: https://github.com/run-z/optionz
[push-iterator]: https://github.com/proc7ts/push-iterator
[rollup-plugin-flat-dts]: https://github.com/run-z/rollup-plugin-flat-dts
[style-producer]: https://github.com/frontmeans/style-producer

[JavaScript iteration protocol]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Iteration_protocols

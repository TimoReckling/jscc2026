# Code Contracts

## Code contracts examples:

- OpenAPI specs
- Class interfaces
- ...

## types in Typescript give us checks at build time and even earlier

boils down to static code analysis (SCA)
linters are another example

## using TypeScript for code contracts

idea: tie things together using types to gain early feedback even with IDE support 

showcased example: AWS Lambda function definition in CDK with a custom mini-DSL for defining lambda functions with certain constraints and checks at build time

```typescript
const myFunction = new lambda.Function(
  this, 
  "my function ID",
  {
    runtime: NodeJS,
    handler: getHandlerFromServiceNodejsBasePath`lambdas/path/${myFunctionLambdaClass}`, // tagged template literal syntax slightly abused here
    memorySize: 512,
    enviroment: {
      API_KEY: getApiKeyValue()  // <-- a typo in the key would break only at runtime 
    } 
  } satisfies ContstrainedByLambdaFunction <typeof myFunctionLambdaClass >
)


interface ContstrainedByLambdaFunction extends {
    ....
}
```

```typescript
export class myFunctionLambdaClass extends  // class 
    both (   // <-- self written function for mixin function AND class
    withHandlerAndResult<{body?: someMessage}>(),  // use AI to do the heavy lifting of the implementaiton of these
    withFilePath(),
){
    ....
};
```

→ mini-DSL for defining lambda functions with certain constraints and checks at build time

##  tagged template literals, tagged types, branded types

makes the function be invoked with a certain signature
useful for ...
useful for parametrized tests

branded types ~= tagged types 
```typescript
type brandedType = Tagged<string, 'TheBrand'>
```

hint: look at type-fest to look these kinds of things up (like `Tagged`)

example: 
```typescript
import type {ExclusifyUnion} from 'type-fest';
```
to make sets of function parameters mutually exclusive

## to make it more typesafe you can even check they type of the handler function 
import the handler in the deployment and check that it has the right type signature for this kind of lambda handler
---
sidebar_position: 1
---

# Blac + Jest + React

> With Jest and Enzyme, we can test React components.

## Setup
In your setup file for "setupFilesAfterEnv", add the following:
```ts title="setupTests.ts"
import state from "../src/state";

// this enables a method we need to mock the state easily.
state.mocksEnabled = true;

afterEach(() => {
  // reset all mocks after each test 
  state.resetMocks();
});
```
It might work in "setupFiles" but I didn't test that yet.

Now in your tests you can use `state.addBlocMock` add add a Bloc to the state which will be reset after each test.

## Usage

The Component
```tsx title="Example.tsx"
export default function Example() {
  const [{something}] = useBloc(ExampleBloc);
  
  return <div>
    {something && <NiceStuff />}
  </div>;
}
```

And the test for it

```tsx title="Example.test.tsx"
import state from "src/state";

describe("Example", () => {
  it("should render NiceStuff if something is set", () => {
    const exampleBloc = new ExampleBloc();
    // this adds the bloc to the global state, it will be removed after each test. 
    state.addBlocMock(exampleBloc);

    // access the 
    exampleBloc.setSomething(true);

    // the component now has access to the mocked state.
    const component = shallow(<Example />);
    expect(component.find(NiceStuff).exists()).toBe(true);
  });
});
```
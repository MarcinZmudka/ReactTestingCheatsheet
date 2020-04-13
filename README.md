# React Testing Cheatsheet

If you will notice a typo or bug make a PR or let me know.

## Instalation

When you use _create-react-app_ jest and react-testing-libraty should be installed. Unless:

```bash
npm install --save-dev @testing-library/react
```

## Import

Usefull things to import:

```javascript
import { render, fireEvent, wait } from "@testing-library/react";
import "@testing-library/jest-dom/extend-expect";
```

Render gives you back many interesting functions to lookup inside components: For more click [here!](https://testing-library.com/docs/dom-testing-library/cheatsheet)

FireEvent click's on compotents or activates it in any other way like:

```javascript
fireEvent.change(name, { target: { value: "name", name: "name" } });
fireEvent.click(button);
```

## Testing

First, you should create a test case. Inside test render component you will be testing:

```javascript
describe("test input email", () => {
	it("checking email input without @", async () => {
		const { getByText, getByLabelText } = render(<FormOfInputs></FormOfInputs>);
		const name = getByLabelText("name");
		const surname = getByLabelText("surname");
		const email = getByLabelText("email");
		/*  rest of code*/
	});
});
```

## Expect

Have a lot of ways to validate output of function or whatever you want to check.

```javascript
expect(getByText("Please write your Name")).toBeInTheDocument();
expect(onChange).toHaveBeenCalledTimes(1);
expect(contentInput.value).toBe("name");
```

## Mocks

You don't want to ask your db everytime you run test. That's why you should mock axios for instance. You also are able to mock class or function that do not already exist.

Out of test case write:

```javascript
jest.mock("axios");
```

Inside each test write your own implementation of function:

```javascript
axios.post.mockImplementationOnce(() => {
	return Promise.resolve({ data: validResponse });
});
```

Where _validResponse_ is a json file. Remember to restore mock implementation after each test to not impact next tests

```javascript
beforeEach(() => {
	axios.post.mockRestore();
});
```

## Re-rendering

When you write integration test, you may want to check if conditional render works or similar. Then comes _wait_ function:

```javascript
fireEvent.click(button);
await wait(() => {
	const sendMessage = getByText("Your data is incorrect");
	expect(sendMessage).toBeInTheDocument();
	expect(axios.post).toHaveBeenCalledTimes(1);
});
```

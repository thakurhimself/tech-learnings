# JEST Framework

Installation: `npm install --save-dev jest`

Add the following section to your package.json

`
	{
		"scripts": {
			"test": "jest"
		}
	}
`

see docs.

## Using Matchers
Jest uses "matchers" to let you test values in different ways. 

-> Common Matchers
The simplest way to test a value is with exact equality.

`
	test('two plus two is four', () => {
		expect(2+2).toBe(4);
	});
`

In this code, expect(2+2) returns an "expectation" object. You typically won't do 
much with these expectation object except call matchers on them. In this code, 
.toBe(4) is the matcher. When jest runs, it tracks all the failing matchers so that
it can print out nice error messages for you.

toBe uses Object.is to test exact equality. If you want to check the value of an 
object, use toEqual. toEqual recursively checks every field of an object or array.

Tip: toEqual ignores object keys with undefined properties, undefined array items, 
array sparseness, or object type mismatch. To take these into account use 
toStrictEqual instead. `expect(a+b).not.toBe(0)`

-> Truthiness
In tests, you sometimes need to distinguish between undefined, null, and false, but
you sometimes do not want to treat these differently. Jest contains helpers that let
you explicit about what you want.

* toBeNull matches only null
* toBeUndefined matches only undefined
* toBeDefined is the opposite to toBeUndefined
* toBeTruthy matches anything that an if statement treats as true
* toBeFalsy matches anything that an if statement treats as false

You should use the matcher that most precisely corresponds to what your code to be
doing.

-> Numbers
Most ways of comparing numbers have matchers equivalents.

* toBeGreaterThan
* toBeGreaterThanOrEqual
* toBeLessThan
* toBeLessThanOrEqual
* toBe
* toEqual

Note - toBe and toEqual are equivalent for numbers

For floating point equality, use toBeCloseTo instead of toEqual, because you don't
want a test to depend on tiny rounding error.

-> Strings
You can check strings against regular expressions with toMatch

-> Arrays and iterables
You can check if an array or iterable contains a particular item using toContain

-> Exceptions
If you want to test whether a particular functions throws an error when it's called,
use toThrow.

`
	expect(() => compileAndroidCode()).toThrow();
`

Tip: The function that throws an exception needs to be invoked within a wrapping 
function otherwise the toThrow assertion will fail.

-> And more: [expect-ref](https://jestjs.io/docs/expect)






# Testing library: React Testing library
React Testing library builds on top of **DOM Testing library** by adding APIs for 
working with React components.

-> Installation:

`
	npm install --save-dev @testing-library/react @testing-library/dom @types/react
	@types/react-dom
`

With Typescript: To get full type coverage install react and react-dom as well.


-> The problem:
	You want to write maintainable tests for your React components. As a part of this
	goal, you want your tests to avoid including implementation details of your 
	components and rather focus on making your tests give you the confidence for 
	which they are intended. As part of this, you want your testbase to be 
	maintainable in the long run so refactors of your components (changes to 
	implementation but not functionality) don't break your tests and slow you and 
	your team down.


-> This solution:
	The RTL is a very light-weight solution for testing React components. It provides
	light utility function on top of react-dom and react-dom/test-utils, in a way 
	that encourages better testing practices. Its primary guiding principle is:

	`
		The more your tests resemble the way your software is used, the more 
		confidence they can give you.
	`

	So rather than dealing with instances of rendered React components, your tests
	will work with actual DOM nodes. The utilities this library provides facilitate
	querying the DOM in the same way the user would. Finding form elements by their
	label text(just like a user would), finding links and buttons from their text
	(like a user would). It also exposes a recommended way to find elements by a 
	data-testid as an "escape hatch" for elements where the text content and label do
	not make sense or is not practical.

	This lib encourages your applications to be more accessible and allows you to get
	your tests closer to using your components the way a user will, which allows your
	tests to give more confidence that your application will work when a real users
	uses it.

	This is replacement for Enzyme.

	This lib is not a test runner. Use with jest (recommended) or with other test 
	runners. see docs.

Note: This lib is built on top of DOM testing lib which is where most of the logic
behind the queries is.





----

# Next.js related issues with Jest and testing-library/react







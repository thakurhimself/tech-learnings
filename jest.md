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

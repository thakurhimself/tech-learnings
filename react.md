# React 

## Installation
You can use exsiting full stack frameworks like NextJS, or build it from 
scratch.

### Building from scratch
Step 1: Install a build tool
Install build like vite, parcel, or rsbuild

Note - Metro for React Native. It is a JS bundler for RN. Metro supports
bundling for platforms like iOS and Android.

Step 2: Building common application patterns
* Routing
* Data Fetching
* Code-splitting
* Improving application performance


## Managing State

### Chosing the state structure
Principles for structuring state:

1. **Group related state**: if you always update two or more state variables
	consider merging them into a single state.
	Another case where you'll group data into an object or an array is when
	you don't know how many pieces of state you'll need. For example, it's 
	helpful when you have a form where the user can add custom fields.
	Note - if your state variable is an object, remember that you can't update
	only one field in it without explicitly copying the other fields.

2. **Avoid contradictions in state**: When the state is structured in a way 
	that several pieces of state may contradict and disagree with each other,
	you leave room for mistakes. Try to avoid this.

3. **Avoid redundant state**: if you calculate some information from the 
	component's props or its existing state variables during rendering, you
	should not put that information into that component's state.

4. **Avoid duplication in state**: When the same data is duplicated between
	multiple state variables, or within nested objects, it is difficult to 
	keep them in sync. Reduce duplication when you can.
	Example: when selecting item from the list store only id instead of whole
	item object. With this it will not be out of sync.

5. **Avoid deeply nested state**: Deeply hierarchical state is not very 
	convenient to update. When possible, prefer to structure state in a flat
	way.
	It makes easier to update, and it helps ensure you don't have duplication
	in different parts of a nested object.
	Sometimes, you can also reduce state nesting by moving some of the nested
	state into the child components.This works well for ephemeral UI state 
	that doesn't need to be stored, like whether an item is hovered.

The goal behind these principles is to make state easy to update without 
introducing mistakes. *Make you state as simple as it can be - but simpler*

Note - All the normal variables are calculated during render.
Note - Don't mirror props in state. lets understand this throught example: 

`
	funtion Message({messageColor}) {
		const [color, setColor] = useState(messageColor);
	}
`

Here a color state variable is initialized to the messageColor prop. The 
problem is that if the present component passes a different value of 
messageColor later(for example, 'red' instead of 'blue'), the color sate 
variable would not be updated! The state is only initialized during the first
render. Instead, use prop directly in your code. This way it won't get out of
sync with the prop passed from the parent component.
"Mirrorin" props into state only make sense when you want to ignore all 
updates for a specific prop. By convention, start the prop name with initial
or default to clarify that it's new values are ignored.


### Sharing state between components
Sometimes, you want the state of two components to always change together. To
do it, remove state from both them, move it to their closest common parent, 
and then pass it down to them via props. This is known as lifting state up, 
and it's one of the most common things you will do writing React code.

Three steps to lift state up:
1. Remove state from the child components.
2. Pass hardcoded data from the common parent.
	To lift state up, you must locate the closest common parent component of
	both of the child components that you want to coordinate. The parent will
	become "source of truth" for children.

3. Add state to the common parent and pass it down together with the event
	handlers.
	The event handlers are passed down to children to allow them to change the
	state explictly.

Controlled and uncontrolled components:
It is common to call a component with some local state "uncontrolled". 
In contrast, you might say a component is "controlled" when the important 
information in it is driven by props rather than its own local state. This 
lets the parent component fully specify its behaviour. 

Uncontrollled components are easier to use withint their parents because they
required less configuration. But they're less flexible when you want to 
coordinate them together. Controlled components are maximally flexible, but 
they require the parent components to fully configure them with props.

In practice, "controlled" and "uncontrolled" aren't strict technical terms -
each component usually has some mix of bothe local and props. However, this is
useful way to talk about how components are designed and what capabilities 
they offer.

Whey writting a component, consider which information in it should be 
controlled (via props), and which information should be uncontrolled (via 
state). But you can always change your mind and refactore later.


-> In a React application, many components will have their own state. Some
state may "live" close to the leaf components( components at the bottom of the
tree) like inputs. Other state may "live" closer to the top of the app. For
expample, even client-side routing libs are usually implemented by storing
the current route in the React state, and passing it down by props.
For each unique piece of state, you will choose the component that "owns" it.
This principle is also known as having a "single source of truth". It doesn't
mean all state lives in one place - but that each piece of state, there is 
specific component that holds that peice of information. Instead of duplicate
shared state between components, lift if up to their common shared parent, and
pass it down to the children that need it.
As your app changes, you move state up and down according to the needs. This
is all part of the process!


### Preserving and Resetting State
State is isolated b/w components. React keeps track of which state belongs to
which component based on their place in the UI tree. You can control when to
preseve state and when to reset it b/w re-renders.

-> State is tied to a position in the render tree:
React builds render trees for the component structure in your UI.

When you give a component state, the state is actually held inside React. React
associates each piece of state it's holding with the correct component by where
that component sits in the render tree. 
Each component on the screen has fully isolated state. React will keep the state
around for as long as you render the same component at the same position in the
tree. When React removes a component, it destroys its state.
React preserves a component's state for as long as it's being rendered at its
position in the UI tree. If it gets removed,  or a different component gets
rendered at the same position, React discards its state.

-> Same component at the same position preserves state: see the Counter example
where isFancy prop is sent passed to it. Whether isFancy is true or false, you
always have Counter as the first child of the div returned from the root App
component.

Note - Remember that it's the position in the UI tree - not in the JSX markup -
that matters to React!

-> Different components at the same position reset state:
When you render a different component in the same position, it resets the state
of its entire subtree.

`
	 {isFancy ? (
	   <div>
			<Counter isFancy={true} /> 
		</div>
	 ) : (
		<section>
			<Counter isFancy={false} />
	   </section>
	   )}
`

In this state gets reset. Although you render a Counter, the first child of the
div changes from a section to a div. When the child section was removed from the
DOM, the whole tree below it was destryed as well.
AS a rule of thumb, if you want to preserve the state between re-renders the
structure of your tree needs to "match up" from one render to another. If the
structure is different, the state gets destroyed because React destroys state
when it removes a component from the tree.

Note - Always declare component functions at the top level, and don't nest their
definitions.

-> Resetting state at the same position:
Sometimes you may want to reset a component's state at the same position.
There are two ways to reset state when switching between them:
1. Render components in different positions
	This solution is convenient when you have few independent components 
	rendered in the same place.

2. Give each component an explicit identiy with key
	More generic way to reset a component's state. You can use keys to make 
	React distinguish b/w any components. By default, React uses order within
	the parent to discern b/w components. But keys let you tell React that this
	is a specific component.
	Switching b/w components does not preserve state. This is because you gave
	them different keys.
	Specifying a key, tells React to use the key itself as part of the position
	, instead of their order within the parent. This is why, even though you 
	render them in the same place in JSX, React sees them as two differnt 
	counter appears on the screen, its state is created. Every time it is 
	removed, its state is destroyed. Toggling b/w them resets their state over
	and over.

Note - Remember the keys are not globally unique. They only specify the position
within the parent.

-> Resetting a form with key:
Resetting state with a key is particularly useful when dealing with forms. 
Whey you pass key the component will be recreated from scracth, including any
state in the tree below it. React will also re-create the DOM elements instead
of resuing them.

Note - Preserving state for removed components:-
Three ways:
1. Render all components and hide them with css and conditionals. Problematic
for large and contain a lot of DOM nodes.

2. Lift the state up. Most common solution

3. You might use a different source in addition to React state. For example,
you probably want a message draft to persist even if the user accidentally 
closes the page. To implement this, you could have the Chat component initialize
its state by reading from the localStorage, and save the drafts there too.

Recap:
* React keeps state for as long as the same component is rendered at the same
position.
* State is not kept in JSX tags. It's associated with the tree position in 
which you put in that JSX.
* You can force a subtree to reset its safe by giving it a different key.
* Don't nest component definitions, or you'll reset state by accident.


### Extracting State Logic into a Reducer
Components with many state updates spread across many event handlers can get 
overwhelming. For these cases, you can consolidate all the state update logic 
outside your component in a single function, called a reducer.

-> Consolidate state logic with a reducer:
To reduce complexity and keep all your logic in one easy-to-access place, you 
can move the state logic into a single function outside your component, called a
"reducer".
Reducers are a different way to handle state. You can migrate from useState to
useReducer in three steps:

1. **Move** from setting state to dispatching actions.
	Managing state with reducers is slightly different from directly setting 
	state. Instead of telling React "what to do" by setting state, you specify
	"what the user just did" by dispatching "actions" from your event handlers.
	Example:

	`
		dispatch({
			type: 'added',
			id: nextId++,
			text: text
		})
	`

	The object you pass to dispatch is called an "action". It is regular JS 
	object. It should contain the minimal information about what happened. An
	action object can have any shape. By convention, it is common to give it a
	string type that describes what happened, and pass any additional info in 
	other fields.

2. **Write** a reducer function.
	A reducer funciotn is where you put your state logic. It takes two arguments
	, the current state and the action object, and it returns next state:

	`
		// use switch statement to track actions. just like redux

		function reducerFunc(state, action) {
			switch(action.type) {
				case 'added': {
					return [...state, { id: action.id, text: action.text }]
				}
				
				default: { throw Error('Unknown action: ' + action.type }
			}
		}

		// use braces so that variables declared inside of different cases don't
		clash with each other.
	`

	React will set the state to what you return from the reducer. Because the
	reducer function takes state as an argument, you can declare it outside of 
	your component. This decreases the indentation level and can make your code
	easier to read.

3. **Use** the reducer from your component.
	Import the useReducer Hook from React and replace the useState with it.

	`
		import { useReducer } from 'React';

		const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);
	`

	The useReducer Hook takes two arguments:
	* A reducer function
	* An initial state

	And it returns:
	* A stateful value
	* A dispatch function (to "dispatch user actions to the reducer)

	You can move the reducer to a different file. Component logic can be easier
	to read when you separate concerns like this. Now the event handlers only 
	specifiy what happened by dispatching actions, and the reducer function
	determines how the state updates in response to them.


-> Writing reducers well:
Two tips:

1. Reducers must be pure : like state updater functions, reducers run during
	rendering (actions are queued until the next render). This means they must
	be pure - same inputs always result in the same output. They should not send
	requests, schedule timeouts, or perform any side effects (operations that 
	impact things outside the component). They should update objects and arrays
	without mutations.

2. Each action describes a single user interaction, even if that leads to 
	multiple changes in the data.



### Passig Data Deeply with Context
Usually you will pass information from a parent component to a child component
via props. But if tree size is large and receiving component is down the tree
then prop drilling is not best way. Context lets the parent component make some
information available to any component in the tree below it - no matter how deep
- without passing it explicitly through props.

You add context in 3 steps:

1. **Create** a context.
2. **Use** the context from the component that needs the data.
3. **Provide** that context from the component that specifies thet data.

Context lets a parent - even a distant one - provide some data to the entire 
tree inside of it.

-> Step 1: Create the context: You need to create the context. You'll need to
export it from a file so that your components can use it. You can declare 
context in the same file as component, but it's better practice to separate the
concern.

`
	import { createContext } form 'react';

	export const LevelContext = createContext(1);
`

-> Step 2: Use the context: import the useContext Hook from React and your 
context:

`
	import { useContext } from 'react';
	import { LevelContext } from './LevelContext.js';

	export default function Heading({children}) {
		const level = useContext(LevelContext);
		//...
	}
`

useContext is a Hook. It tells React that the Heading component wants to read
the LevelContext in the given example.

-> Step 3: Provide the context:

`
	import { LevelContext } from './LevelContext.js';
	export default function Section({level, children}) {
		return (
			<section>
				<LevelContext value={level}>
					{children}
				</LevelCotext>
			</section>
		)
	}
	// This tells React: "if any component inside this <Section> asks for Level
	// Context, give them this level. The component will use the value of 
	// nearest <LevelContext> in the UI tree above it.
`

Note - Context passes through intermediate components. Context lets you write
components that 'adapt to their surroundings' and display themselves differently
depending on where(or, in other words, in which context) they are being rendered
. How context works might remind you of CSS property inheritance. Different 
React contexts don't override each other. Each context that you make with 
createContext() is completely separate from other ones, and ties together 
components using and providing that particular context. One component may use or
provide many different contexts without a problem.

-> Few alternatives before you use context:

1. Start by passing props.
2. Extract components and pass JSX as children to them.

-> Use cases for context

* Theming
* Current accout
* Routing
* Managing state

In general if some information is needed by distant components in differnt parts
of the tree, it's a good indication that context will help you.


### Scaling up with Renducer and Context
Reducers let you consolidate a component's state update logic. Context lets you
pass information deep down to other components. You can combine reducers and 
context together to manage state of a complex screen.

Following are the steps to combine a reducer with context:

1. **Create** the context
2. **Put** state and dispatch into context
3. **Use** context anywhere in the tree

Note - You will create two separate contexts. One for providing the actual state.
Second for the providing the functions that lets the components dispatch actions.
Export them from a separate file so that you can later import them from other 
files. see docs of how to?

Note - You import context in you component. Take state and dispatch returned by
useReducer() and provide them to the entire tree below. Now you don't need to 
pass the state and or the event handler down the tree. Instead use useContext() 
to read both state and dispatch from any component below the tree. The state 
still 'lives' in the top-level component, managed with useReducer.

-> Moving all wiring into a single file:- see docs for how to do. You can create
a custom hook to return context values, and funcs.

## Escape Hatches

### Referencing Values with Refs
When you want a component to remember some info but without triggering new
renders, you can use ref.

-> Adding a ref to your component

`
	import { useRef } from 'react';
	// inside the component

	const ref = useRef(0);
`

Inside your component, call the useRef Hook and pass the initial value that you
want to reference as the only argument. For example see above.

useRef returns an object like this:

`
	{
		current: 0	// The value you passed to useRef
	}
`

You can access the current value of the ref through the `ref.current` property.
This value is intentionally mutable, meaning you can both read and write to it.
It's like a secret pocket of your component that React doesn't track. (This is 
what makes it an 'escape hatch' from React's one-way data flow)
The ref, like state, could point to anything: a string, an object, a number, or
even a function. Unlike state, ref is plain JS object with the current property
that you can read and modify.

Note - Like state, refs are retained by React b/w re-renders. However, setting 
state re-renders a component. Changing a ref doesn't. You can combine refs and
state in a single component.

When a piece of information is used for rendering, keep it in state. When a piece
of information is only needed by event handlers and changing it doesn't require
a re-render, using a ref may be more efficient. When something is displayed on 
the screen it makes sense to use state value for it. Setting a state triggers the
re-render that reflect the new value. If you try to do this with refs React would
never re-render the component and latest update will not be reflected on the 
screen. 
This is why reading ref.current during render leads to unreliable code. if you 
need that, use state instead.

-> When to use refs: use a ref when your component needs to 'step outside' React
and communicate with external APIs - often browser API that won't impact the 
appearance of the component.

* Storing timeout IDs
* Storing and manipulating DOM elements
* Storing other objects that are aren't necessary to calculate the JSX.

If your component needs to store some value, but it doesn't impact the rendering
logic, choose refs.

-> Best practices for refs:

* Treat refs as an escape hatch.
* Don't read or write ref.current during rendering: if some information is needed
during rendering, use state instead. Since React doesn't know when ref.current
changes, even reading it while rendering makes your component's behaviour hard to
prodict. (The only exception to this is code like ` if(!ref.current) ref.current 
= new Thing()` which only sets the ref once during the first render.

Limitations of React state don't apply to refs. For exmaple, state acts like a
snapshot of for every render and doesn't update syncronoulsy. But when you mutate
the current value of a ref, it changes immediately. This is because the ref 
itself is a regular JS object, and so it behaves like so. You can mutate it as 
long as mutating isn't used for rendering.

### Manipulating the DOM with refs
Sometimes you might need access to the DOM elements managed by React - for 
example, to focus a node, scroll to it, or measure its size and position. You 
will need a ref to the DOM node.

-> Getting a ref to the node

`
	import { useRef } from 'react';

	// inside the component
	const myRef = useRef(null);
	<div ref={myRef} >
`

The useRef Hook returns an object with a single property called current.
Initially, myRef.current will be null. When React creates a DOM node for this
<div>, React will put a reference to this node into myRef.current. You can then
access this DOM node from your event handlers and use the built-in browser APIs
defined on it.

Sometimes you might need a ref to each item in the list, and you don't know how
many you will have. One solution is to pass a function to the ref attribute. This
is called a ref callback. React will call your ref callback with the DOM node 
when it's time to set the ref, and call the cleanup function returned from the
callback when it's time to clear it. This lets you maintain your own array or a 
Map, and access any ref by its index or some kind of ID. see docs.

Note - Refs are escape hatch. Manually manipulating another component's DOM nodes
can make your code fragile.

-> Accessing another component's DOM nodes: You can pass refs from parent 
component to child components just like any other prop.
When ref is passed to another component as prop, you may want to restrict the 
exposed functionality. You can do that with useImeprativeHandle

`
	function Myinput({ref}) {
		const realInputRef = useRef(null);
		useImperativeHandle(ref, () => ({
			focus() {
				realInputRef.current.focus();
			}
		}));
		return <input ref={realInputRef} />
	}
`
Here, realInputRef inside MyInput holds the actual input DOM node. However, 
useImperativeHandle instructs React to provide your own special object as the 
value of a ref to the parent component. So inputRef.current inside the Form 
component will only have the focus method. In this case, the ref "handle" is not
the DOM node, but the custom object you create inside useImperativeHandle call.

-> When React attaches the refs:
In React, every update is split in two phases:

* During **render**, React calls your components to figure out what should be on
the screen.
* During **commit**, React applies changes to the DOM.

In general, youi don't want to access refs during rendering. That goes for refs 
holding DOM nodes as well. During the first render, the DOM nodes have not yet
been created, so ref.current will be null. And during the rendering of updates,
the DOM nodes haven't been updated yet. So it's too early to read them.

React sets ref.current during the commit. Before updating the DOM, React sets the
affected ref.current values to null. After updating the DOM React immediately 
sets them to the corresponding DOM nodes.

Usually, you will access refs from event handlers. If you want to do something
with a ref, but there is no particular event to do it, you might need an Effect.

-> Flushing state updates asynchronously with flushSync
Imagine there is list of todos and everytime you add todo, with the help of ref,
you try to display last element using scrollIntoView. But it won't happen and 
only shows the second last todo.
In React, state updates are queued. However, here it causes a problem because
setTodos does not immediately update the DOM. So the time you scroll the list to
its last element, the todo has not yet been added. This is why scrolling always
"lags behind" by one item.
To fix this issue, you can force React to update ("flush") the DOM synchronously.
To do this, import *flushSync* from react-dom and wrap the state update into a 
flushSync call:

`
	flushSync(() => {
		setTodos([...todos, newTodo]);
	});
	listRef.current.lastchild.scrollIntoView();
`

This will instruct React to update the DOM synchronously right after the code
wrapped in flushSync executes. As a result, the last todo will already be in the
DOM by the time you try to scroll to it.

-> Best practices for DOM manipulation with refs:
Refs are an escape hatches. You should only use them when you have to "step 
outside React". Common examples of this include managing focus, scroll position,
or calling browser APIs that React does not expose.
If you stick to non-destructive actions like focusing and scrolling, you 
shouldn't encounter any problems. However, if you try to modity the DOM manually,
you can risk conflicting with the changes React is making.	

* **Avoid changin DOM nodes managed by React**. Modifying, adding children to, or
removing children from elements that are managed by React can lead to 
incosistent visaul results or crashes like above.

* **You can safely modify parts of the DOM that React has no reason to update**.


### Synchronizing with Effects
Effects let you run some code after rendering so that you can synchronize your
component with some system outside of React.

-> What are Effects and how are they different from events?
There are two types of logic inside React components:

1. Rendering code :- Lives at the top level of your component. This is where you
take the props and state, transform them, and return the JSX you want to see on 
the screen. Rendering code must be pure like a math formula.

2. Event handlers (introduced in Adding interactivity):- they are nested funcs 
inside your components that do things rather than just calculate them. An event
handler might update an input field, submit an HTTP post request a buy product,
or navigate the user to another screen. Event handlers cantain "side effects"
caused by a specific user action.

Sometimes this isn't enough. Consider a ChatRoom component that must connect to
the chat server whatever it's visible on the screen. Connecting to a server is 
not a pure calculation (it's a side effect) so it can't happen during rendering.
However, there is no single particular event lika a click that causes ChatRoom
to be displayed.

**Effects let you specify side effects that are caused by rendering itself, 
rather than by a particular events**. Sending a message in the chat is an event
because it is directly caused by the user clicking a specific button. However,
setting up a server connection is an Effect because it should happen no matter
which interaction caused the component to appear. Effects run at the end of a
commit after the commit after the screen updates. This is a good time to sync
the React components with some external system.

-> You might not need an effect:- Don't rush to add effects to your components.
Keep in mind that Effects are typically used to "step out" of your React code
and sync with some external system. This includes browser APIs, third-party 
widgets, network, and so on. If your effect only adjusts some state based on 
other state, you might not need an Effect.

-> How to write an Effect:-
Steps to write an Effect:

1. Declare an Effect:- By default, you Effect will run after every commit.
	Everytime your component renders, React will update the screen and then run
	the code inside useEffect. In other words, useEffect "delays" a piece of 
	code from running until that render is reflected on the screen.

	Note - Don't try to do something with DOM node during rendering. In React,
	rendering should be a pure calculations of JSX and should not contain side
	effects like modifying the DOM. Moreover during first rendering the DOM 
	doesn't exist yet; the app will break. So wrap the side effect with 
	useEffect to move it out of the rendering calculation. By doing this, you
	let the React update the screen first. Then your Effect runs.
	
	Note - Effects run after every render.

2. Specify the Effect dependencies:- Mosts effects should only re-run when 
needed rather than after every render.
	You can tell React to skip unnecessarily re-running the Effect by specifying
	an array of dependencies as the second argument to the useEffect call. Start
	by adding an empty [] array as the second argument. An empty array makes the
	useState runs only once after the render and prevents re-running on every 
	re-render.
	The dependency array can contain multiple dependencies. React will only skip
	re-running the Effect if all of the dependencies you specify have exactly the
	same values as they had during the previous render. React compares the 
	dependency values using the Object.is comparision. 

	Note - The ref object has a stable identity: React guarantees you'll always
	get the same object from the same useRef call on every render. It never 
	changes, so it will never by itself cause the Effect to re-run. Therefore, it
	does not matter whether you include it or not. Including it is fine too.
	The set function returned by useState also have stable identity, so you'll
	often see them omitted from the dependencies too. If the linter lets you omit
	a dependency without errors, it is safe to do.
	Omitting always-stable dependencies only works when the linter can "see" that
	the object is stable. For exmaple, if ref was passed from a parent component,
	you would have to specify it in the dependency array. However, this is good
	because you can't know whether the parent component always passes the same ref
	, or passes one of several refs conditionally. So you Effect would depend on 
	which ref is passed.

3. Add cleanup if needed:- Some Effects need to specify how to stop, undo, or
clean up whatever they were doing.
	The code inside the Effect doesn't use any props or state, so your dependency
	array is []. This tells React to only run this code when the component 
	"mounts", i.e. appears on the screen for the first time.
	Suppose a component makes a connection outside and then you navigate to other
	page. The previous component is unmounted but the connection is not destroyed.
	When you come back that component again then connection is again established.
	This way is connections would keep piling up.
	To help you spot them quickly, in development React remounts every component
	once immediately after its inintal mount.
	To fix this issue, return a cleanup function from your Effect. React will call
	you cleanup function each time before the Effect runs again, and one final 
	time when the component unmounts (gets removed).


-> How to handle the Effect firing twice in development?
React intentionally remounts your components in development to find bugs. The 
right question isn't "how to run an Effect once", but "how to fix my Effect so 
that it works after remounting".
Usually, the answer is to implement the cleanup function. The cleanup function
should stop or undo whatever the Effect was doing. The rule of thumb is that user
shouldn't be able to distinguish between the Effect running once (as in prod) and
a setup -> cleanup -> setup sequence (as you'd see in development).


# Hooks list

1. useState

2. useContext

3. useReducer




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



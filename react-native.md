# Academind RN tutorial

Course Content: 
Getting started
1. Essentials
	* Basics & Fundamentals
	* Dynamic & Adoptive Layouts
	* Navigation
2. React & React Native
	* Redux & RN
	* Complete example App
	* Handling user input
	* Http requests
3. Advanced
	* User authentication
	* Native Device Features
	* Push Notifications
	* Publishing Apps


## The Basics
Diving Into The Core concepts
1. Using RN components & Building UIs
2. Styling RN Apps.
3. Adding Interactivity & Managing State

Note - look for the slides in ott/react-native

The style props is not supported on all the elements but it is supported
on the following:
* View
* Text
* Button - it doesn't have style prop
* Pressable
* Image
* TextInput
* ScrollView
* Modal

Technically inline styles are allowed but it's not the best way to style
the app. The best way is to use stylesheets.

Exploring Layouts & Flexbox:

Note - width property takes number as pixels, and also takes percentage
values in string format.

Flexbox A Deep Dive -
By default every view in a RN uses flexbox even if it is not assigned 
any styles. Every view box organizes its children with the help of flexbox
thing. Flexbox is simply borrowed from CSS, that is all about organizing
child elements in one dimenesional space. For example Column that is also
default in RN not only does every view by default uses flexbox, it also by
default organizes children in a column, so from top to bottom.
You can change this default by using flexDirection - flexDirection: "row"
- property.

Another important thing about flexbox is how child elements are sized. if 
you remove height and width property. The view only as wide and tall as 
it's child require it to be.
In flex there are two axis: main and cross axis. The main axis is the axis
set by flex direction, and the cross direction will be the opposite of the
main axis. You can align items across the main axis using justifyContent
and cross axis using alignItems. 
You can put stretch to alignItems, but not to justifyContent. So to 
stretch the child on main axis you set it in the child element.
The flex property is useful for this.
Add flex: 1 to views to the child element, it tells child element to take
as much space as it can along the main axis. Along the cross axis you have
to do this on the parent like this - alignItems: "stretch".

You can add this to other child elements as well. flex: 'number' tells the
the element to take the proportionate amount of the space. These numbers
are relative number to each other.

Handling event:
you add event handlers in the core components: for example
TextInput can have onChangeText. like this  onChangeText={handler}. This
will automatically pass the text string to the handler you just have to
access it in the handler. like handler(text) {...}

Similarly Button has onPress event handler. Similar to onClick in React.

Styling differences between IOS and Android:
Core components are compiled to native UI widgets. So there are some plat-
form specific differences. For example border radius works on Text in
Android but won't work in IOS because IOS Text widget doesn't support
rounded corners.
One way to work around that is use more versatile View component to wrap
it, and add styles to them.
One another important thing is styles in RN doesn't cascade. That's means
it won't apply to child elements if parent is applied any style say color.
Because we have CSS like property names but it's not CSS.

Making content scrollable with ScrollView:
You can't scroll the down in the view unilike browsers which by default 
support overflow.
If you want to scroll you have to explicitly tell the RN that. For that
ScrollView is there. It is similar to View that has scrollable behaviour.

The ScrollView has the job of making something scrollable, but the area,
but the area that is scrollable is, in the end, determined by the current
container that holds the ScrollView.
So what you should do here is add another normal View that wraps the 
ScrollView, set the height to the that View to add the height you want
ScrollView to have.
see docs for props that are supported by ScrollView. There is vast config-
uration available there. Also there will be platform specific props as
well. Have a close look at them.

Optimizing List with Flatlist:
ScrollView is appropriate for adding scroll feature round some content
like articles etc. It's best when the height is known, and not super long.
For lists it not suitable. Because lists can be very long.
ScrollView renders all the elements insde of it, even if it is not visible
when overall UI is rendered. Imagine if you have to render lists of 
thousands of list items at once. It will slowdown the app.

So for dynamic lists with thousands of items. Flatlist is used. It's a
built-in component. It's built for scrollable list, and internally it
renders only the lists that are visible on the screen. And all the items
that are off-screen will only be loaded and rendered lazily as they are 
needed as the users scrolling.

Internally FlatList will have small threshold to make sure it starts 
loading and rendering items before they are visible. but It will only be
rendered as the user is closing in on the item. if you have thousand item
list majority will not be loaded and rendered, but will be loaded as the 
user comes close to it scrolling.
It pretty much supports most of the props that ScrollView supports because
it has almost same implementation with optimization for lists.

`
	<Flatlist 
	data={goals}
	renderItem={ (item) => {
			return <View>{item.item}</View>	
		}
	}
	keyExtractor={(item, index) => { return item.id } }
	/>
`

The item that is passed on the renderItem is actually an object generated
by FlatList internally. it wrapped around individual data items. It also 
contains some metadata.

For keys if you already have key in the data item. it will automatically
fetches that. It actively looks for key property.
Now if you don't have key as property in the data item. for example you 
are fetching data from api. There is another way to add keys: add an 
another prop - keyExtractor. see above example. item and index is provided
by FlatList itself.

Splitting Componentst into Smaller components:- same as React web app.

Utilizing Props:- same a React web app. You can pass and use using props,
or destructuring.

Handling Taps with the Pressable Component:
On web you can simpley onClick to any dom element. But in RN, you need to 
specifically declare if any element or component is pressable.
There is a built in component 'Pressable' for that. There is also others
built-ins similar to Pressable like Touchable (and related family). 
They will be deprecated in the future. So use Pressable for such cases.

Simply wrap the target element, or component, with Pressable component
and add a prop onPress={} which will take a function. Similar to button.

Deleting an Item from the list:
Simply use filter. Also one another thing. if you pass a function in the
onPress and function required any argument. We can bind the function with
the argument. This bind is a feature of JS - see docs for more.
onPress={props.deleteGoal.bind(this, props.id)} 


Adding an Adroid Ripple Effect & an iOS Alternative:-
For Android there is a prop available for this: Android_ripple. see RN 
docs.

For iOS: it's different method. 
Pass a style prop to Pressable which will take a function and in that 
function it passes a argument that tracks the press states. So you can 
define different style for ripple and add to the element for particular 
state for iOS.
style={({pressed}) => pressed && styles.ripple}

Modal:-
It's a built-in component. see docs.


Note - Buttons are prestyled. So you have to create your own button using
Pressable with some text inside it to have its own style.

Image:-
It's also an builtin component provided by RN.
`
	<Image source={reqruire("")} /> 
	// if it is stored in public folder else
	// normal call
`

Note - When you have multiple screens, adding to background color to each
screen is annoying. So in app.json file add "backgroundColor" for this. 
This will add the background color to all the screen.


StatusBar:-
Status of the screen is not visible clearly. Use expo standalone package
StatusBar component to make it visible. see docs.
Simply add 
`
	<StatusBar />
`
It will make the status bar visible.
There are also props for other usecases. see docs for more


### Fundamentals deep dive
Agenda: Building Real Apps & Layouts 
* Build a Complete Demo App("Guess my number" game)
* New Core Components
* Complex Layouts & Styles
* Adding Reusable Components & Styles

Note - To add shadow to an element, use 'elevation' property which takes 
numbers for android. For ios: use shadowColor, shadowOffset, 
shadowRadius, shadowOpacity to have same effect.

-> With maxLength attribute on TextInput you can restrict the character
lengths.

-> Configuring TextInput: you can add keyboardType prop to TextInput and
configure it show only specific type of keys. for example, only numbers,
alphabets, and more. see docs. Also prop autoCapitalize, autoCorrectionl;
see docs Checkout all the props that are available for TextInput in the 
docs.

-> If you want to add multiple styles variable to an element you can use
array and passed all the styles variables in it.

-> You can add common layout to all the children through the layout file
and using slot imported form 'expo-router'. Also you can add background 
colors to all the pages using backgroundColor attribute in app.json file.

-> linear-gradient is not supported out of the box. So use expo-linear-gra
dient package. Install: npx expo install expo-linear-gradient.
Usage: import { LinearGradient } from expo-linear-gradient
`
	<LinearGradient
	colors={['#444', '#ddd']} // color stops
	>
		<Slot />
	</LinearGradient>
`
see doc





---
Extra learnings:
1. When using pressable for custom buttons, if Text element is inside the
it it will inherit the parent's bg color.

2. For ripple effect in pressable in android: there is issue figure it out
One way to add ripple effect is doing in ios style. it will add to android
as well.
`
	style={({pressed}) => pressed ? [style1, style2] : style2}
`

3. You can wrap buttons inside the View to add flex properties. For example
Say you want 2 buttons to be side by side. So have a parent View component
with flexDirection to row. Then have each button inside its own View element
and apply flex 1 to each View. It will stretch the View to available spaces
and buttons completely occupy the View.



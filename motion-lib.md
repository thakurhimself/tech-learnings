# Motion: A library for creating animations.

Note - This only works for web apps.

### Get started with Motion for React

Motion for React is a React animation lib for building smooth, production
grade UI animations. You can start with simple prop-based animations befo-
re growing to layout, gesture and scroll.

Install: npm install motion	

Features can now be imported via "motion/react"
e.g. import { motion } from 'motion/react'

First animation:
`
	<motion.ul animate={{ rotate: 360 }} />
`
Note - here ul just an <ul/> element you can use any element.

When values in animate change, the component will animate. Motion has
intuitive defaults, but animations can of course be configured via the
transition prop.

`
	<motion.div
	  animate={{
	      scale: 2,
		  transition: { duration: 2 }
	  }}
	/>
`

Enter animation:
When a component enters the page, it will automatically animate to the
values defined in the animate prop.
You can provide values to animate from via the initial prop(otherwise
these will be read from the DOM)

`
	<motion.button initial={{ scale: 0 }} animate={{ scale: 1 }} />
`

Or disable the intial animation entirely by setting initial to false.

Hover & tap animation:
<motion/> extends React's event system with powerful gesture animations.
It currently supports hover, tab, focus, and drag.

`
	<motion.button
	  whileHover={{ scale: 1.1 }}
	  whileTap={{ scale: 0.95 }}
	  onHoverStart={() => console.log('hover started!')}
	/>
`

Scroll animation:
Motion supports both types of scroll animations: Scroll-triggered and 
scroll-linked.
To trigger an animation on scroll, the whileInView prop defines a state to
animate to/from when an element/leaves the viewport.
n
`
	<motion.div
	  initial={{ backgroundColor: "rgb(0, 255, 0)", opacity: 0 }}
	  whileInView={{ backgroundColor: "rgb(255, 0, 0)", opacity: 1 }}
	/>
`

Whereas to link to a value directly to scroll position, it's possible to
use MotionValue's via useScroll.

`
	const { scrollYProgress } = useScroll()
	return <motion.div style={{ scaleX: scrollYProgress }} />
`

Layout animation:
Motion has an industry-leading layout animation engine that supports 
animating between changes in layout using transforms.

It's easy as applying to the layout prop.

`
	<motion.div layout />
`

Or to animate between completely differently elements, a layoutId:
`
	<motion.div layoutId="underline" />
`

see docs for more information

Exit animations:
By wrapping motion components with <AnimatePresence> we gain access to exit
animations. This allows us to animate elements as they're removed from the
DOM.

`
	<AnimatePresence>
	  {show ? <motion.div key="box" exit={{ opacity: 0 }} /> : null}
	</AnimatePresence>
`












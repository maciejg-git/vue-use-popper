# vue-use-popper

Composable function for Vue 3 and Popper.js

```javascript
{
  isPopperVisible: Ref,
  reference: Ref,
  referenceInstance: Ref,
  updatePopperInstance: Function,
  popper: Ref,
  showPopper: Function,
  hidePopper: Function,
  togglePopper: Function,
  lockPopper: Function,
  destroyPopperInstance: Function,
  updateVirtualElement: Function,
} = usePopper(
  options: { placement, offsetX, offsetY, noFlip }: Object,
  modifiers: Array,
  customOptions: Object,
)
```

### What to plug into function

**options** allows setting common Popper options

- placement - is one of valid Popper placement options
- offsetX and offsetY - lets you displace a popper element from its reference element 
- noFlip - can change the placement of a popper when it's scheduled to overflow a given boundary

**modifiers** is optional array of additional Popper modifiers

**customOptions** is optional object with additional non standard Popper options and modifiers

### What you get in return

- isPopperVisible - current visibility state of popper that should be used on popper element as condition for v-if or v-show
- reference - should be set in template as remplate ref for reference element. Value of this ref is always html element even if set on component
- rawReference
- popper - should be set in template as template ref for popper element
- showPopper - function to show popper
- hidePopper - function to hide popper
- togglePopper - function to toggle popper
- lockPopper - function that prevents destroying popper instance (useful for transitions)
- destroyPopperInstance - destroys popper instance (useful for transitions)
- updatePopperInstance - function to update current popper instance
- updateVirtualElement - function to set new position for virtual element if not using real reference element (for example context menu)

### What to do with those

Simple example

```vue
<template>
  <button 
    ref="reference" 
    @click="togglePopper"
  >
    Popper button
  </button>

  <div 
    v-if="isPopperVisible"
    ref="popper"
  >
    Popper content
  </div>
</template>

<script setup>
import usePopper from "usePopper"

{
  isPopperVisible,
  reference,
  popper,
  togglePopper
} = usePopper({
  placement: "auto",
  offsetX: 0,
  offsetY: 0,
  noFlip: false,
})
</script>
```

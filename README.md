# vue-use-popper

Composable function for Vue 3 and Popper.js

```javascript
{
  isPopperVisible,
  reference,
  referenceInstance,
  updatePopperInstance,
  popper,
  showPopper,
  hidePopper,
  togglePopper,
  lockPopper,
  destroyPopperInstance,
  updateVirtualElement,
} = usePopper(
  options,
  modifiers,
  customOptions,
)
```

### What to plug into function

**options** allows setting common Popper options

{ placement, offsetX, offsetY, noFlip }

- placement - is one of valid Popper placement options
- offsetX - and offsetY lets you displace a popper element from its reference element 
- noFlip - can change the placement of a popper when it's scheduled to overflow a given boundary

**modifiers** is optional array of additional Popper modifiers

**customOptions** is object with additional non standard Popper options and modifiers

### What you get in return

- isPopperVisible - current visibility state of popper
- reference - should be set in template as remplate ref for reference element
- referenceInstance
- updatePopperInstance - function to update current popper instance
- popper - should be set in template as template ref for popper element
- showPopper - function to show popper
- hidePopper - function to hide popper
- togglePopper - function to toggle popper
- lockPopper - function that prevents destroying popper instance (useful for transitions)
- destroyPopperInstance - destroys popper instance (useful for transitions)
- updateVirtualElement - function to set new position for virtual element if not using real reference element

### What to do with those

```html
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
```
```javascript
<script setup>
{
  isPopperVisible,
  reference,
  popper,
  togglePopper,
} = usePopper(
  { placement: "auto", offsetX: 0, offsetY: 0, noFlip: false }
)
</script>
```

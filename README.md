# vue-use-popper

Light composable function for Vue 3 and Popper.js

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

**options** allows setting common Popper options and modifiers

- placement - is one of valid Popper placement options. Default: "auto".
- offsetX and offsetY - lets you displace a popper element from its reference element. Default: 0.
- noFlip - can change the placement of a popper when it's scheduled to overflow a given boundary. Default: false.

**modifiers** is optional array of additional Popper modifiers

**customOptions** is optional object with additional non standard Popper options and modifiers

### What you get in return

- isPopperVisible - current visibility state of popper that should be used on popper element as condition for `v-if` or `v-show`
- reference - should be set in template as remplate `ref` for reference element. Value of this ref is always html element even if set on component
- rawReference - if reference is component this is component instance
- popper - should be set in template as template `ref` for popper element
- showPopper - function to show popper
- hidePopper - function to hide popper
- togglePopper - function to toggle popper
- lockPopper - function that prevents destroying popper instance (useful for transitions)
- destroyPopperInstance - destroys popper instance (useful for transitions)
- updatePopperInstance - function to update current popper instance. This can be used to update visible popper position if reference element changes.
- updateVirtualElement - function to set new position of virtual element if not using real reference html element (for example context menu). Position is calculated from MouseEvent in the argument.

### What to do with those

#### Simple example

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

#### Additional modifiers example

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
},
[
  {
    name: 'preventOverflow',
    options: {
      mainAxis: false,
    },
  }
])
</script>
```

#### Popper transition example

```vue
<template>
  <button 
    ref="reference" 
    @click="togglePopper"
  >
    Popper button
  </button>

  <transition
    name="fade"
    @before-leave="lockPopper"
    @after-leave="destroyPopperInstance"
  >
    <div 
      v-if="isPopperVisible"
      ref="popper"
    >
      Popper content
    </div>
  </transition>
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

<style>
.fade-enter-active,
.fade-leave-active {
  transition: opacity 0.4s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
}
</style>
```

#### Virtual element context menu example

```vue
<template>
  <div @context.prevent="showContextMenu">
    Right click to show context menu
  </div>

  <div 
    v-if="isPopperVisible"
    ref="popper"
  >
    Context menu
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

let showContextMenu = (event) => {
  updateVirtualElement(event)
  showPopper()
}
</script>
```

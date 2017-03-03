----

** When do we use blocks? **

Blocks are where we build specific components that make up our project.

A block can contain (and describe) many of the elements inside of it, for example:

    .house
        .house__roof
        .house__wall
        .house__window
        .house__door
        .house__floor

However, when some elements of a block are *re-used* across other components, or have obvious *elements* of their own, we create a new block:

    .window
        .window__frame
        .window__glass
        .window__ledge

Now we can re-use .window inside of .car .boat .plane...

----

** How do we set up blocks? **

When you start a block, you should:

    1. Choose a descriptive name (hyphens for word-breaks)

    2. Create a _block-name.scss partial inside of the /scss/blocks/ folder

    3. @import it into style.scss (after the library files!)

    4. Ensure that the outer-most element in the HTML _is that class name_

----

** Do I need to document anything? **

Let's revisit the house example, and assume that all of those elements grew too large, and became their own blocks. We would now have:

    _door.scss
    _floor.scss
    _house.scss
    _roof.scss
    _wall.scss
    _window.scss

Now that our _house.scss block has been completely compartmentalised, we leave comments to sign-post where all of the parts went.

This is just helpful for other developers, and it keeps a nice map of our blocks without enforcing deeper folder structures.

At the top of _house.scss, we document this in comments:

/*

    .house

        @__roof.scss
        @__wall.scss
        @__window.scss
        @__door.scss
        @__floor.scss

*/

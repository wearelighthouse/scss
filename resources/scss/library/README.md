----

** What is the library for? **

Each project has a series of common features - these are often the foundations of the design.

Typographic scale, grid systems, margins and padding, these are all fundamental to a website's styleguide.


----

** What can I include in these partials? **

You can build partials in this order:

    $variables

    @mixins

    .classes


Variables come first (because they're needed first, and don't require anything else)

Mixins are related to the file's features (like a font-scale() mixin for _typography.scss).


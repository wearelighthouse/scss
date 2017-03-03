----

** What is CONFIG for? **

Strictly for global variables. Not _every_ variable available, just the long lists that get used all over the project.

----

** Why isn't this called VARIABLES? **

We're trying to not use literal SASS/SCSS terminology here, as we're talking more about /how/ we construct the project.

However, our BEM methodology is enforced in the /blocks/ folder, because they are one and the same.

----

** What about Maps? **

We've experimented with maps for lists of values before - but using map-get everywhere SUCKS.

Maps are for looping and that's that kid.

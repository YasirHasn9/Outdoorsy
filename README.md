# Outdoorsy

# what happens when the user opens a page in the browser ?

The browser will load the html and than parse it. Then the browser builds the DOM (Document object model)
that represents the look of our website. when the browser parse the html at that point , it will load the css.
after loading the css it will parse as well . parsing css is kinda more complex that than the html. parsing css requires
2 processes , one called cascade which will resolve the conflicts between different css rules and declaration and another process for
finding the finals values for the declared properties. after this the css will be stored in the CSSOM (css model model)

DOM + CSSOM = Render Tree

# How does the css know what rule to apply when there different values for the same selector ?

css will take into account the importance of the declarations in order

1. user !important declaration
2. author !important declaration
3. normal author declaration
4. normal declaration
5. browser default declaration

## example

.button {
background-color:blue !important
}

.nav .other-selector .button {
background-color:red
}

# they are both author declaration , but which one will apply to the .button ?

this --> background-color:blue !important
because the !important, which it gets property precedence

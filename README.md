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

## How can css determine which value get to be precedent ?

it determines this through specificity, which means the rule that would apply depends on the order (which property or selector to be preceded )

1. inline
2. ids
3. class , pseudo class , attribute
4. element and pseudo elements

# using the !important in your style overrides even the inline style and cant be overridden unless another property been specified with another !important

## How to calculate the specificity of css ?

inline , id , classes/pseudo classes , element = (0 , 0 , 0 , 0)

.button {
....
} = (0 , 0 , 1 , 0)

nav#nav div.pull-right .button {
....
} = (0 , 1 , 2 , 2) we have 2 classes(pull-right and button ) and 2 elements (nav and div)

a {
...
} = (0 , 0 , 0 , 1)

#nav a.button:hover {
...
} = (0 , 1 , 2 because we have 1 for class .button and the hover pseudo class)

**_ note _**
rely on specificity more than the order unless we are using a third party lib

#How css values are processed?
you need to know the 6 steps

1. Declared value which is usually written by the author
2. cascaded value; order
3. specified value ; defaulting value if there is no cascaded value
4. computed value where the engine converts the relative values to absolute
5. used value: final calculations based on layout
6. actual value which is the browser and device restrictions

#example

html

<div class="box">
   <p class="sub-box"> something here </p>
</div>

style
.box {
width:280px;
font-size: 1.5rem;
background-color:green;
}

p {
width:140px;
background-color:red
}

.sub-box {
width:140px;
}

p and sub-box have some conflicts on the width
which one should be applied

1. Declared value
   140px;
   80%
   which value gets to be cascaded ?

2. cascaded value is 80% because the class has more powerful and more specified than the p
   and the cascaded value of the browser is 16px or 1.5 rem
   and this would the specified value of there is no value

3. specified value which is the initial value in case there is no value but here we have the 80%

4. computed value which transform the value to its pixels

the computed font-size to section is 24px ?
because rem = 1.5 \* 16px = 24px

5. used value --> final calculation
   80% X 280px == 224 px

6. actual value
   224px submits to the restrictions of the browser and it may be 223px or 224px

# what do you need to know

1. each property has an initial value if there is no value specified.
2. percentage and relative value are converted to pixels.
3. the browser specify the root's font-size and usually is 16px
4. percentage are measured relative to their parent's font-size, if used to specify font-size.
5. percentage are measured relative to parent's width , if used to specify length.
6. em are measured relative to their parent's font-size, if used to specify font-size.
7. em are measured relative to their current element's font-size, if used to specify length.
8. rem is always measured to the document's root font-size.
9. vh and vw are percentage measured to viewport of the browser.

### why would we want to size stuff using em or rem ?

because we can build a robust responsive layout that we can change the length of of elements by changing the font-size
and this adds a lot of flexibility.

**_ note _**
always remember that every property must have a value even if neither we , the developer nor the browser do specify
so in that case there is no cascaded property.

# Think about the layout

components should be
a. modular
b. reusable
c. independent

for easy debugging and maintains we should divide our app for several component . that would help us to easily spot the error and also save us time for the future growth.

BEM = block element modifier

Architect
we are going to use the method of the 7-1 pattern, which is gonna use different css file for each portion of our app and at the end we are gonna make main file to import all other files into a compiled css stylesheet

7 folders
base/ basic product definition
components/ one file for each component
layout/ define the overall layout of the project
abstracts/ variables and mix-ins , not out put
vendors/ third party
themes/ in case we want to visual different themes
pages/ style for specific page of the project

# now we are gonna use sass

sass is just an extension to css (preprocessor) to play with sass you need a complier to translate(compile) a sass code to the real css under the hood
and also you need you know how to play with npm

# so what is Sass ?

sass is a css preprocessor, an extension that adds elegance and power to the css

sass code ----> |**_compiler_**| =) compiled css code

so we are using sass to fix the problems that we have with css because css gets messy very quickly.

sass provide for us a handy tools that css simply dont have

so we are gonna write sass code instead of css code in sass file extension

first: we need to process our sass code in a processor before we compile it a real css code and this is why we called preprocessor

**_ note _**
the browser has no idea that we are using the sass code because the browser sees only the final code which is the css

what feature that sass will provide for us ?

1. variables : you can use them for colors , font-sizes and other
2. Nesting : you can nest elements inside other
3. operators: math
4. Partials and import: write sass code in one file and you can use in different file by importing them
5. mixins : you can write reusable css code
6. function : similar to mixins with the difference that they can produce value can be used
7. extends: to make different selectors inherit declaration that they are common

# how to declare a variable ?

\$var-name : value;

using semicolon (;) in sass is important

**_ ampersand = & _**

the npm packages that we are using for the purpose of development which means that only used by developers are saved in the devDependencies which means theses are only tools for the development.

# npm install packageName --save-dev

notice that i have created a new file .gitignore. In this file we can include all the the files that we dont want to be on our repo on github

how to compile sass ?
in the package.json file , there is an object called "script" {
inside here we can write the command that would compile sass into
regular css code .

here gonna write our command
compile:sass: node-sass + the path of the file that we want to compile (input file (sass) ) + the path of the file that we want to go to the world and what the browser can read (output file (css))
}

# now how run sass on your machine so the browser can read?

easy by just run the commend line the terminal
**npm run compile:sass** Bum , enjoy it ;).

Engineers don't like to repeat what can be done once because either we are lazy or we consider it not practical. In sass when we update something we need to tell the compiler to compile it but this would be so tedious for us to do so , i mean every time ? come on. in package.json file under the same script object we can write something called a flag indicates the compiler to update itself every time we make some changes to our sass files
"script" {
compile:sass: pathInputFile pathOutputFile -w  
}

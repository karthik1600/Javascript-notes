```css
<style link="app.css>
```
```css
h2{
    color:purple;
    color:rgb(0,34,34);
    color: hexadecimal
    background-color: red
}
```
`text-decoration`
```css
h1{
    text-align:right;
    font-weight:normal;
    text-decoration: wavy overline lime;

}
```
- wavy -style
- overline - line type
- lime - line color

---
`line height`
```css
h1{
line-height: 23;
letter-spacing:3;
}
```
- lineheight- text height
- letterspacing- spacing between letters

---
[font-size](https://developer.mozilla.org/en-US/docs/Web/CSS/font-size)

---
`font-family`
```css
h1{
font-family:verdana,ariel;
}
```
- ariel is backup if verdana not present

---
# Selectors
universal selector
```css
* {
color:black;
}
```
ele selector
```css
h1 {
color:black;
}
h2,h3 {
color:black;
}
```
id selector - only one ele 
```html
    <input type="text" id="search">
```
```css
#search{

}
```
class selector - for more than one ele with same style
```html
<span class="orange">orange text</span>
```
```css
.orange{
color:orange;
}
```
descendent selector
```html
    <span>
        <a href="www.google.com">google</a>
    </span>
```
```css
span a{
    color:teal;
}
.post a{   //inside class

}
```
- only <a> inside span
\
adjacent selector
```html
    <h1>kdskfdmsd</h1>
    <p>PARA</p>
    <p>PARA 2</p>
```
```css
h1+p{

}
```
- selects only para after h1
\
direct child
```html
    <div>
        <li>mksdmksd</li>
    </div>
```
```css
div>li{

}
```
```
- selects only <li> that are direct children of <div> 
```
---
attribute selector
```html
    <input type="text">
    <input type="password">

```
```css
input[type="password"]{
    color:red;
}
section[class="SSS"]{

}
a[href*="google"]{
    color:magenta;
}
```
- selects only input of type password
- selects only section of class SSS
- [attribute selectors](https://developer.mozilla.org/en-US/docs/Web/CSS/Attribute_selectors)
---
# pseudo classes = [mdn](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-classes)
hover - changes when cursor falls on ele
```css
a:hover{
    color: orchid;
}
```
[:checked](https://developer.mozilla.org/en-US/docs/Web/CSS/:checked) for radio button and checkbox

---
nth of type
```css
/* Odd paragraphs */
p:nth-of-type(2n+1) {
  color: red;
}

/* Even paragraphs */
p:nth-of-type(2n) {
  color: blue;
}

/* First paragraph */
p:nth-of-type(1) {
  font-weight: bold;
}

```
Pseudo elements
```css
/* 

selector::pseudo-element {
  property: value;
}
The first line of every <p> element.
*/
p::first-line {
  color: blue;
  text-transform: uppercase;
}
/* Make selected text in a paragraph white on a blue background */
p::selection {
  color: white;
  background-color: blue;
}
```
[pseudo elements](https://developer.mozilla.org/en-US/docs/Web/CSS/Pseudo-elements)

---
# CASCADE
## order
```css
h1{
    color:red;
}
h1{
    color:violet;
}
/* violet wins */
```
## Specificity
- the browser selects the style which is more specific
```css
p{
   color: red;
}
section p{
    color: yellow;
}
/*section p more specific so wins out*/
```
`inline styles > id selector  >  class selector > element selector`

[specificty-calculator](https://specificity.keegan.st/)
```css
p{
    color: yellow !important;
}
```
- dont use inline and important
---
# Inheritance
```css
body{
    color:red;
}
/* everything in body inherits */
```
- some dont inherit by default
```css
button,input{
    color: inherit;
}
/* nearest parent*/
```
---
# CSS BOX MODEL

![](https://www.csssolid.com/images/box-model/css-box-model.png)
\
[slide](resource\_WDB+CSS+Box+Model.pdf)

---
# [BORDER](https://developer.mozilla.org/en-US/docs/Web/CSS/border)
This property is a shorthand for the following CSS properties:
- border-color
- border-style
- border-width
```css

/* width | style | color */
#one{
border: medium dashed green;
}
```
## [box-sizing](https://developer.mozilla.org/en-US/docs/Web/CSS/box-sizing)
- `content-box` gives you the default CSS box-sizing behavior. If you set an element's width to 100 pixels, then the element's content box will be 100 pixels wide, and the width of any border or padding will be added to the final rendered width, making the element wider than 100px.
- `border-box` tells the browser to account for any border and padding in the values you specify for an element's width and height. If you set an element's width to 100 pixels, that 100 pixels will include any border or padding you added, and the content box will shrink to absorb that extra width. This typically makes it much easier to size elements.
```css
h1{
    box-sizing: content-box;
}
```
```css
#one{
    height: 200px;
    width: 200px;
    border-style: solid;
    border-color: tomato;
    border-width: 5px;
    background-color: violet;
    box-sizing: border-box;
    border-radius: 10px; /* rounding corners*/
}
```
---
# [Padding](https://developer.mozilla.org/en-US/docs/Web/CSS/padding)
```css
/* Apply to all four sides */
padding: 1em;

/* vertical | horizontal */
padding: 5% 10%;

/* top | horizontal | bottom */
padding: 1em 2em 2em;

/* top | right | bottom | left */
padding: 5px 1em 0 2em;
```

---
# [Margin](https://developer.mozilla.org/en-US/docs/Web/CSS/margin)
```css
/* Apply to all four sides */
margin: 1em;
margin: -3px;

/* vertical | horizontal */
margin: 5% auto;

/* top | horizontal | bottom */
margin: 1em auto 2em;

/* top | right | bottom | left */
margin: 2px 1em 0 auto;
```
---
# DISPLAY
![](https://i.stack.imgur.com/mGTYI.png)

---

# UNITs
## Percentage
```css
#one {
  height: 200px;
  width: 200px;
  background-color: black;
}
#two {
  height: 50%;
  width: 50%;
  background-color: red;
}
/* 50 % of parent */
h1{
  font-size: 40px;
  line-height: 200%;
}
/* double the font size */
```
## em
- Relative to the font-size of the element (2em means 2 times the size of the current font)
## rem 
- font size is relative to root element only and 1rem is same throughout whether it is a child of element or not

---
# Alpha Channel and opacity - transperancy
```css
#rgba{
  background-color: rgba(255,245,234,0.7);/*0 to 1 - affects only backgroung 
  color not any other ele*/
}
#opacity{
  opacity: 0.3;/* affects the whole ele and all ele inside it*/
}
```
---
# [Position](https://developer.mozilla.org/en-US/docs/Web/CSS/position)
- relative
- static
- absolute
- sticky
- fixed
---
# [Transition](https://developer.mozilla.org/en-US/docs/Web/CSS/transition)
```css
div{
    height:10px;
    width:20px;
    background-color:red;
    transition: margin-right 2s ease-in,color 1s;/* time taken for each transition*/
    transition-timing-function: ease-in;/* speed or change */
}
div:hover{
    margin-right:30px;
    color:blue;
}
```
## [Transition functions](https://developer.mozilla.org/en-US/docs/Web/CSS/transition-timing-function)

---
# [Transform](https://developer.mozilla.org/en-US/docs/Web/CSS/transform)
- animations
```css
#one:nth-of-type(1){
  height: 200px;
  width: 200px;
  background-color: black;
  margin: 20px auto;
  transform: rotate(180deg);  /* rotaate element */
  transition: 1s;
}
#one:nth-of-type(2){
  height: 200px;
  width: 200px;
  background-color: black;
  margin: 20px auto;
  transform: scale(0.5);
  transition: 1s;
}

#one:nth-of-type(2):hover{
  transform: scaleX(1);/* increase or decrease size */
}
#one:nth-of-type(3){
  height: 200px;
  width: 200px;
  background-color: black;
  margin: 20px auto;
  transform: translateX(50px);/* move in x direction*/
  transition: 3s;
}

#one:nth-of-type(3):hover{
  transform: translateX(-50px);
}
```
---
# [Background](https://developer.mozilla.org/en-US/docs/Web/CSS/background)
- background-attachment
- background-clip
- background-color
- background-image
- background-origin
- background-position
- background-repeat
- background-size
---
# font family
- google font
---
```css
img{
    widtht: 30%;
    margin: calc(10%/6);
}
```
---
# Flex
```css
#container {
    background-color: #003049;
    width: 90%;
    height: 500px;
    margin: 0 auto;
    border: 5px solid #003049;
    display: flex;
    flex-direction: row; /*bottom to up*/
    flex-wrap: wrap;
}
```
## [justify-content](https://developer.mozilla.org/en-US/docs/Web/CSS/justify-content)
 spacing between

---
# [flex-wrap](https://developer.mozilla.org/en-US/docs/Web/CSS/flex-wrap)
- opp of flex direction if flex direction horizontal flex wrap top to bottom and if flex direction vertical flex wrap left to right
- - if space needed for elements are greater than the space needed
---
# [align-items](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Aligning_Items_in_a_Flex_Container)

- very important check link
- works on cross axis while justify content workks on major axis
![](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Flexible_Box_Layout/Aligning_Items_in_a_Flex_Container/align1.png)

---
# [align-content](https://developer.mozilla.org/en-US/docs/Web/CSS/align-content)
- align-content property sets the distribution of space between and around content items along a flexbox's cross-axis or a grid's block axis.
```css
.container{
    flex-direction:row;
    justify-content: space-evenly;
    align-items: flex-start;
    align-content:space-evenly
}
```
---
# [align-self](https://developer.mozilla.org/en-US/docs/Web/CSS/align-self)
- for individual elements
- The align-self CSS property overrides a grid or flex item's align-items value. In Grid, it aligns the item inside the grid area. In Flexbox, it aligns the item on the cross axis.
![](img\Untitled.png)

---
# [flex-basis](https://developer.mozilla.org/en-US/docs/Web/CSS/flex-basis)
- defines the initial size of ele through the major axis
```css
#container div {
  height: 200px;
  width: 200px;
  text-align: center;
  flex-basis: 200px;
}
```
- each ele will be 200px wide if major axis is through x coordinate

---
# [flex-grow](https://developer.mozilla.org/en-US/docs/Web/CSS/flex-grow)
- controls the amount of left over space each ele can take 
- if more than one ele has flex grow eg one has 1 and other has 2 so the left over space will be split
in ratio 1:2
```css
div:nth-of-type(1){
      flex-grow: 1;
}
div :nth-of-type(3) {
  flex-grow: 2;
}
```

---
# [flex-shrink](https://developer.mozilla.org/en-US/docs/Web/CSS/flex-shrink)
- If the size of items is larger than the flex container, items shrink to fit according to flex-shrink.
```css
div:nth-of-type(1){
      flex-shrink: 1;
}
div :nth-of-type(3) {
  flex-shrink: 2;
}
```
- here one will shrink twice as the other one if it is bigger than container

---
# flex shorthand
```css
h1{
  /* One value, unitless number: flex-grow
flex-basis is then equal to 0. */
flex: 2;

/* One value, width/height: flex-basis */
flex: 10em;
flex: 30%;
flex: min-content;

/* Two values: flex-grow | flex-basis */
flex: 1 30px;

/* Two values: flex-grow | flex-shrink */
flex: 2 2;

/* Three values: flex-grow | flex-shrink | flex-basis */
flex: 2 2 10%;
}
```
---
- `min-width` and `max-width` can be used to control grow and shrink in flex

---
# [media queries](https://developer.mozilla.org/en-US/docs/Web/CSS/@media)
- With it, you specify a media query and a block of CSS to apply to the document if and only if the media query matches the device on which the content is being used.
```css

@media(min-width:600px){  
    h1{
        color: blue;
    }
}
@media(max-width:500px){
    h1{
        color: red;
    }
}
/* adding 2 mediaqueries*/
@media(min-width:300px) and (max-width:600px){  
    h1{
        color: blue;
    }
}
```

- orientation
```css
@media(orientation:landscape){
    h1{
        color: red;
    }
}
```
---
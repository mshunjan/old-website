--- 
title: A Tutorial on Web Development
date: 2020-07-27 12:11 -0400 
category:  computerscience 
---

# A note on this tutorial
This is half inspired from my own learning and mostly a reccounting of the following resource: https://developer.mozilla.org/. I suggest going to this website for your first time learning web development. 

# Let's Talk HTML
Hypertext Markup Language is a markup language (not a programming language), meaning it is used to define how your website is structured. The anatomy is very simple essentially consisting of multiple HTML elements. Here's an example of an HTML element:

```html
<p> This is an HTML element </p>
```

Let's breakdown the above code. We have our opening tag being <p>, the closing tag </p> and the content in between. This format pretty much applies to almost every HTML element. Exceptions can be like the img element, which is called an empty element, as it lacks content.What I want to get across is that HTML is easy, and as long as you understand some core concepts, such as how an HTML element functions, you will have no problem writing HTML code. 

So now that we understand what an HTML element is, you might wonder how this concept is applicable. Well a big feature is something referred to as nesting, where HTML elements can be in other HTML elements:

```html
<p> This is a <i>nested</i> HTML element </p>
```

# A typical Document
For our first html document, let's create a file called index.html 

```html
<!DOCTYPE html>
<html>
  <head> 
    <meta charset="utf-8">
    <title>HTML format</title>
  </head>
  <body>
    <p> Here's a sentence in a paragraph element </p>
    <img src="myimage.jpg" alt="Example image">
  </body>
</html>
```
The `<!DOCTYPE html>` is a declaration informing the web browser about the type of document it is handeling. There's not much you need to know beyond that it makes our document work properly. Next up we have the `<html> content </html>` element. The root element, it contains all of our html code. You may have noticed that our code starts to form this sort of hierarchy, where elements embed into each other. Thus you can think of html as a sort of tree, with branches that split into smaller branches. Elements you will alwas be using are the head and body elements. The head element contains metadata, css and formatting declarations such as the charset attribute in the above code. It's purpose is to include all symbols currenty present in UTF-8. The body element however contains what is actually displayed on our page, such as images, videos and content. 

# Some important Elements

Here's some stuff you'll be using quite a lot during development. Headings, which go from 1 to 6 and are represented by the tag "h#" and its corresponding closing tag. Paragraphs are exactly how they sound and are enclosed by the "p" tag. An important empty element you will use quite often is the anchor tag "a". Check out the code below for an example:

```html
<a href="https://www.randomwebsite.com"> Link Name </a>
```

# Onto CSS
Cascading Style Sheets or CSS is another markup language, however it is used to style our HTML elements. Think of HTML code as our construction work, versus the CSS as the designing. There are several methods for implementing CSS. You can do it externally in a style.css file then point to it in the HTML file, you can do it in the header of an html to apply it to the entire html body, or you can even do it individually for each html element. The choice is yours. 

Say we want all of our CSS to be external, we can do something like this in example.css

```css
p {
    background-color: red;
}
```

Then to use this style sheet, we use our anchor tag in our head  tag, but instead replace the href with the location of the style.css file and add an additional attribute called rel, which defines the type of relationship for the link. For linking the stylesheet, the value will be exactly that... "stylesheet". 

Now you would only do this on a big project, but for the purposes 

# The CSS code design

Let's look back at this code

```css
p {
    background-color: red;
}
```

You'll notice 4 distinguishing portions to this code. First we have our selector, indicating what html element we are manipulating. In this case, it is the paragraph element. Then we indicate what we want to do with it using the property "background-color" and a value for the property. This statement ends with a a semicolon, and you can keep adding statements within the curly braces. Let's say you want multiple html elements to have the same background color. Isn't it annoying to repeat this code design over and over again? Well we can just extend the ruleset applied to the selector by adding more selectors via the notation "p, selector1, selector2, ... selectorn {}". On the topic of selectors, you aren't limited to just HTML elements, but you can also do much more, for example a specifc id for a specific element. 

# Some CSS goodies
Here are some core concepts you may use over and over again. For example, what if we want a different font? Well we can use the font-family property and our value as our selected font. Spacing can be controlled via the padding and margin property.
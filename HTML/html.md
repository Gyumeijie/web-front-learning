# HTML Entities
There are some characters that have special meaning in HTML document-the obvious ones being
the < and > characters. You will sometimes need to use these characters in you content without
wanting them to be interpreted as HTML. To do this, you use ***HTML entities***.

An entitiy is a code the browser substitutes for the special character. Each special character
has an **entity number** that you can include in you content to represent ther character. But not
every special character has a  corresponding **name entity**; generally speaking the more popular 
speicail characters have a name.

More information:
- [numeric character reference](https://en.wikipedia.org/wiki/Numeric_character_reference)
- [character entity reference](https://en.wikipedia.org/wiki/List_of_XML_and_HTML_character_entity_references)


# HTML Attributes

In html, each element has some attributes and we can **configure** elements using attributes. And
each attributes can fall into the following two categories:

1. ***local attributes***

By it very name, these attributes are defined by each element locally and give the author the ablility to 
control some aspect of the unique behavior of an element.

2. ***global attributes***

Similarly, These **configure** the behaviour that is common to ***all*** elements. You can apply every
global attribute to every element, **although this doesn't always lead to meaningful or useful behaviour 
change.**

# What is the difference between properties and attributes in HTML?

When written HTML source code, you can define ***attribute*** on your HTML elemnets. Then, once the browser
parses your code, a corresponding DOM node will be created. This node is an object, and it has ***property***.

For example, the following HTML element:
```html
<input id="input" type="text" value="default">
```
has three **attribute**(```id```,```type``` and ```value```).

Once the browser parses this code, a **HTMLInputElement** object will be created, and this object contains dozens 
of properties like: accessKey, alt, attributes, checked, and so on.

For a given DOM node object, **properties** are the properties of that object, and attributes are the elements of
the ```attribute``` property of that object.

When a DOM node is created for a given HTML element, many of its properties relate to attributes with the same or
similar names, but it is not always a one-to-one relationship. Again take as example the HTML Element above:
```html
<input id="input" type="text" value="default">
```
the corresponding DOM node will have ```id```, ```type``` and ```value``` properties:
- The ```id``` property is a ***reflected*** property for the ```id``` attribute: Getting the property reads the 
attribute value, and setting the property writes the attribute value. ```id``` is a **pure** reflected property,
it doesn't modify or limit the value of the ```id``` attribute.

- The ```type``` property is a ***reflected*** property for the ```type``` attribute: Getting the property reads
the attribute value, and setting the property writes the attribute value. But ```type``` isn't a pure reflected
property, because it's limited to known values(e.g., the valid types of an input). If you had ```<input type="foo">```,
then ```theInput.getAttribute("type")``` gives you ```"foo"``` but ```theInput.type``` gives you ```"text"```.

- In contrast, the ```value``` property doesn't reflect the ```value``` attribute. Instead, it's the ***current***
value of the input. When the user manually changes the value of the input box, the ```value``` property will reflect
this change.

There are several properties that directly reflect their attribute (```rel```, ```id```), some are direct reflections with slightly-different names (```htmlFor``` reflects the ```for``` attribute, ```className``` reflects the ```class``` attribute), many that reflect their attribute but with restrictions/modifications (```src```, ```href```, ```disabled```, ```multiple```), and so on. The [spec](https://www.w3.org/TR/html5/infrastructure.html#reflect) covers the various kinds of reflection.

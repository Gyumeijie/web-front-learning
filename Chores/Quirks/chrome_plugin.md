# Chrome plugin may modify your html source

Take the following html code sinppet as an example: 
```html
<!DOCTYPE HTML>
<html>
<head>
    <title>Example</title>
    <meta charset="utf-8"/>
</head>
<body>
    the first line
    <p>the second line</p>
</body>
</html>
```
the expected result, in normal case, is that the browser will print "the first line" and "the second line" in order, but if you
have a [JSON View](https://chrome.google.com/webstore/detail/json-viewer/gbmdgpbipfallnflgajpaliibnhdgobh) plugin installed, something
strange happend, the display order of the two lines was reversed, the style of the first line, too, was changed. The following is the 
corresponding structure of DOM:
```html
<!DOCTYPE HTML>
<html>
<head>
    <title>Example</title>
    <meta charset="utf-8"/>
</head>
<body>
    <p>the second line</p>   
    <pre>
      the first line
    </pre>
</body>
```
By turning off the ```JSON Viewer```, I got my result back.

# Conclusion

If you find something strange with your html, and you may have tried lots of ways but it still doesn't work, then you can take
the ```the effect of plugins``` into consideration, try to turn off pulgins and see whether you can got expected result.

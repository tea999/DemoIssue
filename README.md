# DemoIssue
demo the issue for auto size textview

The problem is when you use change line symbol(\\n) in the end of text.

Ex.
```
This is my content.\n
```

It will cause the problem.
This is because source line 83
```
if (i < lineCount - 1 && end > 0 && !isValidWordWrap(text[end - 1], text[end]))
```

"end" is the end index of each line in the orginal text.

In the above example,lineCount will be 2,text length will be 20,and the first line end will be 20.

The line count will add one because of \\n.The line before the end line will be the real last line including characters,and its line end will equal to the text length.
So when this situation(The line before last line) meets the line83,it will pass the first two conditions to the isValidWordWrap function,which contains the second parameter text[end].
It equals to text[length].Apparently this expression will simply cause StringIndexOutOfBoundsException.

I checked the source and found that in fact isValidWordWrap is only used here,and the source is 

```
fun isValidWordWrap(before: Char, after: Char): Boolean {
        return before == ' ' || before == '-'
    }
```

You can find out that the second parameters is never used,so I just remove the second paramater reference,and avoiding this issue.

+++
date = "2017-04-13T12:15:22-04:00"
description = ""
draft = false
slug = "post-formatting"
tags = []
title = "Post Formatting"
topics = []

+++

Let's try a few different formatting options to see how it carries over to [WillowBot](http://www.willowbot.com/).

## List

* Item 1
* Item 2
    * Sub-item 2-1
    * Sub-item 2-2
* Item 3

## Numbered List

1. Item 1
2. Item 2
    1. Sub-item 2-1
    2. Sub-item 2-2
3. Item 3

## Quote

> “There are two great days in a person’s life — the day we are born and the day we discover why.” ~ William Barclay

## Code Block

```
int main( void )
{
  /* Board-specific setup */
  BSP_init();
  
  // start system timer tick
  timer_start();
  
  LOOP_FOREVER
  {
     // TODO: add real logic here
     ;
  }
  
//  return 0;
}
```

## Text Formatting

This is some silly sentence to see if *italic*, **bold**, _underline_, ~~strikethrough~~, and highlighting work.
Horizontal rule below:
* * *

## Table 

Table Version 1

row 1, col 1	row 1, col 2	row 1, col 3	row 1, col 4
row 2, col 1	row 2, col 2	row 2, col 3	row 2, col 4
row 3, col 1	row 3, col 2	row 3, col 3	row 3, col 4
row 4, col 1	row 4, col 2	row 4, col 3	row 4, col 4
row 5, col 1	row 5, col 2	row 5, col 3	row 5, col 4
row 6, col 1	row 6, col 2	row 6, col 3	row 6, col 4

Total failure here :-(

Need to come up with something better.

Table Version 2

col 1	| col 2	| col 3	| col 4
---|---|---|---
row 1 col 1	| row 1 col 2	| row 1 col 3	| row 1 col 4
row 2 col 1	| row 2 col 2	| row 2 col 3	| row 2 col 4
row 3 col 1	| row 3 col 2	| row 3 col 3	| row 3 col 4
row 4 col 1	| row 4 col 2	| row 4 col 3	| row 4 col 4
row 5 col 1	| row 5 col 2	| row 5 col 3	| row 5 col 4
row 6 col 1	| row 6 col 2	| row 6 col 3	| row 6 col 4

Much better now with CSS for formatting.

## Links

External link to [Google](http://www.google.com/).

Internal link to [another post]({{< relref "post/2017/0412-hello-world/hello-world.md" >}}) on [WillowBot](http://www.willowbot.com/) is handled via special `relref` tag.

```
to [another post]({< relref "post/2017/0412-hello-world/hello-world.md" >})
```

Note, a set of {} missing from the above syntax. So internal link really has the format of {{link stuff}}.

## Image

![Image](/post/2017/0413-post-formatting/post-formatting.png)

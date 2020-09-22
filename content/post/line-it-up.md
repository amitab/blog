+++
title = "Line up all the things!"
date = "2015-10-26T18:00:00+07:30"
tags = ["flex", "css"]
draft = false
author = "Amitabh Das"
+++

If you are a Web Designer, at least once in your life, you must have come across a situation where you needed to line up elements side by side, or tried to understand why each element behaves the way it does while lining them up. Here are a list of various ways to achieve this goal.

## Float
```
.container {}
.container .items {
    float: left;
}
.container .clarity {
    clear: both;
}
```

Oldest solution in the book. All we need to do is set the elements we need aligned to float either left or right depending on the look we desire. Of course the container element collapses, but we can work around that with hacks.

Add another element at the end of the floating elements with a style property – clear: both; Now that we have a non-floating element in the container, the container stretches itself to accommodate it. This results in a useless HTML element, but hey – it works!
Add the following styles to the container – overflow: auto; width: 100%; Although it may be a little tricky depending on how your custom css styles work.

## Display Inline
```
.container {}
.container .items {
    display: inline;
}
```
Another great solution. This is the default value of all elements unless your stylesheet states otherwise. It allows other elements to sit on their left and right. No extra HTML elements and no clearing the container. But its not without a few disadvantages:

It ignores any top, down values for margins and paddings. But depending on how you plan your layout, you can delegate the top and down values for margins and paddings to the container.
It cannot have width and height values.

## Display Inline-Block
```
.container {}
.container .items {
    display: inline-block;
}
```
Display block allows height and width values to be set, accepts top and down values for margin and padding unlike display inline, and forces a line break after itself to not allow elements to sit next to it.

And you’ve just read about display inline.

Now combine the desirable factors of both, and you’ve got inline-block!

Inline-Block is just like a block element, except that it does not force a line break after itself, meaning that elements are allowed on its left and right sides.

There is one major weird problem with this. Depending on how you write your HTML code for this, you might see gaps between the lined up items. Solutions from the best to worst:

Remove the actual spaces between the elements in your HTML.
Drop the closing angle bracket to the next line
Skip the closing tag. HTML5 fixes it for you.
Negative margin. I really don’t like putting in unnecessary styles if I can’t help it.
Or you could try other methods of lining up elements?

```
<div class="container">
    <div class=”item”></div><div class=”item”></div><div class=”item”></div>
</div>
<div class=”container”>
    <div class=”item”></div
    ><div class=”item”></div
    ><div class=”item”></div>
</div>
<div class=”container”>
    <div class=”item”>
    <div class=”item”>
    <div class=”item”>
</div>
.container .item {
    display: inline-block;
    margin-right: -4px;
}
Flex
.container {
    display: flex;
    flex-direction: row | row-reverse; /* Based on the direction you want */
}

.container .item {
    flex-grow: 2;
    flex-shrink: 1;
}
```

One of the newer and most sophisticated method of specifying layouts of a webpage. Frankly, I think this is brilliant. I mean less js and more css, what’s not to love?

There are loads of parameters that can be adjusted to get the desired effect, but this post focuses on lining up elements side by side, so lets just do that.

# Styles for the container

```
display: flex; // Duh!
```

`flex-direction` specifies the direction you want the items to be lined up in. The value row means the items will be shown in the order written in HTML, and reverse-row is the opposite.
To be frank we are actually done. No more styles required on the item. But for a better effect, here are a few:

`flex-grow`: It takes an unitless positive integer which specifies, in proportion with the rest of the items, how much an item width can grow if there is enough space.  
`flex-shrink`: It takes an unitless positive integer which specifies, in proportion with the rest of the items, how much an item width can shrink if there is not enough space.

# Conclusion

There are newer CSS display methods coming up, for example the flexbox. Its a really powerful way to specify how flexible we want the layout to be. Less javascript and more CSS, whats not to be happy about? But it is implemented differently in different browsers and if browser support is your concern, you cannot be using this. But don’t worry, the other methods are more than suitable to get the job done along with better browser support.

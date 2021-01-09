---
layout: post
title:      "Dynamically Numbering Pages"
date:       2021-01-09 16:13:21 +0000
permalink:  dynamically_numbering_pages
---


Page numbers seem like such simple things. In theory, they are. The first page is "1", and each above that increments by 1 until the end of the book. Easy! Programmatically it is a simple case iterating through pages (likely elements in an array), and for each element rendering the index value + 1 (so as not to start with page 0). This is all well and good when rendering a complete book, but what happens when a new page gets added later? You could rerender the entire book, but the DOM already displays the previous pages. Why would you want to put your device through the strain of rendering everything all over again?

This problem had me stuck for longer than I care to admit. I build an app, Rails backend with a JavaScript frontend, to be a digitization of "Pass the Story", a classic creative writing game where each person in a group writes a single page of a story before passing it to the next person to write. By the time a person gets the story, several pages are already rendered on the DOM before they write and add their own page.

The process of adding a page is a complex one. At the end of the story's `<div>`  is a form to add a new page. 

```
 <form>
            <label>Your Username: </label>
            <input type="text" id="author"><br>
            <label>Content: </label>
            <textarea rows="4" cols="50" id="content"> </textarea>
            <input type="hidden" id="book_id" value="${book.id}"> <br>
            <input type="submit">
  </form>
```

This form has to collect all of the data to create a new Page in the database, including the foreign key of the Book it belongs to. This is handled through the hidden input field, which stores the id of the book already rendered on the page. On submission, an event listener interrupts the submission, sends the new page's data to the backend with a fetch request using a service class. In the backend, the rails Create route uses the information to create a new entry for the page in the database and then renders the Show page json, which is grabbed by the service class function and returned for use.

This json data is then used to instantiate a new JavaScript object, which can be rendered on the DOM. At this point, though, the data is no longer aware of the array of pages belonging to the Book on display. The code needs a new way to determine the proper page number.

What is a page number, really? At heard, it is a way to identify a page in a book, almost like an `id` attribute in HTML. By placing each page in a `<div id=page-/number/>`, I could use the `id` 's to determine the new page number. I didn't want to use just a number, as `id="5"` might get confusing, which is why I included "page".

To create the number of the new page as it was being added to the DOM, I placed a new `div` on the page

```
    let pageDiv = document.createElement('div')
    main.appendChild(pageDiv)
```

then grabbed the `id` of the previous div `pageDiv.previousElementSibling.id`, split it into parts `.split("-")`, and then grabbed the number at the end. This was still a string, so to get an integer to play with I was left with this wonderfully complex line of chained functions `parseInt(pageDiv.previousElementSibling.id.split("-")[1])`. This was great, but what if the new page is the first of a book? So I placed the whole thing in an `if else` statement, checked whether the first element of the id was "page", and if so, the new page is not the first and I can assign the monstrocity above +1 as `pageNumber`. Otherwise, `pageNumber` is set to 1. That value then gets sent to a Page class method as an argument, and that method renders the page.

It seems so simple looking back on it, but this problem had me stuck longer than anything else while building this app. The benefit, though, was that I got lots of experience with breakpoints and using Chrome's dev tool to explore the DOM while it was active. While I don't find it quite as intuitive as Ruby's `binding.pry` (with pry, I always know what code has run when it hits the pry, with breakpoints it is murkier to me), it was nice being able to step through the code with an interface friendlier than the command line.

All in all, this was an instructive build for me. I went in less confident than I have for my previous builds, but I feel like I leave having learned more than in the past. What's Next?

---
layout: post
title: Udemy - The Complete 2021 Web Development Bootcamp - Part 1 - Frontend Review
categories: webdev
permalink: pretty
---

Course URL: [https://ibm-learning.udemy.com/course/the-complete-web-development-bootcamp/learn/](https://ibm-learning.udemy.com/course/the-complete-web-development-bootcamp/learn/)

---

### Resources:

- Syllabus: [https://drive.google.com/file/d/1FARSa410Z8uQnaHUtHI0V7XwAFT0F0lY/view?usp=sharing](https://drive.google.com/file/d/1FARSa410Z8uQnaHUtHI0V7XwAFT0F0lY/view?usp=sharing)
- Resourcs: [https://www.appbrewery.co/p/web-development-course-resources/](https://www.appbrewery.co/p/web-development-course-resources/)
- 12 Rules to Learn to Code: [https://drive.google.com/file/d/1XWbjAY2uz-a_72HV6gRmq5tPMz4U1UVZ/view?usp=sharing](https://drive.google.com/file/d/1XWbjAY2uz-a_72HV6gRmq5tPMz4U1UVZ/view?usp=sharing)

---

### HTML Notes

#### Meta tags

- Go in the <head> of the document
- Meta tags provide additional information, which helps search engines.
  - meta charset helps ensure your website is rendered correctly and avoids mojibake
    - Mojibake - lit. character transformation; is the garbled text taht is the result of text being decoded using an unintended character encoding.
  - `<meta name="description" content="This is a description">` is displayed in search engine results
  - Other examples:
    - `<meta name="keywords" content="HTML, CSS, XML, Javascript">`
    - `<meta name="author" content="John Doe">`
    - `<meta name="viewport" content="width=device-width, initial-scale=1.0">`

#### Style tags

- `<em>`
  - Style is italicise
  - Preferred over the `<i>` tag, which only styles but doesn't emphasize it.
- `<strong>`
  - Style is bold
  - Preferred over the `<b>` tag, which only styles but doesn't emphasize it.

#### Tables

```html
<table>
  <thead>
    <tr>
      <th>Dates</th>
      <th>Role</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>January 2020 - Present</td>
      <td>Associate Full-Stack Application Developer at GBS, IBM</td>
    </tr>
    <tr>
      <td>February 2018 - December 2020</td>
      <td>Youth System Program Representative, Philadelphia Works</td>
    </tr>
  </tbody>
  <tfoot></tfoot>
</table>
```

_Type emojis on mac with command + control + space_

#### Forms

```html
<form class="" action="mailto:email@email.com" method="post">
  <label>Your Name:</label>
  <input type="text" name="yourName" value="" />
  <label>Email:</label>
  <input type="email" name="yourEmail" value="" />
  <label>Your Message:</label>
  <textarea name="yourMessage" rows="10" cols="30"></textarea>
  <label>Do you want to sign up for the email list?</label>
  <input type="checkbox" name="" value="" />
  <label>Password</label>
  <input type="password" name="" value="" />
  <input type="submit" name="submit" />
</form>

<!-- Will generate an email set to mailto with the following populated in the
body: yourName=input 1 yourEmail=input 2 yourMessage=input 3 where the inputs
are what you input into the form before clicking "submit" -->
```

---

### CSS

- Browsers have default CSS applied, which you may have to override.
- When linking files, note that a leading forward slash means it is in the root of the project, while no leading forward slash means it is on the same level as the current document.
  - A relative reference that begins with a single slash character is termed an absolute-path reference. It ensures the path is absolute to the root directory and not the current directory (termed a "relative-path" reference).
  - The leading slash helps maintain consistency if you choose to change the structure of your site

#### Debugging CSS

- In Chrome dev tools, CSS is displayed in order of hierarchy. The topmost CSS takes precedence.
- The general order of CSS hierarchy is:

  - inline CSS
  - internal CSS (in a script tag in the document)
  - external CSS (stylesheet)
    - id selectors
    - class selectors
    - tag selectors
    - Finally, sort by order specified: if two declarations have the same weight, origin and specificity, the latter specified wins

- Pseudoclasses

  - e.g. `:hover`
  - Select the state of an element

- Add a favicon in the head

  - `<link rel="icon" href="favicon.ico">`

#### CSS box model

- Increasing the size of one box will move the other boxes
- Margins will not increase the size of the box, but will move it away from the other elements
- Even if the div has no height or width, it will have the amount of height or width needed to fit its contents

  - Padding around content will increase the size of the box
  - Padding does not impact the background or background image

#### CSS displays

- Block elements take up the entire width of the screen
  - e.g. paragraph tags
  - You can change height and width
- Inline display elements only take up as much space as it needs to
  - e.g. spans, img, and anchor tags
  - Note you cannot change the width of inline elements. If you want to, change display to block.
- Inline block
  - You can change width and height
  - They get displayed next to each other
- None display
  - Hides element and takes it out of the flow as if it never existed
- Hidden display

  - Hides element but keeps it in the flow

#### CSS Static and Relative Positioning

- Content determines initial size for all display types
- Order comes from code; display top to bottom in code
- HTML children will sit on top of the parent element on the z-axis

#### Position

- Static

  - All HTML elements are static by default.
  - By default, go along with HTML rules outlined above

- Relative

  - Reposition element from base static position
  - e.g. The below code will move the image right 30px from its natural position based on its right edge, so 70px of the 100px box is still visible and the rest is off screen.

  ```css
  img {
    position: relative;
    right: 30px;
  }
  ```

  - Position relative still takes up space where it originally was, even if visually you've moved it to a different location.
  - top: 50px is like adding a 50px margin to the top
  - bottom: 50px is like moving the box up off screen 50px

- Absolute

  - Reposition element relative to the parent element
  - e.g. The below code will move the image so it is 30px from the right edge of the parent

  ```css
  img {
    position: absolute;
    right: 30px;
  }
  ```

  - Position absolute takes the document out of the document flow. It no longer takes up any space.

    - top: 50px is also like adding a 50px margin to the top
    - bottom: 50px though will move the box 50px away from the bottom of the parent

    - Note if you move an element inside of another container and it is absolute to that container, the other elements absolute to the body will automatically move down out of the way of the container without formatting

- Fixed

  - Fixes the location of the element relative to the _browser window_.
  - The element doesn't move when you scroll

#### How to center elements with CSS

- Horizontally
  - If block or inline block:
    - text-align: center inside the element's parent
  - If inline, e.g. if you set a width:
    - set the element's margin left and right to auto

#### Fonts

- `font: Verdana, sans-serif;`
  - Specify the more specific font(s), then the fallback font family.
- Some fonts are web-safe, which means they are more likely to display correctly on browsers
- [https://www.cssfontstack.com](https://www.cssfontstack.com) tells you how common fonts are on operating systems
  - The site also presents fallbacks
  - e.g. Helvetica is installed on 100% of Macs but only 7% of Windows machines. Arial is a good fallback.
- To ensure everyone has the same viewing experience, you can load a font from google
  - Add the fonts you want
  - Paste the link into the head
  - Paste in the font(s) in the css

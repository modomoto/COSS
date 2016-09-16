# COSS

A great way to deal with a large CSS codebase.

__Component Oriented Style Sheets__

While building a large-scale application, it is really important to structure the CSS code in a maintainable way. But large-scale applications were once small applications and managing, mantaining and structuring the CSS was not a problem. There are methodologies for building CSS like BEM, OOCSS, SMACSS or Atomic. All of them are really helpful if implemented since the beginning but they are not really handy when it comes to CSS with no structure and counts more than 30 0000 lines.

In MODOMOTO we came up with the idea of __COSS - Component Oriented Style Sheets__ framework.

__COSS__ is a way of rethinking, refactoring and building our CSS code base using components. The basic idea is to wrap parts of our HTML into components with a meaningful name as the component, style them in a separate CSS file that has the same name of the component and that only contains styles declarations for it (including media queries if necessary).

COSS involves changes in our CSS, HTML and file structure and it requires SASS or LESS.

## PHILOSOPHY

The CSS must be preprocessed (SASS, LESS etc);

A component is a div with only one class and the class name is the name of the component;

The HTML should be split into re-usable components;

All the CSS rules of a component should be defined into a component file;

Pixels, Color Codes and Fonts Families should be defined by a variable with a meaningful name;

It is not allowed to define Pixels, Color Codes and Fonts Families in a component, variables/mixins should be used instead;

The basic structure of files in COSS is ```assets/stylesheets/components```, ```assets/stylesheets/variables/```, ```assets/stylesheets/mixins/``` and ```assets/stylesheets/base.sass``` (where the shared CSS rules of the project are defined);

When forcing the use of components, variables and mixins it is easier to make the css more consistent.

## HTML
  In every HTML document should be possible to identify part of code that can become or are small components. A small component can be a 'payment form', an 'header menu', a 'login sidebar', a 'pennant' etc. Each of them should be identified with a self explanatory name.

  The guideslines for creating HTML components are:

  1. The name of the component should indicate what the component does, how does it look like to easily identify them when browsing the code;
  2. Possibily the component should have be reusable, so should be the name;
  3. The name of it should not be too generic.


  ```HTML
  <!-- good -->
  <div class'sidebar_login'>
  </div>

  <!-- bad -->
  <div class'sidebar_login'>
  </div>

  <!-- goodish -->
  <div class'sidebar_login_homepage'>
  </div>

  ```

## FILE STRUCTURE

  components are defined only in ```assets/stylesheets/components```

  After defining our new component in our HTML, we can create a new file in the ```assets/stylesheets/components``` that has exactly the same name of the component. For instance if we take the component ```sidebar_login``` , we should have a file named ```assets/stylesheets/components/sidebar_login.sass```

  Mixins are defined only in ```assets/stylesheets/mixins```

  It is recommended to use CSS Mixins where possible to avoid code repetition and all the mixins should be placed in the mixin folder.For small projects you can start with one file containing all mixins, as that grows you can split it into multiple files grouped by context.

  Example:


  If we create a new mixin called border-radius, we should place it in a file in ```assets/stylesheets/mixins/border-radius.scss```
  ```SCSS
  @mixin border-radius($radius) {
  -webkit-border-radius: $radius;
     -moz-border-radius: $radius;
      -ms-border-radius: $radius;
          border-radius: $radius;
  }
  ```

  Variables are defined only in ```assets/stylesheets/variables```

  The only way to define pixels and color codes, font names, and images in our CSS is by using varibles and mixins.
  Variables can be placed only in the folder variables and it is recommended to place them grouped by file and context.
  ```assets/stylesheets/variables/colors.scss``` could contain all the color definitions, ```assets/stylesheets/variables/fonts.scss``` all the fonts variables, ```assets/stylesheets/variables/images.scss``` all the variables for the images used in our CSS.

  Example of a variable folder:

  ```Bash
  /assets/
    /variables/
      images.scss # containing all images (i.e. $background_blue: ('assets/background_blue.png') )
      colors.scss
      fonts.scss  # variable names of all the fonts used
      base.scss   # variables of common usage
  ```

## CSS
  Using COSS as a framework/styleguides requires the use of SASS, LESS, variables and Mixins in our CSS code. It is not necessary that our project already uses SASS but it is required to start using COSS.

  It is not allowed to use color codes or pixels in the CSS, only variables

  ```SCSS
   // bad
   font-size: 18px;

   // bad ($base is too generic for a variable)
   font-size: $base;

   // good
   font-size: $base-font-size;

   // good (component)
   .preauthorization
      margin-top: $margin-top-preauth
      padding: $padding-preauth
      font-family: $base-font-family

      .payment-cc-form
        background-color: $beige
        padding: $padding-highlight-box

      .payment-cc-form
        margin-right: $margin-right-payment-cc-form
        .payment-errors
          padding-bottom: $padding-bottom-payment-errors
        form
          margin: $no-margin

    // bad (component with pixels definitions)
   .preauthorization
      margin-top: 10px // bad, no pixel definitions oursize of a /variables file
      padding: $padding-preauth
      font-family: 'Arial' // bad, this should be a variable

      .payment-cc-form
        background-color: $beige
        padding: $padding-highlight-box

      .payment-cc-form
        margin-right: $margin-right-payment-cc-form
        .payment-errors
          padding-bottom: $padding-bottom-payment-errors
        form
          margin: 0px

  ```

  The media query of a compoenet is defined only in the components file

  __assets/stylesheets/components/sidebar_login.sass__

  ```SCSS
    // good
   .sidebar_login
    .login_link
    a
    .picture
   @media (max-width: $max-width-mobile)
    .sidebar_login
      .login_link
  ```

  __assets/stylesheets/components/sidebar_login.sass__
  ```SCSS
    // bad
   .sidebar_login
    .login_link
    a
    .picture
   @media (max-width: $max-width-mobile)
    .some_other_component
      button

  ```

  One component should contain only one class definition and its version by media queries, only nesting is allowed.

  __assets/stylesheets/components/sidebar_login.sass__

  ```SCSS
    // bad (.navigation_menu is at the same level of .sidebar_login,
    // should be defined in  assets/stylesheets/components/navigation_menu.sass)
   .sidebar_login
     .login_link
     a
     .picture
   .navigation_menu
     .active
      // something
   @media (max-width: $max-width-mobile)
    .sidebar_login
      .login_link
  ```

  __assets/stylesheets/components/sidebar_login.sass__
  ```SCSS
     // good
    .sidebar_login
      .login_link
      a
      .picture
   @media (max-width: $max-width-mobile)
    .sidebar_login
      .login_link
  ```

  __The use of important is not allowed and makes klingons cry.__ If there is the need to use ```!important``` it means that something is not well designed, we need to specialize more our component or ```!important``` is used in our context and it is very specialize. Instead of using ```!important``` in our component it is better to refactor other parts of our CSS where it is initially used.

## MIGRATION STRATEGY (if your project is a mess)


It is possible to create a new component from scratch if  the design introduces a new portion of HTML that we can identify immediately as a new component; but if the new design introduces changed to a part of a document that is not yet a component, the following strategy must be applied.

  1. Visit the page containing the potential component with a web browser and inspect the element
  2. Name the component and copy the styles currently applied to it into the newly created component without changing anything, without variables or mixins.
  3. Change the HTML class to make sure that now the styles are applied using our spartan component (and nothing has changed)
  4. Refactor the newly created component using the rules of COSS
  5. Check the old CSS class for the div and delete the old CSS rules applied to it if not used anymore anywhere else


The idea is that changing the CSS classes into something more meaningful and applying the rules of COSS, even with a lot existing CSS, the components allow to style everything they contain and which are only applied withing the component itself.


tip: you may consider using some visual comparison tool like wraith to make sure that your CSS changes are not breaking anything.

## FAQ

Q: __Can't find a good name for my variables__

A: It is not important to name variables with the best names since day 1. Their name can be changed over time with a simple 'search & replace'.

---

Q: __Why pixels, font names and color codes are only allowed in variables files?__

A: Forcing the use of variables pushes developers to be consistent and to find issue in the design or inconsistency. When you find yourself creating a new variable for a new font-size that was never used before, you can start questioning if that should really be a new variable or there is some other kind of issue for adding one more pixel to the base font size. The same goes for color codes and mixins.


---


Q: __Can't find a good name my component__

A: Think of what the component does, how does it look like or how it is used. Can you describe it with 2 words?

---


Q: __How long does it take to delete all the old CSS and move completely to COSS?__

A: The speed is a variable, and it is up to the developer(s). COSS is a transitioning framework and the final end result is not as important as the process. Being able to refactor, create new component from total mess, delete old CSS rules where necessary is already a goal. If, at some point, everything is a component and all the variables are defined and used, even better.

---

Q: __My old CSS rules are overriding my component__

A: When this happens the strategy is to re-define those rules inside the component itself (may look a bit reduntant). It is responsibility of the component to define the styles of object that it contains. If that means being protective and writing a bit more CSS and be more specific it is fine. When refactoring and deleting old CSS rules (non COSS) it will be possible to simplify also the components deleting unnecessary rules like ```position: relative```.

---


Q: __Any question on how to apply COSS to your large-scale application?__

A: open an issue here in Github :)

---


## General Information

V3 works like this:

- User builds Webflow page to replace desired tangram page design
- User connects Webflow page url in app
- Tangram server renders page. If a custom url is present, it loads in the html document from the URL the user inputs, re-assembles the styles
  and javascript to not interfere with tangram styles, and loads the html on the tangram page
- Our serverside html parser processes the modified document and looks for attribute keywords relevant to that page. For example, on sign in
  it looks for tg-attribute=signup-form. It then performs logical actions based on the attribute found.

## Sign In

- On sign in, fields must be formatted as follows:
  - user\[variable_name\]
- The sign in form needs to have the custom attribute "tg-attribute" set to "signin-form"
- The document parser removes the old form element and replaces it with the authenticated Tangram one
- There can be one sign in url per Tangram app

## Sign Up

- On sign up, fields must be formatted as follows:
  - user\[variable_name\]
- The sign up form needs to have the custom attribute "tg-attribute" set to "signup-form"
- The document parser removes the old form element and replaces it with the authenticated Tangram one
- There can be one sign up page per user role

## Navbar

- Nav elements must have the custom attribute "tg-attribute" set to "nav"
- There should only be one nav with this attribute on the webflow page
- The html parser will iterate over the links for the associated user role and populate them into the html page.
- Each link in tangram will cause the templated nav element to be duplicated
- There can be a different navbar for each Tangram role

## Checkout

Checkout is broken into 3 pages to emulate Shopify's checkout process.

### Step 1: Information

Asks for basic customer info

- Requires a form with the attribute as follows:
  - tg-attribute=checkout-information-form
- Fields must be in the following format:
  - tx_instance\[key\] = tx_instance\[value\]

### Step 2: Options

Asks for shipping or related info. Populates with the information from the Ecommerce flow level addons saved in the Tangram app

- Requires a form with the attribute as follows:
  - tg-attribute=checkout-options-form
- Fields must be in the following format:
  - tx_instance\[key\] = tx_instance\[value\]

### Step 3: Pay

Asks for credit card

- Requires multiple things:
  - Form must have following
    - tg-attribute=checkout-payment-form
    - id=payment-form
    - internal div with its id=payment-element
    - internal div with its id=error-message
      -submit button with id=submit

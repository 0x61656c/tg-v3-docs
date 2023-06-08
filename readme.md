## General Information

V3 works like this:

- User builds Webflow page to replace desired tangram page design
- User connects Webflow page url in app
- Tangram server renders page. If a custom url is present, it loads in the html document from the URL the user inputs, re-assembles the styles
  and javascript to not interfere with tangram styles, and loads the html on the tangram page
- Our serverside html parser processes the modified document and looks for attribute keywords relevant to that page. For example, on sign in
  it looks for tg-attribute=signup-form. It then performs logical actions based on the attribute found.

## Prerequisites

- For any of the content on this page to work, page overrides must be enabled for the platform.
- Click the wrench icon, navigate to platform settings, click integrations, navigate to the Page overrides integration, and enable it.

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

## Onboarding Flows

- Each role can have its own onboarding flow
- Once set up, the onboarding flow will be triggered when a user signs up for that role. After onboarding, the user is directed to /users/#{user_id}/edit
- Navigate to the role modification panel for the role you want to integrate the onboarding flow for
- Add a new step with the url set to the source url of the html you want to use for the onboarding page. Add a label so it can be identified later
- In your html file, create a form element and set its custom attribute "tg-attribute" to "user-form"
- For each input in the form, set the name of the field to user[{variable_name}]
  - For example, if you want a field that modifies display name, set the name of the field to user[display_name]

## Edit profile page (/users/{user_id}/edit)

- The edit profile page allows users to edit their profile information
- After completing integration, any user directed to the edit profile page will see your custom page instead of the default Tangram one
- In the role settings page, navigate to the "Page Overrides" tab for the role you want to modify and set the "Edit Profile Source URL" to the source url for your custom page.
- From here, the steps are the same as onboarding flows:
  - In your html file, create a form element and set its custom attribute "tg-attribute" to "user-form"
  - For each input in the form, set the name of the field to user[{variable_name}]
    - For example, if you want a field that modifies display name, set the name of the field to user[display_name]

## Navbar

You can set a nav for each role and a nav for your platform. Generally, you want to have one nav for each role, which means the platform one will only be used for logged out users.

Webflow navbar implementations have two methods that can be used:

Simple navs: use 1 template and duplicate it for each nav link in tangram

- Nav elements must have the custom attribute "tg-attribute" set to "nav"
- There should only be one nav with this attribute on the webflow page
- The html parser will iterate over the links for the associated user role and populate them into the html page.
- Each link in tangram will cause the templated nav element to be duplicated
- There can be a different navbar for each Tangram role

Advanced navs: explicitly define each nav link in webflow, then we populate with the corresponding tangram link. Allows more flexibility but slightly more work

- Nav elements must have the custom attribute "tg-attribute" set to "nav-{integer}
  - An example of this would be tg-attribute=nav-0
- Each nav element corresponding to this format in webflow will be populated with the corresponding link in tangram
- For example, if the 0th link in tangram is "Home", the 0th nav element in webflow will be populated with the 0th nav link for that role in Tangram
- The nav label in tangram will not be used. this is because it allows for more flexibility this way--it means you can use arbitrary html in the link, such as icons and images (you can't do this in tangram)

## Checkout

You can use some special variables in the checkout flow. These are:

- [tg-attribute=checkout-listing-photo] - the listing photo
- [tg-attribute=checkout-listing-title] - the listing title
- [tg-attribute=checkout-listing-user-name] - the listing user's name
- [tg-attribute=checkout-listing-price] - the listing price
- [tg-attribute=checkout-addons-total] - the total of all addons
- [tg-attribute=checkout-total] - the total of the listing and all addons

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

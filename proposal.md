# HTML autocapitalize proposal

## Problem statement

Web pages have no way to express their preference with regards to virtual keyboard. At the moment, browsers consider the virtual keyboard as an implementation detail which means that webpages can’t predict its behaviour. As an example, some browsers consider <input type=’text’> as a text field that should have sentences capitalization while some consider that it should not. Also, regardless of the default, web pages might want to customize the behaviour based on what is intended to be used. For example, a textarea should have a sentence capitalization by default but could definitely be used with no capitalization if it is meant to write code.

## Current implementations

autocapitalize is currently implemented in Safari iOS. It has two different implementations. Old versions of Safari iOS (prior to iOS 5) consider it as a boolean (can be on or off). Newer versions accept different values that are each associated with an autocapitalization behaviour. Obviously, newer versions of Safari are backward compatible and support on/off values.

This proposal will try to be as much as possible compatible with the current implementations.

## Why not inputmode?

inputmode is meant to do solve the same problem, among other things. It is lacking implementations - to the best of my knowledge, only Firefox OS has an implemantion and it is prefixed (x-inputmode).

Contrary to inputmode, autocapitalize is a very simple attribute that solves a very simple problem: customizing the autocapitalize behaviour of the virtual keyboard. inputmode would allow web developers to do that but it would come with side effects. For example, no capitalization would come with no suggestions (verbatim state).

In addition, autocapitalize comes with hundred of thousands of websites already supporting it. It is a defacto standard that it is worth specifying. This is not [yet another standard](https://xkcd.com/927/).

## Proposal

The autocapitalize content attribute is an enumerated attribute that gives a hint to the user agent regarding the autocapitalization behaviour of the virtual keyboard. The user agent may ignore the hint, for example if there is no virtual keyboard being used, if the hint is contradictory with other information or if the system does not allow it.

### IDL changes

autocapitalize applies to HTMLInputElement and HTMLTextAreaElement.

```
partial interface HTMLInputElement {
  attribute DOMString autocapitalize;
};

partial interface HTMLTextAreaElement {
  attribute DOMString autocapitalize;
};
```

### HTMLInputElement states

autocapitalize applies to the following states:
Text, Search, URL, Telephone, E-mail, Password

### Content attribute reflection

The autocapitalize IDL attribute must reflect the content attribute of the same name, limited to only known values.

### Keywords/states association

State | Keywords | Notes
No Capitalization | none | 
 | off | Non-conforming
Characters Capitalization | characters | 
Words Capitalization | words | 
Sentences Capitalization | sentences | 

For HTMLTextAreaElement, the invalid value default is Sentences Capitalization.
For HTMLInputElement, the invalid value default is Sentences Capitalization for Text or Search states and No Capitalization for other states for which the attribute applies.

No Capitalization state gives a hint to the user agent that no capitalization rule should apply to that element by default. For example, under that state, the first letter after the end of a sentence will not be automatically capitalized.

Characters Capitalization state gives a hint to the user agent that every single character should be capitalized by default.

Words Capitalization state gives a hint to the user agent that first characters of every word should be capitalized.

Sentences Capitalization state gives a hint to the user agent that the beginning of every sentence should be capitalized.

Those states implies hint to the user agent, they should not be enforced to the user. That means that even in No Capitalization state, the user should be able to write full capitalized text if needed.

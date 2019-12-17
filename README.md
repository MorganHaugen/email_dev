# email_dev
A collection of useful information and code for email development

[Gmail](#Gmail)  ---  [Outlook](#outlook) ---  [Agillic](#agillic) ---  [Visual Dialogue](#visual-dialogue) 
-- [General Issues](#general-issues) 

<!-- []() []() []()  -->

## Introduction
This repo will contain general information about email development,
browser and device specific information (such as the problems and solution only encountered with gmail or outlook.) and
information regarding specific DE's development environments such as visual dialogue and agillic (and maybe selligent in the future)

## Gmail
- - - - - -

#### Gmail targeting

Sometimes gmail will overwrite the width you have set for mobile.

```css
    @media only screen and (min-width: 480px) {
	.gmailDeviceWidth{
			letter-spacing: 476px;
			line-height: 0;
			mso-hide: all;
		}
		.gmailDeviceWidth:not(:checked) {
			letter-spacing: normal;
			line-height: 0;
			mso-hide: all;
			width:100%!important;
		    max-width: none!important;
		    min-width:100%!important;
		}
    }
```
Place this div just below the `body` tag.

```html
<div class="gmailDeviceWidth" style="line-height: 0; mso-hide: all">&nbsp;</div><!-- nbsp is 4px wide -->
```

#### Noteworthy
Gmail for mobile and web is notorius for being difficult to work with because of Googles strict rules for which atributes is allow and not allowed.

Gmail does read style tags, if certain criterias are met:
* The `<style>`-tag is placed before body and does not exceed the character limit of 16384
* No attribute selector  `div[class="column"] { width: 50%; }` should be `div .column { width: 50%; }`
* No @ within an @ 

    ```css
        @media {
    @font-face {
        font-family: 'Proxima Nova Regular';
        src: url('proximanova-regular-webfont.woff') format('woff'),
        url('proximanova-regular-webfont.ttf') format('truetype');
        font-weight: normal;
        font-style: normal;
    }
    }
    ```
* the pseudo-selector :checked is not supported by gmail, but doesn't break the code when used, which allows for targeted gmail code:
  
  ```css
    .class {
    styles for gmail and shared code(show, hide etc.)
    }
    .class:not(:checked) {
    styles for all other browsers and styles you dont want gmail to have
    }

## Outlook
- - - - -

### Noteworthy

Outlook is using microsoft office to run the html code and therefore doesn't accept many things used in web html. most notably `div`.

Use `table` , `tr` and `td/th` to create the layout for  Outlook.


### Targeting Outlook
#### Web and Desktop

Targeting outlook in body can be done with `<!--if(mso)  -->`

#### iOS Outlook app
You can target iOS Outlook app with the `data-outlook-cycle` -atribute to the body tag.

```html
    <html>
    <head>
    <meta charset="utf-8">
    <title>Untitled</title>
    <style>
        body[data-outlook-cycle] .default {
            display: none !important
        }
        body[data-outlook-cycle] .oa {
            display: block !important;
            background-color: red !important;
            color: #ffffff !important;
        }
        </style>
    </head>
        <body>
            <div style="background-color: blue; color: #ffffff" class="default">Other</div>
            <div style="display: none" class="oa">Outlook App</div>
        </body>
    </html>
``` 

`[data-outlook-cycle*="INSERT_STYLES"]` will target only Microsoft email addresses @hotmail, @live, @outlook etc. on iOS. And only non Microsoft addresses on Android.
#### Padding Issues
## Agillic
- - - - -
#### Noteworthy
## Visual Dialogue
- - - - -
#### Noteworthy
## General Issues
 - - - -

[]() 
<!-- []()  -->
# email_dev
A collection of useful information and code for email development

[Gmail](#Gmail)  ---  [Outlook](#outlook) ---  [Agillic](#agillic) ---  [Visual Dialogue](#visual-dialogue) 
-- [General Issues](#general-issues) --- 

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
- - - - - -

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
[Source](https://litmus.com/community/discussions/7660-targeting-outlook-app-on-android-and-ios)

`[data-outlook-cycle*="INSERT_STYLES"]` will target only Microsoft email addresses @hotmail, @live, @outlook etc. on iOS. And only non Microsoft addresses on Android.
#### Padding Issues
## Agillic

- - - - - -
[Setup](#A1-Setup) - [Editable Content](#A2-Editable-Content)

Agillic lets you code editable html moduels for designers to tweak and publish. The syntax used is read and formated through Agillics page and returns the varible code to common html. [source](https://developers.agillic.com/templates/email)

### A1 Setup

In order to restrict your template usage in Agillic you place a ``content`` meta tag in the head.

```html
    <meta name="agavailability" content="">
```
#### Types

- email - Restrict the template to be used for the creation of emails
- portal - Restrict the template to be used for the creation of web portals
- print - Restrict the template to be used for the creation of print files
- inactive - Disable the template and mark as unavailable for the configurator
  
#### From and Personal From

From and Personal from can be inserted by adding ``<from>`` and ``<personalfrom>`` tags. These tags allows you to preset an email address, from which the email will be sent from and can be replied to. And let's you customize a name to be preset as sender. You can also insert Person Data or Global Data.
```html
    <head>
        <from>serviceteam@company.com</from>
        <personalfrom>Service Team</personalfrom>
    </head>
```
```html
    <head>
        <from>serviceteam@company.com</from>
        <personalfrom><persondata>COMPANY_REP</persondata></personalfrom>
    </head>
```
#### Blocks

Agillic builds content out of ``Content Blocks`` within the template and it is within these blocks your configurators place their content. Anything that is editable by the Agillic Content Editor has to be placed inside a block which there are two different types of. 

- agblock - A static block. Locked in place and cannot be placed anywhere else. Often used for Footer and Header.
- agblockgroup - Used to contain agrepeatingblock and nothing else.
- agrepeatingblock - Similar functionality as agblock but agrepeatingblock can be copied and placed anywhere inside an agblockgroup.

agblockgroup and agrepeatingblock is your go to method for creating dynamic content in your template.

```html
    <table width="100%" border="0" cellspacing="0" cellpadding="0">
        <tr>
            <td agblock="on">Best Brand</td>
        </tr>
    </table>
    <table width="100%" border="0" cellspacing="0" cellpadding="0">
        <tr>
            <td agblockgroup="on">
                <table agrepeatingblock="on" width="100%" border="0" cellspacing="0" cellpadding="0">
                    <tr>
                        <td>Block 1</td>
                    </tr>
                </table>
                <table agrepeatingblock="on" width="100%" border="0" cellspacing="0" cellpadding="0">
                    <tr>
                        <td>Block 2</td>
                    </tr>
                </table>
                <table agrepeatingblock="on" width="100%" border="0" cellspacing="0" cellpadding="0">
                    <tr>
                        <td>Block 3</td>
                    </tr>
                </table>
            </td>
        </tr>
    </table>
    <table width="100%" border="0" cellspacing="0" cellpadding="0">
        <tr>
            <td agblock="">Copyright © Best Brand</td>
        </tr>
    </table>
```
### A2-Editable content

To make content editable elements in Content Editor us

`ageditable`

These are your options to control the WYSIWYG elements in the Content Designer.: 

|   syntax            |   Meaning	                        |syntax                 |   Meaning                             |
|---                  |---                                  |--                     |---                                    |
|                     |   Empty will disable the WYSIWYG    |  ``font_face``        |   Enables standard web safe font faces|
|   ``*``             |   Will allow everything             |  ``font_size``        |   Enables font sizes                  |
|   ``"bold"``        |   Enables bold text                 |  ``indent``           |   Enables text indent                 |
|   ``italic``	      |   Enables italic text               |  ``outdent``          |   Enables text outdent                |
|   ``underline``     |   Enables underlined text           |  ``ordered_list``     |   Enables ordered lists               |
|   ``align_justify`` |   Enables justified text alignment  |  ``unordered_list``   |   Enables unordered lists             |
|   ``align_right``   |   Enables right text alignment      |  ``paragraph_styles`` |   Enables h1 to h6 and p              |
|   ``align_center``  |   Enables center text alignment     |  ``font_color``       |   Enables a color palette for text    |
|   ``align_left``    |   Enables left text alignment       |

``WYSIWYG`` = What You See Is What You Get

```html
    <table width="100%" border="0" cellspacing="0" cellpadding="0">
    <tr>
        <td ageditable="image"><img src="banner.jpg"></td>
    </tr>
    <tr>
        <td ageditable="">Headline</td>
    </tr>
    <tr>
        <td ageditable="bold,italic,underline">Lorem ipsum dolor sit amet</td>
    </tr>
    <tr>
        <td ageditable="hyperlink"><a href="#">Read more</a></td>
    </tr>
</table>
```
#### Links or hyperlinks

The following two are not used in conjunction with the WYSIWYG. They are used to let Agillic know what kind of content are inside the ageditable. Whenever you have an image you use ageditable="image" and whenever you have a standalone link, like at CTA you use ageditable="hyperlink".

#### Restrict font & colors

You can restrict the choices in ``font_face``, ``font_size`` and ``paragraph_styles``. However this is done in the head with the use of meta tags. Like this :

-   ```html
        <!--  Contains list of comma separated font names.--> 
        <meta name="restrict-font" content="Arial,Verdana,Tahoma">
    ``` 
-   ```html
        <!--  Contains list of comma separated numeric sizes. Values starts from 8pt which equals size "1" in the editor. 8pt=1, 10pt=2, 12pt=3 etc. Values less than 8 will be ignored. --> 
        <meta name="restrict-font-size" content="2,4,6,8,10,12">
    ```  
-   ```html
        <!--  Contains list of comma separated hex or (#CCCCC) or RGB (rgb(680,255,0)) colors. Both types can be added in the tag together. There are no restrictions and any wrong color will be interpreted as black. --> 
       <meta name="restrict-font-color" content="#ffffff,rgb(20,120,170),#cccccc">
    ```     

 or `parameters`



Example

1.  

### Agillic specific code




### Noteworthy

## Visual Dialogue
- - - - - -
#### Noteworthy
## General Issues
 - - - - - -


<!-- []()  -->
# email_dev
A collection of useful information and code for email development

[Gmail](#Gmail)  ---  [Outlook](#outlook) ---  [Agillic](#agillic) ---  [Visual Dialogue](#visual-dialogue-portrait) 
-- [General Issues](#general-issues) --- 

Coming soon: `Selligent`  - `Adobe Marketing Cloud` - `Salesforce`
<!-- []() []() []()  -->

## Introduction
This repo will contain general information about email development,
browser and device specific information (such as the problems and solution only encountered with gmail or outlook.) and
information regarding specific DE's development environments such as visual dialogue and agillic (and maybe selligent in the future)

## Gmail
- - - - - -

#### Gmail targeting

Sometimes gmail will overwrite the width you have set for mobile. This will negate that by giving gmail a set width for mobile.

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

Targeting outlook in body can be done with `<!--if(mso) >` and closes with``<![endif]-->``

For  Example:

```html
<!--[if mso]>
<table role="presentation" cellspacing="0" cellpadding="0" border="0" width="100%">
<tr>
<td width="340">
<![endif]-->
    <div style="display:inline-block; width:100%; min-width:200px; max-width:340px;">
        Outlook can’t render the CSS in this DIV but other email clients can,
        so we wrap this in a ghost table that replicates the DIV’s desktop style.
        In this case, a container 340px wide.
    </div>
<!--[if mso]>
</td>
</tr>
</table>
<![endif]-->
```


Different Outlook versions can be targeted by using Microsoft Office Version numbers: 

| `Outlook version(s)` | Code |
| --- | --- |
| Outlook 2000  |``<!--[if mso 9]> your code <![endif]-->``|
|Outlook 2002	|``<!--[if mso 10]> your code <![endif]-->``|
|Outlook 2003	|``<!--[if mso 11]> your code <![endif]-->``|
|Outlook 2007	|``<!--[if mso 12]> your code <![endif]-->``|
|Outlook 2010	|``<!--[if mso 14]> your code <![endif]-->``|
|Outlook 2013	|``<!--[if mso 15]> your code <![endif]-->``|
|Outlook 2016	|``<!--[if mso 16]> your code <![endif]-->``|

Conditional Logic allows you to create expressions targeting multiple Outlook version: 

| Code	| Description |	Example |
| ---| ---| ---|
|``gt``	| greater than	|``<!--[if gt mso 14]> Everything above Outlook 2010 <![endif]-->``|
|``lt``	|less than	|``<!--[if lt mso 14]> Everything below Outlook 2010 <![endif]-->``|
|``gte``	|greater than or equal to	|``<!--[if gte mso 14]> Outlook 2010 and above <![endif]-->``|
|``lte``	|less than or equal to|``<!--[if lte mso 14]> Outlook 2010 and below <![endif]-->``|
| ` `\|` `	|or	|``<!--[if (mso 12)``\|``(mso 16)]> Outlook 2007 / 2016 only <![endif]-->``|
|``!``	|not	|``<!--[if !mso]><!--> All Outlooks will ignore this <!--<![endif]-->``|

[Source](https://stackoverflow.design/email/base/mso)


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


```html
<td style=”padding: 20px;”>
```
``<td>`` padding is generally safe as long as you’re not setting a width property or attribute. Outlook 2007 and 2010 will convert your width pixels to points.


[Source](https://www.emailonacid.com/blog/article/email-development/7_tips_and_tricks_regarding_margins_and_padding_in_html_emails/)

## Agillic

- - - - - -
[Setup](#A1-Setup) - [Editable Content](#A2-Editable-Content) - [Parameters](#A3-Parameters)

Agillic lets you code editable html moduels for designers to tweak and publish.  [source](https://developers.agillic.com/templates/email)

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

|   syntax            |   Meaning                           |syntax                 |   Meaning                             |
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

### A3-Parameters
Making a parameter enables an interface on a block that is available whenever you click the i-icon (in Agillic) of a block. When setting up a template you can work with two types of parameters. Template parameters and Block paramenters.

TThe syntax used is read and formated through Agillics page and returns the varible code to common html. Meaning only Agillic understands the code.

```
    ${scope:namespace::name|TYPE|default_value}
```
Scope can be either: ``templateparam``,  ``blockparam`` or ``ref``.

``Namespace`` is the Label of several Settings and is yours to define. ``Name`` is the Name of a setting.



|   ``TYPE``        |   Meaning                                                                               |
|---                |---                                                                                      |
|   ``IMAGE``       |   Enable the media browser interface                                                    |
|   ``COLOR``       |   Restrict available colors, and enable the default color picker interface              |
|   ``LINK``        |   Enable the link editor interface                                                      |
|   ``STRING``      |   You enable the configurator to type in anything.                                      |
|   ``INTEGER``     |   Whole numbers and presents an easy function to increase of decrease the number.       |
|   ``NUMBER``      |   Any type of numbers and presents an easy function to increase of decrease the number. |

``default_value`` is the default value of the parameter. You can also create options to choose from. That is done with enclosing brackets and semicolon as seperator:

-    ``[#000000;#ffffff] ``

```
    ${blockparam:Button::Font\_Color|COLOR|[#000000;#ffffff]}
```

<br>

Example of each parameter type:

```
    ${templateparam:Body::Background_Color|COLOR|#ffffff}
    ${blockparam:Button::Font_Color|COLOR|#ffffff}
    ${ref:Button::Font_Color}
```

Templateparam will be settings on template level, Blockparam will be settings on block level and Ref will be a reference and a direct copy of either a template- or block param.

all together, it might look like this:
```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<html>
    <head>
        <meta property="og:url" content="webcopy://show" />
        <meta property="og:type" content="website" />
        <meta property="og:title" content="${templateparam:Share::Title|STRING|When Great Minds Don’t Think Alike}" />
        <meta property="og:description" content="${templateparam:Share::Description|STRING|How much does culture influence creative thinking?}" />
        <meta property="og:image" content="${templateparam:Share::Image|IMAGE|http://static01.nyt.com/images/2015/02/19/arts/international/19iht-btnumbers19A/19iht-btnumbers19A-facebookJumbo-v2.jpg}" />
    </head>
    <body>
        <table border="0" cellspacing="0" cellpadding="0" role="presentation" agblock="on" align="center">
            <tr>
                <td align="center" style="padding: ${blockparam:Block::Padding|INTEGER|20}px; background-color: ${blockparam:Block::Background_Color|COLOR|[#ffffff;#cccccc]}">
                    <table width="600" border="0" cellspacing="0" cellpadding="0" role="presentation">                              
                        <tr>
                            <td align="${blockparam:Link::Text_Align|STRING|[left;center;right]}" style="font-family: Arial; color: ${blockparam:Link::Font_Color|COLOR|#000000}; font-size: ${blockparam:Link::Font_Size|INTEGER|12}px; line-height: ${blockparam:Link::Line_Height|NUMBER|1.5};">
                                <a href="${blockparam:Link::URL|LINK|http://www.agillic.com/}" style="color: ${ref:Link::Font_Color}">${blockparam:Link::Text|STRING|Lorem ipsum dolor sit amet}</a>
                            </td>
                        </tr>
                    </table>
                </td>
            </tr>
        </table>
    </body>
</html>
```

## Visual Dialogue Portrait
- - - - - -
[Full documentation](https://www.pitneybowes.com/content/dam/support/software/product-documentation/public/portrait-dialogue/v6-1-0/en-us/portrait-dialogue-v6-1-0-visual-dialogue-user-guide.pdf)

[IMPORTANT for first time users!!](#important)


Visual dialogue Portrai email development is mainly split into two programs:

| ``Name``&zwnj;&nbsp;   | ``Purpose`` | ``Access`` |
| --- | --- | --- |
| ``Visual``&zwnj;&nbsp;``Dialogue`` | Compose emails in Master templates and send emails | Developers and composers/customers |
| ``Dialogue``&zwnj;&nbsp;``Admin`` | Creating  email components | Developers |

### Visual Dialogue

An explanation for different parts of ``Visual Dialogue``

| `Name`| ``Description``| ``Usage`` |
| --- | --- | --- |
| Master&zwnj;&nbsp;Template| A main template working as a framework for message templates. | Used in all mail and for components that will repeat across multiple emails, like header, footer and style|
| Message&zwnj;&nbsp;Template| The setup for mails that are sent. Uses a master template as a platform and imports components from ``Dialogue Admin``.| Used for a single mail or as a template for multiple mails using the same components |
| |||

Visual Dialog is the main component where emails are composed and sent and is used by both developer, while Visual Admin is used only by developers to create modules that can be placed into message templates and other related tasks.

#### HTML CSS support

Since some email clients do not support CSS defined in the `<style>` tags, Visual dialog has made several options to move all the css inline. This can be useful or detrimental depending on how you apply it. 

| Style tag definition | Description |
| --- | --- |
|`<style type="text/css">` |  Default—moves all CSS inline.|
| `<style type="text/css" inlinemode="MoveInline">` | Same as default—moves all CSS inline.|
|`<style type="text/css" inlinemode="CopyInline">` |Copies all CSS inline |
| `<style type="text/css" inlinemode="None">`| Does not do anything—leave the ``<style>`` tag as it is|




#### Importing images

Publishing images in Portrait makes them available to be used in emails for all users of the domain.

``[PC and other external desktop user]:``

1. To import images you first drag or copy the images over from your computer.
2. Open `Visual Dialogue`, go to ``Explore`` and create/find the folder you want to place the images.
3. Right click>New>Published Files and find where you placed them on the external desktop.

``[MAC]``

Skip step `1` and just use the location in finder.
### Dialogue Admin

Dialogue Admin consists of several server hosts, each with multiple firms connected to it. Each firm also has their own folder where all the resources you will need is placed, or will be placed by you.

#### Dialogue Server Hosts

The four servers are called BDN-PORT-AP1, BDN-PORT-AP2, BDN-PORT-AP3 and BDN-PORT-AP4.
If they do not show up, they can be accessed by Pressing the `Dialogue Server Host`, then right clicking and `new` and type the server name in the `Computer name` field.

#### Misc. System Data

The folder where all the modules we create are located, is at ``(FirmName)-> General Admin -> Misc. system data - Emarketing mail definitions -> Body area item defs``

If you cant find the `Misc. system data` folder, it is most likely hidden and can be revealed by going to `View` in the top ribbon and checking System Data.

#### Custom Color Palettes

Colors that are declared in ``Style sheet defs`` on the same level as ``Body area item defs``, will be available in the master template and subsequently in the message templates when editing modules. 

To add a color palette, right click and create a new class and write in CSS the color/colors you want to declare. Save. In message template refresh (f5) and reapply master template (ctrl+f6). This should now apply as an option in the text editor of the modules, both in foreground color and background color. 

#### Noteworthy

## Important

First time users of Visual Dialog when receiving a new Portrait account:

1. Open `Visual Dialogue`.
2. Access a domain, any will do.
3. Go to ``Options``. If already inside Visual Dialogue, it is the blue button to the left of Home.
4. Go to ``Message Designer``.
5. ``HTML message template`` NEEDS to be "Open in Source View" and NOT "Open in Design View"
6. ``Emarketing mail templates`` set to "Open in integrated designer" with "Use old style dialog to selectpublished files" checked.
7. ``Apply`` and close


This is to avoid Portrait from breaking HTML code when it opens  an html file.
 - - - - - -
## General Issues

<!-- []()  -->
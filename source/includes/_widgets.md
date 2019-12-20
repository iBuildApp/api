# Widgets

For the widgets we have available, we pass data around using JSON. This section covers this with explanations and examples, to help with expanding and using this with new widgets.

<!-- ## Feed Widget

### Overview

RSS widget is intended for displaying of news feed, defined in format of RSS-feed or Atom.
General features:
viewing of news list;
viewing of a page with detailed view of each news;
redirect via external link (if link exists in detailed view of news).

### Data

> body (application/json):

```json
{
    "widgetid": "36554786",
    "type": "rss",
    "data": {
        "title": "My RSS page",
        "#url": "https://news.google.com/rss",
        "colorskin": {
            "background": "#000000",
            "primary": "#ffffff",
            "secondary": "#ffffff",
            "text": "#ffffff",
            "accent": "#ff6f28"
        },
        "options": [
            "send_mail",
            "send_sms"
        ]
    }
}

```


Data parameters:
title - widget name. Title is being displayed on navigation panel when widget is launched.
colorskin - color scheme. Contains 5 elements: background, primary, secondary, text and accent colors. If the default color scheme is used, this parameter is missing.
\#url - RSS-feed URL link.
options - an array of included options. May containt elemets such as send_mail and send_sms to allow customers to share the feed via email and SMS.

### Changelog -->

## CustomForm Widget

### Overview

Custom Form widget is intended for construction and displaying of a form for filling information and sending it's result by eMail.
Management elements: text area, entry field, checkbox, radio button, dropdown list, datapicker - can be divided by groups.

### Data

> body (application/json):

```json
{
    "widgetid": "36555161",
    "type": "customform",
    "data": {
        "title": "My custom form",
        "colorskin": {
            "background": "#000000",
            "primary": "#ffffff",
            "secondary": "#ffffff",
            "text": "#ffffff",
            "accent": "#ff6f28"
        },
        "form": {
            "email": {
                "#address": "123@gmail.com",
                "#subject": "",
                "button": {
                    "#label": "Send it!"
                }
            },
            "groups": [
                {
                    "#title": "Reality",
                    "fields": [
                        {
                            "@format": "number",
                            "#label": "789",
                            "#value": "000",
                            "type": "entryfield"
                        },
                        {
                            "@format": "general",
                            "#label": "123",
                            "#value": "456",
                            "type": "entryfield"
                        },
                        {
                            "#label": "Text",
                            "#value": "test",
                            "type": "textarea"
                        },
                        {
                            "#label": "yes",
                            "#value": "checked",
                            "type": "checkbox"
                        },
                        {
                            "#label": "No",
                            "#value": "",
                            "type": "checkbox"
                        },
                        {
                            "#label": "Hi",
                            "#value": "checked",
                            "type": "radiobutton"
                        },
                        {
                            "#label": "Hello",
                            "#value": "",
                            "type": "radiobutton"
                        },
                        {
                            "#label": "Choose the number",
                            "value": [
                                "One",
                                "two",
                                "three",
                                "four",
                                "five",
                                "six",
                                "seven"
                            ],
                            "type": "dropdown"
                        },
                        {
                            "#label": "date",
                            "#value": "10/08/2019",
                            "type": "datepicker"
                        }
                    ]
                },
                {
                    "#title": "0 group",
                    "fields": [
                        {
                            "@format": "general",
                            "#label": "one",
                            "#value": "one",
                            "type": "entryfield"
                        }
                    ]
                },
                {
                    "#title": "New group",
                    "fields": [
                        {
                            "@format": "general",
                            "#label": "empty",
                            "#value": "empty",
                            "type": "entryfield"
                        },
                        {
                            "@format": "general",
                            "#label": "full",
                            "#value": "full",
                            "type": "entryfield"
                        }
                    ]
                },
                {
                    "#title": "xxx",
                    "fields": [
                        {
                            "#label": "",
                            "#value": "10/14/2019",
                            "type": "datepicker"
                        },
                        {
                            "@format": "general",
                            "#label": "",
                            "#value": "",
                            "type": "entryfield"
                        },
                        {
                            "#label": "Image",
                            "#value": "Add Image",
                            "limit": "2",
                            "type": "photopicker"
                        }
                    ]
                }
            ]
        }
    }
}
```

Data parameters:<br>
• title - widget name. Title is being displayed on navigation panel when widget is launched.<br>
• colorskin - color scheme. Contains 5 elements: background, primary, secondary, text and accent colors. If the default color scheme is used, this parameter is missing.<br>
• form - the form object including email, all groups and fields.<br>
• form.email - the object of email for sending form data:<br>
    &ensp;&ensp;&ensp;&ensp;◘ \#address - email address<br>
    &ensp;&ensp;&ensp;&ensp;◘ subject - email subject<br>
    &ensp;&ensp;&ensp;&ensp;◘ button - button parameters (\#label - button label)<br>
• form.groups - an array of form groups, each form has parameters such as:<br>
    &ensp;&ensp;&ensp;&ensp;◘ \#title - the section title/name<br>
    &ensp;&ensp;&ensp;&ensp;◘ fields - an array of the form fields, each field has parameters:<br>
        &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;• \#label - the field name/label<br>
        &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;• \#value - the defaul value, the set of the available values (for dropdowns) or the placeholder(for entryfields and textareas) of the field<br>
        &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;• type - the type of the field from the list:<br>
            &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;◘ entryfield - text or number field<br>
            &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;◘ textarea<br>
            &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;◘ checkbox<br>
            &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;◘ radiobutton<br>
            &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;◘ dropdown<br>
            &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;◘ datepicker<br>
            &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;◘ photopicker - the image file attacment, one or many files can be attached<br>
        &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;• @format (only for entryfields) - general (text) or number field<br>
        &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;• limit (only for photopicker) - the limit of attachments number<br>

### Changelog

| Release       | Notes                            |
|---------------|----------------------------------|
| 1.0 (Current) | Initial release into open source |
|               |                                  |

<!-- ## Map Widget

### Overview

Google map widget is intended for displaying of Google Map and interaction with it.
General features:
locations display on the map;
viewing of each location description in pop-up bubble, which is being displayed while clicking on location icon;
redirect on website in case if location description has website URL address;
detection of a current user location;
creation of the route from current user location to chosen location on the map.

### Data

```json
{
    "widgetid": "36557316",
    "type": "map",
    "data": {
        "title": "Our location",
        "#initialZoom": -1,
        "showCurrentUserLocation": 1,
        "object": [
            {
                "#title": "1st map point",
                "#subtitle": "Subtitle for map point",
                "#latitude": 56.114838,
                "#longitude": 40.3555937,
                "#description": "http://ibuildapp.com",
                "#pinurl": "https://ibuildapp.com/media/wdefaults/map/pins/default-pin.png"
            },
            {
                "#title": "Second map point",
                "#subtitle": "Subtitle for point",
                "#latitude": 56.127428,
                "#longitude": 40.399261,
                "#description": "",
                "#pinurl": "https://ibuildapp.com/media/wdefaults/map/pins/company.png"
            }
        ]
    }
}
```

Data parameters:
title - widget name. Title is being displayed on navigation panel when widget is launched.
initialZoom - scale (zoom)of map display. Integer value. If the scale is not defined or value is not valid, scale is being calculated automatically, so all the locations could be displayed on the map at the same time.
showCurrentUserLocation - defines display of the current user's location. Values - 1 - display and 0 - hide (not display).
object - an array of the map points, each point object contains properties:
\#title - location title on the map. Is being displayed in pop-up bubble while clicking on location icon.
\#subtitle - location description. Is being displayed in pop-up bubble while clicking on location icon.
\#latitude - location coordinate: latitude(real value).
\#longitude - location coordinate: longitude (real value).
\#description - url for redirect on web address while clicking on button in location description pop-up. Field is not required.
\#pinurl - url of an image of icon location on the map. Field is not required.

### Changelog -->

## QR Scanner Widget

### Overview

The module immediately opens up the phone's camera and takes you to a screen that allows for the scanning of barcodes.
The scanner will automatically detect which type of code it is scanning and, upon completion of the scan, it will display the content so you can view the scanned URL/text/product code before the module launches the phone's browser.
The module offers sharing feature that allows users to share scanned content via SMS or email.
You may view the scanned content in a web browser: it will search for scanned text on the web or open a website for scanned URL.

### Data

```json
{
    "widgetid": "36556631",
    "type": "qrscaner",
    "data": {
        "title": "QR-Code Scaner"
    }
}
```

This widget JSON does not contain any special information except for the widget title.

### Changelog

| Release       | Notes                            |
|---------------|----------------------------------|
| 1.0 (Current) | Initial release into open source |
|               |                                  |

## Phonecall Widget

### Overview

Tap to Call widget allows users to make calls on predefined phone number.
General features:
call on predefined phone number
Widget implementation:
mTapToCall.a library (iOS);
CallPlugin library (Android).

### Data

```json
{
    "widgetid": "36556535",
    "type": "taptocall",
    "data": {
        "title": "Button",
        "#phone": "+47(345)4356234523"
    }
}
```

Data parameters:<br>
• title - button name<br>
• \#phone - phone number<br>

### Changelog

| Release       | Notes                            |
|---------------|----------------------------------|
| 1.0 (Current) | Initial release into open source |
|               |                                  |

## HTML Widget

### Overview

Web widget is designed to display web-content, located under provided web-link. User provides the link while creating the app on website. If a user provides HTML-content manually then this content should be displayed in the app.
Widget implementation:
mWebVC.a library (iOS);
WebPlugin library (Android).

### Data

> Web Example

```json
{
    "widgetid": "36556977",
    "type": "htmlurl",
    "data": {
        "#title": "Website",
        "content": {
            "@src": "http://m.ibuildapp.com"
        }
    }
}
```

> HTML Example

```json
{
    "widgetid": "36557059",
    "type": "htmlcode",
    "data": {
        "#title": "Content Management",
        "#content": "<html><head></head><body> Hello, world!<div>:)</div> </body></html>"
    }
}
```

Data parameters:<br>
• \#title - widget name. Title is being displayed on navigation panel when widget is launched.<br>
• content - widget content with URL or HTML string<br>

### Changelog

| Release       | Notes                            |
|---------------|----------------------------------|
| 1.0 (Current) | Initial release into open source |
|               |                                  |

## E-Mail Widget

### Overview

The main features:
Call the mail-client with predefined settings
Widget implementation:
mWebVC.a library (iOS);
EmailPlugin library (Android).

### Data

> Example

```json
{
    "widgetid": "36557168",
    "type": "taptoemail",
    "data": {
        "title": "Button",
        "#mailto": "test@test.com",
        "#subject": "",
        "#message": ""
    }
}
```

Data parameters:<br>
• title - button name<br>
• \#mailto - email address of the recipient<br>
• \#subject - text of the letter<br>
• \#message - default message text<br>

### Changelog

| Release       | Notes                            |
|---------------|----------------------------------|
| 1.0 (Current) | Initial release into open source |
|               |                                  |

<!-- ## Twitter Widget

### Overview

Twitter widget allows to embed Twitter feed into the application.
The main features:
Viewing feed is specified by the creator of the application
Re-twits creation
Answers on twits creation
Viewing Twitter users profiles
Widget implementation:
mTwitter.a library (iOS);
TwitterPlugin library (Android).

### Data

> Example

```json
{
    "widgetid": "36559149",
    "type": "twitter2",
    "data": {
        "title": "Twitter",
        "username": "garik2000",
        "colorskin": {
            "background": "#000000",
            "primary": "#ffffff",
            "secondary": "#ffffff",
            "text": "#ffffff",
            "accent": "#ff6f28"
        }
    }
}
```

Data parameters:
title - widget name. Title is being displayed on navigation panel when widget is launched.
colorskin - color scheme. Contains 5 elements: background, primary, secondary, text and accent colors. If the default color scheme is used, this parameter is missing.
username - username in Twitter system.

### Changelog -->

<!-- ## Contacts Widget

### Overview

Contacts widget is designed for displaying contact information: avatar, name, phone, email, website address. By clicking on the phone number is possible to make a call, by clicking on the e-mail - send a message, go to the website via the link, the place on the map display to the specified address.
MultiContacts is scenario for Contacts module. Used if there are more than one contact in widget. In this case, at the start of the module displays a list of contacts with the ability to go to the selected contact.
GroupedContacts - is scenario for Contacts module. Differs from MultiContacts additional level of hierarchy (contact lists can be divided into groups) and the ability to search the contents of the contacts (in the search mode, the list is filtered by content).
Widget implementation:
mMultiContacts.a library (iOS);
MultiContactsPlugin library (Android).

### Data

> Example

```json

```

### Changelog -->

<!-- ## Facebook Widget

### Overview

Facebook widget was released via web-view (scenario for Web widget). Application owner have to point Facebook wall URL address, which will be displayed in the widget of application.
Widget implementation:
mWebVC.a library (iOS);
WebPlugin library (Android).

### Data

> Example

```json
{
    "widgetid": "36559181",
    "type": "facebook2",
    "data": {
        "#fb_id": "152351011468543",
        "title": "Facebook",
        "token": "207296122640913|8GKTRW57eH_2t5hVgtioNPWvecE"
    }
}
```

Data parameters:
\#fb_id - id of the Facebook page.
title - widget name. Title is being displayed on navigation panel when widget is launched.
token - the access token of our Facebook app.

### Changelog -->

## PDF Reader Widget

### Overview

This widget is used for viewing PDF or text documents on the mobile phone.

### Data

> Example

```json
{
    "widgetid": "36559206",
    "type": "ebook",
    "data": {
        "title": "PDF Reader Content Management",
        "colorskin": {
            "background": "#000000",
            "primary": "#ffffff",
            "secondary": "#ffffff",
            "text": "#ffffff",
            "accent": "#ff6f28"
        },
        "row": [
            {
                "#url": "https://ibuildapp.com/gethtml.php?url=http://ibuildapp.s3-website-us-east-1.amazonaws.com/assets/data/02312/2312789/apps/2568578/36559206/1571755089.txt",
                "#title": "Chapter 1"
            },
            {
                "#url": "http://ibuildapp.s3-website-us-east-1.amazonaws.com/assets/data/02312/2312789/apps/2568578/36559206/upload-5daf146ac4c8a6.35940948.pdf",
                "#title": "Chapter 2"
            },
            {
                "#url": "https://google.com",
                "#title": "Chapter 3"
            }
        ]
    }
}
```

Data parameters:<br>
• title - widget name. Title is being displayed on navigation panel when widget is launched.<br>
• colorskin - color scheme. Contains 5 elements: background, primary, secondary, text and accent colors. If the default color scheme is used, this parameter is missing.<br>
• row - an array of widget sections. Each section has parameters:<br>
    &ensp;&ensp;&ensp;&ensp;◘ \#url - URL of the content  (PDF or text file on our server or external URL).<br>
    &ensp;&ensp;&ensp;&ensp;◘ \#title - section name.<br>

### Changelog

| Release       | Notes                            |
|---------------|----------------------------------|
| 1.0 (Current) | Initial release into open source |
|               |                                  |

<!-- ## Catalog Widget

### Overview

Catalog widget is intended for categorization, products viewing, making purchases.
General features:
viewing a list of products;
viewing a detailed page of each product;
making order with its further sending to seller by email;
viewing the history of purchases;
setting up buyer profile (data is being used in repeated purchase).
Widget configuration can be set up in 2 ways:
integration with shops "Magento" type;
adding products manually.
Widget implementation:
mCatalog.a library (iOS);
CatalogPlugin library (Android).

### Data

> Example

```json

```

### Changelog -->

## Camera Widget

### Overview

Allow customers to take pictures from camera and share it.

### Data

> Example

```json
{
    "widgetid": "36573784",
    "type": "takepicture",
    "data": {
        "title": "Take a Picture",
        "button": {
            "type": "share",
            "email": "",
            "label": "Share"
        }
    }
}
```

Data parameters:<br>
• title - widget name. Title is being displayed on navigation panel when widget is launched.<br>
• button - button object with the properties:<br>
    &ensp;&ensp;&ensp;&ensp;◘ type - the button type from the list:<br>
        &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;• share - for sharing picture via standard sharing services available on device.<br>
        &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;• email - for sending pictures to email.<br>
    &ensp;&ensp;&ensp;&ensp;◘ email - email address for sending pictures (with email type button).<br>
    &ensp;&ensp;&ensp;&ensp;◘ label - button label.<br>

### Changelog

| Release       | Notes                            |
|---------------|----------------------------------|
| 1.0 (Current) | Initial release into open source |
|               |                                  |

<!-- ## Inventory Widget

### Overview

### Data

> Example

```json

```

### Changelog -->

<!-- ## Sales Reporting Widget

### Overview

### Data

> Example

```json

```

### Changelog -->

<!-- ## Calculator Widget

### Overview

Calculator widget is used for the calculations, according to a formula specified by the application author in the app creation process.
The main features:
Final value calculation (by a given formula) based on data provided by the user.
Widget implementation:
mMathCalc.a library (iOS);
CalculatorPlugin library (Android).

### Data

> Example

```json

```

### Changelog -->

## Bookstore Widget

### Overview

BookStore Widget provides the opportunity to customize catalog of books for sale.
The main features:
View the list of books;
View detail page of separate book (Title, Descriprion, Price, etc.);
Filtering books (filters are defined by application author on the app creation stage);
Jump to the shop website in embedded browser (if appropriate link was specified on the app creation stage)
Widget implementation:
mDBViewer.a library (iOS);
SkCataloguePlugin library (Android).

### Data

> Example

```json
{
    "widgetid": "36574049",
    "type": "catalogbooks",
    "data": {
        "menu": [
            {
                "name": "Genre",
                "tag": "Category",
                "icon": "https://ibuildapp.com/media/wdefaults/catalog/images/categories@2x.png",
                "items": [
                    "Action and Adventure",
                    "Art & Photography",
                    "Biography & Memories",
                    "Business & Finance",
                    "Celebrity & Pop Culture",
                    "Children's",
                    "Cookbooks",
                    "Cultural/Social Issues",
                    "Fantasy",
                    "Food & Lifestyle",
                    "Gardening",
                    "Graphic Novels",
                    "History & Military",
                    "Horror",
                    "Home Decorating & Design",
                    "How To",
                    "Humour & Gift Books",
                    "Journalism",
                    "Medical, Health & Fitness",
                    "Music, Film & Entertainment",
                    "Mystery",
                    "Nature & Ecology",
                    "Offbeat or Quirky",
                    "Parenting",
                    "Pets",
                    "Picture Books",
                    "Psychology",
                    "Religion & Spirituality",
                    "Romance",
                    "Science Fiction",
                    "Short Story Collections",
                    "Thrillers and Suspense",
                    "Travel",
                    "Western"
                ]
            },
            {
                "name": "Recommended",
                "tag": "Recommended",
                "icon": "https://ibuildapp.com/media/wdefaults/catalog/images/featured@2x.png"
            },
            {
                "name": "New Releases",
                "tag": "Releases",
                "icon": "https://ibuildapp.com/media/wdefaults/catalog/images/new@2x.png"
            },
            {
                "name": "Bestsellers",
                "tag": "Bestsellers",
                "icon": "https://ibuildapp.com/media/wdefaults/catalog/images/bestsellers@2x.png"
            }
        ],
        "colorskin": {
            "background": "#000000",
            "primary": "#ffffff",
            "secondary": "#ffffff",
            "text": "#ffffff",
            "accent": "#ff6f28"
        },
        "content": [
            {
                "title": "The Atlatis Gene",
                "description": "The Immari are good at keeping secrets. For 2,000 years, they've hidden the truth about human evolution. They've also searched for an ancient enemy - a threat that could wipe out the human race. Now the search is over.nOff the coast of Antarctica, a research vessel discovers a mysterious structure buried deep in an iceberg. It has been there for thousands of years, and something is guarding it.",
                "author": "A.G. Riddle",
                "authorDescription": "Author Bio",
                "thumbnailAuthor": "http://ibuildapp.s3-website-us-east-1.amazonaws.com/assets/data/02312/2312789/apps/2568578/36574049/5db19401cc791.jpg",
                "thumbnail": "http://ibuildapp.s3-website-us-east-1.amazonaws.com/assets/data/02312/2312789/apps/2568578/36574049/5db16f3c26da53.34363284.jpg",
                "url": "https://ibuildapp.com",
                "price": "5",
                "category": "Science Fiction",
                "recommended": 1,
                "releases": 0,
                "bestsellers": 0
            },
            {
                "title": "Towers of Midnight",
                "description": "The Immari are good at keeping secrets. For 2,000 years, they've hidden the truth about human evolution. They've also searched for an ancient enemy - a threat that could wipe out the human race. Now the search is over.nOff the coast of Antarctica, a research vessel discovers a mysterious structure buried deep in an iceberg. It has been there for thousands of years, and something is guarding it.",
                "author": "Robert Jordan, Brandon Sanders",
                "authorDescription": "",
                "thumbnailAuthor": "",
                "thumbnail": "http://ibuildapp.s3-website-us-east-1.amazonaws.com/assets/data/02312/2312789/apps/2568578/36574049/5db16f3c5c3067.75405551.jpg",
                "url": "",
                "price": "",
                "category": "Science Fiction",
                "recommended": 0,
                "releases": 0,
                "bestsellers": 0
            },
            {
                "title": "The Girl Dragon Tattoo",
                "description": "The Immari are good at keeping secrets. For 2,000 years, they've hidden the truth about human evolution. They've also searched for an ancient enemy - a threat that could wipe out the human race. Now the search is over.nOff the coast of Antarctica, a research vessel discovers a mysterious structure buried deep in an iceberg. It has been there for thousands of years, and something is guarding it.",
                "author": "Stieg Larsson",
                "authorDescription": "",
                "thumbnailAuthor": "",
                "thumbnail": "http://ibuildapp.s3-website-us-east-1.amazonaws.com/assets/data/02312/2312789/apps/2568578/36574049/5db16f3c76d575.09594041.jpg",
                "url": "",
                "price": "",
                "category": "Science Fiction",
                "recommended": 0,
                "releases": 0,
                "bestsellers": 0
            }
        ]
    }
}
```

Data parameters:<br>
• colorskin - color scheme. Contains 5 elements: background, primary, secondary, text and accent colors. If the default color scheme is used, this parameter is missing.<br>
• menu - an array of menu items, each item has parameters:<br>
    &ensp;&ensp;&ensp;&ensp;◘ name - item name.<br>
    &ensp;&ensp;&ensp;&ensp;◘ tag - item type: Category, Recommended, Releases or Bestsellers.<br>
    &ensp;&ensp;&ensp;&ensp;◘ icon - item icon link.<br>
    &ensp;&ensp;&ensp;&ensp;◘ items (only for Category tag) - an array of categories.<br>
• content - an array of book objects, each book has properties:<br>
    &ensp;&ensp;&ensp;&ensp;◘ title - the book name.<br>
    &ensp;&ensp;&ensp;&ensp;◘ description - the book description.<br>
    &ensp;&ensp;&ensp;&ensp;◘ thumbnail - the book thumbnail link.<br>
    &ensp;&ensp;&ensp;&ensp;◘ price - the book price.<br>
    &ensp;&ensp;&ensp;&ensp;◘ category - the book category.<br>
    &ensp;&ensp;&ensp;&ensp;◘ url - web store URL.<br>
    &ensp;&ensp;&ensp;&ensp;◘ author - author name.<br>
    &ensp;&ensp;&ensp;&ensp;◘ authorDescription - author bio.<br>
    &ensp;&ensp;&ensp;&ensp;◘ thumbnailAuthor - author photo link.<br>
    &ensp;&ensp;&ensp;&ensp;◘ recommended - boolean parameter for filtering by recommended, 1 or 0.<br>
    &ensp;&ensp;&ensp;&ensp;◘ releases - boolean parameter for filtering by releases, 1 or 0.<br>
    &ensp;&ensp;&ensp;&ensp;◘ bestsellers - boolean parameter for filtering by bestsellers, 1 or 0.<br>

### Changelog

| Release       | Notes                            |
|---------------|----------------------------------|
| 1.0 (Current) | Initial release into open source |
|               |                                  |

## Dashboard Widget

### Overview

The widget with main menu of the app.

### Data

> Example

```json
{
    "type": "dashboard",
    "widgetid": "-1",
    "data": {
        "title": "dashboard",
        "image": [
            {
                "x": 185,
                "y": 71,
                "width": 80,
                "height": 144,
                "data": {
                    "#url": "http://ibuildapp.s3-website-us-east-1.amazonaws.com/assets/data/02312/2312789/apps/3495705/images/1571919334.2.png"
                }
            }
        ],
        "button": [
            {
                "x": 20,
                "y": 237,
                "width": 160,
                "height": 40,
                "icon": "http://ibuildapp.s3-website-us-east-1.amazonaws.com/assets/data/02312/2312789/apps/3495705/buttons/1571919334.3.png",
                "#label": "Button1",
                "#title": "Button1",
                "size": 18,
                "align": "center",
                "color": "#ffffff",
                "style": "normal",
                "widget_id": "36575814",
                "type": "htmlcode"
            },
            {
                "x": 157,
                "y": 305,
                "width": 160,
                "height": 40,
                "icon": "http://ibuildapp.s3-website-us-east-1.amazonaws.com/assets/data/02312/2312789/apps/3495705/buttons/1571919334.4.png",
                "#label": "Button2",
                "#title": "Button2",
                "size": 18,
                "align": "center",
                "color": "#ffffff",
                "style": "normal",
                "widget_id": "36575815",
                "type": "htmlcode"
            }
        ],
        "label": [
            {
                "x": 21,
                "y": 76,
                "width": 160,
                "height": 40,
                "data": {
                    "#title": "My label",
                    "size": 18,
                    "align": "center",
                    "color": "#999999",
                    "style": "normal"
                }
            }
        ]
    }
}
```

Data parameters:<br>
• title - widget name.<br>
• image - an array of the dashboard images, each image has properties:<br>
    &ensp;&ensp;&ensp;&ensp;◘ x - x-offset of the image.<br>
    &ensp;&ensp;&ensp;&ensp;◘ y - y-offest of the image.<br>
    &ensp;&ensp;&ensp;&ensp;◘ width - image width.<br>
    &ensp;&ensp;&ensp;&ensp;◘ height - image height.<br>
    &ensp;&ensp;&ensp;&ensp;◘ data.\#url - image link.<br>
• button - an array of dashboard buttons, each button has properties:<br>
    &ensp;&ensp;&ensp;&ensp;◘ x - x-offset of the button.<br>
    &ensp;&ensp;&ensp;&ensp;◘ y - y-offest of the button.<br>
    &ensp;&ensp;&ensp;&ensp;◘ width - button width.<br>
    &ensp;&ensp;&ensp;&ensp;◘ height - button height.<br>
    &ensp;&ensp;&ensp;&ensp;◘ icon - icon URL.<br>
    &ensp;&ensp;&ensp;&ensp;◘ \#label - button label.<br>
    &ensp;&ensp;&ensp;&ensp;◘ \#title - button title.<br>
    &ensp;&ensp;&ensp;&ensp;◘ size - button font size.<br>
    &ensp;&ensp;&ensp;&ensp;◘ align - button text align.<br>
    &ensp;&ensp;&ensp;&ensp;◘ color - button text color.<br>
    &ensp;&ensp;&ensp;&ensp;◘ style - button text style: normal, bol, italic or italic bold.<br>
    &ensp;&ensp;&ensp;&ensp;◘ widget_id - id of the widget running by this button.<br>
    &ensp;&ensp;&ensp;&ensp;◘ type - widget type.<br>
• label - an array of dashboard labels, each label has properties:<br>
    &ensp;&ensp;&ensp;&ensp;◘ x - x-offset of the label.<br>
    &ensp;&ensp;&ensp;&ensp;◘ y - y-offest of the label.<br>
    &ensp;&ensp;&ensp;&ensp;◘ width - label width.<br>
    &ensp;&ensp;&ensp;&ensp;◘ height - label height.<br>
    &ensp;&ensp;&ensp;&ensp;◘ data - contains button properties, such as:<br>
        &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;• \#title - label title.<br>
        &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;• size - label font size.<br>
        &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;• align - label text align.<br>
        &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;• color - label text color.<br>
        &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;• style - label text style: normal, bol, italic or italic bold.<br>

### Changelog

| Release       | Notes                            |
|---------------|----------------------------------|
| 1.0 (Current) | Initial release into open source |
|               |                                  |

## Photogallery Widget

### Overview

PhotoGallery widget is intended for images viewing, placed into albums, which links are defined in XML configuration of a widget.
General features:
viewing images/albums;
making comments on images;
"Share" feature using Facebook, Twitter, SMS, Email;
"Like" feature using Facebook account.

### Data

> Example

```json
{
    "widgetid": "36573880",
    "type": "photogallery",
    "data": {
        "title": "Photos & Instagram",
        "allowsharing": "on",
        "allowcomments": "on",
        "allowsaving": "on",
        "style": {
            "columns": 2,
            "type": "list"
        },
        "colorskin": {
            "background": "#000000",
            "primary": "#ffffff",
            "secondary": "#ffffff",
            "text": "#ffffff",
            "accent": "#ff6f28"
        },
        "album": [
            {
                "#title": "My album",
                "#allowcomments": "on",
                "#allowsharing": "on",
                "#allowsaving": "on",
                "#url": "",
                "#id": 1648263286121670,
                "#cover": "http://ibuildapp.s3-website-us-east-1.amazonaws.com/assets/data/02312/2312789/apps/2568578/36573880/upload-5db16338d30ad6.jpg",
                "image": [
                    {
                        "#title": "image1",
                        "#description": "description1",
                        "#url": "http://ibuildapp.s3-website-us-east-1.amazonaws.com/assets/data/02312/2312789/apps/2568578/36573880/upload-5db16338d30ad6.jpg",
                        "thumbnail": {
                            "color": "#e0d8cb",
                            "url": "http://ibuildapp.s3-website-us-east-1.amazonaws.com/assets/data/02312/2312789/apps/2568578/36573880/thumb_upload-5db16338d30ad6.jpg"
                        },
                        "#cover": "http://ibuildapp.s3-website-us-east-1.amazonaws.com/assets/data/02312/2312789/apps/2568578/36573880/upload-5db16338d30ad6.jpg",
                        "#id": 1648263286121875
                    },
                    {
                        "#title": "image2",
                        "#description": "description2",
                        "#url": "http://ibuildapp.s3-website-us-east-1.amazonaws.com/assets/data/02312/2312789/apps/2568578/36573880/upload-5db16338ec2d67.jpg",
                        "thumbnail": {
                            "color": "#fefefe",
                            "url": "http://ibuildapp.s3-website-us-east-1.amazonaws.com/assets/data/02312/2312789/apps/2568578/36573880/thumb_upload-5db16338ec2d67.jpg"
                        },
                        "#cover": "http://ibuildapp.s3-website-us-east-1.amazonaws.com/assets/data/02312/2312789/apps/2568578/36573880/upload-5db16338d30ad6.jpg",
                        "#id": 1648263286334677
                    }
                ]
            }
        ]
    }
}
```

Data parameters:<br>
• title - widget name. Title is being displayed on navigation panel when widget is launched.<br>
• colorskin - color scheme. Contains 5 elements: background, primary, secondary, text and accent colors. If the default color scheme is used, this parameter is missing.<br>
• allowsharing - parameter allowing/disallowing to share image in social networks or by email.<br>
Values:<br>
on - enabled;<br>
off - disabled.<br>
• allowcomments - parameter allowing/disallowing for app users make comments for images.<br>
Values:<br>
on - enabled;<br>
off - disabled.<br>
• allowsaving - parameter allowing/disallowing to save images on mobile device.<br>
Values:<br>
on - enabled;<br>
off - disabled.<br>
• style - albums displaying parameters:<br>
    &ensp;&ensp;&ensp;&ensp;◘ columns - number of columns: 2, 3 or 4.<br>
    &ensp;&ensp;&ensp;&ensp;◘ type - grid or list.<br>
• album - an array of album objects, each object contains properties:<br>
    &ensp;&ensp;&ensp;&ensp;◘ \#title - album title.<br>
    &ensp;&ensp;&ensp;&ensp;◘ \#url - media URL, containing images. Specified optionally.<br>
    &ensp;&ensp;&ensp;&ensp;◘ \#id - album identifier in iBuildApp system.<br>
    &ensp;&ensp;&ensp;&ensp;◘ \#cover - album cover image URL.<br>
    &ensp;&ensp;&ensp;&ensp;◘ \#allow_sharing - parameter defining opportunity to share image from album in social networks or by email.<br>
    Values:<br>
    on - enabled;<br>
    off - disabled.<br>
    &ensp;&ensp;&ensp;&ensp;◘ \#allow_comments - parameter allowing/disallowing for app users make comments for images in album.<br>
    Values:<br>
    on - enabled;<br>
    off - disabled.<br>
    &ensp;&ensp;&ensp;&ensp;◘ \#allow_saving - parameter allowing/disallowing to save images from album on mobile device.<br>
    Values:<br>
    on - enabled;<br>
    off - disabled.<br>
    &ensp;&ensp;&ensp;&ensp;◘ image - an array of image objects in the albumdeclares image in album, each image has properties:<br>
        &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;• \#description - short image description.<br>
        &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;• \#title - image title.<br>
        &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;• \#id - image identifier in iBuildApp system.<br>
        &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;• \#url - contains link on resource with image.<br>
        &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;• \#cover - image cover URL.<br>
        &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;• thumbnail - an image thumbnail object with properties:<br>
            &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;◘ color - RGB color.<br>
            &ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;&ensp;◘ url - thumbnail URL.<br>

### Changelog

| Release       | Notes                            |
|---------------|----------------------------------|
| 1.0 (Current) | Initial release into open source |
|               |                                  |

## AudioListWidget

### Overview

Audio widget is implemented to play back audios. Audio links are provided in widget XML-configuration.
General features:
audio play back;
commenting;
share function via Facebook, Twitter, SMS, Email;
Facebook like function.
Widget implementation:
mAudioPlayer.a library (iOS);
AudioPlugin library (Android).

### Data

> Example

```json
{
    "widgetid": "36567915",
    "type": "audioplayer",
    "data": {
        "title": "Audio List",
        "module_id": "9",
        "allowsharing": "on",
        "allowcomments": "on",
        "#cover_image": "http://ibuildapp.s3-website-us-east-1.amazonaws.com/assets/data/02312/2312789/apps/2568578/36567915/1571836533.png",
        "colorskin": {
            "background": "#000000",
            "primary": "#ffffff",
            "secondary": "#ffffff",
            "text": "#ffffff",
            "accent": "#ff6f28"
        },
        "track": [
            {
                "#title": "My track",
                "#description": "My track description",
                "#cover_image": "http://ibuildapp.s3-website-us-east-1.amazonaws.com/assets/data/02312/2312789/apps/2568578/36567915/1571836649.png",
                "#permalink_url": "http://www.WebSite.com/MyMusicFile.mp3",
                "#stream_url": "http://www.WebSite.com/MyMusicFile.mp3",
                "#id": 1648190169118250
            },
            {
                "#title": "My stream",
                "#description": "My stream description",
                "#cover_image": "",
                "#permalink_url": "http://dir.xiph.org/listen/190856/listen.m3u",
                "#stream_url": "http://master01-ent.shoutcast.com:8000/foxnews",
                "#id": 1648190303988620
            }
        ]
    }
}
```

Data parameters:<br>
• title - widget name. Title is being displayed on navigation panel when widget is launched.<br>
• colorskin - color scheme. Contains 5 elements: background, primary, secondary, text and accent colors. If the default color scheme is used, this parameter is missing.<br>
• module_id - widget identifier in iBuildApp system.<br>
• allow_sharing - parameter defining the opportunity to share audio in social networks or by email.<br>
Values:<br>
on - enabled;<br>
off - disabled.<br>
• allow_comments - parameter defining the opportunity for app users comment audios.<br>
Values:<br>
on - enabled;<br>
off - disabled.<br>
• \#cover_image - cover image URL.<br>
• track - an array of track objects. Each object has parameters:<br>
    &ensp;&ensp;&ensp;&ensp;◘ \#stream_url - audio resource URL (audio link, audio flow).<br>
    &ensp;&ensp;&ensp;&ensp;◘ \#description - track description.<br>
    &ensp;&ensp;&ensp;&ensp;◘ \#title - track title.<br>
    &ensp;&ensp;&ensp;&ensp;◘ \#id - audio ID in iBuildApp system.<br>
    &ensp;&ensp;&ensp;&ensp;◘ \#permalink_url - track resource link to be used for sharing.<br>
    &ensp;&ensp;&ensp;&ensp;◘ \#cover_image - track thumbnail URL.<br>

### Changelog

| Release       | Notes                            |
|---------------|----------------------------------|
| 1.0 (Current) | Initial release into open source |
|               |                                  |

## VideoList Widget

### Overview

Video widget is implemented to play back videos. Video links are provided in widget XML-configuration.
General features:
video play back;
commenting;
share function via Facebook, Twitter, SMS, Email;
Facebook like function.
Widget implementation:
mVideoPlayer.a library (iOS);
VideoPlugin library (Android).

### Data

> Example

```json
{
    "widgetid": "36563353",
    "type": "videoplayer",
    "data": {
        "title": "Video List",
        "module_id": "9",
        "allowsharing": "on",
        "allowcomments": "on",
        "allowlikes": "on",
        "colorskin": {
            "background": "#000000",
            "primary": "#ffffff",
            "secondary": "#ffffff",
            "text": "#ffffff",
            "accent": "#ff6f28"
        },
        "video": [
            {
                "#url": "http://www.youtube.com/watch?v=ArpfZWW_-IQ",
                "#description": "\"Вступительный ролик Starcraft II: Wings of Liberty был показан на салоне Blizzard Worldwide Invitational в 2007 в Сеуле.\nhttp://StarCraft.com\"",
                "#title": "Тизер StarCraft II (RU)",
                "#duration": "00:04:01",
                "#creation_time": "1281122058",
                "#cover": "https://i.ytimg.com/vi/ArpfZWW_-IQ/maxresdefault.jpg",
                "#id": 1648164510185175
            },
            {
                "#url": "http://vimeo.com/70479332",
                "#description": "Create your own site design",
                "#title": "Reseller Site Customization",
                "#duration": "00:01:09",
                "#creation_time": "1374051720",
                "#cover": "http://i.vimeocdn.com/video/443754212_640.jpg",
                "#id": 1648164549686466
            }
        ]
    }
}
```

Data parameters:<br>
• title - widget name. Title is being displayed on navigation panel when widget is launched.<br>
• colorskin - color scheme. Contains 5 elements: background, primary, secondary, text and accent colors. If the default color scheme is used, this parameter is missing.<br>
• module_id - widget identifier in iBuildApp system.<br>
• allowsharing - parameter defining the opportunity to share video in social networks or by email.<br>
Values:<br>
on - enabled;<br>
off - disabled.<br>
• allowcomments - parameter defining the opportunity for app users to comment videos.<br>
Values:<br>
on - enabled;<br>
off - disabled.<br>
• allowlikes - parameter defining the opportunity for app users to like videos.<br>
Values:<br>
on - enabled;<br>
off - disabled.<br>
• video - contains an array of video objects, each video has parameters:<br>
    &ensp;&ensp;&ensp;&ensp;◘ \#url - video resource URL (video link, video flow link, Youtube page link or Vimeo page link).<br>
    &ensp;&ensp;&ensp;&ensp;◘ \#description - video description.<br>
    &ensp;&ensp;&ensp;&ensp;◘ \#title - video title.<br>
    &ensp;&ensp;&ensp;&ensp;◘ \#id - video ID in iBuildApp system.<br>
    &ensp;&ensp;&ensp;&ensp;◘ \#duration - video duration.<br>
    &ensp;&ensp;&ensp;&ensp;◘ \#creation_time - video creation timestamp.<br>
    &ensp;&ensp;&ensp;&ensp;◘ \#cover - video cover.<br>

### Changelog

| Release       | Notes                            |
|---------------|----------------------------------|
| 1.0 (Current) | Initial release into open source |
|               |                                  |

<!-- ## Widget

### Overview

### Data

> Example

```json

```
### Changelog -->

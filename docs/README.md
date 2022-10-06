# BBjDrawer Widget

<p>
  <a href="http://www.basis.cloud/downloads">
    <img src="https://img.shields.io/badge/BBj-v22.04-blue" alt="BBj v22.04" />
  </a>
  <a href="https://github.com/BBj-Plugins/BBjDrawer/blob/master/README.md">
    <img src="https://img.shields.io/badge/license-MIT-blue.svg" alt="BBjDrawer is released under the MIT license." />
  </a>
  <a href="https://github.com/necolas/issue-guidelines/blob/master/CONTRIBUTING.md#pull-requests">
    <img src="https://img.shields.io/badge/PRs-welcome-brightgreen.svg" alt="PRs welcome!" />
  </a>
   <a href="https://basishub.github.io/basis-next/#/dwc/bbj-drawer">
    <img src="https://img.shields.io/badge/Component-bbj--drawer-%23006aff" alt="Tag Name">
  </a>
</p>

<!-- Document Links: -->
[bbjchildwindow]: https://documentation.basis.cloud/BASISHelp/WebHelp/bbjobjects/Window/bbjchildwindow/bbjchildwindow.htm
[bbjcontrol]: https://documentation.basis.cloud/BASISHelp/WebHelp/bbjobjects/Window/bbjwindow/bbjwindow.htm
[css-vars]: https://basishub.github.io/basis-next/#/theme-engine/css-variables

`BBjDrawer` is a custom BBj Widget for the DWC (Dynamic Web Client) that allow developers to create a container which slides into the viewport to expose additional options and information.

There is no limit on the number of drawers that an application can create. In this case drawers will be stacked above each other.

## Installation

- Clone the [project](https://github.com/BBj-Plugins/BBjDrawer) locally , then add `BBjDrawer` to your BBj paths
- Or [Use the plugins manager](https://www.bbj-plugins.com/en/get-started)

## Features

- Easy to set up
- Easy to customize
- Stack Drawers
- Ability to position in different locations
- Responsive
- Works with the DWC Dark mode 🌒

And much more !

## The gist

The following sample shows how to use the `BBjDrawer` widget. The widget will provide a container which is 
an instance of [`BBjChildWindow`][bbjchildwindow] which can contain any valid [BBjControl][bbjcontrol].

```bbj
use ::BBjDrawer/BBjDrawer.bbj::BBjDrawer

welcomePage! = new BBjDrawer(wnd!)
welcomePage!.setPlacement(BBjDrawer.PLACEMENT_BOTTOM_CENTER)
welcomePage!.setDrawerSize("90vh")

container! = welcomePage!.getContainer()
container!.addStyle("welcome-page")

container!.addImageCtrl("./assets/fun.svg")
container!.addStaticText("<html><h2>Welcome to DWC</h2></html>")
container!.addStaticText("<html><p>Lorem Ipsum is simply dummy.</p></html>")
btn! = container!.addButton("Get Started")
btn!.setCallback(BBjAPI.ON_BUTTON_PUSH,"toggleDrawer")
btn!.setAttribute("theme","primary")
btn!.setAttribute("expanse","l")

welcomePage!.open()

toggleDrawer:
  welcomePage!.toggle()
return
```

<div class="demo demo--phone">

  <div class="demo__preview">
    <div class="demo__preview__content">
      <iframe src="./demo/welcome-page.html"></iframe>
    </div>
  </div>

  <div class="demo__buttons">
    <a  data-action-toggle-code>
      <bbj-icon name="chevron-down"></bbj-icon> Show code
    </a>
    <a data-action-open-link>
      <bbj-icon name="external-link"></bbj-icon> Open in new tab
    </a>  
  </div>

  <div class="demo__code">

```bbj line-numbers
style! = "
: body,html {overflow: hidden}
:
: .bbj-toolbar {
:    display: flex;
:    align-items: center;
:    gap: var(--bbj-space-m);
:    padding: 0 var(--bbj-space-m);
: }
:
: .bbj-logo {
:    display: flex;
:    align-items: center;
:    justify-content: center;
:    padding: var(--bbj-space-m) 0;
:    margin-bottom: var(--bbj-space-m);
:    border-bottom: thin solid var(--bbj-color-default)
: }
:
: .bbj-logo img {
:    max-width: 100px;
: }
:
: .welcome-page {
:    display: flex;
:    flex-direction: column;
:    align-items: center;
:    justify-content: center;
:    padding: var(--bbj-space-2xl);
:    width: 100%;
: }
:
: .welcome-page img {
:    max-height: 300px;
: }
:
: .welcome-page p {
:    text-align: center;
: }
:"

use ::BBjAppLayout/BBjAppLayout.bbj::BBjAppLayout
use ::BBjDrawer/BBjDrawer.bbj::BBjDrawer

web! = BBjAPI().getWebManager()
rem The app should include a viewport meta tag which contains `viewport-fit=cover`, like the following.
rem This causes the viewport to be scaled to fill the device display.
web!.setMeta("viewport", "width=device-width, initial-scale=1.0, viewport-fit=cover, user-scalable=no")
web!.injectStyle(style!, 0)

sysgui! =  BBjAPI().openSysGui("X0")

wnd! = sysgui!.addWindow("BBjDrawer", $01001000$)
wnd!.setCallback(BBjAPI.ON_CLOSE,"eoj")

app! = new BBjAppLayout(wnd!)

rem Header
rem ==================
header! = app!.getHeader()
toolbar! = header!.addChildWindow("", $00108800$, sysgui!.getAvailableContext())
toolbar!.addStyle("bbj-toolbar")
toolbar!.addStaticText("<html><bbj-icon-button name='menu-2' data-drawer-toggle></bbj-icon-button></html>")
toolbar!.addStaticText("<html><h3>DWC Application</h3></html>")

rem Drawer
rem ==================
drawer! = app!.getDrawer()
drawer!.addStyle("bbj-drawer")

logo! = drawer!.addImageCtrl("./assets/logo.png")
logo!.addStyle("bbj-logo")

empty! = drawer!.addChildWindow("", $00000810$, sysgui!.getAvailableContext())
drawerMenu! = drawer!.addTabCtrl()
drawerMenu!.setCallback(drawerMenu!.ON_TAB_SELECT,"onPageChanged")
drawerMenu!.setAttribute("nobody","true")
drawerMenu!.setAttribute("borderless","true")
drawerMenu!.setAttribute("placement","left")
drawerMenu!.addTab("<bbj-icon name='dashboard'></bbj-icon>      Dashboard"    , empty!)
drawerMenu!.addTab("<bbj-icon name='shopping-cart'></bbj-icon>  Orders"       , empty!)
drawerMenu!.addTab("<bbj-icon name='users'></bbj-icon>          Customers"    , empty!)
drawerMenu!.addTab("<bbj-icon name='box'></bbj-icon>            Products"     , empty!)
drawerMenu!.addTab("<bbj-icon name='files'></bbj-icon>          Documents"    , empty!)
drawerMenu!.addTab("<bbj-icon name='checklist'></bbj-icon>      Tasks"        , empty!)
drawerMenu!.addTab("<bbj-icon name='chart-dots-2'></bbj-icon>   Analytics"    , empty!)

rem Content
rem ==================
content! = app!.getContent()
content!.addStaticText("<html><h1>Application Title</h1></html>")
pageContent! = content!.addStaticText("<html><p>Content for Dashboard goes here</p></html>")

btn! = content!.addButton("Open Welcome Page")
btn!.setCallback(BBjAPI.ON_BUTTON_PUSH,"toggleDrawer")

rem Welcome Page
rem ==================
welcomePage! = new BBjDrawer(wnd!)
welcomePage!.setPlacement(BBjDrawer.PLACEMENT_BOTTOM_CENTER)
welcomePage!.setDrawerSize("90vh")

container! = welcomePage!.getContainer()
container!.addStyle("welcome-page")
container!.addImageCtrl("./assets/fun.svg")
container!.addStaticText("<html><h2>Welcome to DWC</h2></html>")
container!.addStaticText("<html><p>Lorem Ipsum is simply dummy text of the printing and typesetting industry.</p></html>")
btn! = container!.addButton("Get Started")
btn!.setCallback(BBjAPI.ON_BUTTON_PUSH,"toggleDrawer")
btn!.setAttribute("theme","primary")
btn!.setAttribute("expanse","l")

rem give the drawer sometime to load
wait 1
welcomePage!.open()

process_events

onPageChanged:
  event! = sysgui!.getLastEvent() 
  title$ = event!.getTitle().replaceAll("<[^>]*>","").trim()

  pageContent!.setText("<html><p>Content for " + title$ + " goes here</p></html>")
return


toggleDrawer:
  welcomePage!.toggle()
return

eoj:
release
```
  </div>
</div>

## Drawer Placement

The drawer can be placed in six different positions:

1. `BBjDrawer.PLACEMENT_LEFT`
2. `BBjDrawer.PLACEMENT_RIGHT`
3. `BBjDrawer.PLACEMENT_TOP`
4. `BBjDrawer.PLACEMENT_TOP_CENTER`
5. `BBjDrawer.PLACEMENT_BOTTOM`
6. `BBjDrawer.PLACEMENT_BOTTOM_CENTER`

```bbj
use ::BBjDrawer/BBjDrawer.bbj::BBjDrawer

welcomePage! = new BBjDrawer(wnd!)
welcomePage!.setPlacement(BBjDrawer.PLACEMENT_BOTTOM_CENTER)
```

## Drawer Sizing

It is possible to configure the drawer size using the `BBjDrawer` methods

1. **`BBjDrawer::setDrawerSize(BBjString size!)`**: If the drawer is placed at `left` or `right`, the size will be the width and If the drawer is placed at `top` or `bottom`, the size will be the height

2. **`BBjDrawer::setDrawerMaxSize(BBjString size!)`**: If the drawer is placed at `left` or `right`, the size will be the max width and If the drawer is placed at `top` or `bottom`, the size will be the max height

```bbj
use ::BBjDrawer/BBjDrawer.bbj::BBjDrawer

welcomePage! = new BBjDrawer(wnd!)
welcomePage!.setDrawerSize("90vh")
```

When using the the drawer `BBjDrawer.PLACEMENT_BOTTOM_CENTER` or `BBjDrawer.PLACEMENT_TOP_CENTER` it is possible to set the max-with using the [CSS custom property][css-vars] `--bbj-drawer-max-width`. By default the max size will be `576px`

## Events

The `BBjDrawer` supports three events:

1. `ON_OPENED`: Fired when the drawer is opened.
2. `ON_CLOSED`: Fired when the drawer is closed.
3. `ON_TOGGLED`: Fired when the drawer is toggled.

The event's payload is an instance of the `::BBjDrawer/BBjDrawer.bbj::BBjDrawerEvent`

```bbj
use ::BBjDrawer/BBjDrawer.bbj::BBjDrawer
use ::BBjDrawer/BBjDrawer.bbj::BBjDrawerEvent

drawer! = new BBjDrawer(wnd!)
drawer!.setCallback(BBjDrawer.ON_TOGGLED, "onDrawerToggled")

onDrawerToggled:
  declare auto BBjDrawerEvent payload!
  event! = sysgui!.getLastEvent()
  payload! = event!.getObject()

  let x = MSGBOX("Is Opened = " + str(payload!.isrOpened()), 0, "Drawer Toggled")
return 
```
  </div>
</div>

## Contact Picker Sample

The following sample shows how to use the `BBjDrawer` widget to create a contact picker using the `ChileCompany` 
customers data.

<div class="demo demo--phone">

  <div class="demo__preview">
    <div class="demo__preview__content">
      <iframe src="./demo/contact-picker.html"></iframe>
    </div>
  </div>

  <div class="demo__buttons">
    <a  data-action-toggle-code>
      <bbj-icon name="chevron-down"></bbj-icon> Show code
    </a>
    <a data-action-open-link>
      <bbj-icon name="external-link"></bbj-icon> Open in new tab
    </a>  
  </div>

  <div class="demo__code">

```bbj line-numbers
style! = "
: .root {
:   display: flex;
:   align-items: center;
:   justify-content: center;
:   width: 100vw;
:   height: 100vh;
: }
:  
: .contactsList {
:   padding: var(--bbj-space-m);
: }
:
: .bookEntry {
:   display: flex;
:   gap: 1rem;
:   align-items: center;
:   padding: var(--bbj-space-s);
:   cursor: var(--bbj-cursor-click);
:   transition: background-color var(--bbj-transition);
:   border-bottom: thin solid var(--bbj-color-default);
:   border-style: solid !important; 
: }
:
: .bookEntry:hover {
:   background-color: var(--bbj-color-primary-alt);
: }
:
: .bookEntry__avatar {
:   display: flex;
:   align-items: center;
:   width: var(--bbj-size-l);
:   height: var(--bbj-size-l);
:   border-radius: var(--bbj-border-radius-round);
: }
:
: .bookEntry__avatar img {
:   width: 100%;
:   height: 100%;
: }
:
: .bookEntry__info {
:   flex: 1;
:   display: flex;
:   flex-direction: column;
: }
:
: .bookEntry__location {
:   font-size: var(--bbj-font-size-s);
:   color: var(--bbj-color-default-text);
: }  
:"

use ::BBjDrawer/BBjDrawer.bbj::BBjDrawer
use com.basiscomponents.db.ResultSet
use com.basiscomponents.bc.SqlQueryBC

web! = BBjAPI().getWebManager()
web!.injectStyle(style!, 0)

sysgui! =  BBjAPI().openSysGui("X0")

wnd! = sysgui!.addWindow("BBjDrawer", $01101000$)
wnd!.addPanelStyle("root")
wnd!.setCallback(BBjAPI.ON_CLOSE,"eoj")

contactsButton! = wnd!.addButton("Open Contacts Picker")
contactsButton!.setCallback(BBjAPI.ON_BUTTON_PUSH,"onOpenContactsDrawer")
contactsButton!.setAttribute("theme", "primary")
contactsButton!.setAttribute("expanse", "l")

rem Contacts Drawer
rem ==================
contactsDrawer! = new BBjDrawer(wnd!)
contactsDrawer!.setPlacement(BBjDrawer.PLACEMENT_BOTTOM_CENTER)
contactsDrawer!.setDrawerSize("70vh")

container! = contactsDrawer!.getContainer()
container!.addStyle("contactsList")

sql! = new SqlQueryBC(BBjAPI().getJDBCConnection("ChileCompany"))
items! = sql!.retrieve("SELECT * FROM CUSTOMER")
iterator! = items!.iterator()

while iterator!.hasNext()
  item! = iterator!.next()
  firstName! = item!.getField("FIRST_NAME").getString().trim()
  lastName! = item!.getField("LAST_NAME").getString().trim()
  fullName! = firstName! + " " + lastName!
  phone! = item!.getField("PHONE").getString().trim()
  country! = item!.getField("COUNTRY").getString().trim()
  city! = item!.getField("CITY").getString().trim()
  fullLocation! = country! + " - " + city!

  card! = container!.addChildWindow("", $00108800$, sysgui!.getAvailableContext())
  card!.addStyle("bookEntry")

  avatarContent! = "<html><img src=""https://ui-avatars.com/api/?name=" + fullName! + "&&background=random"" /></html>"
  avatar! = card!.addStaticText(avatarContent!)
  avatar!.addStyle("bookEntry__avatar")

  info! = card!.addChildWindow("", $00108800$, sysgui!.getAvailableContext())
  info!.addStyle("bookEntry__info")

  name! = info!.addStaticText(fullName!)
  name!.addStyle("bookEntry__name")

  location! = info!.addStaticText(fullLocation!)
  location!.addStyle("bookEntry__location")

  call! = card!.addButton("<html><bbj-icon name=""phone""></bbj-icon></html>")
  call!.setEnabled(len(phone!) > 0)
  call!.setUserData(phone!)
  call!.setCallback(call!.ON_BUTTON_PUSH, "onCall")
wend

wait 1
contactsDrawer!.open()

process_events

onOpenContactsDrawer:
  contactsDrawer!.open()
return

onCall:
  contactsDrawer!.close()
  
  ev! = BBjAPI().getLastEvent()
  control! = ev!.getControl()
  phone! = str(control!.getUserData())
  script! = "" +
:    "(() => {" +
:    " const link = document.createElement('a');" +
:    " link.href='tel:" + phone! + "';" +
:    " link.style.visibility='hidden';" +
:    " document.body.appendChild(link);" +
:    " link.click();" +
:    " document.body.removeChild(link)" +
:    "})()"
  BBjAPI().getSysGui().executeScript(script!)
return

eoj:
release
```
  </div>
</div>
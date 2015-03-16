# Main Navigation #

{{Spec|Definition}}
Web based icon-bar navigation system.

{{Spec|Responsibilities}}
  * Provides an icon based navigation bar
  * Switches between main pages when an icon is clicked
  * Animates page transitions
  * Saves user preferences on page positions and sizes
  * Adjusts size/position on browser window resize and font size changes

{{Spec|Constraints}}
  * Must work in IE6+, Firefox 1.5+, Opera 9 and Safari
  * All font sizes must be relative (% or ''em'')
  * Widget sizes, positions, etc should all use ''em'' units. http://www.w3schools.com/css/css_units.asp

{{Spec|Composition}}
Two main components: Navigator and Page.
  * '''Navigator'''
    * Add a page/icon to the Navigator
    * Only one page/icon is active at a time
  * '''Page'''
    * Section of the site
    * Adjustable font size
  * '''PageProperties'''
    * A collection of String key/String value pairs
    * Used to save/restore the Page's current state

{{Spec|Uses/Interactions}}
  * '''Navigator'''
    * Adding a page/icon
    * Removing a page/icon
    * User clicks activate a page

{{Spec|Resources}}
  * [CSS Units](http://www.w3schools.com/css/css_units.asp)
  * [GWT](http://code.google.com/webtoolkit/)
  * [GWT Widgets Library (GWL)](http://gwt-widget.sourceforge.net/)
  * [Scriptaculous](http://script.aculo.us/)

In com.geewhizfoundation there is a file called FoundationSite.gwt.xml

This file describes parameters for GWT hosted mode. The only thing that may need to change frequently are these lines:



&lt;servlet path="/foundation/navigation" class="com.geewhizfoundation.server.navigation.NavigationServlet"/&gt;



This causes the embedded Tomcat server to start a servlet on the given "path" using the specified "class". Note, that this class must specify the fully qualified path name to the servlet used.

For more information or to see more options, visit this link: [GWT Modules](http://code.google.com/webtoolkit/documentation/com.google.gwt.doc.DeveloperGuide.Fundamentals.html#Modules)


{{Spec|Processing}}
  * ''Initialization''
    * In the entry point, all main pages must be registered with the Navigator
  * ''Login''
    * When user is logged in, the login system calls the Navigator's ''restoreSession()'' with the current account key, and the Navigator then ''restore()''s all pages.
  * '''Navigator'''
    * A Page is registered with the Navigator in one of the following sections:
      * **'''PLAY''': All game play areas of the foundation (On the ''left'' side of the bar)
      *** '''COMMUNITY''': All "Community" areas, such as forums and chat (In the ''center'' of the bar)
      * **'''CREATE''': All game creation areas, such as level design and assets (In the ''right' side of the bar)
    * All icons are ordered from left to right in their respective sections based on when they were added to the Navigator
    * The Navigator has 2 main visual components
      *** The ''navigation bar'', which takes up a small horizontal strip at the bottom of the browser
      * **The ''display panel'', which fills the rest of the space in the browser
    * When the user clicks an icon, the selected Page is moved to the front (and made visible if necessary)
      *** The new Page preference is saved, so that it is restored when the user returns
  * '''Page'''
    * The DOM ID string is used in the CSS to set position/size defaults
      * **DOM ID '''must''' be unique. The Page will verify that the ID is unique and throw a runtime exception if it is not.
    * All Pages will always be attached to the DOM, so any updates (such as chat) can be made to Pages at any time
    * Pages are filled with GWT Widgets http://code.google.com/webtoolkit/documentation/com.google.gwt.user.client.ui.html
    * When the user modifies a Page, a dirty flag should be set, which is returned when ''Page.isDirty()'' is called.
      *** If a page is dirty, its ''save()'' method will be called with the user/Page's PageProperties object.
  * ''CSS Design''
    * Positioning and size should always be ''relative'' to parent Pages (The CSS Page class will set this property)
    * When writing CSS code for widgets, you may want to use CSS ''em'' units instead of ''px'' or ''%''. This will resize the widget based on the user selected font, so that the proportions of the widgets fit the new font size.
    * Pages should always be 100% x 100%

{{Spec|Interface/Exports}}
  * '''Navigator'''
    * ```
void addPage(int section, Page page, Image icon)``` - Add a Page with the given icon to the specified section
      * ```
Navigator.PLAY``` - Play section
      * ```
Navigator.COMMUNITY``` - Community section
      * ```
Navigator.CREATE``` - Creation section
    * ```
void removePage(Page page)``` - Remove the Page from the Navigator
    * ```
void restoreSession(String accountID)``` - Restore the given account's (user) session/Pages
  * '''Page''' - ''abstract, extends Widget''
    * ```
Page(String id)``` - Create a page with the specified DOM ID
    * ```
abstract boolean isDirty()``` - Called to see if the page needs to be saved
    * ```
abstract void save(PageProperties properties)``` - Save the contents of this Page
    * ```
abstract void restore(PageProperties properties)``` - Restore the contents of this Page

{{Spec|Unit Tests}}

Common Use Cases
  * Adding pages
    * Adding a PLAY page
    * Adding a COMMUNITY page
    * Adding a CREATE page
  * Removing pages
    * Removing a PLAY page
    * Removing a COMMUNITY page
    * Removing a CREATE page
  * Saving a session
    * Saving Page parameters
  * Restoring a session
    * Restoring Page parameters
  * ''click()''ing on Page icons brings the page to the front (active)
Uncommon Use Cases
Edge Cases
  * Adding a million Pages
Corner Cases
Illegal Cases
  * Creating a Page with a ```
null``` ID
  * Creating a Page with an empty ID
  * Adding a ```
null``` Page
  * Adding a page twice
  * Removing a page that does not exist
    * Removing a page twice

# Forums #
{{Spec|Definition}}
Community message board system. Logged in users can read and post topics/replies in categorized forums.
{{Spec|Responsibilities}}
  * Provide message topics categorized into forums
    * One forum per game
    * Several generic forums
  * Allow users to read existing messages
    * Count user views of a given topic
    * Allow users to sort topics by:
      * **Subject
      *** Topic Creator
      * **Date/time posted
      *** Views
      * **Number of replies
      *** Date/time of last reply
      * **Last poster
  * Allow users to post new topics
    * Force users to enter a subject
  * Allow users to post replies
  * Provide a simple message editor
    * Recognize basic "safe" old-skool HTML (&lt;b&gt;, &lt;i&gt;, &lt;u&gt;, &lt;em&gt;, &lt;strong&gt;, etc) and ignore all others
    * Recognize some BBCode ([b](b.md), [i](i.md), [u](u.md), etc)
    * Provide buttons for basic formatting, like '''bold''', ''italics'',**<u>underlining</u> and stupid things like <strike>strikethrough</strike>
{{Proposed|Provide users with a mechanism to report a message as abuse/spam}}

{{Spec|Constraints}}

  * Forum titles will be no longer than 256 characters
  * Topic titles will be no longer than 256 characters
  * Messages will be no longer than 64kB

{{Spec|Composition}}
  * '''ForumsWindow''' extends Window
  * '''Forum'''
    * Title
    * List of Moderators
  * '''Topic'''
    * User that created the topic
    * Subject
    * Date created
    * Views
  * '''Message'''
    * User that posted the message
    * Date posted
    * Message Text, pretranslated to HTML
  * '''MessageDialog''' extends DialogBox
    * Contains all widgets necessary to enter or reply to a message

{{Spec|Uses/Interactions}}
The Forums creates a single Window that contains all forums, threads or messages depending on what was opened the last time the user was logged in.

There will be a breadcrumb at the top of the window which shows the current forum (i.e. "All Forums | General Chat"), users can click on this to go back up levels within the forums.

Replying to a message opens up the MessageDialog box. If the user clicked on "Quote" rather than "Reply", the dialog box will be filled with Quoted text (surrounded by bbcode [user="..."](quote.md)[/quote]).

{{Spec|Resources}}
  * [BBCode](http://en.wikipedia.org/wiki/Bbcode)
{{Spec|Processing}}
  * At ''restore()'', restore the display to the last viewed forum/topic/message.
  * When the user clicks on a forum, display the list of topics in the window
    * By default, sort by topic creation date, descending
      * **There will be sorting arrow icons at each header field, which sort based on that criteria.
      *** Sorting is initially ascending order, clicking again sorts in descending order
    * Show the selected forum at the top
    * Show a "new topic" button at the top left
      * **Clicking on this button opens the new topic dialog
      *** After creating a new topic, the new message dialog is shown
      * **New topic and message are NOT saved until the user clicks submit
      *** When the topic/first post are created, open the message list for that topic
  * When a topic is clicked, show the list of messages in the topic
    * Still show the current forum at the top, followed by the current topic
    * Still show "New Topic" button
    * Show details of each message
      * **Add a Reply and Quote button to each message
        *****These buttons open the new message (reply) dialog
        *****If Quote is clicked, the quoted text appears in the dialog box
  * The message box is a simple text editor
    * Compiles supported HTML tags to CSS styled &lt;span&gt; or &lt;div&gt; tags, converts unsupported to HTML entities (< to &amp;lt; etc)
    * Compiles supported BBCode tags to CSS styled &lt;span&gt; or &lt;div&gt; tags
    * Supported BBCode tags:
      *** [b](b.md) '''Bold'''
      * **[i](i.md) ''Italics''
      *** [code](code.md) ```
Code```
      * **[name="foo"](quote.md) and [quote](quote.md) Blockquote (with or without name)
  * At ''userSave()'', save the currently viewed forum/topic/message.**

{{Proposed|Color and smilies}}

{{Spec|Interface/Exports}}
''None at this time''

{{Spec|Unit Tests}}

Common Use Cases
  * Creating a forum
  * Creating a topic
  * Posting a message
    * BBCode translation
    * Supported HTML translation
    * Unsupported HTML conversion to HTML entities
    * New topics get the first message
    * Replies end up in the right topic
Uncommon Use Cases
  * Posting a long message (32k)
Edge Cases
  * Creating a forum with a really long title (Exactly 256 chars)
  * Creating a topic with a really long title (Exactly 256 chars)
  * Posting a really long message (Exactly 64k)
  * Creating a million forums
  * Creating a million topics
  * Posting a million messages
Corner Cases
  * Creating a million forums with 256 char titles
  * Creating a million topics with 256 char titles
  * Posting one message in each of one million topics
  * Posting a million 64k messages
Illegal Cases
  * Creating a forum with a title longer than 256 chars
  * Creating a topic with a title longer than 256 chars
  * Posting a message greater than 64k
  * Creating a topic with empty title
  * Creating a topic with ```
null``` title
  * Posting a message with empty text
  * Posting a message with ```
null``` text

# Activity 1: Play Games #

## Registration/Login/Cookies ##

{{Spec|Definition}}

This takes care of presenting the initial screen when the user comes to the page.  Either this is the login window or just directly the main view if they've got cookies active.

{{Spec|Responsibilities}}

Displays the login screen or if the cookie is active it immediately goes to the post-login main screen (which the navigation should bring up in the last configuration seen.)  It will assume cookies should be stored.  This also takes care of the Registration of new users coming to the site.

{{Spec|Constraints}}

The initial login screen which comes up for everybody who doesn't already have an active stored cookie must come up in under 5 seconds.

{{Spec|Composition}}

  * Login Manager - Main controlling part of this whole thing.  It is called by the entry point and then calls the other pieces.

  * Login, Password and Generated Password '''Dialogs'''

  * Password Generator

{{Spec|Uses/Interactions}}

There are three entry points:

  * you have previously registered and logged in and you now have a cookie
  * you have previously registered but you have never logged in or your cookie has expired (or you deleted it)
  * you have not registered (or if you have you are using a different email address...)

The main product of this whole thing is an active cookie (through the cookie manager) usable by the other components to understand who is logged in.

{{Spec|Resources}}

Primarily uses the dialog part of the navigation.
Google cookie thing: http://code.google.com/webtoolkit/documentation/com.google.gwt.user.client.Cookies.html
{{Spec|Processing}}

EntryPoint calls LoginManager->Challenge ().  The entry point would have initialized the navigation with a faded out view in the background before calling Challenge.  Once the challenge is successful (it will loop forever while unsuccessful) the login will call Navigation->Restore () with no parameters (loads the default restoration for this user.)

Challenge follows the flow chart from the funcspec: [[Image:LoginFlow.jpg|left|thumb|100px|Flow chart of the login process]]

First it opens the cookie manager (```
new()```) which detects whether or not a cookie is present.  If so, Challenge calls Navigation->Restore().

If there is no cookie, then the Login dialog is created and displayed.  This thread now stops and the answer thread to the login takes over processing.  This will check the login to make sure its a valid email address (using @ and at least one . before the end)  If it is a new email address (not already in the account list) it will call the Password->Generate() which creates a new password.  This will be encrypted and stored in the db with the email address and then the plaintext password will be sent.  This function will then call the display "PasswordGenerated" dialog.

The PasswordGenerated dialog just has an okay button.  The callback for this just displays the Login dialog again.

Finally if the id is already in the database, then it will be stored into a javascript global.  The Password dialog will be shown.  The callback for this will read the password entry and hash it using the ''MD5'' algorithm in the client side and then compare this against the database entry on the server side... if they match then the cookie is set and the navigation->Restore is called.  Otherwise the Password dialog is displayed again in a permanent loop (until we fix it so that account is locked after too many retries.)

{{Proposed|Note that ''just'' sending a hash password (with any algorithm, MD5, SHA1 or otherwise) is not secure.

It can easily be intercepted by a third party and used as if it were a plaintext password; you can't tell what the password ''is'', but it can still be used maliciously. For milestone 2, we need something more secure like a combination of SSL in places and/or a hash challenge/response system.}}

The session cookie should be set to expire 14 days in the future, for semi-persistent logins.

The cookie storage and call to Navigation.restoreSession () is a method called "Continue()" on the login manager.

{{Proposed|Should maybe ask again for password to enter "sensitive" areas on the site, like assets or account management. Keep a second session cookie to signify that the password was entered recently.}}

Accounts are stored in a table ''Accounts''.  The columns are the email (text), encrypted password (text) and the unique key.  The javascript on the client side should encrypt the password before sending it through the network.

When a new entry is created the password is generated randomly, emailed out as plaintext to the email, encrypted, a new row is created in the table and the email, password and key are entered.

[[Image:LoginSchema.jpg|center|thumb|600px|Login Schema]]

{{Spec|Interface/Exports}}

N/A

{{Spec|Unit Tests}}
  * This all assumes a known database containing the account structure a noted above and a single entry in that for the user account "test@test.com" with an encrypted password generated from "testme"

  * In the emulator at least make sure the page comes up in 5 seconds.  This is probably satisfactory as the general network environment is unknowable so covering this at least is a constant benchmark.
  * Fill the entry widget for login with "badtest@test.com" and test the database to make sure that a new entry is in with a new password (regression testing will make sure that email works and generates a usable password that you can type in etc.)
  * Fill the entry widget for login with "test@test.com" and make sure a new database entry isn't made.
  * Repeat the previous step on the password dialog set the password to "failtest" and ensure that it returns false? (what does it actually do here?)
  * Repeat the previous step but set the password to "testme" and ensure that it returns true?
  * After logging in this way, ensure that the cookie was generated with "GetCookie"

## Game Launcher ##

{{Spec|Definition}}

A browser plugin--or rather a set of equivalent plugins for separate browsers-- that verifies the version of the Geewhiz kernel and standard systems installed, installs new versions if necessary, and launches the kernel with the specified game.

{{Spec|Responsibilities}}

  * rovides an easy to install browser plugin for all major browsers
  * hecks its own version for updates
  * hecks the locally installed kernel version
    * Downloads and installs kernel if none exists
    * Installs in the current user directory to avoid permission problems
    * Downloads and installs any necessary patches to an existing kernel
  * tarts a server kernel instance for the specified game
  * tarts a client kernel instance for the specified game

{{Spec|Constraints}}

  * Must work in Internet Explorer 6+, Firefox 1.5+, Safari and Opera 9 (Konqueror, Epiphany, etc would be a bonus)
    * Lynx, Links, Mosiac, WorldWideWeb and others are not supported for obvious reasons (No CSS, Javascript or plugin support)
  * Must work in Linux, Windows XP, Windows Vista and MacOSX (and obviously Windows 7 someday)
    * Must install without root/admin priviledges
    * Must also install with restricted user priviledges in Windows
    * But does not necessarily need to install on a ''guest'' account on any platform

{{Spec|Composition}}

  * Uses BHOs to implement IE plugins
  * Uses NPAPI for plugins in all other supported browsers
    * NPAPI is supported by Firefox, Netscape, Safari, Opera and Konqueror (And probably Epiphany)
{{Spec|Uses/Interactions}}
  * Plugin will be queried by the site after login, and a "Please Download the Game Client Plugin" window will pop up if the plugin is not installed.
  * Plugin will then check for an existing install, and prompt the user for downloads/updates
{{Spec|Resources}}
  * [Netscape Plugin API (NPAPI)](http://developer.mozilla.org/en/docs/Plugins)
  * [IE Browser Helper Objects (BHOs)](http://msdn2.microsoft.com/en-us/library/bb250489.aspx)
  * [BHOs in C#](http://www.developerfusion.co.uk/show/4644/)

{{Proposed|Explore Java/ActiveX options. Plugins are difficult to install reliably, and there are security issues with executing programs based on a MIME-type: any website could hijack a computer through our plugin. Java is much safer, since the signing is based on the domain, and ActiveX is because IE doesn't support Java by default.}}

{{Spec|Processing}}

After the user logs in, the plugin is queried for its existance
  * If the plugin does not exist, the user will be prompted to download and install it
The plugin will then check for updates to itself
  * If a new plugin update exists, the user is prompted to upgrade
The plugin then checks for an installed kernel
  * If there is no kernel, it is downloaded and installed automagically
  * The install location is based in the user's home directory
    * ~/.geewhiz in Linux/MacOS X
    * C:\Documents and Settings\&lt;user&gt\Local Settings\Application Data\Geewhiz in Windows XP
    * C:\Users\&lt;user&gt;\Application Data\Geewhiz in Vista
      * **(In other words, in the Geewhiz subdir of the user's "Application Data" directory on Windows)
  * If a kernel is installed, the version is checked against the latest, and if necessary, it is upgraded automagically
When the user launches a game, a server kernel instance is started via RPC
  * The launcher gets an IP address/randomly assigned port
The installed client kernel is launched with that IP/port**

{{Spec|Interface/Exports}}

  * '''GamePlugin'''
    * ''void checkInstalled()'' - Checks for installed plugin then kernel
    * ''void launchGame(String gameInstance)'' - Launches a game with the specified ''game instance'' key, starting a kernel on the server and a separate kernel on the client

{{Spec|Unit Tests}}

In an embedded browser
  * Test the following for NSAPI and IE functionality
  * #Test that checkInstalled() exists and doesn't throw an exception
  * #Test that the user install dir is accessable
  * #Test that launchGame() with a valid game instance works
  * #Test that launchGame() with an invalid game instance fails with an exception
Communication with the server
  * Version checking gives the latest version of the plugin
  * That a kernel version starts properly
    * Test that the IP/port combo is unique
  * Downloading a client kernel
  * Starting the client kernel

## Download and Play ##

{{Spec|Definition}}

The collection of systems that control game play.

{{Spec|Responsibilities}}

  * Starts server systems to handle things such as physics
  * Connects to a given server IP/port {{Note|See Geewhiz->SystemManager for information on downloading client systems}}
  * Downloading a GUI script
  * Downloading a level/movie
  * Creates the in-game GUI windows and widgets
  * Translates input (including GUI events) into action events
  * Displays the 3D graphics

{{Spec|Constraints}}

{{Spec|Composition}}

  * Kernel
    * Connects to the server IP/port
  * ConsoleSystem
    * Displays 3D graphics (OGRE)
    * Displays the GUI '''view''' (OGRE)
    * Receives and forwards input events (OIS)
    * Configures the GUI '''view''' (CEGUI)
    * Handles input events (OIS/CEGUI)
      * **Generates GUI events and sends them to the GUIScript via callbacks (CEGUI)
      *** The script sets up the GUI '''model''' (GUIScript)
    * Handles the GUI '''controller''' by processing GUI events (GUIScript)
      * **Translates events to ''action events'' and forwards them to the server
        *****Translation table is defined by the user in a game GUI dialog
        *****Per-game user control actions and keyboard defaults are hard coded into the '''GuiScript''' using the QWERTY keyboard layout
  * Actions
    * When sent over the network, have two properties: name and amount
    * The amount is irrelevant other than it needs to be mapped to the controller (For example, action "TURN" coul map KEY\_LEFT to -1.0, KEY\_RIGHT to 1.0, and have the normalized joystick X axis mapped across it)
    * Interpretation of actions is left up to the GameScript**

{{Proposed|Default inputs may need to be set based on locale/keyboard type (ie. QWERTY [wasd](wasd.md) vs. AZERTY [zqsd](zqsd.md) and maybe even Mac-style keyboard)}}
{{Note|Mac-style keyboard differences may be accounted for by using alternative keys. (ie. ALT is equivalent to Command)}}

{{Spec|Uses/Interactions}}

  * '''GameLauncher''' launches a client when requested by the user
  * The client kernel connects to the server kernel started by the '''Game launcher'''

{{Spec|Resources}}

  * [Ogre3D](http://www.ogre3d.org/)
  * [Object-oriented Input System (OIS)](http://sourceforge.net/projects/wgois)
  * [Crazy Eddie's GUI System (CEGUI)](http://www.cegui.org.uk/)

{{Spec|Processing}}

  * A ''server kernel'' is started for a particular user
  * A ''client kernel'' is launched with an IP/port for the server kernel
  * The '''SystemManager''' starts up all server systems (See Geewhiz->System Manager)
    * GameScriptSystem
  * The '''SystemManager''' starts up all client systems (See Geewhiz->System Manager)
    * ConsoleSystem
  * The '''GameScript''' starts the game asynchronously.
  * The GUIScript is passed to the CEGUISystem
    * The GUIScript resource name is hardcoded in the CEGUISystem as the system parameter "script" but each game has its own GUIScript by that name.
    * The script selects a CEGUI theme
    * The script sets up the GUI layout
    * The script configures input translations from input events into '''Actions'''
  * The ''client'' systems query ''ALL''
    * The level is downloaded
    * The level objects are displayed to the user in the ConsoleSystem
  * When input occurs in OIS, it is forwarded to the CEGUI
    * CEGUI translates mouse clicks into corresponding GUI events
    * The GUIScript handles GUI events
      * **Button clicks
      *** List box selections
      * **Text entered
    * The GUI script translates raw input events into the corresponding Action(s)
  * On the server, the '''GameScript''' runs the game (handles game logic)
  * All new game data is being queued up to the database as a write-thru cache
    * To create a save game, the DB locks in a transaction (queues updates), and a copy of the level instance is made**

{{Proposed|GUI layout assets. Possibility of converting existing formats like Windows resources, CEGUI layouts, etc.}}
{{Proposed|"Action" definition assets and control (joystick and keyboard) mapping assets. Analog controllers simply map axes and buttons. Keyboard mappings based on keyboard layout, such as QWERTY, AZERTY, QWERTZ and Dvorak. Others should not matter since non-latin keyboards use QWERTY with alphabet specific modifiers.}}


{{Spec|Interface/Exports}}

  * GUIScript: TDB

{{Spec|Unit Tests}}

  * Connecting from client to server
  * Starting all three client systems
  * Fetching the GUI script
  * Downloading a simple level (complex is tested in Geewhiz)
  * Translating events to actions
  * Sending actions to server
  * Saving game state to DB

## Browsing Games ##

{{Spec|Definition}}

This shows a list of all the games in the system.  It displays the title, the icon, the developer and description information.

{{Spec|Responsibilities}}

The chief role here is that when the player clicks to play a game, this creates a new instance for that game (or deletes the currently paused instance if they request) and sends that instance information into the game downloader.

It must also manage the pop-up and slide the window to hold the descriptive information when clicked as described in the funcspec.

{{Spec|Constraints}}

{{Spec|Composition}}

The GameBrowser (extends Window) is the main component, the view and a set of callbacks.

There is a dialog DeletePause (extends DialogBox) for what to do if there is a pause for this game.

{{Spec|Uses/Interactions}}

If they click then the screen slides down allowing the detailed description in the view at that point.

Clicking on the headings for columns will change the sort order of the rows.

Clicking the play button will check if the DeletePause dialog needs to be popped.  If so then if the pause is deleted it removes it and makes a copy of the game instance as described below in the server section.  If there isn't a pause then the instance is copied immediately.  Finally the game downloader is called with the id of the instance.

{{Spec|Resources}}

Sortable Table - http://psthapar.googlepages.com/simplesortabletable

{{Spec|Processing}}

  * restore (), save() (What will it save and restore is the actual question...)

  * On opening the view:
    * Query all games for Icon, Name, Genre, Creator, Description and key and stores to an Array
    * Display the array sorted alphabetically by name first.
    * Setup one callback for hover and one for click

  * Callback hover:
    * Shows a popup after 500ms immediately below the display line of the current game.  This stays until the mouse moves outside the display line for that game.

  * Callback click
    * Expands the current display line to accommodate the description.  Adds a button for play (Add concept drawing from funcspec)  Callback for play button click

  * Callback for play button click
    * Queries for a current instance of the game, if so displays the DeletePause dialog and creates a callback for that
    * If there isn't a current instance of the game, constructs a query to build all the proper table entries and makes a copy (should be a stored procedure)
    * This should set the game level to the INIT special level setup by the game creator so that all the proper linking is in place for newLevel to use later.
    * When the instance is created its key and control is passed into GameDownloader through loadGame

  * DeletePause callback
    * If delete pause is true, then the current game instance is deleted and replaced with the new instance.
    * If delete pause is fale, then that game instnace is used
    * once the game instance is determined its key and control is passed into the GameDownloader through loadGame

{{Spec|Interface/Exports}}

N/A

{{Spec|Unit Tests}}

For all of these, insert some test data into the database to compare against.

  * Call save and check the values, use programmatic sets and call save again.  Compare against a known set.
  * Call restore with a stored set and then use programmatic visualization.
  * Call each of the column sort buttons twice and iterate over the visualization programmatically.
  * Can check the existence of the play button?
  * Click the play button for a normal, a paused game and ensure that things either add a new active level entry depending on need.

## Persistent Pause/Saves ##

{{Spec|Definition}}

This discusses persistent pauses which after looking at the games browser above, have to do with starting a game and then having the client go away for any number of reasons.  This leaves a game instance in the database.  The pause comes back and resumes that game instance where it left off.  Discussed in the download and play section is how the data is written through to the actual game instance.

It also discusses saves which are more traditional and stored based on some game mechanism through the gamescript interface, but are also stored permanently in the database.

{{Spec|Responsibilities}}

This covers creation and deletion of either pauses or saves (this is the instance based server side control).  Covers display of stored pauses and saves and lets the user play them (web based control)

{{Spec|Constraints}}

All should be written through as the current game state of play.

{{Spec|Composition}}

  * Pause/Save Browser - Displays pauses and saves grouped by game (most recent to least recent)

  * LoadDialog - Shows a dialog when the save comes up asking if they really want to overwrite the pause

{{Spec|Uses/Interactions}}

  * Continuing a Pause - Starts the game using the current default game instance

  * Continuing a Save - Deletes the current pause and copies the save in as the new default game and then starts the game with that.

  * Deleting a Save - Deletes a save from the database.

  * Creating a Save - Copies the currently active game into a save.  This will block any updates to that game set while this is happening (although they will be cached and so saving should appear instantaneous from the usage standpoint.)

{{Spec|Resources}}
{{Spec|Processing}}

  * save ()/restore () on the web interface

mainline activity
  * Queries the level instance table for all saves and pauses
  * displays saves and pauses grouped by game with the pause first (only those that exist doesn't display empty games.)
  * When a pause or save is clicked adds the play button as described above in the game browser
  * when the play button is clicked on a pause the game starts by sending the information about that pauses key to the loadGame in GameDownloader (this is very straightfoward since everything needed is already there...)
  * When the save button is clicked the LoadDialog is shown
  * If the cancel on the LoadDialog is pressed nothing happens, but if the "okay" button is pressed then the current pause is destroyed and the highlighted save is copied over into its place.  GameBrowser is called to load that instance then.

  * Other actions that can come from the gamescript on a running game... that is one instance per user.
  * Create save calls into the server RM which is doing the write-thru cache.  The current game instance (which was passed in through the GameBrowser) is known and copied into a new save location.
  * Load works like changing a level.  First the RM is cleared of all current assets (deleting the current instance) and then the level is copied in over the current instance
  * Deleting a save is a trivial database change, but the interface here is blocked while that happens so that no saves can occur and no loads can occur while a save is being deleted (for a particular user)

  * DB
    * When the game starts a new row is created in the table ''LevelInstances'' which contains rows level (link), account (link), saved (datetime).  The paused level has saved = NULL and is the one that is constantly updated via writethru as the game is run.  When a save occurs a copy of all overwriting changes from the saved=NULL row is written out and a date is given.  When a load occurs, everything is copied from the given date for loading back into the saved=NULL.
    * There is another table called ''GameInstances'' which contains the object instances specific to this game.  Same for ''GameInstanceValues'' as it relates to ''InstanceValues''.
{{Note|'''Technical Note'''
These are defined exactly as above for the counterparts, but the level link is instead a link into the ''LevelInstances'' table.}}
    * What happens on a new game is that when ''LevelInstances'' is created all the ''ObjectInstances'' and ''InstanceValues'' are copied from that particular level into the rows of these tables, but linked instead to the ''LevelInstances'' table.  As the game modifies values they are modified in the ''GameInstanceValues'' so that the ''InstanceValues'' table remains solid.  Any dynamically created objects are created only in ''GameObjectInstances''
    * As noted below a save/load means it copies values around in the ''GameObjectInstances'' and ''GameInstanceValues'' tables only.

[[Image:PersistFlow.jpg|center|thumb|320px|Flowchart for Creating a new Game instance for Persistence]]

{{Spec|Interface/Exports}}
  * Gamescript exports
    * Save (name); Saves the currently running game instance.  Returns failure if name already exists or if space has run out.  Otherwise success.
    * Load (name); Loads a game instance over the currently running one.  Clears the currently running assets and loads in the save game.  This works much like changing levels but works with specific game instances instead of pre-stored levels.
    * Delete (name); Delets a saved game.
    * List saved games.

{{Spec|Unit Tests}}

For all of these, insert some test data into the database to compare against.

  * Call save state and check the values, use programmatic sets and call save again.  Compare against a known set.
  * Call restore state with a stored set and then use programmatic visualization.
  * Call list compare against the known DB.
  * Call Delete as though through a script and then call list to make sure its gone.
  * Test Geewhiz?  That is ... test saving or running a loaded game from the gamescript part.  That's RM related.

# Activity 2: Make Games #

## Creating a New Game/Deploying a Game ##

{{Spec|Definition}}

Games are really just placeholders for levels.  The idea is that the artists can mess around as much as they want until the deploy happens at which point the game is then public and all future modifications will be live.

{{Spec|Responsibilities}}

This takes care of showing the games the user has made, editing those games and deploying them.

{{Spec|Constraints}}

{{Spec|Composition}}

  * GamesList view - Displays the games for this user.  Like the normal GamesBrowser can be sorted by the various columns

  * GameEditor view - Allows the user to edit all of the details of the Game in a form view.  It also has a deploy button if the game has not yet been deployed.

  * DeployDialog - After pressing the deploy button in the GameEditor view, this comes up to confirm the selection before the game becomes public.

{{Spec|Uses/Interactions}}

  * Lists Games

  * New Game/Edit Game

  * Deploy Game

{{Spec|Resources}}


{{Spec|Processing}}

  * save()/restore() as needed.

  * Init
    * Query the database for the games for this user and store in an array
    * Display the array and set up the click callback
    * Enable the new callback

  * New Callback - Create a default instance for the game and requery/display the main view.

{{Note|The default instance for the game should also contain a default empty level called INIT.  This will be used by the game browser to set the initial game with an empty setup but that has all the proper linking in place.}}

  * Click callback
    * highlight this line
    * Add the edit icon and edit callback

  * Edit callback - open the edit view for this instance

  * Edit view
    * Query full information about the game and fill in the display (see funcspec for concept)
    * Add callback for Okay and Deploy

  * Deploy dialog - If the button is pressed, it updates the time stamp.

  * DB Notes
    * Games are stored in a single table ''Games'' with the columns: icon (blob), name (text), description (text(250)), genre (enum), developer (link to account), deployed (date).  There is also a unique key used for foreign indexing.
    * A new game has the deployed entry as NULL.  When a game is deployed a copy is made of the row and the deployed date is set.  All entries which linked to the game at that time get another link to this new entry (assets can have multiple links.)  In this milestone, the game can continue to be edited after deployment.  It is now always public, so there are only two phases (future versions should allow working copies and deployed copies.)

[[Image:CreateDeploySchema.jpg|center|thumb|600px|Create Deploy Schema]]

{{Spec|Interface/Exports}}

N/A

{{Spec|Unit Tests}}

  * Test the save and restore including combinations of programmatic modifications.
  * Do a creation programmatically and verify the modification to the DB.

## Adding an Asset ##

{{Spec|Definition}}

The Object asset is the primary game asset.  Its the melting point for all the graphics and sounds and whatnot that make up the game.  The rest are just dressing.

{{Spec|Responsibilities}}

Displaying assets for either this user or all users

Sorting assets by a variety of options

Creating a new asset and uploading a version for it.

Modifying an asset by downloading the current version editing it in an editor and then uploading that.

Modifies Object assets through the web interface.  Currently it is all string based for name/values.

{{Spec|Constraints}}

Uses ResourceItem

{{Spec|Composition}}

  * AssetList - this is the main view that gets registered into the navigation.
  * AssetView - this is the view for non-object assets that lets you download/upload
  * ObjectView - this is the view that shows Objects.
  * AddAssetDialog - a dialog for adding a new asset

{{Spec|Uses/Interactions}}

  * Users can view the assets generated by themselves or everyone else from the list

  * They can add new assets ('''cannot delete''' this should probably be fixed later, but right now if you delete the game won't run.  However, if someone adds a copyrighted asset its gotta be destroyed.)

  * They can edit existing assets (either by uploading new versions or modifying Objects in place.)

{{Spec|Resources}}

{{Spec|Processing}}

  * Save/Restore will only save the list information (not any asset specific views)  Will save which one is clicked, whether they are browsing full or select and how far down they were viewing in scroll.

  * When the main view comes up it defaults to showing "My Assets"
    * Each time the view comes up it'll requery.  It stores scroll point as a percentage to attempt to be close on reload
    * It'll requery when the asset group is changed between "all" and "mine" .. no dynamic updates for now.
    * if the user clicks on an asset the edit button comes up

  * When the user clicks add new it'll pop up the dialog
    * The dialog requires both name and type and inserts this into the DB when "okay" is pressed.
    * If okay was pressed the view will highlight that new asset so the edit button shows up

  * In the edit view for a non-object asset
    * Only the meta data is queried if its a binary object.
    * When the download button is pressed then the DB is queried for the filename, the mime-type and the binary data and passed to the user this way.  Hopefully they've set their browser to understand the mime-type with the appropriate app.
    * On upload the filename, mime and binary may all be changed, but it is stored in memory on the server until the okay is pressed and then its stored in the DB.

  * IN the edit view for object assets
    * All the data is queried and filled in the display for all name/value pairs
    * Name/value pairs are blind strings stored into the DB... limit of varchar(255) for name and longtext for value.
    * The user may click add item as many times for name/value pairs as they want and blank entries fill in (but only on the display)
    * Once they click okay all blank name/values are culled and not inserted so they'll disappear next time this asset comes up.

  * DB Storage
    * Assets are stored in a single table ''Assets'' (not including levels) with the columns: name (text), type (enum, see the list above), game (link), last modified (date), and creator (link).  There is also a unique key user for foreign indexing.
    * Object assets have a second table involved called ''ObjectValues''.  This table consists of the columns name (text), value (text), asset (link), asset and name form the key.
{{Note|'''Technical Note'''
As far as the funcspec is concerned the value is text, but the techspec should go ahead and use Resource Types and text in the actual implementation.}}

[[Image:AssetSchema.jpg|center|thumb|600px|Asset Schema]]

{{Spec|Interface/Exports}}

N/A

{{Spec|Unit Tests}}

  * Test the save and restore function
  * Add some binary style object and see it show up in the DB
  * download the binary object make sure it matches then upload another and make sure this is what downloads
  * Create an object asset and modify its name/value pairs and ensure they go.
  * Add a large number of empty assets to ensure they don't show in the  DB.

## Adding a Level ##

{{Spec|Definition}}

A section that lists all available levels for a game, and a level editor that can be run within the browser, so that the designer can create a level on the site.

{{Spec|Responsibilities}}

  * ist levels based on a criteria
    * Filter by MyLevels and all levels
  * llow the user to create new levels
    * Given a name and assigned to a game
    * Launches level editor
  * ser can edit existing levels
    * Launches level editor

'''Level Editor'''

  * aunches in browser
  * rovides instance palette
  * rovides prototype palette
  * isplays 3D objects
    * 3D "?" if no 3D view exists
  * llows user to create instances from prototypes
    * Changes to prototypes propagate to instances unless overridden (inheritance)
  * llows the user to place and transform instances within level
  * electing an object in palette, then clicking on the edit button brings up object editor
    * Editing instances only changes that instance

{{Spec|Constraints}}

  * Level editor must run in the browser, embedded in the web page

{{Spec|Composition}}

'''Level List'''
  * Simple list with filter

'''Level Editor'''
  * Java applet using JOGL for rendering
  * Installs JOGL if necessary using JOGLAppletInstaller
  * All buttons (save, edit instance/prototype) are outside of applet

'''JavaScript/Java Communication'''
  * Two-way communication, JavaScript calling the Java applet's methods and Java calling functions in JavaScript through JSObject.
  * Levels are inserted/extracted from the applet as an XML based scene graph file (directly a level asset) {{Note|Note: In this milestone, level graphs are nothing more than a collection of independent nodes}}

'''Scene nodes'''
  * The object instance itself is a scene node (no separate scene node object)
    * Position and orientation are stored within

'''Prototypes'''
  * Prototypes are handles as delegates to an instance
    * If a prototype changes, the subclassed objects will automatically use the new values
    * An instance, however, CAN override prototype values (using copy-on-write semantics)
      * **Overridden values will ''not'' be changed in instances by prototype changes**

{{Spec|Uses/Interactions}}

  * The user can selects a prototype and clicks the plus button to create an instance - also opens the object editor
  * The user can select an instance and click on the edit button to change it

{{Spec|Resources}}

  * [Java 1.5 API](http://java.sun.com/j2se/1.5.0/docs/api/)
  * [JOGL](http://jogl.dev.java.net/)
  * [Java Plug-in Guide (Deploying Applets)](http://java.sun.com/j2se/1.5.0/docs/guide/plugin/developer_guide/contents.html)
  * [JavaScript to Java Communication](http://java.sun.com/j2se/1.5.0/docs/guide/plugin/developer_guide/js_java.html)
  * [Java to JavaScript Communication](http://java.sun.com/j2se/1.5.0/docs/guide/plugin/developer_guide/java_js.html)

{{Spec|Processing}}

'''Level List'''

  * The level list, when opened, fetches all existing levels, filtered by the current filter
    * When the user changes the filter, refetch the level list
  * When a user clicks on a level, the level editor window is opened

'''Level Editor'''

  * The level editor window loads and the applet starts
  * If JOGL is not installed, install it
  * The script calls the applet's method ''loadLevel(...)''
    * The applet fetches all requires assets through the page's RPC
    * The applet displays the level
  * When the user clicks a prototype in the list, all instances of the prototype are highlighted
    * When the user selects a prototype and clicks the add button, a new instance is created by copying the prototype and the object editor opens
  * When the user clicks an instance in the list, it is selected in the applet
    * When the user selects an instance, and clicks the edit button, the object editor opens
    * When the user clicks on an asset in the 3D display, it is selected in the list
  * When the user clicks on the ''Save'' button, the level is retrieved through the applet's ''saveLevel()'' method.
    * This level asset is then saved to the server

''' DB Stuff '''

  * Levels are stored in the table ''Levels'' with columns: name (text), game (link), creator (link) and unique key.

  * The palette window is populated by the Object assets from the asset table.  Instances are stored in a separate table ''Instances''.  This table contains the columns name (text), level (link), prototype (link), and the unique key.  There is another table ''InstanceValues'' which works much like the ''ObjectValues'' above, but contains only those entries which are overwrites of the instance.  If the entry should not be different than the prototype it should be removed from the ''InstanceValue'' table (this is so that changes to the prototype will extend to instances.)

{{Note|'''Technical Note'''
An instances values should first be checked from ''ObjectValues'' then any names that match by ''InstanceValues'' should be overwritten with the value there.}}

  * When the prototype is dragged over onto the table, if the prototype does not contain position and orientation values, it should be modified to do so.  A new entry is created in the instance table pointing to this level and the dragged prototype with the name of the prototype and its unique key number appended into the name (the only purpose of this is so that it stands out to the user.)  Nothing is done to the ''InstanceValues'' table at this time.

[[Image:LevelSchema.jpg|center|thumb|600px|Level Schema]]

{{Spec|Interface/Exports}}

N/A

{{Spec|Unit Tests}}

  * Communication between browser and applet
  * Loading/parsing a level
  * Instantiating a prototype
  * Saving an instance

## Writing the Gamescript ##

{{Spec|Definition}}

Scriptable server-based game API that allows game designers to include game logic, AI and other scriptable features.

{{Spec|Responsibilities}}

  * Provide scriptable interface into Geewhiz
    * Start systems
      * **Forward commands to the client
    * Monitor/control entities
  * Give the game designer an easy to understand scripting environment
    * Global variables stay persistent
    * Script state is saved/restored automagically**

{{Proposed|Handle system properties that are stored by the server.}}

{{Spec|Constraints}}

  * Script state must be serializable
  * Script environment must not be highly restricted. Obviously, we have a function entry point, but otherwise freedom to do whatever is allowed on the server
    * Exceptions may include disallowing direct file I/O, sockets, etc.

{{Spec|Composition}}

[Stackless Python](http://www.stackless.com/) fits the requirements. If the script is run entirely as a ''tasklet'' then it and any sub-tasklets can be serialized. In addition, since it is an extension to Python, it supports many nice features like OOP and functional programming styles.

  * '''ScriptSystem'''
    * Runs as ''BlockingSystem'' thread
    * Uses Stackless' ''channels'' and ''tasklets''
    * Starts the script thread
    * Forwards events to the scripts
  * '''Script Thread'''
    * Runs the script until end of game
    * Starts the tasklet using one entry point function ''startGame()''
{{Note|Starting the script as a tasklet allows the state to be saved using Python's ''pickle'' module. Additionally, since functions cannot modify the global environment, there is no need to serialize the global namespace, which either can't be done, or can't easily be done.}}

{{Spec|Uses/Interactions}}

  * Script->Geewhiz API (TODO)
    * ```
RM::SetLevel```
    * ```
RM::AddObject```
    * ```
RM::RemoveObject```
    * To Be Determined?

{{Spec|Resources}}

  * [Python Documentation](http://www.python.org/doc/)
  * [Stackless Python](http://www.stackless.com/)
  * [Stackless Examples](http://code.google.com/p/stacklessexamples/wiki/StacklessExamples)
  * [Stackless Tutorial](http://members.verizon.net/olsongt/stackless/why_stackless.html)
{{Note|[Lua](http://www.lua.org/) is out because you cannot serialize the current script state.}}

{{Spec|Processing}}

  * The '''ScriptSystem''' is started as a BlockingSystem
  * The game script is loaded and compiled as a module
  * The '''script thread''' is created
  * A channel is created to facilitate communication between the ScriptSystem and the tasklet(s)
  * A channel callback, ''saveCallback()'' is created (a C-function) that facilitates saving state and exiting when a save or pause event is received {{Note|A channel callback is used because it appears to be the easiest way to halt execution of the tasklets for serialization. There is no ''stackless.halt()'' function to stop execution otherwise}}
    * Iterates through all tasklets, adding each one to a list
    * Pickles the list to a string
    * Converts the Python string into a C-string
    * Saves the C-string to the database
    * On ''persistent pause'', stop execution of the script
  * If there is an existing saved state, unpickle the list of tasklets and add each one to  the scheduler in the '''script thread'''
    * Otherwise, add a tasklet to call the python function ''startGame()'' in the game script module
  * ''stackless.run(n)'' (aka watchdog) is called in a loop to run for ''n'' instructions before stopping (in other words, simple preemptive multitasking)
    * After each iteration of the ''run(n)'' cycle, the number of running/blocked tasks is checked to see if it needs to be run again
    * The '''game script''' can loop, sleep, block/wait for events, start sub-tasklets that run AI, create new channels, etc
    * ''stackless.schedule()'' can be called in a tasklet to yield to other tasklet(s), but it is not necessary
  * The '''ScriptSystem''' blocks/waits for events
    * When an event is received, it is forwarded to the channel created by the '''ScriptSystem'''
{{Proposed|Importing user modules (libraries) into the game script via the database}}

{{Spec|Interface/Exports}}

;ScriptSystem
:Defined in the blocking system.
;GameScript
:```
def startGame():<br>...```

{{Spec|Unit Tests}}

  * Test the script API
  * The Python Script is in a separate thread
  * Saving/restoring state brings back the same script at the same state
  * Forwarding events

# Build Process #

{{Spec|Definition}}

A set of Eclipse projects, Scons buildfiles and Ant buildfiles that compile the code as well as deploy it to the server.

{{Spec|Responsibilities}}

  * Compile the code for any supported platform
    * Win32 (MSVC 8)
    * Linux/x86 (GCC 4.1)
    * Java 1.4 and 1.5
  * Package the binaries
  * Deploy all packages to the foundation servers

{{Spec|Constraints}}

Must run with hopefully only a single command to build and run tests (one for each platform).  The deploy should be its own.

{{Spec|Composition}}

'''SITE\_TOP''' will refer to the top directory of the web site (e.g. /var/www/geewhiz.tosos.com/htdocs)
'''DATABASE''' refers to the database name where it is stored.  Files will be modified (who the hell knows which ones because its all just guesswork and wishful thinking) to find this.
The build will be divided into several projects:

  * '''Site''' (Java 1.4) [SVN: foundation-site]
    * Site front-end Java packages are:
      * com.geewhizfoundation.client.navigation
      * com.geewhizfoundation.client.registration
      * com.geewhizfoundation.client.community.forums
      * com.geewhizfoundation.client.play.gamebrowser
      * com.geewhizfoundation.client.play.savepausebrowser
      * com.geewhizfoundation.client.play.gamelauncher
      * com.geewhizfoundation.client.create.gamecreator
      * com.geewhizfoundation.client.create.assetbrowser
      * com.geewhizfoundation.client.create.levelbrowser
    * Common RPC Java packages are:
      * com.geewhizfoundation.client.rpc {{Note|Using parent package "client" because of GWT constraints. Makes it much easier to compile in.}}
    * Site back-end Java packages are:
      * com.geewhizfoundation.server.common
        * Contains database access wrapper class
      * com.geewhizfoundation.server.navigation
      * com.geewhizfoundation.server.registration
      * com.geewhizfoundation.server.community.forums
      * com.geewhizfoundation.server.play.gamebrowser
      * com.geewhizfoundation.server.play.savepausebrowser
      * com.geewhizfoundation.server.play.gamelauncher
      * com.geewhizfoundation.server.create.gamecreator
      * com.geewhizfoundation.server.create.assetbrowser
      * com.geewhizfoundation.server.create.levelbrowser
    * GWT code for front-end
      * DEPLOYS TO: '''SITE\_TOP'''/*** Java servlet for back-end
      * DEPLOYS TO: Tomcat Manager at /foundation-backend
  * erver Components
    * Server Kernel (C++) [SVN: ''shared'' foundation-serverkernel] - Contains the SERVER kernel code, but also a lot of the driver interfaces, at least everything in top level geewhiz/include
      * DEPLOYS TO: /opt/geewhiz/libkernel.so
    * Stackless (C++) [SVN: foundation-stackless] - Server only. System wrapper that bridges events to stackless as a BlockSystem
      * DEPLOYS TO: /opt/geewhiz/sysStackless.so
    * ODE (C++) [SVN: foundation-ode] - Server only. ODE Physics System
      * DEPLOYS TO: /opt/geewhiz/sysODE.so
  * lient Components
    * Client Kernel (C++) [SVN: ''shared'' foundation-kernel] - Contains the CLIENT kernel code, but also a lot of the driver interfaces, at least everything in top level geewhiz/include
      * DEPLOYS TO: '''SITE\_TOP'''/client/kernel-'''VERSION'''-'''PLATFORM'''.zip
    * OgreConsole (C++) [SVN: foundation-ogreconsole] - Client only. Contains Ogre3D graphics, GUI, GUIScript, Sound and Input
      * DEPLOYS TO: '''SITE\_TOP'''/client/ogreconsole-'''VERSION'''-'''PLATFORM'''.zip
        * Examples: ''ogreconsole-1.1.15-win32.zip'' and ''ogreconsole-1.1.15-linux-x86.zip''
  * rowser Plugins
    * NSAPI Game Launcher Plugin (C++) [SVN: foundation-nsplugin]
      * DEPLOYS TO: '''SITE TOP'''/launcher/nsplugin-'''VERSION'''-'''PLATFORM'''(extension)
    * IE Game Launcher Plugin (C#) [SVN: foundation-ieplugin]
      * DEPLOYS TO: '''SITE TOP'''/launcher/ieplugin-'''VERSION'''-'''PLATFORM'''.exe
    * Level Editor (Java 5 Applet) [SVN: foundation-leveleditor]
      * DEPLOYS TO: '''SITE\_TOP'''/tools/level-editor.jar**

'''File Naming Conventions'''

  * Exe extension is .exe for Windows, and otherwise empty
  * Version strings: ''major''-''minor''-''patchlevel''
  * Platforms strings: ''linux-x86'' and ''win32''

'''Project Layout'''

  * Every project will have one source dir called 'src'
    * Every C++ project will also have a second source dir called 'include' (only externalized interfaces are stored here, internal includes are stored with the .cpp)
  * Every project (that needs it) should keep building/testing docs in a dir called 'doc'
  * Javadocs/Doxygen docs should compile into a dir called 'api-doc'
  * Every project will build into a dir called 'build'
    * Intermediate C++ object files (.o/.obj, etc.) will be placed in 'build/''subproject\_name'''
    * Java class files will be placed in 'build/classes/''fully\_qualified\_Java\_package\_name'''
    * Binary executables (including .so/.dll) will be built into 'build/bin'
    * Static library files (.a/.lib) will be built into 'build/lib'
    * GWT web pages will be placed in 'build/www/''fully\_qualified\_GWT\_module\_name'''
  * All servlet data will be placed in 'WebContent' (META-INF and WEB-INF)
  * All binaries/packages ready for deployment (EXEs, DLLs, JARs and WARs) will be placed in 'dist'

'''Unit Testing'''

  * Each component will have its own unit test suite to minimize component build time by not running every unit test on a small change
  * Some unit tests will auto-run during build cycle
    * An exception is the GWT JUnit tests, which will be manually run normally and auto-run just before packaging
  * Unit tests should be run again before deployment on target machine
    * Install scripts will auto-run the unit tests again before installing
  * Java unit tests will be performed using JUnit
  * C++ unit tests will be performed using CppUnit
  * C# unit tests will be performed using csUnit
  * Unit tests should be run after every change

{{Spec|Uses/Interactions}}

After compilation, run the package process to create the appropriate deployment file.

  * or the site, an install package tarball will be created which shall be untarred into a temp directory and then can install to any directory
  * nstall on test server
    * **Perform regression tests
  * f test server succeeds, install on live server test site
    ***Perform regression tests
    * **If regression tests succeed, install site to live server
  * or server components, an install package tarball will be created which shall be untarred into a temp directory and then can install to any directory
    * Install on the test server
      ***Perform regression tests
    * If test server succeeds, install on live server test site
      * **Perform regression tests
      ***If regression tests succeed, install site to live server
  * or client components, a zip package will be created for each target platform
    * On the test target platforms, run the unit tests
    * If the unit tests succeed, upload the package to the test server
      * **The regression tests will be run on sample game(s)
    * If the regression tests succeed, upload the package to the live server
  * or the browser plugins, a platform-specific installer package will be created for each target platform
    * Upload the package to the test server
      ***The regression tests will be run on sample game(s)
    * If the regression tests succeed, upload the package to the live server
  * or the level editor, a JAR file for the applet will be created
    * Run the unit tests on all test platforms (In addition to at-build unit tests)
    * If the unit tests succeed, upload the JAR to the test server
      * **The regression tests will be run by launching any game
    * If the regression tests succeed, upload the JAR to the live server**

{{Spec|Resources}}

  * [org.apache.cataline.ant documentation](http://tomcat.apache.org/tomcat-5.0-doc/catalina/docs/api/org/apache/catalina/ant/package-summary.html)
  * [GWT JavaDocs for dev package](http://www.screaming-penguin.com/GWTDocs)
  * [JUnit](http://junit.sourceforge.net/) - Java Unit Test Framework
  * [CppUnit](http://cppunit.sourceforge.net/) - C++ Unit Test Framework
  * [csUnit](http://www.csunit.org/) - C# Unit Test Framework similar

#Site
#**To build:
#**#Eclipse Project: Dynamic Website / GWT Builder
#**#CLI: Ant 'build.xml'
#**To deploy:
#**#Ant 'build.xml' to create an install package
#**#Ant 'build.xml' to create a WAR file for deployment on Tomcat server
#Server Components
#**To build:
#**#Eclipse Project: Scons C++
#**#CLI: Scons SConstruct
#**To deploy:
#**#Scons SConstruct to create install packages
#Client components
#**To build:
#**#Eclipse Project: Scons C++
#**#CLI: Scons SConstruct
#**To deploy:
#**#Scons SConstruct to create install packages
#NSAPI Plugin
#**To build:
#**#Eclipse Project: Scons C++
#**#CLI: Scons SConstruct
#**To deploy:
#**#Scons SConstruct to create a platform-dependent installer
#IE Plugin
#**To build:
#**#Eclipse Project: Scons C#
#**#CLI: Scons SConstruct
#**To deploy:
#**#Scons SConstruct to create a Windows installer
#Level Editor
#**To build:
#**#Eclipse Project: Java Applet
#**#CLI: Ant 'build.xml'
#**To deploy:
#**#Ant 'build.xml' to create a JAR file**

  * Build Templates
    * System
    * Plugin

{{Spec|Processing}}

This describes the three automated processes

**Building**#Compiles/links all source
**#Runs the unit tests and complains loudly if they fail**Packaging
**#Creates install packages, such as tarballs, JARs, WARs, EXEs and ZIPs**Deploying (Install Packages)
**#Auto-runs unit tests again, checking for inconsistencies on target machine.**#Installs the files into the specified directory

{{Spec|Interface/Exports}}

N/A

# Schema #

[[Image:FoundationDBSchema.jpg|center|thumb|800px|Database Schema]]

Foreign Key Restrictions:

All updates are set to cascade.
All deletes are set to restrict except in the following cases:
In ForumMessage deleting an account will set the Creator reference to NULL.  This means the message will stay in the forums, but the creator will be empty.
This is the same for ForumTopics created by a deleted account.  The topic may continue to be active even if the creator has been removed.
It is not the case for assets, levels and game as a decision must be made about what to do with those things created by someone who is being removed from the system.  This is not in the scope of this milestone.

# Geewhiz #

## Hub ##

{{Spec|Definition}}

One ring to rule them all one ring to bind them one ring to bring them all and in the darkness bind them.

{{Spec|Responsibilities}}

Initializes the other aspects of the kernel, the various managers.  Acts as a central inheritance point for shared libraries to get access to kernel elements.
This includes the main().

{{Proposed|Handle exceptions to do various tricks like reconnect or communicate with the plugin to inform the server to try again... Depending on what went wrong.}}

{{Spec|Constraints}}

Must export a shared library readable function to get the hub instance (must remap all functions from other kernel classes)

{{Spec|Composition}}

Consists of the Event Manager, System Manager, Resource Manager and Dictionary Manager.

{{Spec|Uses/Interactions}}

Establishes the kernel acts as a bridge from external libraries to kernel

{{Spec|Resources}}

{{Spec|Processing}}

First there is a common library segment where most of the kernel exists.  This is almost a standalone kernel that can run by itself.  Then using plugins to handle the specific functionality of communication, the client and server can add to the common kernel.  This allows a majority of the shared functions to stay that way.

**Main(argc,argv)****Calls Hub Startup** In server mode argv[1](1.md) is Port with an optional ",timeout" and argv[2](2.md) is instance in the DB.
**In client mode argv[1](1.md) is IP/host:port with an optional ",timeout" and argv[2](2.md) is site.****Calls EM Dispatch which is an endless loop.****When Dispatch exits due to a quit, calls Shutdown.**

Currently uses singletons for startups.  Should probably do creation in this.

**On startup:****Calls the Managers' Init which registers with the RM for RPC functions.****: Called in order: DM, EM and then SM.****Calls the RM Connect() (with either instance/port or IP/port)**

**On shutdown it reverses:****Calls RM Disconnect****Then calls DeInit for each of the manager which unregisters them.**

**Whenever a connection goes away (or is dropped by the other side) the RM will call EM quit which drops out and causes the hub to shutdown.**

{{Spec|Interface/Exports}}

**Relays all Manager exports.** Startup and Shutdown.

{{Spec|Unit Test}}

# The individual unit tests are noted below.
# Call startup and make sure all the Managers' RPCs are present in the RM.  Make sure RM's isConnected returns true.
# Call shutdown and make sure all the Managers' RPCs are gone from the RM.  Make sure isConnected is false.

## Event Manager ##

{{Spec|Definition}}

Kernel module that manages sending and dispatching of events.

{{Proposed|In what ways are Event and the object asset similar?  Should events be persistent in any way?  Should these be aligned?}}

{{Proposed|Get rid of callbacks (especially in the query) in favour of using events.  This goes back to original geewhiz with the events for "new object" but is then backed up by the normal query techniques and iteration.  The event name can simply be the string version of the query.}}

{{Proposed|What about the signal/slots mechanism.  This allows you to send an event to a **specific**, but anonymous receiver.  In other words only allow one listener for a particular event type.}}

{{Proposed|Event groups/types.  Along with this we can have protection.  One obvious group set is ClientOnly versus ServerOnly and some of those that may get broadcast from one player to the others... like chat.  Group types would be an easy way to filter the networking.}}

{{Spec|Responsibilities}}

**Allows systems to queue events**Dispatches events to all event listeners registered for those events

{{Proposed|Sending events is almost like calling the RPC except there's no gauranteed reply.  Should this just **be** the RPC?}}

{{Spec|Constraints}}
{{Spec|Composition}}

#EventManager
#**Queues events
#**iEventListeners register for events
#**Dispatches events to event listeners
#iEventListener
#**Receivs events
#Event
#**Contains event specific data
#**There are two classes of events: (A simple flag specifies the type)
#''Local'' - Events that are not sent over the network, such as input events.
#''Distributed'' - Events that are forwarded over the network - Events from the server go to all or specific clients. Events distributed from the client go '''only'' to the server. The server resends them to clients if necessary.
#**Even though events are very similar to Objects, at this time they are ''not'' objects, to keep things simple (and to avoid hassle with Exchange Policies)**

{{Spec|Uses/Interactions}}

#Any iEventListener can register for an event or all events
#Any system can queue up an event
#The events are dispatched to all registered listeners

{{Spec|Resources}}

**'''include/iEventManager.h''' EventManager interface**'''include/iEventListener.h''' EventListener interface
**'''include/Events/Event.h''' ''abstract'' Event base class**

{{Spec|Processing}}

#An event listener registers with the EM
#{{Note|TODO Who calls dispatch?}}
#Events are dispatched to their respective registered listeners
#**Listeners handle events
#**Distributed events are forwarded over the network


{{Spec|Interface/Exports}}

#iEventManager
#**''void queueEvent(Event event)'' - Queue an event
#**''bool dispatch()'' - dispatch the next event. Blocks until an event is received. Returns false if a "quit" event was received.
#**''void addEventListener(iEventListener listener)'' - add an event listener for all events
#**''void addEventListener(String name, iEventListener listener)'' - add an event listener for a specific event
#**''void removeEventListener(iEventListener listener)'' - remove an event listener from all events
#**''void removeEventListener(String name, iEventListener listener)'' - remove an event listener from a specific events
#iEventListener
#**''void event(Event event)'' - called when an Event is dispatched to a listener
#Event - ''abstract''
#**''Resource::Item getProperty(const string& name)'' - get a named event property
#**''bool isLocal()'' - returns**```
true``` if the event should not be forwarded across the network

{{Spec|Unit Tests}}

#Dispatch blocks forever (for tests, forever = a second)
#Dispatch unblocks when an event is queued
#Receiving specific events
#Receiving all events

## System Manager ##

{{Spec|Definition}}

Kernel module that manages threads and systems on the client or server.

{{Spec|Responsibilities}}

**Multithreading of systems, one primary thread per system (However, a system can spawn it's own sub-threads)**Providing an interface to start/stop systems
**Monitors system performance**

{{Spec|Constraints}}

{{Spec|Composition}}

#SystemManager
#**Controls execution of Systems
#**One SystemManager for the ''server'' and one for each ''client''
#**RPC: '''SystemManager''' ''(Server)''**

&lt;-&gt;

 '''SystemManager''' ''(Client)''
#**addSystem() takes a name which is a generic system name, such as "Ogre3D" or "ODE"
#****The name is translated by the ''client'' or ''server'' '''SystemManager''' into a real resource name containing specifics and platform, such as "ODE-Client-linux-x86"
#''System'' are defined in the '''System''' section
#There are two levels of per-system configuration which are loaded by the respective SystemManager
##One on the server, game-based, stores game-specific properties (like which guiscript to load)
##One locally on the client PC, that stores user specific properties (like graphics settings, keyboard mappings)**

{{Spec|Uses/Interactions}}

**Game scripts (and possibly other control mechanisms) can start and stop systems on the server SystemManager**Client System Manager receives and starts all systems through RPC

{{Spec|Resources}}

{{Spec|Processing}}

#The game script tells the '''SystemManager''' to start a set of systems
#**The ''server'' '''SystemManager''' identifies the types of the requested systems
#**#''Server'' systems are started within that kernel immediately
#**#''Client'' systems are queued for all future clients
#**#**The ''client'' '''SystemManager''' starts the systems when it receives the RPC**


{{Spec|Interface/Exports}}

```
iSubSystem* addSystem (String name, float utility);``` - Adds and starts a system
```
void removeSystem (iSubSystem*);``` - Stops and removes a system

{{Spec|Unit Tests}}

**Starting/stopping a system**Loading/saving ''server'' system parameters
**Loading/saving ''client'' system parameters**Test server->client RPC

## Dictionary Manager ##

{{Spec|Definition}}

This replaces strings with integers so that all the string based config is not slow.  It also manages this between client and server so there its time wasted there either.

{{Spec|Responsibilities}}

Manages the mapping from strings to integers and back.

{{Spec|Constraints}}

All strings are unique.

{{Spec|Composition}}

Consists of a hash table.

TODO: desc. AbstractString class

{{Spec|Uses/Interactions}}

Lookup a string or lookup an integer.

{{Spec|Resources}}

{{Spec|Processing}}

Updates are pushed server to client only.  The client may maintain internal strings as needed to handle local side stuff.

{{Spec|Interface/Exports}}

**StringtoInteger**IntegerToString
**Client RPC has a specialized add value.**

{{Spec|Unit Tests}}

# Test string to integer with a new string
# Test the string again make sure the integer is the same
# New string another new integer (not the same)
# Test the integer and make sure the string comes back
# Test a non-existent integer and make sure nothing comes back
# Test adding a value to the client via RPC call, ensure the string and integer come back correct
# Test overlapping a value to the client via RPC (that is the string was added locally so now it gets a new integer.)

## Abstract String ##

{{Spec|Definition}}

The abstract string class can represent a real string (std::string/wstring) or a Dictionary Object (with an associated number.)  The idea being that not all strings need to become dictionary objects and use up hash spaces, but those that do use this kudzu algorithm here to slowly creepily turn each string into a DO that needs to become one... broohoohah.

{{Spec|Responsibilities}}

Once a string has become a DO it stays a DO.  So anytime there is a comparison or an attempt to convert the string to an integer it and all instances it relates to become DOs.  So it also must maintain a shallow copy ... or copy-on-write... regime.

{{Spec|Constraints}}

{{Spec|Composition}}

**String class - This is the main class where all the interfaces are.**

**StringContainer class - This is the flyweight that is the shallow-copy/copy-on-write behavior.**

{{Spec|Uses/Interactions}}

**Create from Native (static functions)****fromLocale - std::string****fromUTF8 - the 8bit unicode****fromUTF16 - the 16bit unicode**

**Construct****from wstring****from DictionaryObject**

**Conversion to****DictionaryObject****Wstring****std::string (locale)****utf8****utf16**

**IsObject - This is whether or not it has become an object or has remained a string.**

**basic\_string<>-like interface (i.e., ==, !=, clear, size, etc.)**

{{Spec|Resources}}

{{Spec|Processing}}

In most cases the processing is obvious.  Its a pass-through to either the DictionaryObject's string or the std::wstring backend (Handled through StringContainer)

The two places where it makes the diode-like switch is comparison and conversion to Object.  The second just returns the underlying object if it is, otherwise it converts it.  In the first place if one of the two is an object, both are converted and compared by their integers instead of doing the $O(n)$ method.

{{Spec|Interface/Exports}}

{{Spec|Unit Tests}}

Compare two strings
Compare one string one DO make sure it converts
Convert a DO
Ccnvert a string (make sure it converts)

## Resource or Asset ##

{{Spec|Definition}}

Resource is the code version of an asset.  Most of the assets are very internal or specific to a system like graphics or physics.  All meta data is recorded in the Objects.

The other major piece is the DLL resource.

{{Spec|Responsibility}}

Provides the model for the game.  In MVC land the Resource is the model, the server's gamescript and ode are the controllers and the client is the view.

The DLL resource is also handled entirely by the RM.

{{Spec|Constraints}}

{{Spec|Resources}}

mathwhiz

{{Spec|Composition}}

**object - This is the prime Resource/Asset.  It is used by the gamescript system to set variables assigned to particular entities, its used by the Resource manager to set queries and its used by other systems for metadata about the remaining assets.  The object has delegates, prototypes and exchange items.  Delegates are out of scope for this spec.  Prototypes are used by the level to create flyweight instances.  That is, if you want hundreds of soldiers that are almost all identical there is a solider prototype and then a soldier instance which overrides bits of the prototype in the instance (like hitpoints or location.)  The exchange items are how the locking works (or doesn't) with that particular item in a multi-threaded situation.  It also helps to identify how that item is going to be updated through the network.  The exchanges are locked, pingpong, writeonce and readonly.**

**3d model - This will be a binary version of a model including normals and texture coordinates** 2d Image - Usually a texture but it can be just about anything image like.
**sound - Wav file, mp3 or ogg.** Music - mp3 or ogg
**script - byte code** text - ascii or unicode

{{Spec|Uses/Interactions}}

**iResources****Name - a unique name for this object.****Type - enumeration over the types listed above.**

**Object****Name/Value pairs.  The value is a ResourceItem.****Delegates read-only values to its Prototype (does not actually copy them or use the space)**

**Prototype****Name/Value pairs.  The value is a ResourceItem.**

{{Proposed|Since the Object is already doing reach-thru to the prototype it would be a simple matter to extend the Prototype like it was before with the delegates and also borrow the pure virtual using the Null().  This would require extensive support in the Level editor to make sure prototypes placed into a level specify valid Assets.}}

**ResourceItem****A boost::Variant which is like a union****Types -** NULL
**Bool** Number - An arbitrary precision number.
**String - A Dictionary String.** mathwhiz::Point3

&lt;Number&gt;


**mathwhiz::Vector3**

&lt;Number&gt;


**mathwhiz::Quaternion**

&lt;Number&gt;


 Exchanges - These define how a resource item can be changed by multiple threads.
**PingPong - This is a triple buffering scheme, it works by using integer copy. Its expected these are read/written to very frequently.** LockedExchange - This is just a traditional lock.  Its expected to change infrequently.
**Read-only - This should never change (beyond when its loaded initially)** Write-Once - This loads in an undefined state and can be written to as set then becomes read-only forever.
 Prototypes - These are objects which get copied to become instances the details of the object then are modified in the instance
**See the level editor for descriptions of the propagation of changes.**

{{Proposed|Discuss delegates... or make prototypes a delegate.}}

**The resources other than object are discussed in more detail below.**

{{Spec|Processing}}

{{Spec|Interfaces/Exports}}

{{Spec|Unit Tests}}

## Resource Factory ##

{{spec|Definition}}

This is how you create new iResource objects.

{{spec|Responsibility}}

Provides the abstract factory framework for understanding a serialized iResource and turning it back into the appropriate thing.

{{spec|Constraints}}

{{spec|Resources}}

This uses the same set of types as the resources.  This is how resources get stored into the RM (so new resources would just need to add their factory to the RM.)

{{spec|Composition}}

**3d model - This will be a binary version of a model including normals and texture coordinates** 2d Image - Usually a texture but it can be just about anything image like.
**sound - Wav file, mp3 or ogg.** Music - mp3 or ogg
**script - byte code** text - ascii or unicode

{{spec|Uses/Interactions}}

**register factories**unregister factories
**construct new empty resources that can be serialized or possibly loaded...**

{{spec|Processing}}

These are handled as shared libraries/DLLS that are inserted into the RM as resources.

{{spec|Interfaces/Exports}}

```
enum getType () ()```
```
register (Factory)```
```
unregister (Factory)```
```
iResource* construct()```
```
Serialize (iResource)```

{{Spec|Unit Tests}}

## Misc Resources ##

{{Spec|3D Model}}

3D Models are broken down into Levels of Detail(LoDs), then into frames, then into vertex data.

#LoDs - A model can have an arbitrary number of LoDs, 0..''n'' (''n'' &ge; 0), where 0 is the '''most''' detailed level, and ''n'' is the '''least''' detailed.
#Frames - Any LoD can have an arbitrary number of animation frames for non-skeletal animation.
##Primitive type - Point, LineList, LineStrip, TriList, TriStrip, TriFan
##Indices - A list of vertex indices
##Vertex - An arbitrary number of vertex streams represented by double-precision floating point numbers.
##**The first stream is always vertex positions, for ease of display in the level editor.
##**Order of other streams is only important to the vertex program. (We don't force NORMALS and TEXCOORDS because we don't use fixed-function or VS 1.x)
##**Each vertex parameter can contain 1, 2, 3 or 4 floating point components.**

{{Proposed|TODO: Skeleton}}

{{Spec|2D Image}}

2D images are laid out in a way that reflects faces (for cube maps) and mipmap levels.

Images are preprocessed on the foundation server, and upconverted to a compatible format. For example, GIFs are upconverted to RGBA (A if it had transparency), 16bit BMPs are upconverted to 32bit RGB. If there existed a 24-bit floating point format, it would be upconverted to 32-bit floating point. However, images are never upconverted to a higher level than necessary (like converting 16-bit BMPs to 64-bit floats) because that just wastes space and bandwidth.

This spec does not take into account compression for transmission (such as wavelet compression), but only what is presented at the end level. {{Note|In fact, it may make more sense to present only raw image data and leave texture compression to the graphics system..?}}

#'''Image'''
#**Broken down into faces (Non-cubemaps have ''one'' face)
#'''Face'''
#**Broken down into mipmap levels (There is always at least one)
#'''Mipmap'''
#**Contains image size and pixel data**

{{Proposed|TODO: Animation frames}}


**Raw Components****For Raw format images, the components are ''always'' laid out in RGBA order. If less than four components are used, the latter components are dropped (e.g. RGB or R)****Compressed formats are subject to their respective standard**Format strings (compression)
"RAW" - Raw pixel data, such as straight RGBA, RGB, etc
"DXTn" - DXT compression (1 &le; ''n'' &le; 5)
**FaceDirection****POSX - Positive X direction****NEGX - Negative X direction****POSY - Positive Y direction****NEGY - Negative Y direction****POSZ - Positive Z direction****NEGZ - Negative Z direction**ComponentType
SBYTE - Signed byte
UBYTE - Unsigned byte
SSHORT - Signed 16-bit int
USHORT - Unsigned 16-bit int
SINT - Signed 32-bit int
UINT - Unsigned 32-bit int
SLONG - Signed 64-bit int
ULONG - Unsigned 64-bit int
HALF - 16-bit half-precision floating point
SINGLE - 32-bit single-precision floating point
DOUBLE - 64-bit double-precision floating point
QUADRUPLE - 128-bit quadruple-precision floating point

**'''Interface'''**

#'''Image'''
#**''const std::wstring& format ()'' - Get the image data format
#**''int32 components ()'' - Number of components per pixel
#**''ComponentType componentType ()'' - Type of component
#**''const std::vector

&lt;ImageFace&gt;

& faces ()'' - The image faces
#'''ImageFace'''
#**''FaceDirection direction ()'' - The face's direction (for non-cubemaps, this will be POSX)
#**''const std::vector

&lt;Mipmap&gt;

& mipmaps ()'' - The face's mipmap levels
#'''Mipmap'''
#**''int32 width ()'' - Get the mipmap level width
#**''int32 height ()'' - Get the mipmap level  height
#**''int32 depth ()'' - Get the mipmap level depth (0 == non-volume images)
#**''uint64 size ()'' - Size in bytes of the mipmap level
#**''const std::basic\_string**

&lt;uint8&gt;

& data ()'' - Pixel data

{{Spec|Sound}}
{{Spec|Music}}
{{Spec|Script}}

**Interface****''const std::wstring& language ()'' - Get the script language****''const std::basic\_string**

&lt;uint8&gt;

& byteCode ()'' - Get the script bytecode/text {{Note|If the language does not support loading bytecode, this will just be script text}}

{{Spec|Text}}

## Resource Manager ##

{{Spec|Definition}}
The whole idea of doing the queries is that the game data is in a "scene database" not necessarily a scene graph or a scene tree.  The real reason for a query though is that if the server knows what kinds of things you're looking for then it knows what to give you before you need it (so that you **have** it when you need it) In other words, this is an excuse to talk about the asset streaming.  But its also the way the systems (like ogre and ode) get the data they need to operate on.

{{Spec|Responsibilities}}
Real-time on-demand streaming.  The assets need to be there when they are needed in game space.  Much of this is taken care of by the queries on the server which inform through their use what will be needed when.  More on this below.

The queries must be populated and the systems notified of the new data (or removed data) when they register for it.

{{Spec|Constraints}}
The whole thing must start within 30 seconds... at some point.

{{Spec|Composition}}

'''RM server''' - This understands the level construct, reads data from the mysql database, maintains references to this data, provides an RPC to client-side RMs so that they can request these data items and packages the data items in a reloadable way so they can be directly loaded client side and potentially cached to a faster disk than the mysql db will be.  Understands queries and asks for the relevant data from the server RM pertaining to that query.  It fills queryobjects with this information including queryobjects which may only be used by the client side EM.

'''RM client''' - this knows how to connect to a server and reflect queries for data upstream.  It can read the packages that the server RM reads and if it offloads onto disk and out of memory it keeps track of this (including through separate runs) in order not to overload network requests.  It will fill objects as part of the query list through RPC requests unless they are already stored to the disk.

'''QuerySpec''' - this defines the complex queries (currently only ALL)

'''QueryObject''' - This maintains the object references into the RM. Can be browsed via an iterator. Can have listeners attached for notification when the query changes (add/remove).

'''QueryObjectCallback''' contains the types of functions called (basically added or removed)

'''ResourceIterator''' is like any ol' iterator except over resource

'''Resource''' is a general interface into the data.  There is a server resource and client resourse.  The server resource knows how to write-through to the database as it is updated.  It also can serialize and get sent down to the client.  The client can unpack into its own resource.  The server also sends updates of itself down to the client and the client resource can read those updates and modify accordingly.

{{Spec|Uses/Interactions}}
The idea is that either client or server systems can generate a query with their RM.  The server RM then loads those things in order to fill the query.  These get pushed to the client which then also fills the queries on that end.  Either way they inform the requesting system of the new objects (the systems can also just iterate over the queries if they are going to be iterating anyway...)

'''Iterator''' This is managed by something external to the RM (usually the ODE physics system, but if the game isn't using physics, just call it after every game loop)  The iterator increments an internal counter that represents time.  This acts like an instantaneous snapshot of the state of every Object at that moment in time.  This state is then copied back into the mysql database to keep track of the game up to that point and also sends this information down to the
client to keep it up to date.  Client side iteration is ignored for now.

{{Proposed|There are many things to propose about the iterator.  In no particular order:  Saving the state allows replays.  The iteration on the client side should be the place where network updates occur as this means that whoever is normally modifying the state is blocked waiting on the iteration to complete.  Queries, once implemented normally, can be updated at this time so that everyone can be notified (hopefully asynchronously although maybe not necessary or wanted...) Especially useful for very frequently modified data.  Change-only (which is not implemented in this version) can be handled by querying the object for only those items that have changed this iteration... That's all I can think of during '''this''' writing}}

{{Spec|Resources}}
General Discussion: [[Future Features|Scenegraph As Database](Geewhiz.md)]

{{Spec|Processing}}

**Query Handling****''QueryObject'' is maintained by reference count.****''query()'' returns the same ''QueryObject'' for the same ''QuerySpec'' (increments ref)****On the server side new objects that get created which match the query are added in.****The same sort of thing happens when objects get deleted (removed from the query)****Queries can have callbacks attached for both create and delete to let the listener know if the query is modified.****Queries also have an iterator for walking through the currently available objects.****: The QueryAll in vos is equivalent to requesting all children from the site object.  That object will be inherently related to the current level.  In the future we might be able to pick better site queries than "ALL".  The site will be cached via cacheChildren[http://www.interreality.org/static/docs/api-html/class_v_o_s_1_1_vobject_base.html#5f3e16caf1296f231c8ba5d78b90015f ] ''Is this recursive?''****When the client receives a query it forwards it directly to the server as long as it exists on the client.** This extra maintenance is handled by a stub on the server
**The client only receives a list of the objects as part of the query**: the client uses the list to pull the objects needed.  This is done by using a VOS query.
**: Whenever a resource is requested through the RM this is done via the Object URL** The client's query only maintains those objects that have fully arrived.

**Resource Handling****Only the server '''RM''' needs knowledge of Mysql.  It does write-thru on the server to the current level instance in the DB.  The server will store the resources in some serializable form and then send that to the client '''RM''' which will unpack them as they show up.  Much of the assets in a resource are properties of a vobject.****The client '''RM''' will contain those objects that are referenced by Query.  It will be important to control the types of queries the client can pull so that it can't get too much data, but that is a later concern.  The client RM will simply request a pull from the server to fill a query as described above.****Level objects are vobjects and the properties are locally cached.[http://www.interreality.org/static/docs/api-html/class_v_o_s_1_1_property.html#543ab66a9aaea9ff80e86af0b0ae59f1 ]****All assets which are used as VOS properties are treated as read-only.****RM has an API call to "newLevel" and "loadGame" which changes the current instance set and thus invalidate all queries (turn them empty.)  It also has a "saveGame" which saves the state of all the resources at that moment.  These work the same as going through the web interface, but since its all in game, the instance its working with is the current "Pause".** The newLevel, loadGame and saveGame are all mysql procedures.  This allows them to be reused by site (at least newLevel and loadGame are).
 Any objects created local side will be un-duped onto the server because the server would have to query them explicitly anyway.  This is true of properties as well.
 The server side creates the Vobjects for the site dynamically by querying through the DB tables.

**Database Handling****The server site will be specialized to load objects directly from the database and convert them into objects as described above.****Changes to these objects will also create INSERT DELAYED or UPDATE DELAYED statements into mysql... this is the "queuing" that goes on.****When a save occurs, the GameLevelInstance, GameObjectInstance and GameObjectInstanceValues tables will be locked and a copy made (this will block those DELAYED inserts.)  Once the copy is done, the locks are lifted and the save date will be updated.****For a load the same tables will be locked again and the save instance will be copied over Instance that was passed in through connect.****On ChangeLevel (also can be used to set the level in the first place) will lock the three tables as well as Level, ObjectInstances and ObjectInstanceValues in order to copy from the latter three into the Game Instances associated with the current Level instance.**

{{Note|Is it really going to be necessary to lock the level instance?  I don't think it will, but for now just in case.}}

{{Spec|Interface/Exports}}

**RM**```
QueryObject query (QuerySpec)```
 ```
void createObject (...)```
 ```
void deleteObject (...)```
 ```
void saveLevel (String``` - Saves out a saved level instance with that name.
 ```
void loadLevel (String)``` - Loads a saved level instance with that name (each of these keeps track of the game associated to the current level instance so that it can maintain that link.)
 ```
void changeLevel (String)``` - Copies the prestored instance into the current pause set.

**QueryObject**```
void addCallback (QueryObjectCallback)```
 ```
void removeCallback (QueryObjectCallback)```
 ```
QueryIterator begin ()```
 ```
QueryIterator end ()```

**QueryObjectCallback**```
abstract void resourceAdded (Resource)```
 ```
abstract void resourceRemoved (Resource)```

```
QuerySpec QuerySpec::ALL```

**Resource - Base class, is inherited by the actual types (which are defined in the download/play section)**```
ResourceType getType ()```
 ```
string getName ()```

**Object Resource - Name value pairs.
{{Spec|Unit Tests}}
# call newLevel and make sure the queries fill with only those things (make a new level that contains no crossover assets)
# call loadLevel and make sure the same kinds of things about it
# call saveLevel and make sure all peices of the new instance are stored in the tables
# Get a query object and ensure its got the proper items (known DB) on both client and server
# Add an object to the server RM and make sure the callbacks are done on both the server and the client side
# Delete an object check the callbacks
# check the iterator for each of those cases**

## NetworkManager ##
{{spec|Definition}}
The network library right now will just handle RPC commands.  Everything the RM and other systems do will be through RPC.  The RM will be in the interface for this to everything else.  The only reason the netlib exists is to have a center point for client vs server RM.

{{spec|Responsibilities}}
**Allows registration of callback functions that take a vector of name/value pairs strings (such as show up in object.  The values should become ResourceType)** Forwards any exceptions that happen from the call.
**Returns a set of name/value pairs from the function as well.** Allows low level registration of a binary array call (over the name/value pair style.)

{{spec|Constraints}}
**Doesn't check parameters.
{{spec|Composition}}** Net - Creates the connection in the constructor and drops it in the destructor.  Handles the registration and the transport.

**Callback - for the RPC is just a boost function that takes a std::map (for the name value pairs)
{{spec|Uses/Interactions}}** Register an RPC call
**Unregister a call** call an RPC
**connect and disconnect - Both connects have a timeout parameter associated with it which is accurate to withing x to 2x.  Thus if the specified timeout is 10 then the timeout range is guaranteed to be between 10 and 20.**

{{spec|Resources}}

{{spec|Processing}}
The network processing happens in a separate thread from the other processing.  RPC calls run in the thread of the calling process.
RPC calls first start with a sync word followed by a string representing the RPC call.  The string is preceded by its length.  Its followed by the name value strings in the same manner (length precedes the string) or by the low level byte data.
The return message contains a sync word for the return (different word) followed by either the name value pairs of hte return or hte low level bytes.
If the return is an exception its yet another sync word followed by the type of the exception and the content used to reconstruct the exception on the callers side.

The socket writes all take place in the caller thread.  It will marshal and then write out waiting on the return packet where it will then need to unmarshal and return.
The unmarshal returns either an Object or the lowlevel bytes or it will throw an exception.

{{Proposed|In this version, there is no timeout, but its TCP so it shouldn't be a problem.  However there might be an issue with dropping connections.}}

{{spec|Interface/Exports}}
```
void registerRPC (string name, function, lowlevel = false);```
```
void unregisterRPC (string name)```
```
std::map callRPC (string name, std::map)```
```
std::vector callRPC (string, std::vector)```
```
connect (string site)``` - If host is empty its a server connection, otherwise its a client.
```
disconnect()``` - disconnects.

## System ##

{{Spec|Definition}}

A common system interface and two default base abstract implementations for different system behaviors.

{{Spec|Responsibilities}}

**Provide a common interface to the systems**Provide a blocking behavior base system implementation
**Provide a looping behavior base system implementation**

{{Spec|Constraints}}

**Must provide initialization in the '''SystemManager''' thread as well as the System's own thread**

{{Spec|Composition}}

**iSubSystem - Common interface to all systems****iSubSystems is very similar to an Object in that you can get/set name/value pairs, but at this time it is ''not'' an object**AbstractBlockSystem - Abstract base system that blocks on events
**AbstractLoopSystem - Abstract base system that loops at a given frequency**

{{Spec|Uses/Interactions}}
{{Spec|Resources}}

**include/system/iSubSystem.h**include/system/AbstractBlockSystem.h
**include/system/AbstractLoopSystem.h**

{{Spec|Processing}}

{{Spec|Interface/Exports}}

**Interface iSubSystem****'''Number''' ''frequency()'' -- returns the frequency this system should run at****void ''init()'' -- System initialization in the SystemManager thread. Happens at ''addSystem()'' time. Will block later systems from initialization until complete.****void ''threadStart()'' -- System initialization ''inside'' the system thread****void ''threadStop()'' -- System de-initialization ''inside'' the system thread****void ''deinit()'' -- System de-initialization in the SystemManager thread. Happens at ''removeSystem()'' time. Will block earlier systems from de-initialization until complete.****void ''blockStep()'' -- If frequency() returns 0, this will be called and should wait until a condition is met, then returns. ''step()'' is called once, then ''blockStep()'' is called again.****void ''step(iSystemMonitor, float)'' -- Runs the system for a single step****void ''stop()'' -- Stops the current step, if blocked****Resource::Item ''getParam(String name)'' -- Get a system parameter****void ''setParam(String name, Resource::Item value)'' -- Set a system parameter**

{{Spec|Unit Tests}}

**Functions are called in correct threads (''thread**()'' and step called in system thread)
**AbstractBlockSystem blocks until event is received**AbstractLoopSystem does not block
Loops at approx. the right freq.

## Dynamic Package ##

A dynamic class package specification for loading/storing dynamic classes.

All data is stored as a flat binary file that is equivalent to data stored in a '''Resource::Payload'''.

All numbers are stored in network (''big-endian'') order.

{| border="1" cellspacing="2" cellpadding="2"
! Size
! Description
|-
|''uint32''
|Version of package
|-
|''uint32''
|Size of module string
|-
|size ''bytes''
|UTF-8 string: architecture specific module file to load (eg. libGenericSystem.so)
|-
|''uint32''
|Size of path string
|-
|size ''bytes''
|UTF-8 string: relative path to load system from (eg. "GenericSystem") Note that this must equal the .so's RPATH
|-
|''uint32''
|Size of the self-extracting archive
|-
|size ''bytes''
|Data consisting of the package's self-extracting archive file to be unpacked.
|}

## Components ##

{{Note|Related to the question above about resources...}}
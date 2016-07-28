Attendees of this course may use either Java & Eclipse or C# & Visual Studio. Depending on the language you want to use, the tools you'll want to install will vary.

## Java

### Source code

There is a preconfigured project that we will work from. Download it and verify everything works on your machine by following the instructions in its README files:

* [java-testing-example](https://github.com/testdouble/java-testing-example) 

Once you've imported and installed the top-level project, you'll see that it defines a number of child projects that we'll work from over the course of the week:

* [example](https://github.com/testdouble/java-testing-example/tree/master/example) - a sandbox for ad hoc exercises
* [greeting](https://github.com/testdouble/java-testing-example/tree/master/greeting) - a project for practicing the [[Greeting Kata]]
* [unusual-spending](https://github.com/testdouble/java-testing-example/tree/master/unusual-spending) - a project for practicing the [[Unusual Spending Kata]] (this [vendor](https://github.com/testdouble/java-testing-example/tree/master/unusual-spending-vendor) project supports it)
* [example-cucumber](https://github.com/testdouble/java-testing-example/tree/master/example-cucumber) - a sandbox for writing cucumber examples
* [bank-ocr](https://github.com/testdouble/java-testing-example/tree/master/bank-ocr) - a project for using Cucumber to practice the [[Bank OCR Kata|Bank OCR Kata in Cucumber]]
* [selenium](https://github.com/testdouble/java-testing-example/tree/master/selenium) - a project for practicing writing JUnit and Cucumber tests with Selenium
* [gilded-rose](https://github.com/testdouble/java-testing-example/tree/master/gilded-rose) - a project for practicing the [[Gilded Rose Kata]]
* [game-of-life](https://github.com/testdouble/java-testing-example/tree/master/game-of-life) - a project to work from for a [[Code Retreat]]

### IDE Plugins

There are a couple IDE plugins that we will use to reduce the amount of friction when running tests inside Eclipse.

* [MoreUnit](http://moreunit.sourceforge.net/#download)
* [EclEmma](http://www.eclemma.org/installation.html)
* [Natural](https://github.com/rlogiacco/Natural)

All three of these plugins can be installed via the Eclipse marketplace, their own update sites, or manually. If a plugin's update site is inaccessible, download the source and manually install the plugin by placing the expanded plugin into the `dropins` folder in your Eclipse installation directory and then restarting Eclipse.

Verify MoreUnit is installed by right-clicking anywhere in a Java source listing and verifying a "MoreUnit" item appears in the context menu.

Verify EclEmma is installed by right-clicking a JUnit test and seeing a "Coverage As…" item in the context menu.

Verify Natural is installed by opening a Cucumber feature file (example: `bank-ocr/src/test/resources/bank/bank-ocr.feature`) and verify that syntax highlighting is enabled like so:

<img width="440" alt="screen shot 2016-07-27 at 11 00 20 am" src="https://cloud.githubusercontent.com/assets/79303/17180119/5ef04be0-53e9-11e6-8482-901443fda216.png">

### IDE Settings

#### Download source & docs

By default, Eclipse will only fetch compiled dependencies, but not the source attachments or JavaDoc. Doing so adds little overhead and allows you to navigate to and read the code you depend on with a simple ctrl-click. Just visit `Preferences` -> `Maven` and checking `Download Artifact Sources` and `Download Artifact JavaDoc`

<img width="646" alt="screen shot 2016-07-27 at 9 57 14 am" src="https://cloud.githubusercontent.com/assets/79303/17177689/97ae6d80-53e0-11e6-90ad-49f5d7029fd7.png">

#### Wildcard static imports

In order to clear warnings, it's common to use the "Organize imports" command (ctrl-shift-o), but it has the side effect of creating an explicit import statement for every static import, clearing out any wildcards. This has the side effect of making [DSL](https://en.wikipedia.org/wiki/Domain-specific_language) libraries like Hamcrest harder to use.

To change the behavior of organize imports to wildcard static imports more aggressively, visit  `Java` -> `Code Style` -> `Organize Imports` and change "Number of static imports needed for .*" to `1`

<img width="643" alt="screen shot 2016-07-27 at 10 00 51 am" src="https://cloud.githubusercontent.com/assets/79303/17177827/154a119a-53e1-11e6-8310-1fe13d680f5c.png">

#### Organize packages hierarchically

When creating a lot of classes, it's easier to organize them into well-considered package hierarchies, but by default Eclipse will list them all out in a flat view that makes it harder to navigate. To display them hierarchically, click the Project Explorer pane's downward-facing triangle, and selecting `Package Presentation` -> `Hierarchical`:

<img width="666" alt="screen shot 2016-07-27 at 10 02 47 am" src="https://cloud.githubusercontent.com/assets/79303/17177879/4abb7e54-53e1-11e6-9d94-20d675d5d1e2.png">

#### Organize projects hierarchically

When working on a multi-project Maven module or a working set, you can group those projects together in Eclipse's Project Explorer by clicking the downward-facing triangle, and selecting `Projects Presentation` -> `Hierarchical`:

<img width="660" alt="screen shot 2016-07-27 at 9 55 10 am" src="https://cloud.githubusercontent.com/assets/79303/17177939/82a06aaa-53e1-11e6-83f3-fb52251976c6.png">

#### Autosave 

When juggling a lot of files, switching tabs after forgetting to manually save can cause needless disruption to one's workflow—debugging only to realize the root cause was that a file wasn't saved can be really frustrating! 

To configure Eclipse (4.6 & later) to autosave dirty editors, visit `Preferences` -> `General` -> `Editors` -> `Autosave` and enable autosave for dirty editors. Unless you have a slow build and "build automatically" enabled, reduce the default 20 second timer to something shorter (I have mine set to 1 second here):

<img width="662" alt="screen shot 2016-07-27 at 10 13 47 am" src="https://cloud.githubusercontent.com/assets/79303/17178286/dcb3b384-53e2-11e6-8977-f3e360df7e9d.png">

#### Auto-format

To reduce whitespace commits, it can be helpful to auto-format & auto-organize imports on stage. To set it up, 
visit `Preferences` -> `Java` -> `Editors` -> `Save Actions` and tick "Perform the selected actions on save", along with "Format source code" and "Organize imports"

<img width="760" alt="screen shot 2016-07-28 at 11 05 35 am" src="https://cloud.githubusercontent.com/assets/79303/17217701/45c63ccc-54b3-11e6-9462-0a6d50b02e4f.png">

## C&#35;
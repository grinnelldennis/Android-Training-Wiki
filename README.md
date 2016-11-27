# AppDev Android Training Wiki

## Sessions
* [11/23](#11-23)
* [11/9](#11-9)
* [11/2](#11-2)
* [10/12](#10-12)

<a name="11-23">
# 11/23/16
</a>

This week's homework is more of a reading assignment than a programming assignment. We've been building upon all the major topics, and it will culminate in a final, useable product. Here's the big picture: you will create an app that users can sign up and login for, then view a list of "stories" retrieved over the network with JSON, a detailed view of the story, and finally logout. That seems like a lot, but it will all be built up in parts. This week is reading in greater depth about some of the topics we've discussed.

First, layouts. Recall that we use XML layouts to describe how we want our Activity to look. Here is an article that goes into further detail on the different types of layouts and the various attributes you can use: https://guides.codepath.com/android/Constructing-View-Layouts. You should try to use this to make your login and signup pages look prettier if you can -- this isn't homework you must complete for next class, but you will need to do this eventually.

Second, buttons. We talked briefly about onClick and how to use buttons, but there are many more things you can do with them: https://guides.codepath.com/android/Basic-Event-Listeners

You don't need to memorize every detail about the various types of layouts or know exactly how to use all the listeners, but you should know that they all exist in case you need to use them in the future.

Finally, ListViews. A common pattern in mobile apps is having a small scrollable list of content. For example, [here](img/listview.png) is an example of a simple `ListView` with radio buttons, which has a static source of options. As a sidenote, we use a `RecyclerView` for larger datasets to decrease memory consumption, and we'll go into more detail on this in a later session. The visuals and data are broken down into three major components: the `DataSource`, `Adapter`, and Adapter View. The `DataSource` holds the "raw" data in the form of Java objects. For example, take this JSON file:

```json
{  
   "status":0,
   "users":[  
      {  
         "id":1,
         "name":"Mattori"
      },
      {  
         "id":2,
         "name":"Larry"
      }
   ]
}
```

If you are unfamiliar with JSON you can read up on it [here (although skip the Javascript part)](http://www.w3schools.com/js/js_json_intro.asp), but it's a very straightforward data format. Objects are contained in `{}`. Objects must use pairs of `names` or `keys`, which are strings, with `values`. A `value` can be a string, number, boolean, object, null, or an array. Arrays are contained in `[]` and use commas to separate values.

The above JSON object has a `status` of 0 and an array of `users`. Each user has an `id` and a `name`. Only two users are defined: `Mattori` with `id` 1 and `Larry` with `id` 2. Typically, we would parse the JSON and see that since the `status` is 0 then there were no issues retrieving the data. Then, we would take the `users` array and pass that as a `DataSource`, possibly as an `ArrayList`.

An `Adapter` is, generally, something that bridges between the data and the `View`. One major application is with lists of data since you can then define a `View` for each item in the list. For example, we might not want to actually show what each user's `id` is but we would probably want to show the `name` in big text.

The Adapter View is the way we want to represent this set of data. The basic variants are a `ListView` and a `GridView`.

Take a look through this article on `ArrayAdapter`s and `ListView`s: https://guides.codepath.com/android/Using-an-ArrayAdapter-with-ListView. The important sections are `Using a Custom ArrayAdapter`, `Attaching the Adapter to a ListView`, `Populating Data into ListView` (although don't worry too much about parsing JSON yet), and `Attaching Event Handlers Within Adapter`. Everything else is useful but extra.

We will go into further detail on what exactly you are making and what the time frame is next class.

<a name="11-9">
# 11/9/16
</a>

## TOC
* [Prerequisites](#11-9-prereqs)
* [Homework](#11-9-hw)
* [Mockups](#11-9-mockups)

<a name="11-9-prereqs">
### Prerequisites
</a>

So far, we've learned about Activities and Intents. Let's tie it all up!

For this assignment, you will need to be able to produce new Activities with custom content, respond to buttons, read data from widgets, and start new Activities with extra data/parameters. If you are unclear on any of this, look at previous lessons and the [Android API Reference](https://developer.android.com/reference/packages.html).

<a name="11-9-hw">
### Homework
</a>

Implement an application that features a login screen and subsequent greeting screen based off of their login. The login screen should ask for their username and password and then pass that information on to the greeting screen, which can simply greet the user by their username (here's a hint: look into [EditText](https://developer.android.com/reference/android/widget/EditText.html)). Once you complete that, try to add a registration feature. When the user tries to login with an invalid combination they should get a response -- a `Toast` works.

I realize there is a login Activity template in Android Studio; do not use it. Not only does it defeat the purpose of learning, but it also contains a lot of junk and unnecessary content. It is only one of many different ways to make a login screen for Android.

This is going to be a bit different than what we've done in the past. This is a real project that you will be building upon in the future, and it will culminate in a complete, usable app. You don't need to worry about making it look pretty yet, but if you have extra time and want to play around feel free to add some polish or bonus features!

<a name="11-9-mockups">
### Mockups
</a>

When you are developing an app for AppDev, you will be given mockups from designers of what the app should look like. This includes all screens and panels, including multiple mockups for side panels and settings. Here are a couple mockups for AppDev's Publications app for SPARC, which is still a work in progress. You can use them as a model for your own login screens. The only caveat is that you should not try to duplicate the logo.

![Splash screen](img/splash-screen.png)

![Sign up screen](img/sign-up.png)

---

<a name="11-2">
# 11/2/16
</a>

## TOC
* [Prerequisites](#11-2-prereqs)
* [Intents](#11-2-intents)
* [Sending Data](#11-2-sending)

<a name="11-2-prereqs">
### Prerequisites
</a>

At this point, you should understand classes vs layouts, how to add and interact with widgets (like a `Button`), and how to respond to events (like on click events). If you are still confused about any of these, please refer to the 10/12 wiki page. Here are a few brief summaries:

#### Classes vs Layouts

All of your application logic should live in your Java classes, and all (most) of your application visuals should live in your XML layouts. Logic should not go in XML because it's very hard to express anything complex with XML, outside of some simple behavior like on click, which you should generally avoid in favor of Java anyway. Visuals should not go in Java because it's very hard to express hierarchies and attributes like positioning with Java.

#### Adding Widgets and Events

Widgets should be added via your layouts and given arguably the most important attribute: the `id`. You should put as much detail as you can in the XML representation of your widgets. Here is a concrete example of adding a `Button` widget:

```xml
<Button
    android:text="Hello, World!"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:id="@+id/button" />
```

As a reminder, this `Button` does not actually do anything yet, other than the default animation for when you tap it. The way that you give it behavior is by referencing it in Java and then giving it on-something events. The easiest one to see is the on click event, so here's an example of listening to that and making a Toast:

```java
// in your onCreate function
final Button button = (Button) findViewById(R.id.button);
button.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        Toast.makeText(getApplicationContext(), "Clicky clicky", Toast.LENGTH_LONG).show();
    }
});
```

<a name="11-2-intents">
### Intents
</a>

Now that we're caught up, we can move onto the meat of today: Intents! Intents are the way that we communicate between Activities in Android. You can think of it as our intent to go to something else; for now, it's just within our app with an explicit intent, but down the line we can start `Intent`s that cause other apps to open with an implicit intent. An explicit intent looks something like this:

```java
final Intent popupIntent = new Intent(this, PopupActivity.class);
startActivity(popupIntent);
```

Note that the first parameter, `this`, is supposed to be a `Context`. An `Activity` is a `Context` because it implements `Context`, but if you're writing an `OnClickListener` then you won't be able to just write `this`. The easiest way out of this dilemma is to write `getApplicationContext()` there instead; we'll get to `Context` at a later time. This code specifies a specific component, the `PopupActivity`, to be opened. That class is not defined yet, so let's throw together a quick little `Activity` to demonstrate that our `Intent` worked. Here's the Java code, which should go in `PopupActivity.java`:

```java
public class PopupActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_popup);
    }
}
```

...and here's the corresponding XML layout, which should go in `activity_popup.xml` in `res/layouts`. Note that the only significant thing here is a `TextView`, just to indicate the `Activity` even appeared.

```xml
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:id="@+id/activity_popup"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:paddingBottom="@dimen/activity_vertical_margin"
    android:paddingLeft="@dimen/activity_horizontal_margin"
    android:paddingRight="@dimen/activity_horizontal_margin"
    android:paddingTop="@dimen/activity_vertical_margin"
    tools:context="edu.appdev.android.androidtraining.PopupActivity">

    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="New activity!"
        android:id="@+id/text" />

</RelativeLayout>
```

The last step is to put the new `Intent` code in the correct place: in the on click method of the button in the `MainActivity`. Here's the full `MainActivity.java code`:

```java
// whatever your package setup is

import android.content.Intent;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;
import android.widget.Button;

public class MainActivity extends AppCompatActivity {
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        final Button button = (Button) findViewById(R.id.button);
        button.setOnClickListener(new View.OnClickListener() {
            @Override
            public void onClick(View view) {
                final Intent popupIntent = new Intent(getApplicationContext(), PopupActivity.class);
                startActivity(popupIntent);
            }
        });
    }
}
```

<a name="11-2-sending">
### Sending Data
</a>

Sometimes you want to actually send some information to the new `Activity`. In this case, let's send an arbitrary constant String. This variable can be sourced from anywhere; in fact, you can put any type as long as it is a primitive (ie. `int`, `boolean`) or implements the [`Parcelable` interface](https://developer.android.com/reference/android/os/Parcelable.html). But for simplicity, let's just send a name for the `PopupActivity` to display. Update the `Intent` creation code to look like this:

```java
final Intent popupIntent = new Intent(getApplicationContext(), PopupActivity.class);
popupIntent.putExtra(PopupActivity.EXTRA_NAME, "Larry");
startActivity(popupIntent);
```

Notice that `PopupActivity.EXTRA_NAME` is referenced here. The first parameter of the `putExtra` function is always some String to identify the second parameter. It should typically be a constant since it never changes and is only used when making `Intent`s for the `PopupActivity`. You get partial credit for thinking about the `strings.xml` XML file in `res/values`; yes, we generally want to put String literals there, but usually only Strings that are seen by the user. This String constant is just an internal identifier, much like a regular variable name, so it's sufficient to just make it a Java constant instead of an XML constant. You can make the `PopupActivity.EXTRA_NAME` constant by adding the following line:

```java
// ...
public class PopupActivity extends AppCompatActivity {
    public static final String EXTRA_NAME = "extra_name"; // add this line inside PopupActivity but outside of any methods

    @Override
    protected void onCreate(Bundle savedInstanceState) {
// ...
```

Now we just need to update the `onCreate` method to actually use this extra data. We can get the String extra with the following line:

```java
String name = getIntent().getStringExtra(EXTRA_NAME);
```

If we were to send an extra of a non-String type, we need to use a slightly different method. There are a number of default methods defined for getting extras, such as `getIntExtra` and `getStringArrayExtra`. To get an extra that implements `Parcelable` -- again, you need to do this if you're getting a non-String non-primitive extra -- use the `getParcelableExtra`. The next line we need is to make the String we got appear on screen. We can do that by getting the existing `TextView` and updating its text, like so:

```java
TextView txt = (TextView) findViewById(R.id.text);
txt.setText("Hi " + name);
```

If you put both of these lines at the bottom of the `onCreate` method of `PopupActivity`, run the app, and then tap the button you should get your new `PopupActivity` with some text that says "Hi Larry"!

## Conclusion

As always, if any of this was confusing, please do not hesitate to contact Larry or Mattori. `Intent`s can get somewhat complicated, but for now things are fairly simple and straightforward. You will use this content in the upcoming homework, so make sure you understand it.

---

<a name="10-12">
# 10/12/16
</a>

Have any questions? Feel free to ask [Larry Asante](mailto:boatenga17@grinnell.edu) or [Mattori Birnbaum](mailto:birnbaum@grinnell.edu), via email or in person.

## TOC
* [Android Studio](#10-12-android-studio)
* [Github](#10-12-github)
* [Emulator/Genymotion (optional)](#10-12-emu)
* [The HelloWorld App](#10-12-helloworld)
    - [Classes vs Layouts](#10-12-classes-layouts)
    - [Resources Overview](#10-12-resources)
    - [Event-driven Programming Overview](#10-12-events)
    - [Polymorphism](#10-12-poly)

<a name="10-12-android-studio">
### Android Studio
</a>

At this point, you should have [Android Studio](https://developer.android.com/studio/index.html) installed and configured. You will also need the necessary components from the Android SDK Manager, with preferably the latest version:

* Tools
    - `Android SDK Tools`
    - `Android SDK Platform-tools`
    - `Android SDK Build-tools` (note that new revisions are listed as a separate download, so when you want to update you'll need to install the new one rather than simply patch the existing download)
* Android 7.0 (API 24)
    - `SDK Platform`
* Android 4.0.3 (API 15)
    - `SDK Platform`
* Extras
    - `Android Support Repository`

We created new Android projects for our HelloWorld app last time, but here are the important options if you want to make a new one:

* `Application name` can be whatever. In this case, it's `HelloWorld`.
* `Company Domain` is like the URL for our "company." Typically, we use `android.appdev.edu`.
* Your target `Minimum SDK` should be `API 15`. This is the minimum version of Android that can run our app. As of this writing, 97.4% of devices are on that API level or higher, which is good enough for us.
* You can add an `Empty Activity` to your project. All these options just generate different amounts of "boilerplate" code, which is just the basic content required to get something running.

<a name="10-12-github">
### Github
</a>

You should go to [Github](https://github.com) and create an account if you don't already have one. You probably won't need it for a bit for this class, but if you plan on doing any programming in the real world you will need a Github account, and you will definitely need one for AppDev. If you're not comfortable with git yet, you can review with [Github's git tutorial](https://try.github.io/).

We'll go through the functionality of Github when it's necessary, although you can feel free to look around or even take a look at [AppDev's Github account](https://github.com/GrinnellAppDev/).

<a name="10-12-emu">
### Emulator/Genymotion (optional)
</a>

If you don't have an Android device or want to be able to test purely on your computer, it is advisable to use [Genymotion](https://genymotion.com) instead of the default Android emulator as it's significantly faster and a bit nicer to use. For some reason, Genymotion tries to imply that the only option is to pay for use of its emulator, but if you go to the ["Fun Zone"](https://www.genymotion.com/fun-zone/) you can download the personal edition. You need to create an account, and the emulator technically does not have all the features unlocked, but it's still very effective for development.

There is a convenient [Genymotion plugin](https://www.genymotion.com/plugins/) for Android Studio, which allows you to start and manage our emulator devices.

<a name="10-12-helloworld">
### HelloWorld App
</a>

<a name="10-12-classes-layouts">
#### Classes vs Layouts
</a>

An important concept in Android, which also happens to transfer over to iOS and indeed many other GUI-related frameworks, is the separation of application logic and views. The idea is that visuals can be better represented in a more specialized language, which in the case of Android is [XML with a variety of widgets already defined](https://developer.android.com/guide/topics/ui/declaring-layout.html) (and more can be added easily by developers), while more complicated tasks like reacting to buttons and doing network communication can be handled in code, which in the case of Android is Java. These are not 100% strict rules as you can dynamically create visual widgets in code (for example, if you wanted to create an image when a button is pressed) and can set up some larger components, like scrollable lists, in XML, but generally you want to keep things separated for clarity.

A good general rule, both in Android and in any at least moderately sized project, is to keep things clear, even if that means you have to write a bit more. For example, in Java the `@Override` annotation, used in polymorphism, is not actually required, but it's helpful to clarify to yourself and anyone else that wants to read your code that that method is overriding one of the parent type's methods (if this is confusing, look at the [Polymorphism](#10-12-poly) section below).

Just like how we use variables and give them arbitrary names in Java (ie. `Button btn = ...`), we give layout widgets `id`s in order to identify and do stuff with them. Take, for example, the following `TextView`:

```xml
<TextView
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:text="Hello, World!"
    android:id="@+id/text" />
```

This `TextView` has both a width and height of `wrap_content`, meaning it will only be as big as the text it contains; its text value is `Hello, World!`, so that's what it will show; and its `id` is `text`. The `id` attribute is arguably the most important attribute, more than the layout or text ones, because it's the only way we can access it from our code. Everything else about virtually any widget can be configured through code (it's a bit uglier, but it can be done), but it's only possible by using the `id`.

The way that we take a widget defined in a layout and use it in code is as follows:

```java
TextView txt = (TextView) findViewById(R.id.text);
```

The important parts to note here are:

* We want this variable to be a `TextView`
* There needs to be a cast `(TextView)` to clarify what type of `View` we want (more on that in [Polymorphism](#10-12-poly))
* The `findViewById` method is how we get anything from a layout
* The `id` attribute of the `TextView` we created is used in Java through `R.id`

The `R` class is a special class generated by the Android compiler that you should *never* try to mess with or risk breaking major parts of your project. It is a representation of any resource in your project that you give an id to. More on this in the next section, [Resources Overview](#10-12-resources).

Now, let's create a widget that actually has some interesting functionality to it, like a `Button`! Here's the layout:

```xml
<Button
    android:text="Click me!"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_alignParentLeft="true"
    android:layout_alignParentStart="true"
    android:id="@+id/button" />
```

Don't worry about the layout-related attributes for now. The most important part is, again, the id. Right now, if you were to put this `Button` in your main activity's layout and run it there would be a corresponding button that allows you to click it... and that's it. Usually, when we use buttons we want them to actually do something, so let's add an on click event:

```java
Button btn = (Button) findViewById(R.id.button);
btn.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        Toast.makeText(getApplicationContext(), "hello", Toast.LENGTH_LONG).show();
    }
});
```

As before, this gets the `Button` we defined in the layout by its id, `button`. It then sets a listener for the click event, which triggers whenever the element is clicked. A bunch of that is boilerplate code that you don't need to worry about right now, so focus more on the fact that we are setting it so the code in the `onClick` function runs when the button is clicked. More on event-driven programming in [Event-driven Programming Overview](#10-12-events).

If this were a simple Java program, we would probably use `System.out.println("hello");` to confirm that the `onClick` function was actually running, and then check the console to see if it actually appears. Unfortunately, since this is an Android application and the console tends to have a ton of informative but irrelevant output, we need something easier to spot. A typical replacement for sysout in Android is to use a `Toast`, which is just a little textual popup.

You can make a `Toast` by calling the `makeText` method, which takes a `Context`, `CharSequence` (or the id of a resource string, see [Resources Overview](#10-12-resources)), and an `int`. For now, just call `getApplicationContext()` for the first argument; you'll get to `Context`s at a later time. A `CharSequence` is a more generic version of a `String`, so we can just put in a String literal for the second argument. The last argument is, importantly, *not whatever time you want*! It *must* be either `Toast.LENGTH_LONG` or `Toast.LENGTH_SHORT`! Do not make the same mistake I have made countless times.

Also take note of the `show()` method. If you do not call that method *nothing will appear*. Thankfully, Android Studio will complain to you if it's missing, but it's another extremely common mistake.

That's it for the meat of the training session. Everything else is abstract concepts that will certainly be helpful to know but aren't necessary to get the app running.

<a name="10-12-resources">
#### Resources Overview
</a>

Similar to how the visual layout of our Android app should be separate from the logic, certain values/constants should also be defined as resources rather than in the code. The main resource types are:

* layouts
* drawables/mipmaps (ie. images)
* colors
* dimensions
* strings
* styles

Dimension and string resources exist for a simple reason: it is very easy to lose track of your constants if you have them defined in every individual class/layout. For example, say you have a version string, `1.0`, that you use in a couple places. Perhaps you need it for the splash screen, the about screen, and it's sent in bug reports. However, the bug report feature is not really updated all that much since it works and doesn't need anything new. If you make some changes and update the version string to `1.1`, without a string resource you need to manually find every time you use the version string and update it yourself. In a large application, you might forget where everything is, especially if there are other variables that have similar purposes or names. A quick solution is to create a class that holds all your constants, but instead of doing that we just use another XML file. Here's an example `strings.xml` file:

```xml
<resources>
    <string name="app_name">AndroidTraining</string>
    <string name="button_txt">Click me!</string>
    <string name="toast_txt">Hello</string>
</resources>
```

Now that we have these string resources, if we want to change the text in our button we just need to hook it up and then look in the `strings.xml` file. To set up our button to use string resources we make the following change:

```xml
<Button
    android:text="@string/button_txt"
    android:layout_width="wrap_content"
    android:layout_height="wrap_content"
    android:layout_alignParentLeft="true"
    android:layout_alignParentStart="true"
    android:id="@+id/button" />
```

Note how the `android:text` attribute's value now starts with `@string/` and ends with the name of the string resource. Without the `@string/` part, it interprets the text as how you literally want it to appear without looking up any variables. If we want to change the `Toast` text, we make this change:

```java
btn.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        Toast.makeText(getApplicationContext(), R.string.toast_txt, Toast.LENGTH_LONG).show();
    }
});
```

Again, note the change of the second argument from `hello` to `R.string.toast_txt`. Just like when we were referencing the `Button` by its id in `findViewById` above, we use `R.string` and then the name of the string resource.

It may not be obvious, but these values in `R.string` or `R.id` or anything else you use off of `R` are `int`s. This is related to the internals of Android and is a bit too low level to really worry about, but it's worth pointing out that they are `int`s and not some special `String` or `View` class.

You'll use resources more in the future, but the two most commonly used resources are layouts and strings, so make sure you understand how to create and reference them.

<a name="10-12-events">
#### Event-driven Programming Overview
</a>

The basic idea of event-driven programming is that when certain events occur you want something to happen. Those events in Android are usually related to when something is clicked or some other user input, but there are many other cases where you want to pay attention to non-graphical events.

The Android terminology for something that listens for events is a `Listener`. In the examples above, we implemented a `OnClickListener`, which unsurprisingly listens for clicks. The first and only parameter for a `Button`'s `setOnClickListener` method is a `View.OnClickListener`. The details of this are covered in [Polymorphism](#10-12-poly), but the important part is the `public void onClick(View view)` method. This is how we signify that we want some code to run in response to the click event. When the `Button` is clicked, that method is run, and we can have it do anything from showing a `Toast` to running further complex code.

That's the gist of event-driven programming. It's best taught by example, so it'll be easier to understand once we use it more later on.

<a name="10-12-poly">
#### Polymorphism
</a>

Polymorphism is a fancy word for a really powerful but potentially confusing concept. Java's object-oriented programming design lends itself well to polymorphism, which is likely one of the main reasons why it was chosen to run Android.

There are two types of polymorphism: sub-type and parametric/generic. Generic polymorphism will be used later, but sub-type polymorphism is used right off the bat in Android. The idea is that a basic or "parent" class definition has sub- or child-types that extend the parent's functionality. Again, this is best described using examples:

```java
public class Animal {
    String color;

    public String getColor() {
        return color;
    }

    public void setColor(String color) {
        this.color = color;
    }

    public void makeSound() {
        System.out.println("rawr");
    }
}
```

We have defined an `Animal` as a thing that has a color, which is a `String`, can tell your its color (`getColor`), can change its color (`setColor`), and can output its sound to the console (`makeSound`). Now, let's define a sub-type of `Animal`:

```java
public class Dog extends Animal {
    String name;

    @Override
    public void makeSound() {
        System.out.println("woof");
    }
}
```

Now we have a `Dog`, which is a type of `Animal`. This is denoted by the `extends` keyword. If we create a `Dog` and use its `color` field, which is inheritted from its parent `Animal`, it works the same as its `name` field:


```java
Dog rover = new Dog();
rover.color = "brown"; // could also be rover.setColor("brown");
System.out.println(rover.color); // => "brown"

rover.name = "Rover";
System.out.println(rover.name); // => "Rover"
```

As for the `makeSound` method, since `Dog` overrides the definition in `Animal` the functionality is completely replaced:

```java
rover.makeSound(); // => "woof"
// this does not print "rawr"!
```

Note that the `@Override` attribute is not strictly necessary. It's simply there to remind us and anyone else that wants to read this code that the `makeSound` method does in fact override the version defined in `Animal`. On the other hand, if we create another type of `Animal` that **does** make a "rawr" sound:

```java
public class Lion {
    // ...Fields

    // ...Methods
}
```

If we call the `makeSound` method on a `Lion`, it will use the version it inheritted from `Animal`:

```java
Lion tiger = new Lion();
lion.makeSound(); // => "rawr"
```

Polymorphism is used a ton in Android, and sometimes it gets very complicated. We'll get into that later, but for now a simple example is looking at how the `findViewById` method works. This method actually returns a `View`, but in the examples above we needed to get a `Button`. The reason this works is because `Button` is a sub-type of `View` (more generally, every visual widget extends `View`). By adding a cast, `(Button)`, we're telling the compiler that we know `findViewById` doesn't actually return a `Button` but to instead trust us when we say it's a `Button`.

A slightly more complex example is the use of `View.OnClickListener`. Here's the snippet where we give the button an on click listener:

```java
button.setOnClickListener(new View.OnClickListener() {
    @Override
    public void onClick(View view) {
        // ...
    }
});
```

We create a new instance of the `View.OnClickListener` class, and then follow it with curly brackets (`{}`) and a method. This is called an anonymous class in Java, and it's equivalent to the following code:

```java
//in ButtonToastClickListener.java

// ...imports

public class ButtonToastClickListener implements View.OnClickListener {
    @Override
    public void onClick(View view) {
        // ...
    }
}

// in MainActivity.java, after finding the button
button.setOnClickListener(new ButtonToastClickListener());
```

There is one new thing here: the `implements` keyword. In Java, in addition to classes you can make interfaces. They are exactly the same except none of the methods are defined (ie. `public void onClick(View view);`). Otherwise, these two code snippets have the same effect.

## Conclusion

If any of this was confusing, please do not hesitate to contact Larry or Mattori. These are some rather big concepts, and you're likely to use them even if you don't end up programming in Android. Each section would probably take a couple full days in an actual class with much simpler Java.


# AppDev Android Training - 10/12/16
Have any questions? Feel free to ask [Larry Asante](mailto:boatenga17@grinnell.edu) or [Mattori Birnbaum](mailto:birnbaum@grinnell.edu), via email or in person.

## TOC
* [Android Studio](#android-studio)
* [Github](#github)
* [Emulator/Genymotion (optional)](#emu)
* [The HelloWorld App](#helloworld)
    - [Classes vs Layouts](#classes-layouts)
    - [Resources Overview](#resources)
    - [Event-driven Programming Overview](#events)
    - [Polymorphism](#poly)

<a name="android-studio">
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

<a name="github">
### Github
</a>

You should go to [Github](https://github.com) and create an account if you don't already have one. You probably won't need it for a bit for this class, but if you plan on doing any programming in the real world you will need a Github account, and you will definitely need one for AppDev. If you're not comfortable with git yet, you can review with [Github's git tutorial](https://try.github.io/).

We'll go through the functionality of Github when it's necessary, although you can feel free to look around or even take a look at [AppDev's Github account](https://github.com/GrinnellAppDev/).

<a name="emu">
### Emulator/Genymotion (optional)
</a>

If you don't have an Android device or want to be able to test purely on your computer, it is advisable to use [Genymotion](https://genymotion.com) instead of the default Android emulator as it's significantly faster and a bit nicer to use. For some reason, Genymotion tries to imply that the only option is to pay for use of its emulator, but if you go to the ["Fun Zone"](https://www.genymotion.com/fun-zone/) you can download the personal edition. You need to create an account, and the emulator technically does not have all the features unlocked, but it's still very effective for development.

There is a convenient [Genymotion plugin](https://www.genymotion.com/plugins/) for Android Studio, which allows you to start and manage our emulator devices.

<a name="helloworld">
### HelloWorld App
</a>

<a name="classes-layouts">
#### Classes vs Layouts
</a>

An important concept in Android, which also happens to transfer over to iOS and indeed many other GUI-related frameworks, is the separation of application logic and views. The idea is that visuals can be better represented in a more specialized language, which in the case of Android is [XML with a variety of widgets already defined](https://developer.android.com/guide/topics/ui/declaring-layout.html) (and more can be added easily by developers), while more complicated tasks like reacting to buttons and doing network communication can be handled in code, which in the case of Android is Java. These are not 100% strict rules as you can dynamically create visual widgets in code (for example, if you wanted to create an image when a button is pressed) and can set up some larger components, like scrollable lists, in XML, but generally you want to keep things separated for clarity.

A good general rule, both in Android and in any at least moderately sized project, is to keep things clear, even if that means you have to write a bit more. For example, in Java the `@Override` annotation, used in polymorphism, is not actually required, but it's helpful to clarify to yourself and anyone else that wants to read your code that that method is overriding one of the parent type's methods (if this is confusing, look at the [Polymorphism](#poly) section below).

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
* There needs to be a cast `(TextView)` to clarify what type of `View` we want (more on that in [Polymorphism](#poly))
* The `findViewById` method is how we get anything from a layout
* The `id` attribute of the `TextView` we created is used in Java through `R.id`

The `R` class is a special class generated by the Android compiler that you should *never* try to mess with or risk breaking major parts of your project. It is a representation of any resource in your project that you give an id to. More on this in the next section, [Resources Overview](#resources).

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

As before, this gets the `Button` we defined in the layout by its id, `button`. It then sets a listener for the click event, which triggers whenever the element is clicked. A bunch of that is boilerplate code that you don't need to worry about right now, so focus more on the fact that we are setting it so the code in the `onClick` function runs when the button is clicked. More on event-driven programming in [Event-driven Programming Overview](#events).

If this were a simple Java program, we would probably use `System.out.println("hello");` to confirm that the `onClick` function was actually running, and then check the console to see if it actually appears. Unfortunately, since this is an Android application and the console tends to have a ton of informative but irrelevant output, we need something easier to spot. A typical replacement for sysout in Android is to use a `Toast`, which is just a little textual popup.

You can make a `Toast` by calling the `makeText` method, which takes a `Context`, `CharSequence` (or the id of a resource string, see [Resources Overview](#resources)), and an `int`. For now, just call `getApplicationContext()` for the first argument; you'll get to `Context`s at a later time. A `CharSequence` is a more generic version of a `String`, so we can just put in a String literal for the second argument. The last argument is, importantly, *not whatever time you want*! It *must* be either `Toast.LENGTH_LONG` or `Toast.LENGTH_SHORT`! Do not make the same mistake I have made countless times.

Also take note of the `show()` method. If you do not call that method *nothing will appear*. Thankfully, Android Studio will complain to you if it's missing, but it's another extremely common mistake.

That's it for the meat of the training session. Everything else is abstract concepts that will certainly be helpful to know but aren't necessary to get the app running.

<a name="resources">
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

<a name="events">
#### Event-driven Programming Overview
</a>

The basic idea of event-driven programming is that when certain events occur you want something to happen. Those events in Android are usually related to when something is clicked or some other user input, but there are many other cases where you want to pay attention to non-graphical events.

The Android terminology for something that listens for events is a `Listener`. In the examples above, we implemented a `OnClickListener`, which unsurprisingly listens for clicks. The first and only parameter for a `Button`'s `setOnClickListener` method is a `View.OnClickListener`. The details of this are covered in [Polymorphism](#poly), but the important part is the `public void onClick(View view)` method. This is how we signify that we want some code to run in response to the click event. When the `Button` is clicked, that method is run, and we can have it do anything from showing a `Toast` to running further complex code.

That's the gist of event-driven programming. It's best taught by example, so it'll be easier to understand once we use it more later on.

<a name="poly">
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


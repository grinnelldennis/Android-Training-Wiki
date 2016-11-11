# AppDev Android Training - 11/2/16
Have any questions? Feel free to ask [Larry Asante](mailto:boatenga17@grinnell.edu) or [Mattori Birnbaum](mailto:birnbaum@grinnell.edu), via email or in person.

## TOC
* [Prerequisites](#prereqs)
* [Intents](#intents)
* [Sending Data](#sending)

<a name="prereqs">
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

<a name="intents">
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

<a name="sending">
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

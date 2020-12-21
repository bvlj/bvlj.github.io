---
layout: post
title: "Recording awesomeness"
date: 2017-11-01
---

![header]({{ site.baseurl }}/assets/images/posts/2017-11-01/header.png)

In this story I’m going to analyze the new LineageOS’ Recorder app.
This was the first app of the Lineage era: its developement started a couple
of days before everyone went mad for picking a suitable name for
the CyanogenMod successor.

It was initially developed to replace the old AOSP SoundRecorder that was still
rocking a Gingerbread User Interface and integrate with it a Screen Recorder
to reduce the amount of "extra applications" shipped within the os.

The most common usecase for sound recording is saving lectures, while screen
recording turned out useful for bug reporting or helping people doing stuffs
with their devices by showing them what to do.

## Exit poop...

The first version of the app, according to the
[Summer Survey](https://lineageos.org/Summer-Survey/), wasn’t
really appreciated, mostly because of the main color (brown) which is often
associated with 💩 `¯\_(ツ)_/¯` , also users complained about an
higher number of steps required to manage screen records compared to sounds.

## ...Enter ease

Here comes the needed redesign. The goals were:

**Easiness**

> Recording is about your content, the process of acquiring data
> shouldn’t bother you

**Delight**

> You’re about to archive something you’ll likely find useful (and / or
> interesting): having a negative imprinting with it is not a good idea

This app comes with a small set of features by design: for example there’s no
"list of all the recordings", you can manage your files from other built-in
apps such as Music and Gallery.

You don’t have to keep the app open in order to record: you will still be able
to take notes while the sound recorder works in the background.

The screen record can be started whenever you want: when the screen record
button is pressed a floating bubble appears that allows you to navigate where
you want the record to beging from.

## User interface

The app has only one simple 50%:50% screen that lets you access
everything within one tap:

![main ui]({{ site.baseurl }}/assets/images/posts/2017-11-01/screen.png)

The top contains the Screen section: one FAB to start the record, a button
for showing the latest record and another one for screen recorder settings
(toggling audio from microphone).

The bottom part is similar the top one: at the center there’s the FAB for
recording sound and at the bottom there’s the button that shows you the
latest sound record.

When you’re recording, the "other section" disappears and the FAB allows
you to terminate the record process.

Clicking the "last item" button will show a dialog with information
about the previously created record and the ability of previewing, sharing
or deleting it.

## OnBoarding and feature discovery

<video controls src="{{ site.baseurl }}/assets/video/posts/2017-11-01/onboarding.mp4" type="video/mp4" width="320" height="560"></video>

A such simple app needs no tutorial to be used, but those small icons
at the bottom of each section may go unnoticed: to prevent this the app
has a subtle _-yet delighting-_ onboarding experience:

When the “last” button is first shown, it animates with a ripple effect
that encourages the user to click on it.

The settings button instead rotates to make the user curious about it
so it’ll be clicked.

## Br0tips: animations with ConstraintLayouts

<video controls src="{{ site.baseurl }}/assets/video/posts/2017-11-01/last.mp4" type="video/mp4" width="320" height="560"></video>

Do you wonder how many lines of code you need to achieve this animation?
It looks really complex: one layout expands vertically while the other is
shrinked, fabs are scaled down and so on.

Recorder is part of LineageOS, so it’s fully [openSource](https://github/LineageOS),
you could explore the magic behind it on your own, but I’ll explain
it here regardeless.

This animation only requires **4 lines** of java code. How comes?

The secret ingredient is ConstraintLayouts:

```java
/*
 * This has been stripped to show only the animation-related code
 */
private void refresh() {
    ConstraintSet set = new ConstraintSet();
     if (Utils.isRecording(this)) {
        boolean screenRec = Utils.isScreenRecording(this);
        // ...
        if (screenRec) {
            // ...
            set.clone(this, R.layout.constraint_screen);
        } else {
            // ...
            set.clone(this, R.layout.constraint_sound);
        }
    } else {
        // ...
        set.clone(this, R.layout.constraint_default);
    }
    // ...
    TransitionManager.beginDelayedTransition(mConstraintRoot);
    set.applyTo(mConstraintRoot);
}
```

Taken from the `refresh()` method in the [RecorderActivity](https://github.com/LineageOS/android_packages_apps_Recorder/blob/cm-14.1/app/src/main/java/org/lineageos/recorder/RecorderActivity.java) class.

The `mConstraintRoot` layout has [3 child views](https://github.com/LineageOS/android_packages_apps_Recorder/blob/cm-14.1/app/src/main/res/layout/activty_constraint.xml)
that have their constraint-related attributes updated using `ConstraintSet`.
The `ConstraintSet.clone()` method reads an [xml layout](https://github.com/LineageOS/android_packages_apps_Recorder/blob/cm-14.1/app/src/main/res/layout/constraint_sound.xml)
and updates the “constraint attributes” of matching views (by id) while the
`TransitionManager` takes care of animating them.

Let’s look how layouts are defined: in this case, the “Is recording”
layout is initialized as _hidden_ in the [main layout](https://github.com/LineageOS/android_packages_apps_Recorder/blob/cm-14.1/app/src/main/res/layout/activity_constraint.xml):

```xml
<android.support.constraint.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/main_root"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:background="@color/sound">

    <!-- Is Recording -->
    <RelativeLayout
        android:id="@+id/main_recording"
        android:layout_width="0dp"
        android:layout_height="0dp"
        android:gravity="center"
        app:layout_constraintHeight_default="percent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toBottomOf="@id/main_screen_layout"
        app:layout_constraintHeight_percent="0.5">

         <TextView
            android:id="@+id/main_recording_text"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:maxLines="2"
            ...
```

And then it’s shown while recording (this snippet is from sound
recording constraint set) by editing the View’s constraints:

```xml
<android.support.constraint.ConstraintLayout
    xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    android:id="@+id/main_root"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <!-- Is Recording -->
    <View
        android:id="@+id/main_recording"
        android:layout_width="0dp"
        android:layout_height="0dp"
        app:layout_constraintHeight_default="percent"
        app:layout_constraintLeft_toLeftOf="parent"
        app:layout_constraintRight_toRightOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toTopOf="@id/main_sound_layout"
        app:layout_constraintHeight_percent="0.5"/>
```

## `finish();`

This is another step forward in the goal of bringing a good User
Experience to the open source apps in custom android aftermarket firmwares
such as [LineageOS](https://lineageos.org).

The redesign commits are available in our gerrit, and we’re open
to contributions from anyone!

You’ll be able to enjoy this update in both Lineage 14.1 and the upcoming
15.0 release.

Happy flashing!

[Originally posted on Medium](https://medium.com/@jrizzoli/recording-awesomeness-bc1c5ffe2826)

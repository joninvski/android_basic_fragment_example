Android Basic Fragment Example
==============================

Simple example of an activity using two fragments.

If there's space the screen is split in half and the two fragments are shown simultaneously

![two fragment screenshot](https://github.com/joninvski/android_basic_fragment_example/raw/master/images/app.png)


Compile
-------

    # Optional
    ANDROID_HOME=/home/.../android/sdk; export ANDROID_HOME
    ./gradlew compileDebug

Test
----

    # Make sure emulator is running or connected to real device
    ./gradlew connectedInstrumentTest


Code highlights
---------------

Layouts are defined as follows:

    * In small screens res/layout/main\_activity.xml

        <?xml version="1.0" encoding="utf-8"?>
        <FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
            android:id="@+id/fragment_container"
            android:layout_width="match_parent"
            android:layout_height="match_parent" />


    * In large screen res/layout-large/main\_activity.xml

        <?xml version="1.0" encoding="utf-8"?>
        <LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
            android:id="@+id/frags"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:orientation="horizontal"
            android:baselineAligned="false">

            <fragment
                android:id="@+id/friends_frag"
                android:layout_width="0dp"
                android:layout_height="match_parent"
                android:layout_weight="1"
                class="course.labs.fragmentslab.FriendsFragment" />

            <fragment
                android:id="@+id/feed_frag"
                android:layout_width="0dp"
                android:layout_height="match_parent"
                android:layout_weight="3"
                class="course.labs.fragmentslab.FeedFragment" />

        </LinearLayout>



On the activity's onCreate add the fragment to the Fragment Manager if single pane mode.

        if (!isInTwoPaneMode())
            ...

            // TODO 1 - add the FriendsFragment to the fragment_container
            FragmentManager fragmentManager = getFragmentManager();
            FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();
            fragmentTransaction.add(R.id.fragment_container, mFriendsFragment);
            fragmentTransaction.commit();

        else
            // Otherwise, save a reference to the FeedFragment for later use
            mFeedFragment = (FeedFragment) getFragmentManager().findFragmentById(R.id.feed_frag);



 On the activity's onItemSelected:

    onItemSelected(int position) /* Clicked one of the twitter accounts */
        ...

        // If there is no FeedFragment instance, then create one
        if (mFeedFragment == null)
            mFeedFragment = new FeedFragment();

        if (!isInTwoPaneMode()) {

            // TODO 2 - replace the fragment_container with the FeedFragment
            FragmentManager fragmentManager = getFragmentManager();
            FragmentTransaction fragmentTransaction = fragmentManager.beginTransaction();
            fragmentTransaction.replace(R.id.fragment_container, mFeedFragment);
            fragmentTransaction.addToBackStack(null); /* To put the fragment on the "back" stack */
            fragmentTransaction.commit();

            // execute transaction now
            getFragmentManager().executePendingTransactions();
        }

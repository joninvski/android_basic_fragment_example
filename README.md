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

On the activity's onCreate


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



 On the activity's onItemSelected


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



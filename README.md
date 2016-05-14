# My Third View Pager
Skeleton app using: TabLayout, ViewPager and 3 fragments: "My Pictures", "All Pictures" and "Gallery"

1. Can be used as a skeleton app putting something useful inside the fragments
2. Can be used to understand how to work with a TabLayout or a PagerView or with multiple fragments

Features:

![mythirdviewpager](https://cloud.githubusercontent.com/assets/8282404/15268422/4e47526a-19de-11e6-8526-207e9d8e03e2.JPG)



## TabLayout
To create this:
![TabLayout](https://cloud.githubusercontent.com/assets/8282404/15269708/338e56e2-1a08-11e6-99fa-16095103c9fa.JPG)
place the following xml inside the CoordinatorLayout of activity_main.xml
```xml
        <android.support.design.widget.TabLayout
            android:background="@color/colorPrimary"
            android:layout_alignParentTop="true"
            android:id="@+id/tab_layout"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            app:tabSelectedTextColor="#ffffff"
            >

            <android.support.design.widget.TabItem
                android:id="@+id/tabItem1"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:icon="@xml/mypics_image"
                android:text="My Pictures"/>
            <android.support.design.widget.TabItem
                android:id="@+id/tabItem2"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="All Pictures"/>
            <android.support.design.widget.TabItem
                android:id="@+id/tabItem3"
                android:layout_width="wrap_content"
                android:layout_height="wrap_content"
                android:text="Gallery"/>

        </android.support.design.widget.TabLayout>
```
As you can see, each item is created using android.support.design.widget.TabItem
You can do this in java code, as well. In my case, I like this solution because I can see the TabLayout growing while modifying activity_main.xml 


## Dependencies
The MainActivity extends AppCompatActivity, therefore you must include 
```xml
    compile 'com.android.support:appcompat-v7:23.3.0'
```
in the dependencies section of build.gradle of the Module: app
  
TabLayout, TabItem and ViewPager are components of com.android.support, therefore you must include 
```xml
dependencies {
...
    compile 'com.android.support:appcompat-v7:23.3.0'
    compile 'com.android.support:design:23.3.0'
    compile 'com.android.support:support-v4:23.3.0'
...
}

```



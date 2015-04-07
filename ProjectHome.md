
# Kids Place API Usage Example #

This project includes sample code to demonstrate how to use **Kids Place** API. This can assist android developers who develop Apps for Children with following features:
  1. **Locking of home button**
  1. **Android Parental Controls Settings**
  1. **Central authentication mechanism** (to make sure only parents can access the area of app like app settings; in-app purchases and upgrades).
  1. **Add/Remove approved apps from Kids Place.**

<a href='https://play.google.com/store/apps/details?id=com.kiddoware.kidsplace'>Kids Place - With Child Lock</a>, by <a href='http://www.kiddoware.com'>kiddoware</a>, is a free Android app that provides parental controls with child lock to childproof android devices. Kiddoware provides a simple to use integration code that android developers can use in their project.

Kids Place SDK Can be downloaded from here:
<a href='https://code.google.com/p/kidsplace-api-samples/downloads/detail?name=kidsplace_sdk_1.0.jar&can=2&q='>Download Kids Place SDK Jar File </a>

SDK Javadocs can be browsed from here: <a href='http://kiddoware.com/doc/com/kiddoware/kidsplace/sdk/KPUtility.html'>Browse Kids Place SDK Javadoc</a>

For quick and basic implementation refer to source code of
[simple](http://code.google.com/p/kidsplace-api-samples/source/browse/#svn%2Ftrunk%2Fsamples%2Fsimple) project and for more advanced integration refer to [advanced](http://code.google.com/p/kidsplace-api-samples/source/browse/#svn%2Ftrunk%2Fsamples%2Fadvanced) project.

Please open defects/enhancements requests in the [issue tracker](http://code.google.com/p/kidsplace-api-samples/issues/list).

Some of the apps on Google Play that uses Kids Place SDK:
  * [Kids Video Player](https://play.google.com/store/apps/details?id=com.kiddoware.kidsvideoplayer)
  * [Letters With Ally](https://play.google.com/store/apps/details?id=com.kiddoware.letters)

# Basic Setup #
Step by step installation of the Kids Place library in an existing application project, with the simplest settings:
  * Download the Kids Place library http://kidsplace-api-samples.googlecode.com/files/kidsplace_sdk_1.0.jar
  * Open your Eclipse project
  * Create a libs folder in your project
  * Add the kidsplace\_sdk\_1.0.jar from your download directory in the libs folder
  * Right-click on the jar file > build path > add to build path
  * Add following code in your activity from where you want to invoke Kids Place
    1. Import Kids Place library
```
import com.kiddoware.kidsplace.sdk.KPUtility;
```
    1. Add following code in onCreate Method of your activity
```
KPUtility.handleKPIntegration(this, KPUtility.GOOGLE_MARKET);
```

  * For sample source code for simple integration please refer [simple](http://code.google.com/p/kidsplace-api-samples/source/browse/#svn%2Ftrunk%2Fsamples%2Fsimple) project code.
  * For advanced integration please see [Advanced Usage Guide](http://code.google.com/p/kidsplace-api-samples/wiki/AdvancedUsage).
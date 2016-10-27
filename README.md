# iOS Universal Links Troubleshooting Guide

Deep linking is all about connecting users to content within your app, and links that don't open your app are a huge barrier to this flow. With iOS 9.0, Apple introduced Universal Links and deprecated URI schemes. While many iOS applications such as Google Chrome still rely on URI Schemes for opening apps, Apple's apps now use Universal Links.

The easiest way to check if your Branch links are functioning properly as Universal Links is with the iOS Notes app:

1. Paste one of your Branch links into a new note
2. Perform a long press on the link (note: if you see a "preview" window pop open, you are pressing too hard)


After the long press, you will see an options menu slide up from the bottom of the screen. If you see an option to ‘Open in «App»’ then your Universal Links are configured properly.


If you don't see an option to open the link in your app, there is an issue with your Universal Links configuration.




What if I *do* see 'Open in <<App>>' but the links still don't open the app?


Verify that the use case being tested is one supported by Universal Linking


If you do see the 'Open in «App»' option but are concerned because your links are not behaving as expected, it may be due to limitations in how Universal Links function.


Links pasted into the Safari address bar will not function as Universal Links
Apple requires that Universal Links be tapped on directly by the end user. This means:
Redirecting to a Branch link will prevent it from functioning as a Universal Link
Universal Links cannot be triggered programmatically
Wrapping a Branch link for click tracking or any other purpose will prevent it from functioning as a Universal Link




Verify that Universal Linking for the app has not been disabled on the device


When an app is opened via a Universal Link, tapping on the "app.link" button in the upper right-hand corner of the screen, as shown in the screenshot below, will disable Universal Links for the app.

To re-enable Universal Links for an app, tap on a link from Notes and select the "Open in..." option, as shown in the below screenshot.

What to check if Universal Linking is not working


When testing, be sure to uninstall/reinstall the app after each change


iOS does not re-scrape the app's apple-app-site-association file until an app is deleted and reinstalled on the device. As a general rule it is best to uninstall/reinstall the app whenever making a change that may impact Universal Linking. 




Verify that the app is listed in the Apple App Site Association file (AASA file)


For Branch links to work as Universal Links, the app's Apple App Prefix and Bundle Identifier must be listed in an AASA file and that file must have been scraped by Apple. Branch automatically takes care of these details for you, but it can take up to thirty minutes for the process to complete.


Use Branch's Universal Link Validator (here: http://branch.io/resources/universal-links/) to verify your app's AASA file entries are correct. If there is a problem, review the rest of the troubleshooting steps in this document.


Important Note: it can take up to 30 minutes for changes that impact Universal LInking to be recognized by Apple.




Verify that the entitlements file is included in your build target


It seems that Xcode, by default, will not include the .entitlements file in your build. You have to check the box in the right sidebar against the correct target to ensure it’s included in your app.

Verify that the Branch dashboard's Apple App Prefix and Bundle Identifier settings are correct


Ensure that the app's settings in for Apple App Prefix and Bundle Identifier (here: https://dashboard.branch.io/settings/link) match the app's settings on the Apple developer dashboard (here: https://developer.apple.com/membercenter/index.action#accountSummary) as shown in the screenshot below.

Verify that the associated-domains section of the .entitlements file contains correct information


Find the Branch link domain for the app on the bottom of the Link Settings tab of the dashboard, as shown in the screenshot below.

Confirm that the .entitlements file contains the appropriate "applinks" entries, as shown in the screenshot below.

Is the app using a bnc.lt link domain in conjunction with a Test key?


Universal Links will not work on Test apps using the bnc.lt domain. Please test Universal Links using your Live app. For more information on this, see: http://status.branch.io/incidents/b0c19p6hpq58




If you are using a custom link domain, verify that it is configured correctly


Use Branch's Universal Link Validator to confirm that the app's custom link domain is configured properly and has the apprpriate AASA file entries. 


If the Universal Link Validator reports issues with the domain, review the process for setting a custom link domain, here: https://dev.branch.io/getting-started/link-domain-subdomain/guide/#setting-a-custom-link-domain


The following error message will appear in your OS-level logs if your domain doesn’t have SSL set up properly:


Sep 21 14:27:01 Derricks-iPhone swcd[2044] <Notice>: 2015-09-21 02:27:01.878907 PM [SWC] ### Rejecting URL 'https://examplecustomdomain.com/apple-app-site-association' for auth method 'NSURLAuthenticationMethodServerTrust': -6754/0xFFFFE59E kAuthenticationErr
These logs can be found for physical devices connected to Xcode by navigating to Window > Devices > choosing your device and then clicking the “up” arrow in the bottom left corner of the main view.




If you are using the Facebook SDK, verify that it is not blocking Universal Linking


The Facebook SDK can cause application:didFinishLaunchingWithOptions: to return "NO" in some situations. When this happens, the handling of Universal links will be blocked. Please ensure that application:didFinishLaunchingWithOptions: always returns YES/true.


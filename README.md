# Admob Easy Banner Widget
This repository contains a very simple widget that makes implementing AdMob banners easy. I had struggled with this myself, and thus created a simple widget that handles states and the advertisement ids automatically. Created to help others that have also struggled with banner implementation. The widget itself is placed within banner_ad.dart. It can be downloaded, distributed, modified, or whatever you like to the code so it suits your specific needs.

## More About it
This widget was made particularly for mobile applications and ads, but can be adapted to suit desktop and/or web.

A few things this banner widget can do:
<ul>
  <li>Accepts parameters:</li>
  <ul>
    <li>androidId (String) - The Id of the banner to be shown on Android</li>
    <li>iosId (String) - The Id of the banner to be shown to iOS users</li>
    <li>isTest (Boolean) - If true, Google's default test Ids will be used. If false, the provided Ids will be used</li>
    <li>isShown (Boolean) - Shows the ad based on if the value is true or false, your program can dynamically change this</li>
    <li>bannerSize (AdSize) - Accepts different banner sizes from the AdMob SDK, from the class AdSize </li>
  </ul>
  <li>Fits wll in smaller spaces with default code</li>
  <li>Reloads the ad when a new ad request is called</li>
  <li>Accepts different sizes of ads and adapts to their size</li>
</ul>

## Setup
<strong>Step 1:</strong> First off, make sure you have included the Google Admob SDK in your Flutter project. It can be installed from <a href="https://pub.dev/packages/google_mobile_ads">pub.dev</a>

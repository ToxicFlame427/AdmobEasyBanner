# Admob Easy Banner Widget
This repository contains a very simple widget that makes implementing AdMob banners easy. I had struggled with this myself, and thus created a simple widget that handles states and the advertisement ids automatically. Created to help others that have also struggled with banner implementation. The widget itself is placed within `banner_ad.dart`. It can be downloaded, distributed, modified, or whatever you like to the code so it suits your specific needs.

## About
This widget was made particularly for mobile applications and ads, but can be adapted to suit desktop and/or web.

A few things this banner widget can do:
<ul>
  <li>Accepts parameters:</li>
  <ul>
    <li><strong>androidBannerId</strong>strong> (String) - The Id of the banner to be shown on Android</li>
    <li><strong>iosBannerId</strong> (String) - The Id of the banner to be shown to iOS users</li>
    <li><strong>isTest</strong> (Boolean) - If true, Google's default test Ids will be used. If false, the provided Ids will be used</li>
    <li><strong>isShown</strong> (Boolean) - Shows the ad based on if the value is true or false, your program can dynamically change this</li>
    <li><strong>bannerSize</strong> (AdSize) - Accepts different banner sizes from the AdMob SDK, from the class AdSize </li>
  </ul>
  <li>Fits wll in smaller spaces with default code</li>
  <li>Reloads the ad when a new ad request is called</li>
  <li>Accepts different sizes of ads and adapts to their size</li>
</ul>

## Setup
**Step 1:** Make sure you have included the Google Admob SDK in your Flutter project. It can be installed from <a href="https://pub.dev/packages/google_mobile_ads">pub.dev</a>.

**Step 2:** Follow the steps to setup your application to receive ads through the </a href="https://developers.google.com/admob/flutter/quick-start">AdMob Flutter Quick-Start</a>

**Step 3:** Download the supplied `banner_ad.dart` file and add it to your project, or copy the code below and paste it within a new file and name it whatever you like:
```
import 'dart:io';

import 'package:flutter/material.dart';
import 'package:google_mobile_ads/google_mobile_ads.dart';

class BannerAdView extends StatefulWidget {
  const BannerAdView(
      {super.key,
      required this.androidBannerId,
      required this.iOSBannerId,
      required this.isTest,
      required this.isShown,
      required this.bannerSize});

  final String androidBannerId;
  final String iOSBannerId;
  final bool isTest;
  final bool isShown;
  final AdSize bannerSize;

  @override
  State<StatefulWidget> createState() {
    return _BannerAdViewState();
  }
}

class _BannerAdViewState extends State<BannerAdView> {
  BannerAd? _bannerAd;
  bool _isLoaded = false;

  // Initially use test ad units
  String adUnitId = Platform.isAndroid
      ? 'ca-app-pub-3940256099942544/9214589741'
      : 'ca-app-pub-3940256099942544/2435281174';

  /// Loads a banner ad.
  void loadAd() async {
    _bannerAd = BannerAd(
      adUnitId: adUnitId,
      request: const AdRequest(),
      size: widget.bannerSize,
      listener: BannerAdListener(
        // Called when an ad is successfully received.
        onAdLoaded: (ad) {
          debugPrint('$ad loaded.');
          setState(() {
            _isLoaded = true;
          });
        },
        // Called when an ad request failed.
        onAdFailedToLoad: (ad, err) {
          debugPrint('BannerAd failed to load: $err');
          // Dispose the ad here to free resources.
          ad.dispose();
        },
      ),
    )..load();
  }

  @override
  Widget build(BuildContext context) {
    // Determine if the ad is a test, if not use real ids
    if (!widget.isTest) {
      if (Platform.isAndroid) {
        adUnitId = widget.androidBannerId;
      } else {
        adUnitId = widget.iOSBannerId;
      }
    }
    // If else, the test ad units still persist

    // Make sure that the ad does not keep making requests if it doesnt have to
    if (!_isLoaded) {
      loadAd();
    }

    if (_bannerAd != null && _isLoaded && widget.isShown) {
      print("Ad loaded with id $adUnitId and testing is ${widget.isTest}");

      return Align(
        alignment: Alignment.bottomCenter,
        child: SafeArea(
          child: SizedBox(
            width: _bannerAd!.size.width.toDouble(),
            height: _bannerAd!.size.height.toDouble(),
            child: AdWidget(ad: _bannerAd!),
          ),
        ),
      );
    }

    return SizedBox(
      height: 0,
      width: 0,
    );
  }
}
```

Now that the base setup is complete, here is an example use case of this widget.

## Example Implementation
When using the banner widget, it can be placed freely around your application. Here is an example of how it could be used:
```
@override
  Widget build(BuildContext context) {
    return DefaultTabController(
      length: 2,
      child: Scaffold(
          appBar: AppBar(
            toolbarHeight: 0,
            bottom: TabBar(
              controller: _tabController,
              labelColor: Colors.green,
              unselectedLabelColor: Colors.green,
              indicatorColor: Colors.green,
              indicatorSize: TabBarIndicatorSize.tab,
              tabs: const [
                Tab(
                  icon: Icon(Icons.search),
                  text: "Search",
                ),
                Tab(icon: Icon(Icons.favorite), text: "Favorites"),
              ],
            ),
          ),
          body: Column(children: [
            Flexible(
                child: TabBarView(
                    controller: _tabController,
                    children: const [PlantSearch(), FavoritePlants()])),
            // MARK: USAGE - Banner ad is shown here
            BannerAdView(
              androidBannerId: "android id",
              iOSBannerId: "iOS id",
              isTest: true,
              isShown: true,
              bannerSize: AdSize.banner,
            ),
          ])),
    );
```
A short clip that shows the appearance and reload of the advertisement:
![Demonstration of ad loading and reloading](/assets/clip.gif)

I hope that this widget helps those out that had a hard time implementing banner ads as much as I did. If you like this implementation or think that this widget/repo is useful feel free to give this repo a star!

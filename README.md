# stacked_list_carousel

**This package allows you to create a carousel of stacked items that is highly customizable and interactive. It is particularly well-suited for specific use cases, such as in-app banners.**

<img alt="Demo" src="https://raw.githubusercontent.com/hongduc6/stacked_list_carousel/master/example/asset/demo.gif" height="500">

## Features
* Support customize widget's items builder which provides variety attributes. The itemBuilder provides displayed size of built target, its index inside item list and whether built target is outermost displayed widget.

* Allow config fixed aspect ratio of display items. If cardAspectRatio is not provided, the rendered view will occupy all remain space.

    * 4 x 3
    <img alt="Demo" src="https://raw.githubusercontent.com/hongduc6/stacked_list_carousel/master/example/asset/4_3.png" height="300">

    * 16 x 9
    <img alt="Demo" src="https://raw.githubusercontent.com/hongduc6/stacked_list_carousel/master/example/asset/16_9.png" height="300">

* Provide item discard listener, which provides its information and discarded swipe direction.

## Documentation

### Installation
Add `stacked_list_carousel` to your `pubspec.yaml`:

```
dependencies:
  flutter:
    sdk: flutter

  stacked_list_carousel: <latest version>
```
### Implements

Import the package:

```
import 'package:stacked_list_carousel/stacked_list_carousel.dart';
```

Then use StackedCardCarousel widget

```
import 'package:flutter/material.dart';
import 'package:stacked_list_carousel/stacked_list_carousel.dart';

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Stacked Cards Example',
      theme: ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: const Home(title: 'Awesome Card Carousel'),
    );
  }
}

class Home extends StatefulWidget {
  const Home({Key? key, required this.title}) : super(key: key);
  final String title;

  @override
  State<Home> createState() => _HomeState();
}

class _HomeState extends State<Home> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      backgroundColor: Colors.grey,
      body: StackedListCarousel<AwesomeInAppBanner>(
        items: banners,
        // Highly customizable builder function which actual widget's size,
        // its index inside item list, and whether built item is outermost
        itemBuilder: (context, size, index, isOutermost) => ClipRRect(
          borderRadius: BorderRadius.circular(6.0),
          child: Stack(
            children: [
              Image.network(
                banners[index].imgUrl,
                width: size.width,
                height: size.height,
              ),
              Align(
                alignment: Alignment.bottomRight,
                child: Padding(
                  padding: const EdgeInsets.all(16.0),
                  child: Text(
                    banners[index].title,
                    style: TextStyle(
                      fontWeight: FontWeight.bold,
                      color: Colors.white,
                      fontSize:
                          30.0 * size.width / MediaQuery.of(context).size.width,
                    ),
                  ),
                ),
              ),
              if (!isOutermost)
                SizedBox.expand(
                  child: Container(color: Colors.grey.withOpacity(0.65)),
                )
            ],
          ),
        ),
        // Config card's aspect ratio
        cardAspectRatio: 2 / 3,
        // Config outermost card height factor relative to view height
        outermostCardHeightFactor: 0.7,
        // Config max item displayed count
        maxDisplayedItemsCount: 3,
        // Config view size height factor relative to view height
        viewSizeHeightFactor: 0.85,
        // Config animation transitions duration
        autoSlideDuration: const Duration(seconds: 4),
        transitionDuration: const Duration(milliseconds: 250),
        outermostTransitionDuration: const Duration(milliseconds: 200),
        // You can listen for discarded item and its swipe direction
        onItemDiscarded: (index, direction) {
          ScaffoldMessenger.of(context).clearSnackBars();
          ScaffoldMessenger.of(context).showSnackBar(
            SnackBar(
              content: Text(
                  'banner ${banners[index].title} discarded in $direction direction!'),
            ),
          );
        },
      ),
    );
  }
}

class AwesomeInAppBanner {
  final String imgUrl;
  final String title;
  final Color color;

  const AwesomeInAppBanner(
    this.imgUrl,
    this.title,
    this.color,
  );
}

List<AwesomeInAppBanner> banners = <AwesomeInAppBanner>[
  AwesomeInAppBanner(
    'https://picsum.photos/id/100/600/900',
    'My awesome banner 1',
    Colors.green.shade300,
  ),
  AwesomeInAppBanner(
    'https://picsum.photos/id/200/600/900',
    'My awesome banner 2',
    Colors.red.shade300,
  ),
  AwesomeInAppBanner(
    'https://picsum.photos/id/300/600/900',
    'My awesome banner 3',
    Colors.purple.shade300,
  ),
  AwesomeInAppBanner(
    'https://picsum.photos/id/400/600/900',
    'My awesome banner 4',
    Colors.yellow.shade300,
  ),
  AwesomeInAppBanner(
    'https://picsum.photos/id/500/600/900',
    'My awesome banner 5',
    Colors.blue.shade300,
  ),
  AwesomeInAppBanner(
    'https://picsum.photos/id/600/600/900',
    'My awesome banner 6',
    Colors.orange.shade300,
  ),
];
```

### Contribution

Please contact via [**Github**](https://github.com/hongduc6)

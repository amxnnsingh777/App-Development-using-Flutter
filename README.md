# THAPAR UNIVERSITY 2025 SESSION
## Hi there, let's get started!

### Sources:
1.) https://pub.dev/packages/image

<br/>

2.) https://pub.dev/packages/image_picker

<br/>

3.) https://pub.dev/packages/path_provider

<br/>

4.0.) we are NOT using colorpicker, we can though, in just the addText() function, add this: ``color: img.getColor(
  selectedColor.red,
  selectedColor.green,
  selectedColor.blue,
)``, you would also need to add a dialog to pick the color tho, way too much of mess acc. to me :/

<br/>

4.1.) https://pub.dev/packages/flutter_colorpicker

<br/>

5.) https://flutter.dev/

<br/>

6.) we will be using: https://zapp.run/

first, we will go ahead and put these in ``pubspec.yaml`` file:
```
  image: ^3.2.2
  image_picker: ^0.8.7+4
  path_provider: ^2.0.11
  flutter_colorpicker: ^1.0.3
```

Let's go ahead and create a new file called ``editor-screen.dart``:

Now, I want you to import all of that:
```
import 'dart:io';
import 'dart:typed_data';
import 'dart:ui' as ui;

import 'package:flutter/material.dart';
import 'package:image/image.dart' as img;
import 'package:path_provider/path_provider.dart';
import 'package:flutter_colorpicker/flutter_colorpicker.dart';
import 'package:image_picker/image_picker.dart';
```

let's go and create this a stateful widget instead of a stateless one,
now, let's create some variables:

```
  Uint8List? imageBytes; // this thing right here is a list of bytes (basically collections, we will store img as bytes instead of files)
  img.Image? originalImage; // this will be the original image, it could only be Image? originalImage too, but we r importing it 'as immg'
```
now we need a function to pick image from the user:
```
Future<void> pickImage() async {
    final picker = ImagePicker(); // create an instance
    final XFile? picked = await picker.pickImage(source: ImageSource.gallery); // get the image in XFile format 
    if (picked != null) {
      final bytes = await picked.readAsBytes(); // convert to bytes
      final decoded = img.decodeImage(bytes); // get decoded img
      setState(() { // re render screen to show the image
        imageBytes = bytes;
        originalImage = decoded;
      });
    }
  }
```

HELPER FUNCTIONS:
1.) Grayscale function, so the package 'image' provides a function called .grayscale and requires it in Image format

```
  void applyGrayscale() {
    if (originalImage != null) {
      final gray = img.grayscale(originalImage!);
      setState(() {
        originalImage = gray; // now set the img to gray image
        imageBytes = Uint8List.fromList(img.encodeJpg(gray));
      });
    }
  }

```

2. Rotate 90 degrees, again the package 'image' provides that function
```
void rotate90() {
    if (originalImage != null) {
      final rotated = img.copyRotate(originalImage!, 90);
      setState(() {
        originalImage = rotated;
        imageBytes = Uint8List.fromList(img.encodeJpg(rotated));
      });
    }
  }
```

3.) Adding text, we create a copy, put some text to it, and return it as original

```
Future<void> addText(String text) async {
  // Exit early if there's no image to modify
  if (originalImage == null) return;

  // Clone the original image and draw the provided text onto it
  final temp = img.copyResize(originalImage!, width: originalImage!.width);
  img.drawString(temp, img.arial_24, 20, 20, text);

  setState(() {
    originalImage = temp;
    imageBytes = Uint8List.fromList(img.encodeJpg(temp));
  });
}
```

Finally, let's design the interface:

```
@override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text("Image Editor")), // an app you can create a separate component for that, or put it here like that
      body: Center(
        child: imageBytes == null
            ? const Text("No image selected")
            : Image.memory(imageBytes!),
      ),
      floatingActionButton: Column( // instead of this, you can show btns on the screen itself
        mainAxisAlignment: MainAxisAlignment.end,
        children: [
          FloatingActionButton(
              heroTag: "pick", onPressed: pickImage, child: const Icon(Icons.add_a_photo)),
          FloatingActionButton(
              heroTag: "rotate", onPressed: rotate90, child: const Icon(Icons.rotate_right)),
          FloatingActionButton(
              heroTag: "grayscale", onPressed: applyGrayscale, child: const Icon(Icons.grain)),
          FloatingActionButton(
              heroTag: "text",
              onPressed: () async {
                await addText("CodeBeetles OP");
              },
              child: const Icon(Icons.text_fields)),
        ],
      ),
    );
  }
}
```

Additionally, we can also save the img if you want(do the hot reload thingy if you have not done once earlier, coz this will talk with the system to get directory and stuff, if you don't it will give you missing platform channel plugin exception (idr the name properly))

```Future<void> saveImage() async {
  if (imageBytes == null) return;

  final directory = await getApplicationDocumentsDirectory();
  final path = '${directory.path}/edited_image.jpg';
  final file = File(path);
  await file.writeAsBytes(imageBytes!);

  // Optional: Show confirmation (or flex with a print)
  print("Image saved at: $path");
}
```

Assignment: add a button, I know you can do it, create a Container widget, and add Inkwell on top of it.

Q/A Link:https://app.sli.do/event/vrsyu7DVzdrkcUD6BQR92Q

Thanks! :D


```
import 'package:flutter/material.dart';
class HomePage extends StatefulWidget {
  const HomePage({super.key});

  @override
  State<HomePage> createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  @override
  Widget build(BuildContext context) {
    return const Placeholder();
  }
}



```

```
import 'package:flutter/material.dart';
class HomePage extends StatefulWidget {
  const HomePage({super.key});

  @override
  State<HomePage> createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {
  @override
  Widget build(BuildContext context) {
    return  Scaffold(
      body: Column(
        children: [
          Container(
  color: Colors.black, 
  child: Text("click me", style: TextStyle(color: Colors.white)),
),
        ],
      ),
    );
  }
}



  ```



```
import 'dart:io';
import 'dart:typed_data';
import 'dart:ui' as ui;

import 'package:flutter/material.dart';
import 'package:image/image.dart' as img;
import 'package:path_provider/path_provider.dart';
import 'package:flutter_colorpicker/flutter_colorpicker.dart';
import 'package:image_picker/image_picker.dart';

class HomePage extends StatefulWidget {
  const HomePage({super.key});

  @override
  State<HomePage> createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {

    Uint8List? imageBytes; // this thing right here is a list of bytes (basically collections, we will store img as bytes instead of files)
    img.Image? originalImage; // this will be the original image, it could only be Image? originalImage too, but we r importing it 'as immg'


  

  @override
  Widget build(BuildContext context) {
    return  Scaffold(
      body: Column(
        children: [
          Container(
  color: Colors.black, 
  child: Text("click me", style: TextStyle(color: Colors.white)),
),
Container(
  color: Colors.black, 
  child: Text("click me", style: TextStyle(color: Colors.white)),
),

Container(
  color: Colors.black, 
  child: Text("click me", style: TextStyle(color: Colors.white)),
),

        ],
      ),
    );
  }
}


```

```
import 'dart:io';
import 'dart:typed_data';
import 'dart:ui' as ui;

import 'package:flutter/material.dart';
import 'package:image/image.dart' as img;
import 'package:path_provider/path_provider.dart';
import 'package:flutter_colorpicker/flutter_colorpicker.dart';
import 'package:image_picker/image_picker.dart';

class HomePage extends StatefulWidget {
  const HomePage({super.key});

  @override
  State<HomePage> createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {

    Uint8List? imageBytes; // this thing right here is a list of bytes (basically collections, we will store img as bytes instead of files)
    img.Image? originalImage; // this will be the original image, it could only be Image? originalImage too, but we r importing it 'as immg'


  Future<void> pickImage() async {
    final picker = ImagePicker(); // create an instance
    final XFile? picked = await picker.pickImage(source: ImageSource.gallery); // get the image in XFile format 
    if (picked != null) {
      final bytes = await picked.readAsBytes(); // convert to bytes
      final decoded = img.decodeImage(bytes); // get decoded img
      setState(() { // re render screen to show the image
        imageBytes = bytes;
        originalImage = decoded;
      });
    }
  }

  @override
  Widget build(BuildContext context) {
    return  Scaffold(
      body: Column(
        children: [
           Center(
        child: imageBytes == null
            ? const Text("No image selected")
            : Image.memory(imageBytes!),
      ),
          InkWell(
             onTap:  () async {
                await pickImage();
             },
            child: Container(
  color: Colors.black, 
  child: Text("click me", style: TextStyle(color: Colors.white)),
),
          ),
Container(
  color: Colors.black, 
  child: Text("click me", style: TextStyle(color: Colors.white)),
),

Container(
  color: Colors.black, 
  child: Text("click me", style: TextStyle(color: Colors.white)),
),

        ],
      ),
    );
  }
}



```

```
Center(
        child: imageBytes == null
            ? const Text("No image selected")
            : Image.memory(imageBytes!),
      ),
```


```
import 'dart:io';
import 'dart:typed_data';
import 'dart:ui' as ui;

import 'package:flutter/material.dart';
import 'package:image/image.dart' as img;
import 'package:path_provider/path_provider.dart';
import 'package:flutter_colorpicker/flutter_colorpicker.dart';
import 'package:image_picker/image_picker.dart';

class HomePage extends StatefulWidget {
  const HomePage({super.key});

  @override
  State<HomePage> createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {

    Uint8List? imageBytes; // this thing right here is a list of bytes (basically collections, we will store img as bytes instead of files)
    img.Image? originalImage; // this will be the original image, it could only be Image? originalImage too, but we r importing it 'as immg'


  Future<void> pickImage() async {
    final picker = ImagePicker(); // create an instance
    final XFile? picked = await picker.pickImage(source: ImageSource.gallery); // get the image in XFile format 
    if (picked != null) {
      final bytes = await picked.readAsBytes(); // convert to bytes
      final decoded = img.decodeImage(bytes); // get decoded img
      setState(() { // re render screen to show the image
        imageBytes = bytes;
        originalImage = decoded;
      });
    }
  }

    void applyGrayscale() {
    if (originalImage != null) {
      final gray = img.grayscale(originalImage!);
      setState(() {
        originalImage = gray; // now set the img to gray image
        imageBytes = Uint8List.fromList(img.encodeJpg(gray));
      });
    }
  }


  @override
  Widget build(BuildContext context) {
    return  Scaffold(
      body: Column(
        children: [
         Center(
        child: imageBytes == null
            ? const Text("No image selected")
            : Image.memory(imageBytes!),
      ),
          InkWell(
             onTap:  () async {
                await pickImage();
             },
            child: Container(
  color: Colors.black, 
  child: Text("click me", style: TextStyle(color: Colors.white)),
),
          ),
           InkWell(
             onTap:  () async {
                applyGrayscale();
             },
            child: Container(
  color: Colors.black, 
  child: Text("click me", style: TextStyle(color: Colors.white)),
),
          ),
          

        ],
      ),
    );
  }
}


```


```
import 'dart:io';
import 'dart:typed_data';
import 'dart:ui' as ui;

import 'package:flutter/material.dart';
import 'package:image/image.dart' as img;
import 'package:path_provider/path_provider.dart';
import 'package:flutter_colorpicker/flutter_colorpicker.dart';
import 'package:image_picker/image_picker.dart';

class HomePage extends StatefulWidget {
  const HomePage({super.key});

  @override
  State<HomePage> createState() => _HomePageState();
}

class _HomePageState extends State<HomePage> {

    Uint8List? imageBytes; // this thing right here is a list of bytes (basically collections, we will store img as bytes instead of files)
    img.Image? originalImage; // this will be the original image, it could only be Image? originalImage too, but we r importing it 'as immg'


  Future<void> pickImage() async {
    final picker = ImagePicker(); // create an instance
    final XFile? picked = await picker.pickImage(source: ImageSource.gallery); // get the image in XFile format 
    if (picked != null) {
      final bytes = await picked.readAsBytes(); // convert to bytes
      final decoded = img.decodeImage(bytes); // get decoded img
      setState(() { // re render screen to show the image
        imageBytes = bytes;
        originalImage = decoded;
      });
    }
  }

    void applyGrayscale() {
    if (originalImage != null) {
      final gray = img.grayscale(originalImage!);
      setState(() {
        originalImage = gray; // now set the img to gray image
        imageBytes = Uint8List.fromList(img.encodeJpg(gray));
      });
    }
  }

  void rotate90() {
    if (originalImage != null) {
      final rotated = img.copyRotate(originalImage!, 90);
      setState(() {
        originalImage = rotated;
        imageBytes = Uint8List.fromList(img.encodeJpg(rotated));
      });
    }
  }


  @override
  Widget build(BuildContext context) {
    return  Scaffold(
      body: SingleChildScrollView(
        child: Column(
          children: [
           Center(
          child: imageBytes == null
              ? const Text("No image selected")
              : Image.memory(imageBytes!),
        ),
            InkWell(
               onTap:  () async {
                  await pickImage();
               },
              child: Container(
  color: Colors.black, 
  child: Text("select image", style: TextStyle(color: Colors.white)),
),
            ),
             InkWell(
               onTap:  () async {
                  applyGrayscale();
               },
              child: Container(
  color: Colors.black, 
  child: Text("grayscale", style: TextStyle(color: Colors.white)),
),
            ),
            
             InkWell(
               onTap:  () async {
                  rotate90();
               },
              child: Container(
  color: Colors.black, 
  child: Text("rotate", style: TextStyle(color: Colors.white)),
),
            ),
            
            

          ],
        ),
      ),
    );
  }
}

```
https://app.sli.do/event/vrsyu7DVzdrkcUD6BQR92Q

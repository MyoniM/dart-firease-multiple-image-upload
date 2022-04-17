# dart-firease-multiple-image-upload
```
import 'dart:io';

import 'package:firebase_storage/firebase_storage.dart';
import 'package:image_picker/image_picker.dart';

// ! upload multiple files
Future<List<String>> uploadFiles(String destination, List<XFile> images) async {
  var imageUrls = await Future.wait(images.map(
      (image) => uploadImage("$destination/${image.name}", File(image.path))));
  return imageUrls;
}

Future<String> uploadImage(String destination, File image) async {
  try {
    final storageRef = FirebaseStorage.instance.ref(destination);
    // ! start upload task
    // ! wait till it finishes and get the download url back
    UploadTask uploadTask = storageRef.putFile(image);
    final snapshot = await uploadTask.whenComplete(() {});
    final downloadUrl = await snapshot.ref.getDownloadURL();

    return downloadUrl;
  } catch (_) {
    throw Exception("Something went wrong!");
  }
}
```


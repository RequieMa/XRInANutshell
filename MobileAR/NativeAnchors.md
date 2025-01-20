Got it! Here are simple, complete demos for ARKit in Swift and ARCore in C++ using the NDK.

### ARKit in Swift

#### ARAnchor Demo
```swift
import UIKit
import ARKit

class ViewController: UIViewController, ARSCNViewDelegate {
    @IBOutlet var sceneView: ARSCNView!

    override func viewDidLoad() {
        super.viewDidLoad()
        sceneView.delegate = self
        sceneView.session.run(ARWorldTrackingConfiguration())
    }

    override func viewWillDisappear(_ animated: Bool) {
        super.viewWillDisappear(animated)
        sceneView.session.pause()
    }

    func addAnchor() {
        let anchor = ARAnchor(transform: matrix_identity_float4x4)
        sceneView.session.add(anchor: anchor)
    }
}
```

#### ARPlaneAnchor Demo
```swift
import UIKit
import ARKit

class ViewController: UIViewController, ARSCNViewDelegate {
    @IBOutlet var sceneView: ARSCNView!

    override func viewDidLoad() {
        super.viewDidLoad()
        sceneView.delegate = self
        let configuration = ARWorldTrackingConfiguration()
        configuration.planeDetection = [.horizontal, .vertical]
        sceneView.session.run(configuration)
    }

    func renderer(_ renderer: SCNSceneRenderer, didAdd node: SCNNode, for anchor: ARAnchor) {
        if let planeAnchor = anchor as? ARPlaneAnchor {
            let plane = SCNPlane(width: CGFloat(planeAnchor.extent.x), height: CGFloat(planeAnchor.extent.z))
            let planeNode = SCNNode(geometry: plane)
            planeNode.position = SCNVector3(planeAnchor.center.x, 0, planeAnchor.center.z)
            planeNode.eulerAngles.x = -.pi / 2
            node.addChildNode(planeNode)
        }
    }
}
```

#### ARFaceAnchor Demo
```swift
import UIKit
import ARKit

class ViewController: UIViewController, ARSCNViewDelegate {
    @IBOutlet var sceneView: ARSCNView!

    override func viewDidLoad() {
        super.viewDidLoad()
        sceneView.delegate = self
        let configuration = ARFaceTrackingConfiguration()
        sceneView.session.run(configuration)
    }

    func renderer(_ renderer: SCNSceneRenderer, didUpdate node: SCNNode, for anchor: ARAnchor) {
        if let faceAnchor = anchor as? ARFaceAnchor {
            // Handle face anchor updates
        }
    }
}
```

#### ARImageAnchor Demo
```swift
import UIKit
import ARKit

class ViewController: UIViewController, ARSCNViewDelegate {
    @IBOutlet var sceneView: ARSCNView!

    override func viewDidLoad() {
        super.viewDidLoad()
        sceneView.delegate = self
        let configuration = ARWorldTrackingConfiguration()
        if let referenceImages = ARReferenceImage.referenceImages(inGroupNamed: "AR Resources", bundle: nil) {
            configuration.detectionImages = referenceImages
        }
        sceneView.session.run(configuration)
    }

    func renderer(_ renderer: SCNSceneRenderer, didAdd node: SCNNode, for anchor: ARAnchor) {
        if let imageAnchor = anchor as? ARImageAnchor {
            // Handle image anchor
        }
    }
}
```

#### ARObjectAnchor Demo
```swift
import UIKit
import ARKit

class ViewController: UIViewController, ARSCNViewDelegate {
    @IBOutlet var sceneView: ARSCNView!

    override func viewDidLoad() {
        super.viewDidLoad()
        sceneView.delegate = self
        let configuration = ARWorldTrackingConfiguration()
        if let referenceObjects = ARReferenceObject.referenceObjects(inGroupNamed: "AR Resources", bundle: nil) {
            configuration.detectionObjects = referenceObjects
        }
        sceneView.session.run(configuration)
    }

    func renderer(_ renderer: SCNSceneRenderer, didAdd node: SCNNode, for anchor: ARAnchor) {
        if let objectAnchor = anchor as? ARObjectAnchor {
            // Handle object anchor
        }
    }
}
```

### ARCore in C++ and NDK

#### Anchor Demo
```cpp
#include <arcore_c_api.h>

void createAnchor(ArSession* session, ArPose* pose) {
    ArAnchor* anchor = nullptr;
    ArSession_createAnchor(session, pose, &anchor);
    // Use the anchor
}
```

#### Plane Demo
```cpp
#include <arcore_c_api.h>

void detectPlanes(ArSession* session, ArFrame* frame) {
    ArTrackableList* plane_list = nullptr;
    ArTrackableList_create(session, &plane_list);
    ArFrame_getUpdatedTrackables(session, frame, AR_TRACKABLE_PLANE, plane_list);

    int32_t plane_count = 0;
    ArTrackableList_getSize(session, plane_list, &plane_count);
    for (int i = 0; i < plane_count; ++i) {
        ArTrackable* trackable = nullptr;
        ArTrackableList_acquireItem(session, plane_list, i, &trackable);
        ArPlane* plane = ArAsPlane(trackable);
        // Use the plane
        ArTrackable_release(trackable);
    }
    ArTrackableList_destroy(plane_list);
}
```

#### PointCloud Demo
```cpp
#include <arcore_c_api.h>

void getPointCloud(ArSession* session, ArFrame* frame) {
    const ArPointCloud* point_cloud = nullptr;
    ArFrame_acquirePointCloud(session, frame, &point_cloud);
    // Use the point cloud
    ArPointCloud_release(point_cloud);
}
```

#### AugmentedImage Demo
```cpp
#include <arcore_c_api.h>

void detectAugmentedImages(ArSession* session, ArFrame* frame) {
    ArTrackableList* image_list = nullptr;
    ArTrackableList_create(session, &image_list);
    ArFrame_getUpdatedTrackables(session, frame, AR_TRACKABLE_AUGMENTED_IMAGE, image_list);

    int32_t image_count = 0;
    ArTrackableList_getSize(session, image_list, &image_count);
    for (int i = 0; i < image_count; ++i) {
        ArTrackable* trackable = nullptr;
        ArTrackableList_acquireItem(session, image_list, i, &trackable);
        ArAugmentedImage* image = ArAsAugmentedImage(trackable);
        // Use the augmented image
        ArTrackable_release(trackable);
    }
    ArTrackableList_destroy(image_list);
}
```

#### AugmentedFace Demo
```cpp
#include <arcore_c_api.h>

void detectAugmentedFaces(ArSession* session, ArFrame* frame) {
    ArTrackableList* face_list = nullptr;
    ArTrackableList_create(session, &face_list);
    ArFrame_getUpdatedTrackables(session, frame, AR_TRACKABLE_AUGMENTED_FACE, face_list);

    int32_t face_count = 0;
    ArTrackableList_getSize(session, face_list, &face_count);
    for (int i = 0; i < face_count; ++i) {
        ArTrackable* trackable = nullptr;
        ArTrackableList_acquireItem(session, face_list, i, &trackable);
        ArAugmentedFace* face = ArAsAugmentedFace(trackable);
        // Use the augmented face
        ArTrackable_release(trackable);
    }
    ArTrackableList_destroy(face_list);
}
```

To run the provided ARKit and ARCore demos on an iOS phone and an Android device, you'll need to follow these steps:

### Running ARKit Demos on iOS

1. **Set Up Xcode**:
   - Download and install Xcode from the Mac App Store.
   - Ensure you have the latest version of Xcode and iOS SDK.

2. **Create a New ARKit Project**:
   - Open Xcode and create a new project.
   - Choose the "Augmented Reality App" template.
   - Select Swift as the language and SceneKit as the content technology.

3. **Add the Code**:
   - Replace the default `ViewController.swift` content with the provided ARKit demo code.
   - Ensure you have the necessary ARKit imports and configurations.

4. **Run on a Device**:
   - Connect your iOS device to your Mac.
   - Select your device as the target in Xcode.
   - Click the "Run" button to build and deploy the app to your device.

### Running ARCore Demos on Android

1. **Set Up Android Studio**:
   - Download and install Android Studio from the official website.
   - Ensure you have the latest version of Android Studio and Android SDK.

2. **Create a New ARCore Project**:
   - Open Android Studio and create a new project.
   - Choose the "Empty Activity" template.

3. **Add ARCore Dependencies**:
   - Open the `build.gradle` file and add the ARCore dependencies:
     ```gradle
     implementation 'com.google.ar:core:1.31.0'
     ```

4. **Add the Code**:
   - Create a new C++ file and add the provided ARCore demo code.
   - Ensure you have the necessary ARCore imports and configurations.

5. **Configure CMake**:
   - Configure CMake to include your C++ code in the build process.
   - Update the `CMakeLists.txt` file to include your source files.

6. **Run on a Device**:
   - Connect your Android device to your computer.
   - Select your device as the target in Android Studio.
   - Click the "Run" button to build and deploy the app to your device.

### Additional Tips

- **Permissions**: Ensure your app has the necessary permissions to access the camera and other AR features.
- **Testing**: Test your app in a well-lit environment with sufficient features for AR tracking.
- **Documentation**: Refer to the official ARKit documentation and ARCore documentation for more detailed information and troubleshooting.

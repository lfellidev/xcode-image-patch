# Xcode (React Native) patches
Steps to fix image Xcode bug.


## Getting Started
1. Run:
```
yarn global add patch-package
```

2.  Make a new folder called patches

3. Create a new file react-native+VERSION.patch inside the folder patches. 
Please change the word version to your React Native version eg:
react-native+0.63.4.patch for reactive native 0.63.4. Plus charactere must be inclued.

4. Inside the file add the code below:

```
diff --git a/node_modules/react-native/Libraries/Image/RCTUIImageViewAnimated.m b/node_modules/react-native/Libraries/Image/RCTUIImageViewAnimated.m
index 21f1a06..2444713 100644
--- a/node_modules/react-native/Libraries/Image/RCTUIImageViewAnimated.m
+++ b/node_modules/react-native/Libraries/Image/RCTUIImageViewAnimated.m
@@ -272,6 +272,9 @@ - (void)displayDidRefresh:(CADisplayLink *)displayLink
 
 - (void)displayLayer:(CALayer *)layer
 {
+  if (!_currentFrame) {
+    _currentFrame = self.image;
+  }
   if (_currentFrame) {
     layer.contentsScale = self.animatedImageScale;
     layer.contents = (__bridge id)_currentFrame.CGImage;
diff --git a/node_modules/react-native/scripts/.packager.env b/node_modules/react-native/scripts/.packager.env
new file mode 100644
index 0000000..361f5fb
--- /dev/null
+++ b/node_modules/react-native/scripts/.packager.env
@@ -0,0 +1 @@
+export RCT_METRO_PORT=8081
```

5. On the root of the project run:
```
patch-package
```

6. Inside the folder ./ios delete the folder pod e the file Podfile.

7. Still inside ./ios folder type:
```
pod install 
```

## Reference
- [https://www.npmjs.com/package/patch-package](https://www.npmjs.com/package/patch-package)
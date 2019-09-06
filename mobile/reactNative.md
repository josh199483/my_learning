# react usage
## tutorial
[first](https://ithelp.ithome.com.tw/users/20103341/ironman/1023?page=3)

[second](https://ithelp.ithome.com.tw/articles/10188878)

[simple](https://blog.patw.me/archives/1481/as-f2e-using-react-native-to-develop-app-experience-sharing/)

## feature
[react-native doc](https://reactnavigation.org/docs/en/getting-started.html)

[eslint](https://codeburst.io/setting-up-eslint-and-editorconfig-in-react-native-projects-31b4d9ddd0f6)

[css like sheet](https://github.com/vhpoet/react-native-styling-cheat-sheet)

[icon vector list](https://oblador.github.io/react-native-vector-icons/)

[component lib](https://react-native-training.github.io/react-native-elements/docs/overview.html)

### symlink with library
because RN is not support symlink now, so i use wml to listen change
[wml](https://github.com/wix/wml)

```bash
# copy src to dest automatically
wml add <src> <dest>
# start monitor server, while src is changed,then copy to dest
wml start
```

```bash
# problem 1
package.json 的 main path 要使用 babel compile 過的 file path
使用 src 下的 source code 會出現一點問題 ~ (待釐清)
```

[react native dot env](https://www.reactnativeschool.com/easily-manage-different-environment-configurations-in-react-native)

[responsive react native](https://www.freecodecamp.org/news/how-to-make-your-react-native-app-respond-gracefully-when-the-keyboard-pops-up-7442c1535580/)

[specific usage of react navigation](https://itnext.io/handle-tab-changes-in-react-navigation-v2-faeadc2f2ffe)

[customized tab bar](https://itnext.io/react-native-tab-bar-is-customizable-c3c37dcf711f)

### use react native svg
[manually linking](http://facebook.github.io/react-native/docs/linking-libraries-ios.html#manual-linking)
```bash
npm i react-native-svg
react-native link react-native-svg
cd ios
pod install # need to install by cocoapods!
```

[use nodejs core module on react native](https://www.npmjs.com/package/rn-nodeify)

[native app link](https://github.com/FiberJW/react-native-app-link)

[app version check](https://github.com/kimxogus/react-native-version-check)

[error boundary](https://dotblogs.com.tw/wasichris/2018/01/24/110201)

[add icon image to app(ios/android)](https://medium.com/better-programming/react-native-add-app-icons-and-launch-screens-onto-ios-and-android-apps-3bfbc20b7d4c)

[testflight](https://medium.com/%E5%BD%BC%E5%BE%97%E6%BD%98%E7%9A%84-swift-ios-app-%E9%96%8B%E7%99%BC%E6%95%99%E5%AE%A4/%E6%B8%AC%E8%A9%A6%E5%90%A7-testflight-20f1f77a2ce0)

[react naitve exception handler](https://github.com/master-atul/react-native-exception-handler)

# problem
## iOS
[clear cache](https://medium.com/@abhisheknalwaya/how-to-clear-react-native-cache-c435c258834e)

[using react-native-unimodules](https://github.com/unimodules/react-native-unimodules)

[using react-native-unimodules1](https://gist.github.com/brentvatne/94960dacb343310b76be9cc157d90049/revisions)

### module import error
[Application has not been registered error](https://stackoverflow.com/questions/38340360/react-native-application-has-not-been-registered-error)

[linker command failed with exit code 1](https://www.jianshu.com/p/c39a931f63ad)
```swift
// linker command failed with exit code 1，這次遇到兩次這問題的解決方法
// 1.在iOS的專案下，新建一個empty swift file，然後建立bridge
// 2.使用pod install後，會產生一個pod專案，原本開啟.xcodeproj要改開成.xcworkspace
{
  self.moduleRegistryAdapter = [[UMModuleRegistryAdapter alloc] initWithModuleRegistryProvider:[[UMModuleRegistryProvider alloc] init]];
  RCTBridge *bridge = [[RCTBridge alloc] initWithDelegate:self launchOptions:launchOptions];
  RCTRootView *rootView = [[RCTRootView alloc] initWithBridge:bridge moduleName:@"yourProjectName" initialProperties:nil]; // yourProjectName記得改成自己專案的名字
  rootView.backgroundColor = [[UIColor alloc] initWithRed:1.0f green:1.0f blue:1.0f alpha:1];

  self.window = [[UIWindow alloc] initWithFrame:[UIScreen mainScreen].bounds];
  UIViewController *rootViewController = [UIViewController new];
  rootViewController.view = rootView;
  self.window.rootViewController = rootViewController;
  [self.window makeKeyAndVisible];
  return YES;
}
```
### link library
```
1. Right Click Libraries "Add Files to Project"
2. /node_modules/react-native-gesture-handlers/ios/RNGestureHandler.xcodeproj
3. Go to build phases and add libRNGestureHandler.a

```


### request to local api server
```js
// should not access 'http://localhost:8080'
// should modify to your host machine ip 'http://192.168.0.1:8080'
```

### archive encounter duplicate symbol arm error
[solution](https://github.com/facebook/react-native/issues/12814)

[multasking ipad upload error](https://stackoverflow.com/questions/32559724/ipad-multitasking-support-requires-these-orientations)

## android
### Permission
[ble permission](https://hsiangyu.com/blog/2017/09/04/react-native-permissionsandroid/)


## about xcode
[Connection has no connected handler](https://stackoverflow.com/questions/44081674/react-native-connection-has-no-connection-handler-error-meaning)

# boilerplate tool
[ignite](https://github.com/infinitered/ignite)

# 上架
[上架url](https://appstoreconnect.apple.com)

# iOS
## settimeout
[implement settimeout in swift with javascript](https://ask.helplib.com/javascript/post_13724104)

[modify bundle identifier](https://stackoverflow.com/questions/36119754/how-to-change-the-bundle-identifier-within-react-native)

[app store guide](https://help.apple.com/app-store-connect/#/dev82a6a9d79)
# react usage
## tutorial
[first](https://ithelp.ithome.com.tw/users/20103341/ironman/1023?page=3)

[second](https://ithelp.ithome.com.tw/articles/10188878)

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

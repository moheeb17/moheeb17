<span align="center">
  <br>
<img src="https://developer.apple.com/assets/elements/icons/swiftui/swiftui-96x96_2x.png" width="100">
  <br>
</span>

# PermissionsSwiftUI: A SwiftUI package to handle permissions
<img src="https://img.shields.io/github/workflow/status/jevonmao/PermissionsSwiftUI/Swift?label=CI%20Build"> <img src="https://img.shields.io/github/contributors/jevonmao/PermissionsSwiftUI"> <img src="https://img.shields.io/badge/License-MIT-blue.svg"> <img src="https://img.shields.io/github/issues/jevonmao/PermissionsSwiftUI?color=orange"> <img src="https://img.shields.io/github/commit-activity/w/jevonmao/PermissionsSwiftUI?color=yellowgreen&logoColor=yellowgreen"> 

`PermissionsSwiftUI` displays and handles permissions in SwiftUI. It is largely inspired by [SPPermissions](https://github.com/varabeis/SPPermissions).
The UI is highly customizable and resembles an **Apple style**. If you like the project, don't forget to `star ★` and follow me on GitHub. <br />
<br />
<img src="https://github.com/jevonmao/PermissionsSwiftUI/blob/main/Resources/screenshot_main_modal.PNG?raw=true" height="550"/>
&emsp; &emsp;
<img src="https://github.com/jevonmao/PermissionsSwiftUI/blob/main/Resources/screenshot_main_alert.PNG?raw=true" height="550"/> <br />
<p align="center"> PermissionsSwiftUI looks equally gorgeous on both ☀️light and 🌑dark mode. </p>

## Navigation
-  [Installation](#installation)
-  [Quickstart](#quickstart) 
<details>
  <summary>Usage</summary>
  
-  [Usage](#usage)
    -  [Customize Permission Texts](#customize-permission-texts)
    -  [Customize header texts](#customize-header-texts)
    -  [`onAppear` and `onDisappear` Override](#onappear-and-ondisappear-override)
    -  [Auto Check Authorization](#auto-check-authorization)
    -  [Auto Dismiss](#auto-dismiss)
    -  [Customize Colors](#customize-colors)
    -  [Restrict Dismissal](#restrict-dismissal)
    -  [Configuring Health Permissions](#configuring-health-permissions)
</details>

-  [Supported Permissions](#supported-permissions)
-  [Additional Information](#additional-information)
    -  [Acknowledgement](#acknowledgement)
    -  [License](#license)

## Installation
### Requirements
* iOS 13 or iPadOS 13
* Xcode 12 and Swift 5.3
* No tvOS, MacOS, and WatchOS support for now
### Install
You can install PermissionsSwiftUI into your Xcode project via Swift Package Manager. To learn more about Swift Package Manager, click [here](https://swift.org/package-manager/)
1. In Xcode, open your project and navigate to **File** → **Swift Packages** → **Add Package Dependency...**
2. Paste the repository URL (`https://github.com/jevonmao/PermissionsSwiftUI`) and click **Next**.
3. For **Version**, select **Up to next major**.
4. Click **Next** and click **Finish**.
5. You are all set, have fun using PermissionsSwiftUI!

## Quickstart
> Before you start, please `star ★` this repository. Your star is my biggest motivation to pull all-nighters and maintain this open source project.

### Modal Style
To use PermissionsSwiftUI to show a beauitful permission modal, simply add the `JMPermission`to any view. <br />
`.JMPermissions(showModal: $showModal, for: [.locationAlways, .photo, .microphone])`
Pass in a `Binding<Bool>` to show the modal view, and add whatever permissions you want to show.
```Swift
   struct ContentView: View {
       @State var showModal = false
       var body: some View {
           Button(action: {
               showModal=true
           }, label: {
               Text("Ask user for permissions")
           })
           .JMModal(showModal: $showModal, for: [.locationAlways, .photo, .microphone])
       }
   }
 ```
### Alert Style
<img src="https://github.com/jevonmao/PermissionsSwiftUI/blob/main/Resources/alert_view_screenshot.png?raw=true" height="300" align="left" />
The alert style is equally gorgeous, and allows for more versatile use. It is recommended when you have less than 3 permissions.  <br />
To show a permission pop up alert, use: 

```Swift
.JMAlert(showModal: $showModal, for: [.locationAlways, .photo])
```
Similar to the previous `JMPermissions`, you need to pass in a `Binding<Bool>` to show the view, and add whatever permissions you want to show.

## Usage
### Customize Permission Texts
😱 Be aware. Features ahead will wow you - the customization is so advanced, yet so simple. Have fun!

To customize permission texts, use the modifier `setPermissionComponent()`
For example, you can change title, description, and image icon:
```Swift
.setPermissionComponent(for: .camera, 
                        image: AnyView(Image(systemName: "camera.fill")), 
                        title: "Camcorder",
                        description: "App needs to record videos")
```
and the result:
<div style="text-align:center">
<img src="https://github.com/jevonmao/PermissionsSwiftUI/blob/main/Resources/Screenshot-camera.png" height="70">
</div>
<br />
Or only change 1 of title and description:

```Swift
setPermissionComponent(for: .tracking, title: "Trackers")
```
```Swift
setPermissionComponent(for: .tracking, description: "Tracking description")
```

**Note:** 
* The parameters you don't provide will show the default text
* Add the `setPermissionComponent` modifier on your root level view, after `JMPermissions` modifier

The `image` parameter accepts **AnyView**, so feel free to use [SF Symbols](https://developer.apple.com/design/human-interface-guidelines/sf-symbols/overview/) or your custom asset:
```Swift
.setPermissionComponent(for: .camera, 
                        image: AnyView(Image("Your-cool-image"))
```
Even full SwiftUI views will work😱:
```Swift
.setPermissionComponent(for: .camera, 
                        image: AnyView(YourCoolView())
```
You can use custom text and icon for all the supported permissions, with a single line of code.
### Customize Header Texts
To customize the header title, use the modifier `changeHeaderTo`:
<img align="right" src="https://github.com/jevonmao/PermissionsSwiftUI/blob/main/Resources/Header%20annotation.png?raw=true" alt="Annotated for headers screen" height="400" />
```Swift
.JMPermissions(showModal: $showModal, for: [.camera, .location, .calendar])
.changeHeaderTo("App Permissions")
```
To customize the header description, use the modifier `changeHeaderDescriptionTo`:
```Swift
.JMPermissions(showModal: $showModal, for: [.camera, .location, .photo])
.changeHeaderDescriptionTo("Instagram need certain permissions in order for all the features to work.")
```
To customize the bottom description, use the modifier `changeBottomDescriptionTo`:
```Swift
.JMPermissions(showModal: $showModal, for: [.camera, .location, .photo])
.changeBottomDescriptionTo("If not allowed, you have to enable permissions in settings")
```
### `onAppear` and `onDisappear` Override
You might find it incredibly useful to execute your code, or perform some update action when a PermissionsSwiftUI view appears and disappears. <br />
You can perform some action when PermissionsSwiftUI view appears or disappears by:
```Swift
.JMPermissions(showModal: $showModal, for: [.locationAlways, .photo, .microphone], onAppear: {}, onDisappear: {})
```
The `onAppear` and `onDisappear` **closure parameters will be executed** everytime PermissionsSwiftUI view **appears and disappears.** <br />
The same view modifier closure for state changes are available for the `JMAlert` modifier:
```Swift
.JMAlert(showModal: $showModal,
                     for: [.locationAlways, .photo],
                     onAppear: {print("Appeared")},
                     onDisappear: {print("Disappeared")})
```
### Auto Check Authorization
PermissionsSwiftUI by default will automatically check for authorization status. It will only show permissions that are currently `notDetermined` status. (iOS system prevent developers from asking denied permissions. Allowed permissions will also be ignored by PermissionsSwiftUI). If all permissions are allowed or denied, PermissionsSwiftUI will not show the modal or alert at all.
To set auto check authorization, use the `autoCheckAuthorization` parameter:
```Swift
.JMModal(showModal: $showModal, for: [.camera], autoCheckAuthorization: false)
```
same applies for JMAlert
```Swift
.JMAlert(showModal: $showModal, for: [.camera], autoCheckAuthorization: false)
```
### Auto Dismiss
PermissionsSwiftUI by default will automatically dismiss the modal or alert after user allows the last permission item. However, you can override this behavior.
```Swift
func JMModal(showModal: Binding<Bool>, for permissions: [PermissionType], autoDismiss: Bool) -> some View
```
Pass in `true` or `false` to select whether to automatically dismiss the view.

### Customize Colors
Using PermissionSwiftUI's capabilities, developers and designers can customize all the UI colors with incredible flexibility. You can fully configure all color at all states with your custom colors. <br />
To easily change the accent color:
```Swift
.setAccentColor(to: Color(.systemPurple))
```
To change the primary (default Apple blue) and tertiary (default Apple red) colors:
```Swift
.setAccentColor(toPrimary: Color(.systemPurple),
                toTertiary: Color(.systemGreen))
```
<img align src="https://github.com/jevonmao/PermissionsSwiftUI/blob/main/Resources/Button_color_customize_showcase.png?raw=true" alt="Alert pop up with customized colors" height="300">

> ⚠️ `.setAccentColor()` and `.setAllowButtonColor()` should never be used at the same time.

To unleash the full customization of all button colors under all states, you need to pass in the `AllButtonColors` struct:
```Swift
.setAllowButtonColor(to: .init(buttonIdle: ButtonColor(foregroundColor: Color,
                                                               backgroundColor: Color),
                                       buttonAllowed: ButtonColor(foregroundColor: Color,
                                                                  backgroundColor: Color),
                                       buttonDenied: ButtonColor(foregroundColor: Color,
                                                                 backgroundColor: Color)))
```
For more information regarding the above method, reference the [official documentation](https://jevonmao.github.io/PermissionsSwiftUI/Structs/AllButtonColors.html).

### Restrict Dismissal
PermissionsSwiftUI will by default, prevent the user from dismissing the modal and alert, before all permissions have been interacted. This means if the user has not explictly denied or allowed EVERY permission shown, they will not be able to dismiss the PermissionsSwiftUI view. This restrict dismissal behavior can be overriden by the `var restrictModalDismissal: Bool` or `var restrictAlertDismissal: Bool` properties.
To disable the default restrict dismiss behavior:
```Swift
.JMModal(showModal: $show, for permissions: [.camera], restrictDismissal: false)
```
You can also configure with the model:
```Swift
let model: PermissionStore = {
        var model = PermissionStore()
        model.permissions = [.camera]
        model.restrictModalDismissal = false
        model.restrictAlertDismissal = false
        return model
    }
    ......

    .JMModal(showModal: $showModal, forModel: model)
```
### Configuring Health Permissions
Unlike all the other permissions, the configuration for health permission is a little different. Because Apple require developers to explictly set read and write types, PermissionsSwiftUI greatly simplifies the process.
#### `HKAccess`
The structure HKAccess is required when initalizing health permission’s enum associated values. It encapsulates the read and write type permissions for the health permission.

To set read and write health types (`activeEnergyBurned` is used as example here):
```Swift
let healthTypes = Set([HKSampleType.quantityType(forIdentifier: .activeEnergyBurned)!])
.JMModal(showModal: $show, for: [.health(categories: .init(readAndWrite: healthTypes))])

//Same exact syntax for JMAlert styles
.JMAlert(showModal: $show, for: [.health(categories: .init(readAndWrite: healthTypes))])

```
To set read or write individually:
```Swift
let readTypes = Set([HKSampleType.quantityType(forIdentifier: .activeEnergyBurned)!])
let writeTypes = Set([HKSampleType.quantityType(forIdentifier: .appleStandTime)!])
.JMModal(showModal: $showModal, for: [.health(categories: .init(read: readTypes, write: writeTypes))])
```
You may also set only read or write type:
```Swift
let readTypes = Set([HKSampleType.quantityType(forIdentifier: .activeEnergyBurned)!])
.JMModal(showModal: $showModal, for: [.health(categories: .init(read: readTypes))])

```
## Supported Permissions
Here is a list of all permissions PermissionsSwiftUI already supports support(health not in image but is supported). Yup, even the newest `tracking` permission for iOS 14 so you can stay on top of your game. All permissions in PermissionsSwiftUI come with a default name, description, and a stunning Apple native SF Symbols icon.
<br /> <br /> <br />
<img align="center" src="https://github.com/jevonmao/PermissionsSwiftUI/blob/main/Resources/All-permissions-card.png" alt="A card of all the permissions" width="100%">

## Additional Information

### Acknowledgement
SPPermissions is in large a SwiftUI remake of famous Swift library **[SPPermissions](https://github.com/varabeis/SPPermissions)** by @verabeis. SPPermissions was initially created in 2017, and today on GitHub has over 4000 stars. PermissionsSwiftUI aims to deliver a just as beautiful and powerful library in SwiftUI. If you `star ★` my project PermissionsSwiftUI, be sure to checkout the original project SPPermissions where I borrowed the UI Design, some parts of README.md page, and important source code references along the way.
### License
PermissionsSwiftUI is created by Jingwen (Jevon) Mao and licensed under the [MIT License](https://jingwen-mao.mit-license.org)

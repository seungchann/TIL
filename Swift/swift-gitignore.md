# .gitignore 파일 간단하게 추가하기  
> 
* git repository에 iOS 프로젝트를 업데이트 시 **gitignore 템플릿을 제공하는 사이트**가 있다.  
* 이용 방법  
  * https://gitignore.io 에 접속한다.  
  * `swift`, `xcode`, `cocoapods` 키워드로 검색한다.  
  * 결과로 나온 코드를 `.gitignore`로 사용한다.  

* `swift`, `xcode`, `cocoapods` 키워드로 검색된 파일은 아래와 같다.  
```

# Created by https://www.toptal.com/developers/gitignore/api/swift,xcode,cocoapods
# Edit at https://www.toptal.com/developers/gitignore?templates=swift,xcode,cocoapods

### CocoaPods ###
## CocoaPods GitIgnore Template

# CocoaPods - Only use to conserve bandwidth / Save time on Pushing
#           - Also handy if you have a large number of dependant pods
#           - AS PER https://guides.cocoapods.org/using/using-cocoapods.html NEVER IGNORE THE LOCK FILE
Pods/

### Swift ###
# Xcode
#
# gitignore contributors: remember to update Global/Xcode.gitignore, Objective-C.gitignore & Swift.gitignore

## User settings
xcuserdata/

## compatibility with Xcode 8 and earlier (ignoring not required starting Xcode 9)
*.xcscmblueprint
*.xccheckout

## compatibility with Xcode 3 and earlier (ignoring not required starting Xcode 4)
build/
DerivedData/
*.moved-aside
*.pbxuser
!default.pbxuser
*.mode1v3
!default.mode1v3
*.mode2v3
!default.mode2v3
*.perspectivev3
!default.perspectivev3

## Obj-C/Swift specific
*.hmap

## App packaging
*.ipa
*.dSYM.zip
*.dSYM

## Playgrounds
timeline.xctimeline
playground.xcworkspace

# Swift Package Manager
# Add this line if you want to avoid checking in source code from Swift Package Manager dependencies.
# Packages/
# Package.pins
# Package.resolved
# *.xcodeproj
# Xcode automatically generates this directory with a .xcworkspacedata file and xcuserdata
# hence it is not needed unless you have added a package configuration file to your project
# .swiftpm

.build/

# CocoaPods
# We recommend against adding the Pods directory to your .gitignore. However
# you should judge for yourself, the pros and cons are mentioned at:
# https://guides.cocoapods.org/using/using-cocoapods.html#should-i-check-the-pods-directory-into-source-control
# Pods/
# Add this line if you want to avoid checking in source code from the Xcode workspace
# *.xcworkspace

# Carthage
# Add this line if you want to avoid checking in source code from Carthage dependencies.
# Carthage/Checkouts

Carthage/Build/

# Accio dependency management
Dependencies/
.accio/

# fastlane
# It is recommended to not store the screenshots in the git repo.
# Instead, use fastlane to re-generate the screenshots whenever they are needed.
# For more information about the recommended setup visit:
# https://docs.fastlane.tools/best-practices/source-control/#source-control

fastlane/report.xml
fastlane/Preview.html
fastlane/screenshots/**/*.png
fastlane/test_output

# Code Injection
# After new code Injection tools there's a generated folder /iOSInjectionProject
# https://github.com/johnno1962/injectionforxcode

iOSInjectionProject/

### Xcode ###

## Xcode 8 and earlier

### Xcode Patch ###
*.xcodeproj/*
!*.xcodeproj/project.pbxproj
!*.xcodeproj/xcshareddata/
!*.xcworkspace/contents.xcworkspacedata
/*.gcno
**/xcshareddata/WorkspaceSettings.xcsettings

# End of https://www.toptal.com/developers/gitignore/api/swift,xcode,cocoapods
```

## References
* https://silver-g-0114.tistory.com/32  
# Cocoapods frameworks version number fix

This script changes all Cocoapods frameworks' version numbers to match the app's plist, which is required as of 21 Oct 2015.

Thanks to [@orta](https://github.com/orta) for the initial idea!

## Install

1) Add this script at the end of your Podfile.

    post_install do |installer|
        app_plist = "MyApp/Info.plist"
        plist_buddy = "/usr/libexec/PlistBuddy"
        
        version = `#{plist_buddy} -c "Print CFBundleShortVersionString" "#{app_plist}"`.strip
        
        puts "Updating CocoaPods frameworks' version numbers to #{version}"
      
        installer.pods_project.targets.each do |target|  
            `#{plist_buddy} -c "Set CFBundleShortVersionString #{version}" "Pods/Target Support Files/#{target}/Info.plist"`  
        end  
    end
    
2) Update the path in `app_plist` variable so that it matches the path to your app's .plist file.

3) Run `pod install` afterwards.

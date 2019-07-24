---
title: React Native 遇到的一些问题
date: 2019-07-24 08:59:11
tags:
---

1. `node: command not found`

   Oh, yeah sorry that's probably because when you run `/bin/sh` from your terminal it's picking up the `PATH` from the parent shell, which allows node to work.

   Different approach then, try (from your usual terminal where node is working):

   `ln -s $(which node) /usr/local/bin/node`

   That should put a symlink to node somewhere in the `PATH` that `sh` uses. If it already exists, I'm stumped.

   [参考](https://github.com/realm/realm-js/issues/1448#issuecomment-340757479)

2. `/node_modules/react-native/scripts/third-party/glog-0.3.5/test-driver'. Couldn't follow symbolic link.`

   ```
   unlink node_modules/react-native/third-party/glog-0.3.5/test-driver
   ```

   或者

   ```
   rm -rf node_modules/react-native/scripts/third-party/glog-0.3.5/
   ```

   [参考 1](https://github.com/facebook/react-native/issues/14464#issuecomment-309289808),[参考 2](https://github.com/facebook/react-native/issues/21832#issuecomment-448077778)

3. `Xcode 10: Build input file double-conversion cannot be found`

   I was facing a similar issue after upgrading. To solve it I ran the following commands:

   ```shell
   $ cd node_modules/react-native/scripts && ./ios-install-third-party.sh && cd ../../../
   $ cd node_modules/react-native/third-party/glog-0.3.5/ && ../../scripts/ios-configure-glog.sh && cd ../../../../
   ```

   如果没有以上文件，先执行`react-native run-ios`，再执行以上命令

   [参考](https://github.com/facebook/react-native/issues/21168#issuecomment-422431294)

4. android `Duplicate resources`

   For those who are doing this before generating apk

   `react-native bundle --platform android --dev false --entry-file index.js --bundle-output android/app/src/main/assets/index.android.bundle --assets-dest android/app/src/main/res`

   it generate unnecessary drawable images in drawable folder. so make sure to remove it and try again.

   `android-> app -> src -> main -> res -> drawable`

   [参考](https://github.com/facebook/react-native/issues/19239#issuecomment-414564404)

5. Could not find iPhone 6 simulator after upgrade to Xcode 10

   I went to

   ```
   node_modules/react-native/local-cli/runIOS/findMatchingSimulator.js
   ```

   And i've replaced:

   ```
   if (version.indexOf('iOS') !== 0 )
   ```

   with

   ```
   if (!version.includes("iOS" ))
   ```

   And

   ```
   if (simulator.availability !== '(available)')
   ```

   with

   ```
   if (simulator.isAvailable !== true)
   ```

   [参考](https://stackoverflow.com/a/55560977)

6. ld: library not found for -lstdc++.6.0.9

   [参考](https://www.jianshu.com/p/61083ef4eb84)

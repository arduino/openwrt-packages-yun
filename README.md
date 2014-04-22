# OpenWrt Yún packages

## What you'll find here

Not every available package is stored on [OpenWrt Yún repository](https://github.com/arduino/openwrt-yun): as there are thousands of them, separating the core ones from the rest allows to keep things cleaner.

When you [build the image](https://github.com/arduino/openwrt-yun#how-to-build) on your computer, [this file](https://github.com/arduino/openwrt-yun/blob/master/feeds.conf.default#L1) is used to download additional packages list. As You can see, the first line mentions this very repository.

Just as the main repository, this one is a fork of the [official OpenWrt one](https://dev.openwrt.org/wiki/GetSource#GIT), with a handful of Yún specific additions and customizations.

## What can you do with this repo

If you wish to contribute to the list of available packages, please consider forking this repo, changing the url on [feeds.conf.default](https://github.com/arduino/openwrt-yun/blob/master/feeds.conf.default#L1) to point to your fork and share your work with the community

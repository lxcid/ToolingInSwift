# [fit] Tooling<br />in<br />**Swift** 

^ Greeting!

^ Today I'm up here to share about Tooling in Swift.

---

# [fit] I'm Stan!
## A.K.A **lx**cid

^ I'm Stan!

^ Also known as lxcid.

^ *My project usually begin with LX.

---

> What you do today can be **automated** tomorrow
-- Douglas Mcllroy

^ Start of with this quote.

^ As a programmer, it is your job to put yourself out of business.

---

# [fit] **Automate**

^ There's mainly 4 types of automation I would like to talk about.

---

# [fit] **Automate**
## Transformation

^ Data: To and fro machine readable to human readable format

^ Enable scalability

---

# [fit] **Automate**
## Abstraction

^ Programming Paradigm

^ Ever evolving: From C (functional) to Objective-C (object-oriented) to Swift (protocol-oriented)

^ Increase our contextual bandwidth

^ How much we can understand or describe

---

# [fit] **Automate**
## Flow

^ Natural flow of events

^ Visualize as (directed) graphs

^ Restrict our choices

^ Build automation

^ Dependency management system

---

# [fit] **Automate**
### Error Detection/Prevention

^ Useful in highly error prone environment like Collaboration

^ Can't merge without resolving conflicts

^ Continuous integration

^ Tests

^ Gave us confidence

---

# [fit] *Automate*
## Build

---

# Build

- *On-demand*
Makefile
nomad/shenzhen
fabric/fastlane
custom scripts

^ Most tools are ruby backed.
^ [nomad/shenzhen](https://github.com/nomad/shenzhen)
^ [fabric/fastlane](https://fastlane.tools) recently got acquired by Twitter, part of Fabric.

---
# Build

- **Scheduled/Triggered**
Xcode Server
Travis CI
Circle CI (beta)

---
# Build

- **Scheduled/Triggered**
☞ Xcode Server ☜
Travis CI
Circle CI (beta)

^ Focus on Xcode Server 

---

![fit](images/os_x_server_icon.png)
# OS X Server <br /> *19.99 USD*

---

![fit](images/xcode_server_settings.png)

---

![fit](images/xocde_preference_accounts_servers.png)

---

![fit](images/xcode_menu_product_create_bot.png)

---

![200%](images/schedule_on_commit.png)

---

![200%](images/schedule_periodically.png)

---

![100%](images/triggers.png)

---

![inline, 50%](sources/HackerNewsReader/Design/Icon Rendered/iTunesArtwork@2x.png)

[rnystrom/HackerNewsReader](https://github.com/rnystrom/HackerNewsReader)

**Demo**

^ We attempt to set up continuous integration for HackerNewsReader.

---

![fit](images/integrate_no_cocoapods_error.png)

---

# Integration Error

```
Error: /../Podfile.lock: No such file or directory
Error: /Manifest.lock: No such file or directory
Error: The sandbox is not in sync with the Podfile.lock.
Run 'pod install' or update your CocoaPods installation.
```

---

# [fit] Cocoapods
#### **automate dependency management**

---

# Run

```bash
sudo gem install cocoapods
```

---

# Pre-Trigger

```bash
pod install
```

---

![fit](images/integrate_resolve_cocoapods_with_sudo.png)

---

# **Without** <br /> ~~`sudo`~~ <br /> **?**

---

![fit](images/os_x_server_users__xcsbuildd.png)

^ Integration are run under the user `_xcsbuildd` (likely acronym for Xcode Server Build Daemon).
^ We needs set up cocoapods in user space.

---

# Pre-Trigger

```bash
#!/bin/sh
export WORKING_DIR="HackerNewsReader"
# Cocoapods require us to define the language
export LANG=en_US.UTF-8
# Add user space bin directory of rubygem in $PATH
if which ruby >/dev/null && which gem >/dev/null; then
	PATH="$(ruby -rubygems -e 'puts Gem.user_dir')/bin:$PATH"
fi
# Install cocoapods in user space of rubygem
gem install --user-install cocoapods
cd $WORKING_DIR
pod install
```

---

^ Show Xcode Web: https://khinboon.local/xcode

# DEMO

^ [After Demo]
^ We gotten the project set up for continuous integration.
^ Periodically, the CI will detect new commits and spawn an integration.
 
---

# Nightly Build

---

# Nightly Build

☐ Versioning

☑︎ **Integration**

☐ Commit & Tag

☐ Distribute

---

# [fit] *Automate*
## Versioning

^ Mostly incremental

---

# Versioning
- **Apple Generic Versioning**

---

# [fit] agvtool
### **Apple Generic Versioning**

---

## **Apple Generic Versioning**

- Version (Marketing)
*CFBundleShortVersionString*
- Build (Internal)
*CFBundleVersion*

---

![fit](images/about_xcode.png) 

---

## **Apple Generic Versioning** Xcode

- 7.1
*CFBundleShortVersionString*
- 9079
*CFBundleVersion*
- 7B91b
*ProductBuildVersion*

---

![fit](images/about_maps.png)

---

## **Apple Generic Versioning** Map

- 2.0
*CFBundleShortVersionString*
- 1844.0.15
*CFBundleVersion*

---

![fit](images/about_server.png)

---

## **Apple Generic Versioning** OS X Server

- 5.0.15
*CFBundleShortVersionString*
- 662
*CFBundleVersion*
- 15S4033
*ProductBuildVersion*

---

## Cons of **Apple Generic Versioning**

- No specification/No version graph
- Inconsistent
- Not expressive
- No git support
- No versioning for Version (Marketing)
- **Limited versioning for Build (Internal)**

---

# Limited versioning for Build (Internal)
#### **agvtool next-version | bump**

⭕️ 1844.0.15 ➤ 1845

❌ 1844.0.15 ➤ 1844.1

❌ 1844.0.15 ➤ 1844.0.16

---

# Versioning
- ~~*Apple Generic Versioning*~~
- **Semantic Versioning 2.0.0**

---

# [fit] Major.Minor.Patch-Prerelease(DSV)+Metadata(DSV)
#### **DSV =Dot Separated Values**

---

![fit](images/semver_graph.png)

---

# [fit] **LXSemVer.framework**
### [lxcid/LXSemVer](https://github.com/lxcid/LXSemVer)

^ Carthage: Share your project scheme.
^ Cocoapods-Rome: Create a podspec.

---

# Scripting CLI

*Podfile*

```Ruby
plugin 'cocoapods-rome'
pod 'LXSemVer', '~> 1.0'
```

*semver*

```Bash
#!/usr/bin/env xcrun swift -F Rome
```

---

# DEMO

---

# Scripting Nightly Triggers

- Prerelease is always tagged with **nightly**
- Retrieve project's versions from its Info.plist
- Keep a nightly counter in its Info.plist
**LXNightlyBuildVersionString**

---

# [fit] **LXXcodeTools.framework**
### [lxcid/LXXcodeTools](https://github.com/lxcid/LXXcodeTools)

---

# **LXXcodeTools.framework**

- Spawn new process with **NSTask**
- Parses the output of Xcodebuild
**#/usr/bin/xcrun xcodebuild -showBuildSettings**

# DEMO

---

# [fit] **DevToolsCore.framework**
#### /Applications/Xcode.app/Contents/PlugIns/Xcode3Core.ideplugin/Contents/Frameworks/DevToolsCore.framework

---

# **DevToolsCore.framework**

- Private framework
- Generate our own headers
- Import the framework into Swift by
defining a module with modulemap

---

# **DevToolsCore.framework**
#### Generating headers

 - *class-dump*
 - *Partially declared headers is fine*

---

# **DevToolsCore.framework**
#### Defining modulemap

*Create umbrella header*

```C
#import <DevToolsCore/PBXObject.h>
#import <DevToolsCore/PBXProject.h>
#import <DevToolsCore/PBXProjectItem.h>
#import <DevToolsCore/PBXTarget.h>
```

*Create modulemap*

```C
module DevToolsCore {
	umbrella header "DevToolsCore.h"
	link framework "DevToolsCore"
}
```

---

# **DevToolsCore.framework**
#### Import the modularized Framework

```Swift
#!/usr/bin/env xcrun swift
	-F Rome
	-I modules
	-F /Applications/Xcode.app/Contents/SharedFrameworks
	-F /Applications/Xcode.app/Contents/Frameworks
	-F /Applications/Xcode.app/Contents/PlugIns/Xcode3Core.ideplugin/Contents/Frameworks
	-framework DevToolsFoundation
	-framework DevToolsSupport
	-framework DVTSourceControl
	-framework IDEFoundation
	-framework DVTFoundation
	-framework IBFoundation
	-framework IBAutolayoutFoundation

…

import DevToolsCore
```

---

# DEMO

---

## Cons of **Semantic Versioning**

- Can't submit to iTunes

---

# Nightly Build

☑ **Versioning**

☑︎ **Integration**

☐ Commit & Tag

☐ Distribute

---

# [fit] *Automate*
## Code Generation

^ Example: mogenerator

---

Automating your development process with Swift

---

# Agenda

- When is swift the right tool for the right job?
 - Hook into your build processes:
  - Development process
  - Continuous integration process
  	- Beta/App Store Release
- Xcode Plugin

---

No matter what, our grand master is Xcode.

---

# Tools I loved

- mogenerator (objective-c)
- cocoapods (ruby)
- catherage (swift)

---

# Custom Tools

- Generates enum/extension from asset catalog.
- Alert slack of integration result.
- Automating versioning versioning.
- mogenerator.

---

# Automating versioning

---

## Existing tool

- agvtool


---

# Automating versioning
## Version Bump

- 2 type of version:
 - Version (CFBundleShortVersionString): Marketing
 - Build (CFBundleVersion): Internal

## Apple Answer

 - Technical Q&A QA1827

- agvtool
 - Current Project Version (CURRENT_PROJECT_VERSION): Integer or floating point number
 - Versioning System (VERSIONING_SYSTEM): None or Apple Generic

---

## Why it suck

- No specification of Apple Genric versioning. (man agvtool)
- No auto figuring out of next marketing version.
- No controling of next version granularity.
- No GIT support (CSV and SVN only) 

---

## TestFlight

- Waiting for beta review is a **DUMB** idea
- No support for semver

---

## When to use agvtool

- If you follows the Apple Generic versioning (Whatever it might be)
- You are an Apple employee
- If you like to populate your project with more settings.

---

## Write your own!

- Semantic Versioning 2 (At least it has a specification)
- Find Xcode Project
- Bump version

[Graph showing the possible path from current version to next] 

---

## #!/usr/bin/swift
### Scripting in Swift

---

## Continuous Integration

- Triggers
- Xcode Server API

## http://www.robertvojta.com/2015/06/30/swift-hidden-options/

## google search: swift module map

---

https://xkcd.com/1319/

^ Incomplete without an xkcd reference

---

![fit](https://imgs.xkcd.com/comics/the_general_problem.png)

---

![fit](https://imgs.xkcd.com/comics/the_general_problem.png)
## Worth it?

^ Might not be worth it.

---

# Overview
This project is a magical modification of the https://github.com/snailuncle/webpack-autojs project.

The goal of this project is to make a development kit for the [autoxjs](https://github.com/kkevsekk1/AutoX) (new open source autojs) project, autox-cli   
Meet engineering (away from slash-and-burn): automate the management of a js class library, automate the source code, compile, obfuscate, dex encrypt, package, deploy, and let developers focus on writing business.   
Of course, for engineering, you need to have some basic knowledge of nodejs development. So far, cli and dex encryption have not been encapsulated, but they have been implemented in addition to that. 
So you will see a lot of files at the beginning, don't be afraid, just care about the few files mentioned in the documentation below.   
The project directly converts the compiled js to class to dex without automation.   
(Unless a developed feature to license and so on, in other cases there is no need to convert to dex. to charge a license, and autojs design of the original intention and open source agreement are deviated.)   
The next step will mainly solve this problem. Welcome interested fork projects to implement together.   

[Youku video explanation](https://v.youku.com/v_show/id_XNDg2NjA3NTYyMA==.html)  

## Project feature list
- [x] js source code auto-compile, obfuscate, package, deploy mobile, re-run
- [x] vscode auto-tip methods, and instructions, there is a part to be improved, welcome pull code
- [x] compiled js package into dex
- [x] Multi-project management
- [x] auto-identify ui module, source file is "ui"; start then, compile also, source file is not, compile also not
- [x] Add multi-module compilation, a project consists of multiple scripts, compiled as different files, can call each other

# How to use
1. you need to install nodejs, please note that the installation process to [ add node to PATH ] and install npm both options should be checked. (General front-end engineers have this link)
2. install [vscode](https://code.visualstudio.com/) and install autoxjs development plugin that is: [Auto.js-VSCodeExt-Fixed](https://marketplace.visualstudio.com/items? itemName=aaroncheng.auto-js-vsce-fixed) Note that it is version 0.3.11 or above. (ctr+ shift+p select autojs to start the service)
3. install global installation of webpack: ``` npm i -g webpack webpack-cli --registry=https://registry.npm.taobao.org ```
4. [download this project](https://github.com/kkevsekk1/webpack-autojs/archive/master.zip) or git clone project ``` git clone https://github.com/kkevsekk1/ webpack-autojs.git ``` 5. 
    
5. cmd to the project and run the command to install the dependencies
    ```npm install --registry=https://registry.npm.taobao.org ```

6. to this basically you can say that the development environment is complete, (you also have to install autoxjs), the following said the project configuration file and the form of development.

# project development, compilation, packaging, deployment introduction

work directory: This is the general directory of our project, that is, each folder in this is an autoxjs project. For example, we have demo,demo1,dy that is 3 projects. 2. 
2. scriptConfig.js file: how we want to compile the project that is configured in this file, open the file, there are comments can be changed in accordance with the comments.
3. header.txt irrelevant file, the contents of which will be added to the compiled js code header as is
4. adjust the above 3 content will be available to compile our project
5. package.json This file specifies Look at lines 6-9, there are two commands start and build corresponding to the development environment and build environment compilation respectively, no need to modify. Just know that they correspond to npm run start and npm run build, respectively. 6.
6. run ``npm run start ``` that is, the development environment, not every time you modify the code, the code will automatically compile, and scriptConfig.js in the wath configuration to 'rerun' or 'deploy' then the code will automatically run in the phone or automatically save the recompiled project to the phone.
7. dist directory: If you run the above compile command (start or build), you will have the compiled result in the dist directory, and each directory in this directory represents a compiled autoxjs project. The name of the compiled directory can be configured with a prefix to distinguish it from the pre-compiled project (necessary when they are all kept in hand as projects).
8. ``` npm run start ``` This

# Compile dex
1. source for [use tool](https://github.com/molysama/auto.pro/wiki/dex). I use this tool to package and don't plan to build the wheel repeatedly
2. [Install jre](https://www.baidu.com/s?ie=UTF-8&wd=jre)   
3. install auto-cli ``` npm i "@auto.pro/cli" -g ```
4. Run the compile command ``` auto-cli dex . /dist/demo/main.js ``` 
5. if due to a willingness to write a webpapck plugin to execute a few commands here to automate the willingness to pull code, I have no intention of compiling my code to dex and then reinforcing it, so no incentive to implement this plugin!

# Other notes 1. 
1. the main configuration file is a `scriptConfig.js` 2.
2. `header.txt` The contents of this file will be added to the header of the packaged file, which is empty by default. 3.
3. `uiMode` true: ui mode, false non-ui mode
4. `base64` whether the webpack is base64 encoded after packaging
5. `base64RandomStrLength` The length of the random character added in front of the string after base64 encoding
6. common This directory is intended to be placed in the public generic package, the page hopes to have to use the closed package specification of the autoxjs lib can provide, I know a called [autojs_sdk](https://github.com/kangour/autojs_sdk) okay, but this package is still not enough, I solve can also have Better. 7.
7. plugin This directory is for custom webpack plugins, for compilation and automation, so you don't need to care.

# Other notes 2. 1.
1. There are currently four supported ui's, ` ui.layout, ui.inflate, floaty.rawWindow, floaty.window `. 2.
2. If layoutContent is a string variable instead of xml, you can try to define `floaty.window` as `floatyWindow` and the other ` ui.layout, ui.inflate, floaty.rawWindow, floaty.window ` as well: ` ui.layout, ui.inflate, floaty.rawWindow, floaty.window are also the same.
```
let floatyWindow = floaty.window.
var w = floatyWindow(layoutContent).

```
3. the loader file is `node_modules\webpack-autojs-loader\index.js`

# Other notes 3.
1. webview packaging recommendation: `https://github.com/molysama/auto.pro` 2.
2. require can only use relative paths for webpack to package properly
3. if you use absolute path, please use global.require instead, such as global.require("D:\\module1.js")

# Common errors
1. xml use round brackets, because loader use regular match, round brackets will affect regular, please use Chinese brackets
2. require uses absolute path, please avoid using absolute path. 3.
3. If require uses absolute path, webpack can't find it. webpack is a tool for computer, not for cell phone, it can't find /sdcard, please use global.require instead.
4. If there is a list in xml, please don't omit this, because the loader regular will distinguish whether the {{}} in xml has this or not, to handle it differently
5. variables are not defined, please note that all variables should be defined first, and then used; pay attention to js variable lifting, which can lead to runtime errors after packaging. 6.
6. if used without definition, please add global. the 7 characters in front of the variable, can avoid some variable lifting errors
7. If importing jar or dex, using java object, it is recommended to use the full name of the object directly, such as `var url = new java.net.URL(myUrl);`, instead of `var url = new URL(myUrl);`
## js to dex, you can refer to this repository
[batchJs2Dex](https://github.com/snailuncle/batchJs2Dex)

## execute autojs script in so, see this repository
[autojsNativeJs](https://github.com/snailuncle/autojsNativeJs)

## Build applications using webview, etc.
[auto.pro](https://github.com/molysama/auto.pro) can ts to write scripts to build the application. If you want the project application to automatically deploy to cell phones and other features, please configure webpack.config.js yourself

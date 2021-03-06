<img align="center" src="meta/colt_logo.png" >

# Colt - COmmand Line Translation
Colt is a command line tool that allows a developer to automate strings translations for localization. It will parse your existing file(s), translate each string to your desired language, and save them into a new file. This can be a tool for developers who do not have the resources to hire a translator, yet want to make their apps more accessible. Or, for those simply curious at how their app would look in a different language.

# Pre-Installation Instructions
### RapidAPI/SYSTRAN.io Api Key
It took some searching to find a free translation api with minimal sign up. I settled on using the SYSTRAN.io api through RapidAPI, which can be found [here on their website](https://rapidapi.com/systran/api/systran-io-translation-and-nlp). To use Colt, you must register to get a key. Click the "Sign Up" button, and after completing registration you'll see that the `X-RapidAPI-Key` value has been populated on the api page. That is the api key you will use in the following step.

### .colt File
Colt will look for the SYSTRAN.io api key in a hidden file in your user's home directory. Open any text editor and enter the following, inserting your api key value in place of `123456789abcdefg`, and save with the name `.colt` in your user's home directory (Users/yourusername/). You may be prompted that _"Names that begin with a dot are reserved for the system."_ but still use this format. It just means that your file will be hidden. If you need to find this file in the future, you can opt to show your hidden files by typing `Command + Shift + Dot`.
```
[keys]
	rapidapi = 123456789abcdefg
```

# Installation
_NOTE: These instructions will create a release build of this project on your machine so that you can use Colt from any directory._

- Clone this repo
- Open Terminal and run the following commands:
```
cd [path to colt directory]
swift build --configuration release
cp .build/release/colt /usr/local/bin/colt
```

# Get Started
Your files must be in the format used for .strings files. In this format, keys and values are separated by an equal sign, and each key-value pair is terminated with a semicolon. The value is optional—if not present, the equal sign is also omitted. The keys and values themselves are always strings enclosed in straight quotation marks. Comments may be included, delimited by `/*` and `*/`, or `//`. 

To start Colt, run the following in Terminal:
```
colt [sourceLanguageCode] [translationLanguageCode] [pathToFileOrDirectory]
```
Which would look something like:
```
colt en es /Users/angiesanders/Documents/WORK/MyProject
```


Language codes will be used for both `sl` (source language) and `tl` (translation language) arguments. For codes and supported language pairs, please refer to the SYSTRAN.io section below. 

The path argument can be used two different ways:

1. Point to a **directory**, and Colt will enumerate through all the files and translate all strings files that have the format of `[sourceLanguage].proj/[fileName].strings` (the Xcode default). It will not find the source language file if it is in the `Base.lproj` folder. If there is a file that you would like for Colt to ignore, just add `coltIgnore` anywhere in your file name. This can be useful for things like text styles or html that doesn't need translation. Or, if you'd made any changes to the translated file that you don't want to be written over.

2. Point to a **single file**, and as long as it's in the correct strings file format, it will be translated. It does not need to have the `.strings` extension. 

### Optional Arguments
There is an optional output path argument (`-o` or `--outputPath`), where you can set the destination for your translated strings file(s) to be saved. This can be used as following:

```
colt [sourceLanguage] [translationLanguage] [pathToFileOrDirectory] -o [outputPath]
```

_Warning: If you set an output path while enumerating through a project, and there are multiple source language .strings files with the same name in separate folders, they will overwrite each other in the output folder. I will be preventing this in my post-MVP version._ 

### Requirements
Let's repeat these again for the people in the back...

1. Set up the api key properly.
2. Your files must be in the format used for .strings files.
3. If enumerating through a project, directory format must be `[sourceLanguage].proj/[fileName].strings`.
4. If translating a single file, parent folder and extension doesn’t matter.
5. If you specify an output folder and enumerate through a project, those .strings files must have unique names. (Can’t be Localizable.strings in multiple folders of the same language)

### SYSTRAN.io
Colt uses the SYSTRAN.io api for translation. A list of supported language pairs can be found here, under the Machine Translation section. https://platform.systran.net/index (FYI, if wanting to translate to Chinese, you must use the code "zh-Hans" with a capital H. It is not listed like this in the pairs supported pairs list.)

There is a disclaimer of "SYSTRAN Platform is free for small volumes and testing purposes, monthly subscriptions are available for higher volumes", however with all my testing I never hit this limit. I'm banking on your project not including Les Misérables (although maybe it should).

# License

MIT License

Copyright (c) 2020 Angie Sanders

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.

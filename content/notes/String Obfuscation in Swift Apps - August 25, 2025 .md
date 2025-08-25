---
title: "String Obfuscation in Swift Apps"
date: "2025-08-25"
---

In your app, you may run into times where you want to obfuscate the content of sensitive strings. In the case of an app that leverages AI by making either API calls to a provider like OpenAI or Anthropic, or when running models locally; you may have stored the prompts for interacting with the models as strings on the device. In this case, you may want to ensure that a competitor can’t figure out the text of the prompts you use.	

The Apple AppStore does take some precautions against this by encrypting uploaded binaries for any apps distributed through the store. 

However, if you are distributing your app outside of the App Store (in the case of downloading a MacOS app from the internet), that encryption won’t be present. You’ll want to take some extra steps to ensure that it’s not possible for users to decompile your apps to read the content of sensitive strings. 

There are a couple of steps you can take in this case:

1. Xor encoding
2. Base 64
3. Breaking up strings into parts
4. Keeping the parts of the string in separate files
5. Using vague names for these variable names / files



There are a number of Swift packages that can be used to help further obfuscate your strings as well: 

- [SwiftConfidential](https://github.com/securevale/swift-confidential)
- [Swift-String-Obfuscator](https://github.com/pykaso/Swift-String-Obfuscator)
- [techprimate/TPObfuscation: Toolbox to obfuscate Strings in a Swift Project](https://github.com/techprimate/TPObfuscation)



There’s no gurantee that a determined hacker won’t be able to get access to your prompts, keys, or other sensitive strings; but, you can take practical steps to lower that likelihood. 




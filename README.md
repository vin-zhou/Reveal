# Reveal调试配置

1. 复制 RevealServerCommands.py 到  /Applications/Reveal.app/Contents/SharedSupport/Scripts/
2. 在~/.lldbinit 中添加：
```
### Reveal LLDB commands support - DO NOT MODIFY
command script import /Applications/Reveal.app/Contents/SharedSupport/Scripts/RevealServerCommands.py
###
```
3. 在工程的Build Phases中添加RunScript:
```
EAL_APP_PATH=$(mdfind kMDItemCFBundleIdentifier="com.ittybittyapps.Reveal2" | head -n 1)
BUILD_SCRIPT_PATH="${REVEAL_APP_PATH}/Contents/SharedSupport/Scripts/reveal_server_build_phase.sh"
if [ "${REVEAL_APP_PATH}" -a -e "${BUILD_SCRIPT_PATH}" ]; then
"${BUILD_SCRIPT_PATH}"
else
echo "Reveal Server not loaded: Cannot find a compatible Reveal app."
fi
```
4. 在工程中添加断点 Symbolic BreakPoint: UIApplicationMain,

* 对于XCode10以下或Reveal 15以上的，debug commands:  ```reveal load```
* 否则，debug commands： 
```expr (Class)NSClassFromString(@"IBARevealLoader") == nil ? (void *)dlopen("/Applications/Reveal.app/Contents/SharedSupport/iOS-Libraries/RevealServer.framework/RevealServer", 0x2) : ((void*)0)```

## 参考：

* http://support.revealapp.com/kb/getting-started/load-the-reveal-server-via-an-xcode-breakpoint
* http://support.revealapp.com/kb/known-issues/breakpoint-integration-of-reveal-15-and-older-is-not-compatible-with-xcode-10

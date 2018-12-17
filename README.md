# Reveal调试配置

<<<<<<< HEAD
首先从 https://github.com/vin-zhou/Reveal 中， 下载
RevealServerCommands.py 和 reveal_server_build_phase.sh 到本地  /Applications/Reveal.app/Contents/SharedSupport/Scripts/ 文件夹下。（若无Scripts文件夹，则新建之）

然后：

1. 在~/.lldbinit 中添加：
=======
1. 复制 RevealServerCommands.py 到  /Applications/Reveal.app/Contents/SharedSupport/Scripts/
2. 在~/.lldbinit 中添加：
>>>>>>> 48bdf94d115a06c009471e68eebcec107a3fc3f3
```
### Reveal LLDB commands support
command script import /Applications/Reveal.app/Contents/SharedSupport/Scripts/RevealServerCommands.py
###
```
2. 在工程的Build Phases中添加RunScript:
```
REVEAL_APP_PATH=$(mdfind kMDItemCFBundleIdentifier="com.ittybittyapps.Reveal2" | head -n 1)
BUILD_SCRIPT_PATH="${REVEAL_APP_PATH}/Contents/SharedSupport/Scripts/reveal_server_build_phase.sh"
if [ "${REVEAL_APP_PATH}" -a -e "${BUILD_SCRIPT_PATH}" ]; then
"${BUILD_SCRIPT_PATH}"
else
echo "Reveal Server not loaded: Cannot find a compatible Reveal app."
fi
```
3. 在工程中添加断点 Symbolic BreakPoint: UIApplicationMain,

* debug commands:  ```reveal load```


若xcode执行出现permissi denied错误，则cd 到Scripts文件夹，
执行
```
chmod 775 reveal_server_build_phase.sh
chmod 775 RevealServerCommands.py
```

## 参考：

* http://support.revealapp.com/kb/getting-started/load-the-reveal-server-via-an-xcode-breakpoint
* http://support.revealapp.com/kb/known-issues/breakpoint-integration-of-reveal-15-and-older-is-not-compatible-with-xcode-10

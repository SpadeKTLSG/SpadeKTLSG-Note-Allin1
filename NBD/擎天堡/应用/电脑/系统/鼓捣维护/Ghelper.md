[华硕Ghelper拷打工具](https://github.com/seerge/g-helper/blob/main/docs/README.zh-CN.md)

#### 显卡设置模式

1. 集显模式 : 只启用低功耗的内置显卡, 核显连接笔电内置屏幕
2. 标准模式 (MS Hybrid) : 同时启用核显与独显, 核显连接笔电内置屏幕
3. 独显直连: 同时启用核显与独显, 但独显直连笔电屏幕 (仅在幻14 2022版等机型上支持)
4. 自动切换: 使用电池时关闭独显(集显模式)，并在插上电源后重新启用独显(混合输出)

‍

#### 自定义热键行为

软件支持热键自定义配置. 如要设置，在按键旁的选项框中选择"自定义设置"，然后执行下面的操作(任选其一):

1. 要想运行任意应用 - 向 "action" 文本框中粘贴应用文件exe的完整路径，例如: `C:\Program Files\EA Games\Battlefield 2042\BF2042.exe`​
2. 要想模拟任意windows按键 - 向"action"文本框中粘贴相对应的 keycode，例如 `0x2C`​ 为屏幕截图键.  Keycodes的完整列表: [https://learn.microsoft.com/en-us/windows/win32/inputdev/virtual-key-codes](https://learn.microsoft.com/en-us/windows/win32/inputdev/virtual-key-codes)

‍

#### 按键绑定

* ​`Fn + F5 / Fn + Shift + F5`​ - 向前/向后切换性能模式
* ​`Ctrl + Shift + F5 / Ctrl + Shift + Alt + F5`​ - 向前/向后切换性能模式
* ​`Ctrl + Shift + F12`​ - 打开G-Helper窗口
* ​`Ctrl + M1 / M2`​ - 屏幕亮度调低/调高
* ​`Shift + M1 / M2`​ - 键盘背光亮度调低/调高
* ​`Fn + C`​ - Fn锁定
* ​`Fn + Shift + F7 / F8`​ - 光显矩阵/光线矩阵亮度调低/调高
* ​`Fn + Shift + F7 / F8`​ - 屏幕亮度调低/调高
* ​`Ctrl + Shift + F20`​ - 麦克风静音
* ​`Ctrl + Shift + Alt + F14`​ - 集显模式
* ​`Ctrl + Shift + Alt + F15`​ - 标准模式
* ​`Ctrl + Shift + Alt + F16`​ - 静音模式
* ​`Ctrl + Shift + Alt + F17`​ - 平衡模式
* ​`Ctrl + Shift + Alt + F18`​ - 增强模式
* ​`Ctrl + Shift + Alt + F19`​ - 自定义 1（如果存在）
* ​`Ctrl + Shift + Alt + F20`​ - 自定义 2（如果存在）
* [自定义键绑定/热键](https://github.com/seerge/g-helper/wiki/Power-user-settings#custom-hotkey-actions)

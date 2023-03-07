# windows主题切换快捷

我不太懂shell编程和exe。在Chat GPT的帮助下，实现了一个这个，解决了我的需求。专业人士勿笑😄。

## 创建一个shell文件

方法1：直接sublimtetx之类的创建 alternative.ps1

方法2： 记事本txt。但是改文件名它还是txt。这时候终端里

```jsx
Rename-Item "light.txt" "light.ps1”
```

## 写内容

chatgpt给我的是if，但是就是else那里不走，不知道为什么。让他给我换switch，就好了。

```jsx
$theme = Get-ItemPropertyValue -Path "HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Themes\Personalize" -Name "AppsUseLightTheme"
switch ($theme) {
    0 {  # 当前为暗色模式，切换到亮色模式
        Set-ItemProperty -Path "HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Themes\Personalize" -Name "AppsUseLightTheme" -Value 1
        Set-ItemProperty -Path "HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Themes\Personalize" -Name "SystemUsesLightTheme" -Value 1
    }
    1 {  # 当前为亮色模式，切换到暗色模式
        Set-ItemProperty -Path "HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Themes\Personalize" -Name "AppsUseLightTheme" -Value 0
        Set-ItemProperty -Path "HKCU:\SOFTWARE\Microsoft\Windows\CurrentVersion\Themes\Personalize" -Name "SystemUsesLightTheme" -Value 0
    }
    default {  # 如果获取的值不为 0 或 1，则输出错误信息
        Write-Host "无法切换主题模式，当前值为 $theme"
    }
}
```

## 编译成exe，并加图标，并且让不弹窗

为了更方便的切换，像mac那样。找了个icon格式图标（[7,250,000+ free and premium vector icons, illustrations and 3D illustrations](https://www.iconfinder.com/)）

问GPT，用ps2exe编译shell到exe，主要是因为shell没法固定到任务栏

[如何使用PS2EXE将PowerShell脚本编译为可执行程序](https://www.notion.so/PS2EXE-PowerShell-843fc0ba0da54fbab2d45e6c2a9e510d) 

安装好后，终端输入

```jsx
Invoke-ps2exe alternative.ps1  theme.exe  -iconFile C:\Users\zheyong\Desktop\a.ico -noConsole
```

拖到任务栏就好了

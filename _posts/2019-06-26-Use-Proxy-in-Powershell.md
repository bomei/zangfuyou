---
layout: article
title:  "在powershell中使用代理"
excerpt: "我的Powershell终于用上代理了！"
date:   2019-06-26 23:01:04 +0800
categories: blog
tags: [code, shell]
---

### 我的Powershell终于用上代理了！

在Ubuntu中我`export all_proxy={proxy}`就可以使用代理了，但是在windows下，无论是powershell还是cmd这样子都不管用，终于找到了下面的powershell代码解决了这个便秘的问题。代码来自于[使用PowerShell Profile快速设置 HTTP 代理](https://async.sh/2018/07/30/quick-setup-http-proxy-using-powershell-profile/)。

* 首先你要有一个http的代理，不能是socks5的。

* 然后找到你powershell配置文件放置的地方，我的是`{User}\Documents\WindowsPowerShell\Microsoft.PowerShell_profile.ps1`，打开之后在最后把下面这些代码放进去：
  ```powershell
  # Set-Proxy command
  Function SetProxy() {
      Param(
          $Addr = $null,
          [switch]$ApplyToSystem
      )
      
      $env:HTTP_PROXY = $Addr;
      $env:HTTPS_PROXY = $Addr; 
      $env:http_proxy = $Addr;
      $env:https_proxy = $Addr;
    
      if ($addr -eq $null) {
          [Net.WebRequest]::DefaultWebProxy = New-Object Net.WebProxy;
          if ($ApplyToSystem) { SetSystemProxy $null; }
          Write-Output "Successful unset all proxy variable";
      }
      else {
          [Net.WebRequest]::DefaultWebProxy = New-Object Net.WebProxy $Addr;
          if ($ApplyToSystem) {
              $matchedResult = ValidHttpProxyFormat $Addr;
              # Matched result: [URL Without Protocol][Input String]
              if (-not ($matchedResult -eq $null)) {
                  SetSystemProxy $matchedResult.1;
              }
          }
          Write-Output "Successful set proxy as $Addr";
      }
  }
  Function SetSystemProxy($Addr = $null) {
      Write-Output $Addr
      $proxyReg = "HKCU:\Software\Microsoft\Windows\CurrentVersion\Internet Settings";
  
      if ($Addr -eq $null) {
          Set-ItemProperty -Path $proxyReg -Name ProxyEnable -Value 0;
          return;
      }
      Set-ItemProperty -Path $proxyReg -Name ProxyServer -Value $Addr;
      Set-ItemProperty -Path $proxyReg -Name ProxyEnable -Value 1;
  }
  Function ValidHttpProxyFormat ($Addr) {
      $regex = "(?:https?:\/\/)(\w+(?:.\w+)*(?::\d+)?)";
      $result = $Addr -match $regex;
      if ($result -eq $false) {
          throw [System.ArgumentException]"The input $Addr is not a valid HTTP proxy URI.";
      }
  
      return $Matches;
  }
  Set-Alias set-proxy SetProxy
  ```

* 之后重新开一个powershell窗口，在窗口中输入：
  ```powershell
  set-proxy '{your-http-proxy}'
  curl ifconfig.me
  # remote.proxy.ip.address
  ```

**成功！**

---
title:  パッケージ管理ツールの Chocolatey をインストール
date: 2013-07-03T00:52:28+09:00
slug: 84ea6b98d4356c5de5c75ca1afdc591a6754adb1
---

対応しているパッケージが多そうなので試してみることに。  
最近は chef に興味があって、連携できないかとか考えてみたり。  

[Chocolatey Gallery](http://chocolatey.org/ "Chocolatey Gallery")

インストールは簡単で、コマンド一つでインストールが完了。  
一緒に PATH を通してくれるため、環境変数の設定は必要ない。

```
@powershell -NoProfile -ExecutionPolicy unrestricted -Command "iex ((new-object net.webclient).DownloadString('https://chocolatey.org/install.ps1'))" && SET PATH=%PATH%;%systemdrive%\chocolatey\bin
```

以下のディレクトリにインストールされる。  
引数などでインストール先を変えられるかわかるかはわからない。  
```
C:\Chocolatey
```

インストールできるパッケージはこちらに公開されているようで、今現在だと 1088 個のパッケージに対応している。
[Chocolatey Gallery | Packages](http://chocolatey.org/packages "Chocolatey Gallery | Packages")

Power Shell で書かれているんだな。当然といえば当然か。

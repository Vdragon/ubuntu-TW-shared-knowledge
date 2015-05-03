# 要如何避免 vesafb 作業系統核心模組自動載入呢？
## 問題來源
[林博仁](https://www.facebook.com/buo.ren.lin.taiwan) 於 [Ubuntu 正體中文社團](https://www.facebook.com/groups/ubuntu.zh.hant)發表的[貼文](https://www.facebook.com/groups/ubuntu.zh.hant/883956151659708/)

## 問題內容
### 問題描述
我想要使用 uvesafb 來當作 NVIDIA 專有驅動下的 framebuffer 驅動，做了這些事情：

1. 我修改了 `/etc/default/grub` 檔案，在 `GRUB_CMDLINE_LINUX_DEFAULT` 設定中加上了 `video=uvesafb:scroll:ywrap,mtrr:3,mode_option:1280x720-32`，然後再以 root 身份執行 update-grub 重新產生 /boot/grub/grub.cfg
2. 我於 `/etc/modprobe.d/blacklist-framebuffer.conf` 設定檔中加上了 `blacklist vesafb` 將 vesafb 模組加入黑名單
3. 我於 `/etc/initramfs-tools/modules` 檔案中加上了 `uvesafb` 列，然後以 root 身份執行 `update-initramfs -k all -uv` 重新產生 initramfs（執行結果：http://pastebin.com/index/7Mcvn0WY ）
4. 我修改了 `/etc/default/grub` 檔案，在 `GRUB_CMDLINE_LINUX_DEFAULT` 設定中加上了 `modprobe.blacklist=vesafb`，然後再以 root 身份執行 update-grub 重新產生 /boot/grub/grub.cfg
5. 我安裝了 uvesafb 依賴的 v86d 軟體（# apt install v86d）
6. 重新啟動系統

但是 vesafb 還是早 uvesafb 一步被載入到 Linux 作業系統核心中（作業系統核心紀錄檔：http://pastebin.com/SH23Mkar ），要怎麼樣才能避免 vesafb 模組的載入呢？

### 作業系統散佈版名稱、版本與相容的中央處理器架構
Ubuntu 14.04 i386

### （如果是硬體問題的話）硬體的型號與連接介面
NVIDIA GTS-250 via PCI Express

### 可以重現問題的頻率（有時候、常常、每次都會）
每次都會

### 可以提供的協助（比方說遠端控制等，讓別人可以連進去的方法）
目前沒有

### 可能有關或您建檔的軟體缺陷報告網址（請到 https://bugs.launchpad.net/ubuntu/ 搜尋）
* [Bug #87158 “vesafb automatically loaded while blacklisted” : Bugs : initramfs-tools package : Ubuntu](https://bugs.launchpad.net/ubuntu/+source/initramfs-tools/+bug/87158)

## 解答
因為 Ubuntu 提供的 Linux 作業系統核心把 vesafb FrameBuffer 裝置驅動程式靜態地編譯到核心中所以在沒有 kernel mode settings 顯示驅動下沒辦法使用其他的 FrameBuffer 實作

## 參考資料
* [BootGraphicsArchitecture - Ubuntu Wiki](https://wiki.ubuntu.com/BootGraphicsArchitecture)
* [/usr/share/initramfs-tools/scripts/init-top/framebuffer](file:///usr/share/initramfs-tools/scripts/init-top/framebuffer)

## 特別感謝<br />Credits
感謝[林耕宇](https://www.facebook.com/kengyu.lin)指點問題所在位置

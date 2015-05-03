# 關於外接硬碟的 UEFI 開機問題
## 問題來源
[Jia-ming Ma](https://www.facebook.com/gai00layer) 於 [Ubuntu 正體中文社團](https://www.facebook.com/groups/ubuntu.zh.hant)發表的[貼文](https://www.facebook.com/groups/ubuntu.zh.hant/883956151659708/)

## 問題內容
````````````````````````````````
我最近遇到一個我不太了解的問題。
1. 我安裝了Ubuntu 14.04.2 LTS 到我的隨身硬碟。
2. 切割為
sdx1:EFIboot - 512MB
sdx2:ext4 - 200GB 給 /
sdx3:ext4 - 600GB 給 /home
sdx4:swap - 20GB
3. 通常裝好可以正常開機，也可以用一段時間，但是我USB拔出來以後，有機率的完全進不了。就是一個底線在那邊閃爍這樣。
4. 用LiveUSB可以看到分區沒有問題，資料也都在，EFI那一槽我沒辦法確認。
我在想，會不會是我需要額外做什麼處理，這隨身Ubuntu才能隨插隨用 =x=?
PS: 本機的硬碟win7好像因為被自動安裝grub，現在準備重灌...win7安裝片好像對SSD的硬碟支援度不夠。
PS2: 上述狀況在本機的硬碟全部拔除，仍可能發生。
````````````````````````````````

## 解答
我覺得問題出在您的 Grub EFI 安裝上

* 剛安裝完可以正常運行可能是因為 Ubuntu 安裝程式把 Grub 開機載入程式安裝到您的 Windows 所在硬碟的 EFI 系統分割區(ESP)（而非隨身硬碟的 EFIboot 磁碟分割區）的緣故
* 因為 UEFI 「沒有 bootloader 被蓋掉的問題」所以您不需要重灌 Windows 7，直接在 BIOS 設定介面中把預設執行的 bootloader 切回 Microsoft 的實作即可
* USB 外接硬碟要能夠自 UEFI BIOS 啟動應將 bootloader EFI 應用程式安裝到 EFI 系統分割區(ESP)底下的 `EFI/BOOT` 目錄底下，您可以於 Ubuntu 安裝媒體 Live 系統中先掛載 EFIboot 檔案系統再以 root 身份執行下列命令安裝：
	* AMD64（x86 64 位元）處理器架構的 UEFI 軔體
		* \# `grub-install --target=x86_64-efi --uefi-secure-boot  --removable --verbose --efi-directory=〈EFIboot 檔案系統的根目錄〉` 
	* intel x86（32 位元）處理器架構 UEFI 軔體（極少硬體是這個）
		* \# `grub-install --target=i386-efi --uefi-secure-boot  --removable --verbose --efi-directory=〈EFIboot 檔案系統的根目錄〉` 

## 問題排解
### 以 root 身份執行的 grub-install 無法存取 `/media/<username>/<filesystem_id>` 目錄
這是因為 Udisks 設定進階 ACL 權限設定限制使用者自己掛載的檔案系統只有使用者自己能存取的緣故。

## 參考資料
* grub-install(8) 的 manpage 格式說明文件

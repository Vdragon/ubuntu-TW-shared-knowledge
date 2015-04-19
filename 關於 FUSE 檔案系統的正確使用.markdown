# 關於 FUSE 檔案系統的正確使用
## 問題來源
[JuHo Dan](https://www.facebook.com/juho.dan) 於 [Ubuntu 正體中文社團](https://www.facebook.com/groups/ubuntu.zh.hant)發表的[貼文](https://www.facebook.com/groups/ubuntu.zh.hant/permalink/880120592043264/)

## 問題內容
`````
各位大大 安安
小弟最近遇到一個問題
一直不知道怎麼解決
-
大意是這樣的
我在ubuntu 用指令
sudo curlftpfs user:password@192.168.0.100 /mnt/tmp_dir
成功的把這個FTP系統掛載到 /mnt/tmp_dir
然後就遇到問題了
-
狀況
我使用指令
sudo du -a
或是 linux 執行例行工作 updatedb.mlocate
掃描檔案系統時
只要碰到路徑中含有 # (井號) 的時候
就會卡在那邊
-
我從FTP的host端會看到他一直讀這個資料夾
徒增系統跟硬碟的負擔
-
問題
除了把資料夾或檔案重新命名 避免 # (井號)
一般有其他方法迴避或解決這種問題嗎?
-
備註
這種出現 # (井號) 的路徑是windows的檔案系統
目前我有必要把它透過FTP放出來
而且不太可能修改檔名
-
結果還打得挺長的
謝謝各位有耐心看到這邊 smile 表情符號
`````

## 解答
> sudo curlftpfs user:password@192.168.0.100 /mnt/tmp_dir

* 請改用 `-o user=<username>` 取代 `user:password@host` 的用法，避免密碼紀錄在殼程式(shell)的執行命令歷史中

* 請避免以 root 身份掛載 FUSE 檔案系統，因為用不到（引用 `mount.fuse`(8) 的 manpage 格式使用說明：FUSE also aims to provide a secure method for *non privileged users* to create and mount their own filesystem implementations.）

* 要讓 root 或是其他使用者可以存取 FUSE 檔案系統可以參考 `mount.fuse`(8) 的 manpage 格式使用說明修改 /etc/fuse.conf 

* 以普通使用者身份掛載的 FUSE 檔案系統預設禁止其他身份存取，應能避免 `du` 與 `updatedb.mlocate` 存取造成無謂的資源損耗

## 參考資料
* mount.fuse(8) 的 manpage 格式說明文件


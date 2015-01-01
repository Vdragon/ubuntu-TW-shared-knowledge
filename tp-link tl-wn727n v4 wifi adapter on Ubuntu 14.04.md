# tp-link tl-wn727n v4 wifi adapter on Ubuntu 14.04
## 問題（警告：未獲得授權之資源）<br>Issue(Warning: unlicensed resource)
![tp-link tl-wn727n v4 wifi adapter on Ubuntu 14.04 question](Resources/Pictures/tp-link%20tl-wn727n%20v4%20wifi%20adapter%20on%20Ubuntu%2014.04%20question.png)

### 問題來源<br />Issue source
[王詠然](https://www.facebook.com/profile.php?id=100008367465845)於[Ubuntu 正體中文社團](https://www.facebook.com/groups/ubuntu.zh.hant/806597276062263)發問的問題

## 答案<br>Answer
在產品廠商(TP-LINK)不提供 GNU/Linux 平台驅動的情況下， GNU/Linux 作業系統能不能支援硬體跟廠牌型號比較沒關係，關鍵是在它的控制器晶片有沒有被 Linux 作業系統核心所支援
很不幸地該產品的規格資料表(datasheet)[1]跟使用手冊[2]中沒有提及 Wifi 控制器晶片的型號，請跟廠商抱怨無法方便查詢其硬體在 Linux 平台上的相容性造成消費者採購上的困擾
使用「tl-wn727n v4 linux」搜尋關鍵字的第一個搜尋結果[3]可以得知該型號使用的 Wifi 控制器晶片型號為聯發科(MediaTek)的 MT7601U（廠商識別編號(VID)為 148f、產品識別編號(PID)為 7601），很不幸地目前地 Linux 作業系統核心不支援此晶片，但是晶片製造商有釋出 Linux 平台的驅動程式來源程式碼[4]，只要 Ubuntu 14.04 提供的 Linux 作業系統核心版本的程式介面與該驅動程式相容即可成功建構並安裝該驅動。
再次很不幸地該來源程式碼與 Ubuntu 14.04 提供的 Linux 作業系統核心版本的程式介面不相容因此無法建構驅動程式，將編譯器的錯誤訊息(os/linux/rt_linux.c:1122:20: error: incompatible types when assigning to type ‘int’ from type ‘kgid_t’)拿去搜尋的第二個搜尋結果[5]有人釋出了修正建構問題的修正檔(patch file)，將修正檔套用到來源程式碼上即可成功建構驅動程式。

## 注意<br>Note
Linux 作業系統核心來源程式碼樹外的驅動程式當系統的 Linux 作業系統核心升級之後會失效，需再重新建構並安裝才能恢復。

## 微不足道的小細節<br>Trivials
事實上 TP-LINK TL-WN727N 的前幾代因使用了別的 Wifi 控制器晶片在 Linux 底下是支援的。

## 參考資料<br>Reference data
[1]<http://www.tp-link.tw/resources/document/TL-WN727N_V4_Datasheet.pdf>  
[2]<http://www.tp-link.tw/resources/document/TL-WN727N_V4_Datasheet.pdf>  
[3]<https://wikidevi.com/wiki/TP-LINK_TL-WN727N_v4>  
[4]<http://www.mediatek.com/en/downloads/mt7601u-usb>  
[5]<http://www.arnelborja.com/compiling-rt2870-wifi-driver-in-fedora/>  
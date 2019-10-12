---
layout: post
title: 阿里雲轉 ESXi
date: 2019-07-22
tags: esxi vCenter 阿里雲 alibabacloud
---

紀錄一下 .....

#### 阿里雲 linux 可用 vConverter 轉 只是開機要處理 GRUB 問題 uuid 問題

#### 阿里雲 win2012 可用 windows backup 的 VHDX

#### VHDX 用 starwindconverter 轉成 VMDK

#### 在 vm 內用 WINPE 内的 分區助手( AOMEI Partition Assistant) 將 HDD 磁區合併為一大塊

#### 用 分區助手 將 GPT 轉成 MBR

####  掛入一顆空碟 用 分區助手 做 HDD to SSD ....

#### 用這棵 SSD 還需用 win開機修護 bcdboot ..... 後續寫個文章吧

#### vConverter 的 proxy mode ... 後面詳述 ....

linux 轉到 vCenter 發生錯誤 ...... 3% 的地方 DNS 錯誤 用proxy mode 也一樣 .....

轉 VMDK 沒測出來 ... 網芳問題

#### 後來用轉到 ESXi 就正常了 ... 只是開機要處理 GRUB 問題 uuid 問題

阿里雲 Windows 2012 用 vconverter 轉會發生 DNS 錯誤 用proxy mode 也一樣 .....

ESXi 與 vCenter 一樣 ....

vconverter 轉 VMDK 沒測出來 ... 網芳問題

用 Windows backup 來備份到網芳,理論上將 整個目錄帶回可以 還原 .... 

這部分還要試試(對windows backup 異機還原沒信心)

後來用 starwindconverter 直接轉 C: 到 ESXi ... 聽說會失敗 ... 果真無法開機 

用 Windows backup 來備份 出來的是 VHDX ... 就用 starwindconverter 轉成 VMDK (workstation) 無法開機

以下都是在 VM workstation 內處理 , 掛入一顆空碟及 starwindconverter 轉出的 VMDK 共兩顆

用 WINPE 内的 分區助手( AOMEI Partition Assistant) 將 starwindconverter 轉出的 VMDK 磁區合併為一大塊, 再將 GPT 轉成 MBR 

最後保證能開機用  分區助手 的 HDD 轉成 SSD 將複製到另一個 HDD 內 , 關機

將 starwindconverter 轉出的 VMDK 那顆移除 ... 用 加入的那顆開機


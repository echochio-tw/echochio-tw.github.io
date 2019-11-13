---
layout: post
title: python 寫入 google sheets API (V4)
date: 2019-11-13
tags: python google api sheets
---

打開 https://console.developers.google.com/apis/library/drive.googleapis.com?q=drive

要 啟用 Google drive API

打開 https://console.developers.google.com/apis/library/sheets.googleapis.com?q=Sheets

要 啟用 Google Sheets API

管理 API

選 憑證

API 和服務中的憑證頁面

建立 憑證

建立服務帳戶金鑰

服務帳戶名稱 => 給一個如 default

Project -> 擁有者

金鑰類型 -> json

建立 

之後就有下載檔案了

json 檔案放到 C:\python\google_api.json

找到檔案內的
```
"client_email": "default@quickstart-1565583639630.iam.gserviceaccount.com",
```

在 google sheets 開UploadByPython 檔案

要加共用 給 default@quickstart-1565583639630.iam.gserviceaccount.com

給讀寫

然後程式可以寫了

```
import gspread
from oauth2client.service_account import ServiceAccountCredentials
scope = ['https://spreadsheets.google.com/feeds','https://www.googleapis.com/auth/drive']
creds = ServiceAccountCredentials.from_json_keyfile_name('C:\\python\\google_api.json', scope)
client = gspread.authorize(creds)
GSpreadSheet = 'UploadByPython'
sh = client.open(GSpreadSheet)
yesterday = str((datetime.date.today()-datetime.timedelta(days=1)).strftime("%Y-%m-%d"))
#wks2 = sht.worksheet_by_title("Sheet2")
worksheet = sh.add_worksheet(title=yesterday, rows="100", cols="20")
worksheet.update_acell('A1', "Domian name")
worksheet.update_acell('B1', "flow (MB)")
listdomains = listdomain()
id = 1
for domain in listdomains:
    id = id + 1
    worksheet.update_acell('A'+str(id), domain)
    worksheet.update_acell('B'+str(id), flowdomain(yesterday,domain))
```

其 他找 gspread github 有說明

比較重要的 位置資訊
```
A2 -> (2,1)
```

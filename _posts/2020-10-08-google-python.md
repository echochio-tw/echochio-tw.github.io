---
layout: post
title: google python 網管雜亂記
date: 2020-10-08
tags: python google
---

google python 網管做個紀錄

network.py
```
#!/usr/bin/env python3
import requests
import socket
def check_localhost():
    localhost = socket.gethostbyname('localhost')
    return localhost == "127.0.0.1"
def check_connectivity():
    localhost = socket.gethostbyname('localhost')
    request = requests.get("http://www.google.com")
    return request.status_code == 200
```

helth_check.py
```
#!/usr/bin/env python3
import shutil
import psutil
from network import *
def check_disk_usage(disk):
    """Verifies that there's enough free space on disk"""
    du = shutil.disk_usage(disk)
    free = du.free / du.total * 100
    return free > 20
def check_cpu_usage():
    """Verifies that there's enough unused CPU"""
    usage = psutil.cpu_percent(1)
    return usage < 75
# If there's not enough disk, or not enough CPU, print an error
if not check_disk_usage('/') or not check_cpu_usage():
    print("ERROR!")
elif check_localhost() and check_connectivity():
    print("Everything ok")
else:
    print("Network ERROR!")
```

employees.csv (下面那個 python 有用到)
```
Full Name, Username, Department
Audrey Miller, audrey, Development
Arden Garcia, ardeng, Sales
Bailey Thomas, baileyt, Human Resources
Blake Sousa, sousa, IT infrastructure
Cameron Nguyen, nguyen, Marketing
Charlie Grey, greyc, Development
Chris Black, chrisb, User Experience Research
Courtney Silva, silva, IT infrastructure
Darcy Johnsonn, darcy, IT infrastructure
Elliot Lamb, elliotl, Development
Emery Halls, halls, Sales
Flynn McMillan, flynn, Marketing
Harley Klose, harley, Human Resources
Jean May Coy, jeanm, Vendor operations
Kay Stevens, kstev, Sales
Lio Nelson, lion, User Experience Research
Logan Tillas, tillas, Vendor operations
Micah Lopes, micah, Development
Sol Mansi, solm, IT infrastructure
```

generate_report.py
```
#!/usr/bin/env python3
import csv
def read_employees(csv_file_location):
    csv.register_dialect('empDialect', skipinitialspace=True, strict=True)
    employee_file = csv.DictReader(open(csv_file_location), dialect = 'empDialect')
    employee_list = []
    for data in employee_file:
        employee_list.append(data)
    return employee_list
def process_data(employee_list):
    department_list = []
    for employee_data in employee_list:
        department_list.append(employee_data['Department'])
    department_data = {}
    for department_name in set(department_list):
        department_data[department_name] = department_list.count(department_name)
    return department_data
def write_report(dictionary, report_file):
  with open(report_file, "w+") as f:
    for k in sorted(dictionary):
      f.write(str(k)+':'+str(dictionary[k])+'\n')
    f.close()
employee_list = read_employees('/home/student-03-fea6647d12a9/data/employees.csv')
print(employee_list)
dictionary = process_data(employee_list)
print(dictionary)
write_report(dictionary, '/home/student-03-fea6647d12a9/test_report.txt')
```

```
import re
def check_web_address(text):
  pattern = '[.](\w+)$'
  result = re.search(pattern, text)
  return result != None

print(check_web_address("gmail.com")) # True
print(check_web_address("www@google")) # False
print(check_web_address("www.Coursera.org")) # True
print(check_web_address("web-address.com/homepage")) # False
print(check_web_address("My_Favorite-Blog.US")) # True

# https://regex101.com/
# for test
```

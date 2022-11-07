---
layout: post
title: Python Project1(data error check) - (1)
tags: [Python]
---




# Python Project - daily error check
엑셀 파일을 파이썬으로 다루는 프로젝트를 한번 진행해보았습니다.

<br>
제가 하는 업무 중에서 매일 각 매장의 전체 상품 데이터터 중에서, 항목별로 기준에 맞지 않는 상품들을 추출해서
매장 IT 담당자에게 전달해야 하는 업무가 있습니다.

<br>

![Image of sql]({{ "/assets/img/2022-11-01-python-1/1.png" | relative_url }})

<br>
위의 엑셀파일 처럼, 데이터들이 항목별로 지점별로 3만개~6만개 정도의 데이터가 있습니다.
제가 하고 싶은 것은 이 데이터들 중에서 항목별로 기준에 맞지 않는 데이터들만 따로 걸러내서 새로운 시트를 만드는 것입니다.
<br>
우선 파이썬에서 엑셀 파일을 다루기 위해선 openpyxl 모듈을 설치해줘야 합니다.
<br>
<br>

```python
# 1st STEP
# 파일의 경로나 폴더에 관한 처리를 하기 위해서 import os를 해줍니다.
# openpyxl을 설치 후에 import 해줍니다.
import os
from re import sub
# import pandas as pd
from openpyxl import load_workbook

#엑셀 셀서식을 설정하기 위해 import 해줘야하는 것들입니다
#Alignment는 정렬 관련 서식, Font는 글씨관련 서식
from openpyxl.styles import Alignment
from openpyxl.styles.fonts import Font

#데이터를 가져올 엑셀 파일 경로를 path 변수에 담아줍니다.
path = "../python/raw"

#path에 있는 파일 리스트를 file_list에 담아줍니다.
file_list = os.listdir(path)

#title_list에 1열에 표시할 제목 데이터들을 리스트로 담아줍니다.
title_list = ['UPC', 'BRAND', 'DESC1', 'DESC2', 'SIZE', 'FAMILY', 'FNAME', 'REPORT', 'RNAME', 'SDEPT', 'SNAME',
 'CATEGORY', 'CNAME', 'TAX1' ,'TAX2', 'FSTMP', 'SCALE', 'MANUAL', 'PLU', 'VENDOR', 'VNAME', 'AUTH', 'VENDOR CODE',
  'BASECOST', ' CASEQTY', 'UNITCOST', 'ACTIVE', 'REG', 'TPR', 'SALE', 'NFS']

```
GA55, IL70, PA88 이렇게 총 세 가지의 지점이 있기 때문에, for문을 활용해서 이 세 파일을 모두 작업할 것입니다.

```python

#파일 리스트에 있는 파일들 for문으로 하나씩 처리하기
for f in file_list:

  #파일 이름 s
  file_name = "../python/raw/" + f

  #엑셀 파일 불러오기기 data_only=True : 공식 적용된 값을 가져옴(False면 셀안 공식을 가져옴)
  wb = load_workbook(filename = file_name, data_only=True)

  #첫 번쨰 시트
  ws = wb.active

  #데이터가 들어있는 마지막 열
  last_raw = ws.max_row
 

  #엑셀 데이터를 담을 딕셔너리
  #UPC를 key값으로 하고 value에는 해당 UPC의 전체 정보를 담을 것입니다.
  #그런데 UPC에 대한 정보가 2개 이상 있을 수 있기 때문에 value를 리스트로 만들어서 담을 예정입니다.
  result = {}

  #첫번 째 열은 제목이기 때문에 2열부터 시작해서 데이터 담기 시작, last_raw는 마지막 열
  for i in range(2, last_raw):
      #만약 A2에 value값이 없다면 빈 리스트를 value로 추가(맨처음에 빈리스트를 value로 만들어주고 여기에 전체 열을 리스트로 담아서 넣어주기 위함)
      if result.get(ws['A'+str(i)].value) == None:
          result[ws['A'+str(i)].value] = []
      #key값에 맞게 각 데이터 전체 열을 리스트 value에 추가해줍니다. 
      result[ws['A'+str(i)].value].append([str(ws['A'+str(i)].value), ws['B'+str(i)].value, ws['C'+str(i)].value, ws['D'+str(i)].value, ws['E'+str(i)].value, ws['F'+str(i)].value, ws['G'+str(i)].value,
       ws['H'+str(i)].value, ws['I'+str(i)].value, ws['J'+str(i)].value, ws['K'+str(i)].value, ws['L'+str(i)].value, ws['M'+str(i)].value, ws['N'+str(i)].value, ws['O'+str(i)].value, ws['P'+str(i)].value,
        ws['Q'+str(i)].value, ws['R'+str(i)].value, ws['S'+str(i)].value, ws['T'+str(i)].value, ws['U'+str(i)].value, ws['V'+str(i)].value, ws['W'+str(i)].value, float(ws['X'+str(i)].value),
        "{:.2f}".format((float(ws['Y'+str(i)].value))), "{:.2f}".format((float(ws['Z'+str(i)].value))), "{:.2f}".format((float(ws['AA'+str(i)].value))),
         "{:.2f}".format((float(ws['AB'+str(i)].value))),"{:.2f}".format((float(ws['AC'+str(i)].value))),"{:.2f}".format((float(ws['AD'+str(i)].value))), ws['AE'+str(i)].value])

```
이렇게 하면 딕셔너리의 형태로 엑셀파일에 있는 데이터를 구조적으로 가져올 수 있습니다.
<br>
<br>

```python

      result = {
        '0081439202263': [
                          ['0081439202263', 'DGF', 'FROZ SWAI FISH(BASA) WHOLE CUT STEAK', 'FROZ SWAI FISH(BASA) WHOLE CUT STEAK', '20LB', '201', 'CHINA', '12', 'CHN/VTN/TL/PHL', '1901', 'FR. PRODUCE', '1901001', 'FZ. VEGETABLE', '1', None, '1', None, None, None, '1222', 'MY-A & CO', '1', 'FR5190', 75.0, '20.00', '3.75', '6.49', '6.49', '0.00', '0.00', 'NUL']
                         ],
        '0081439202273': [
                          ['0081439202273', 'DGF', 'FR WHOLE ROUND RIVERBARB FISH 200UP', 'FR WHOLE ROUND RIVERBARB FISH 200UP', '30LB', None, None, '12', 'CHN/VTN/TL/PHL', '1905', 'FROZEN ETC PRODUCTS', '1905001', 'FZ. ETC PRODUCTS', '1', None, '1', None, None, None, '1222', 'MY-A & CO', '1', 'FR2530', 170.0, '30.00', '5.67', '9.99', '9.99', '0.00', '0.00', 'NUL'] 
                         ],
        '0081593400010': [
                           ['0081593400010', None, 'COCO RICO SODA(CAN)', '코코리코 소다(캔)', '4X6X12OZ', '501', 'CENTRAL AMERICA', '12', 'CHN/VTN/TL/PHL', '2201', 'BEVERAGE(SOFT DRINKS)', '2201002', 'CARBONATED', '1', None, '1', None, None, None, '1243', 'METRO CHEF', '0', '00107', 12.99, '24.00', '0.54', '0.99', '0.99', '0.00', '0.00', 'NUL'],
                           ['0081593400010', None, 'COCO RICO SODA(CAN)', '코코리코 소다(캔)', '4X6X12OZ', '501', 'CENTRAL AMERICA', '12', 'CHN/VTN/TL/PHL', '2201', 'BEVERAGE(SOFT DRINKS)', '2201002', 'CARBONATED', '1', None, '1', None, None, None, '1209', 'NEW INTERNATIONAL FOOD', '1', 'COCO987', 13.0, '24.00', '0.54', '0.99', '0.99', '0.00', '0.00', 'NUL']
                         ]
      }

```

위의 구조처럼 value에 이중 리스트를 이용해서 key값 에 해당하는 데이터 정보를 리스트 형식으로 담았습니다.
<br>
<br>

```python
  #2nd STEP
  #이제 에러로 판단되는 데이터들을 추출해서 담을 새로운 엑셀 파일을 만들어줍니다.
  from openpyxl import Workbook
  wb = Workbook()

  #전체 데이터에서 필요 없는 데이터를 걸러내기 위한 작업을 먼저 해줍니다.
  #temp_list에 필요 없는 데이터를 임시로 담아줄 것입니다.
  temp_list= []

  # value값에 접근 하는 방법은 result[key][raw][column]입니다
  # 예를 들면 result['0081593400010'][0][30]의 의미는 '0081593400010' value값에서 첫번째 리스트에 있는 리스트의 30번째 index 값을 나타냅니다. 
  # index는 0부터 시작하므로 실제 엑셀 데이터에서는 31번째 column인 NFS행을 의미하는 것입니다.
  for i in result.keys():
    for j in range(0, len(result[i])):
        if str(result[i][j][30]) == '1' or \
          str(result[i][j][1]).lower() in ['happy hour', 'coupon', 'event'] or \
          str(result[i][j][0]) in ['0000000000091', '0000000000092', '0000000000093', '0000000000094','0000000000095', '0000000000096', '0000000000097', '0000000000098', '0000000000999', '0000000009999', '0000000099999'] or \
          str(result[i][j][2]) in ['ASSI SERVICE CHARGE', 'BOTTLE REFUND ITEM', 'H/W SERVICE FEE', 'BEER TEST', 'WINE TEST'] or \
          str(result[i][j][7]) in ['99', '999'] or result[i][j][7]==None :
            temp_list.append(result[i][j][0])

  #위의 for문에서 걸러내고 싶은 데이터는
  #31번째 column값이 1인 데이터
  #2번쨰 column값이 happy hour, coupon, event 중에 하나인 데이터
  #1번째 column값이 '0000000000091', '0000000000092', '0000000000093', '0000000000094','0000000000095', '0000000000096', '0000000000097', '0000000000098', '0000000000999', '0000000009999', '0000000099999' 중에 하나인 데이터
  #3번째 column값이 'ASSI SERVICE CHARGE', 'BOTTLE REFUND ITEM', 'H/W SERVICE FEE', 'BEER TEST', 'WINE TEST'중에 하나인 데이터
  #8번째 column값이 99, 999 이거나 None인 데이터
  #위의 기준에 해당하는 데이터는 temp_list에 추가해줍니다.

    for i in temp_list:
    result.pop(i, None)

  #그리고 pop메서드를 활용해서 temp_list에 있는 값들을 원래 result 딕녀서리에서 제거해줍니다.
  #이젠 result 딕셔너리에는 걸러내준 데이터를 제외한 데이터들만 남아있게 됩니다.
```

위의 작업까지 마쳤다면 result에는 우리가 에러사항을 체크하기 위해 필요한 원본데이터들만이 담겨있습니다.
<br>
이제는 result 딕셔너리에서 각 시트별로 기준에 적합하지 않은 데이터들을 걸러내는 작업을 할 것입니다.
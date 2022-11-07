---
layout: post
title: Python Project1(data error check) - (2)
tags: [Python]
---




# Python Project - daily error check
앞서 만든 result 딕셔너리의 구조를 참고해서 본격적으로 오류 데이터들을 추출하도록 하겠습니다.

<br>
<br>
첫 번째로 만들 시트는 MARGIN 시트입니다.
<br>
이 시트는 GA55파일에만 따로 만들어주고 싶기 때문에, 파일 명에 GA55가 포함된 경우에만 진행하도록 조건을 걸어주었습니다.

```python
  # f는 맨 처음에 위에 file_list에서 각 파일명들을 담은 변수 입니다.
  # f에 GA55가 들어갈 경우에 MARGIN이란 시트를 생성합니다.
  if 'GA55' in f:

    ws0 = wb.create_sheet("MARGIN")
    #MARGIN에러 데이터들을 추출해서 담을 리스트
    ga55_margin = []

    #MARGIN 시트에 필요한 column 내용만을 margin_title_list에 담아줍니다.
    margin_title_list = ['UPC', 'BRAND', 'DESC1', 'DESC2', 'SIZE', 'FAMILY', 'FNAME', 'REPORT', 'RNAME', 'SDEPT', 'SNAME',
    'CATEGORY', 'CNAME', 'TAX1' ,'TAX2', 'FSTMP', 'SCALE', 'MANUAL', 'PLU', 'VENDOR', 'VNAME', 'AUTH', 'VENDOR CODE',
      'BASECOST', ' CASEQTY', 'UNITCOST', 'ACTIVE', 'REG', 'TPR', 'SALE', 'MARGIN']



    for i in range(1, 32):
        #첫번째 열에 margin_title_list에서 제목 데이터들을 넣어줍니다.
        ws0.cell(row=1, column=i).value = margin_title_list[i-1]
        #첫번째 열에 가운데 정렬 서식을 지정해줍니다.
        ws0.cell(row=1, column=i).alignment = Alignment(horizontal = 'center', vertical = 'center')
        #첫번 째 열에 font에 bold서식을 지정해줍니다.
        ws0.cell(row=1, column=i).font = Font(bold=True)

    # result 딕셔너리에서 value값 하나씩 꺼내보기
    for i in result.keys():
      #각 key 값마다 value 값 리스트의 개수가 다르기 때문에 각 리스트의 개수만큼 for문을 돌릴 수 있도록 해야됩니다.
      for j in range(0, len(result[i])):
          #27번째 column(ACTIVE)이 0이 아닐경우
          if float(result[i][j][26]) != 0:
            #(ACTIVE-UNITCOST)/ACTIVE*100 -> margin구하는 공식
            #margin이 20 보다 작고, 8번째 column(report)이 11, 12, 14, 17에 중에 하나이고, 22번쨰 column(AUTH)가 1인 데이터를 ga55_margin에 담아줍니다.
            if ((float(result[i][j][26])-float(result[i][j][25]))/float(result[i][j][26]))*100 <=20 and result[i][j][7] in ['11', '12', '14', '17'] and result[i][j][21] == '1':
              ga55_margin.append(result[i][j])

```

ga55_margin list는 이중리스트로 구성되었습니다.
<br>
아래는 ga55_margin 의 예시입니다.

```python
    [
    ['0000081100063', 'NONGSHIM', 'NONGSHIM ONION RINGS(M)', '양파링(중),농심 3.17OZ', '20X3.17OZ', '101', 'KOREA', '11', 'KOR/JAP', '2401', 'SNACKS', '2401001', 'SNACKS', '1', None, '1', None, None, None, '1100', 'RHEE BROS.,INC', '1', '09405K', 41.51, '1.00', '41.51', '45.99', '45.99', '0.00', '0.00', 'NUL'],
    ['0000081100092', 'ASSI', 'ASSI BOILED FERN (GOSARI)', '아씨 생 고사리', '24X1LB', '101', 'KOREA', '11', 'KOR/JAP', '1707', 'REF. VEGETABLE', '1707001', 'REF. ETC VEGETABLE1', '1', None, '1', None, None, None, '1100', 'RHEE BROS.,INC', '1', '19101C', 68.34, '1.00', '68.34', '79.99', '79.99', '0.00', '0.00', 'NUL']
    ]

```

<br>
이제는 ga55_margin에 있는 데이터들을 MARGIN 시트에 입력해주는 작업을 해주면 됩니다.

```python

    #margin_length에 ga55_margin 리스트 길이를 담아줍니다.
    margin_length = len(ga55_margin)
    #for i in range(n, m)은 n에서 부터m-1까지 검사하는 것에 유의하여 range값을 설정해줘야됩니다.   
    for i in range(1, margin_length+1):
        #첫 번째 열은 title이 있기 때문에 2번째 열부터 데이터를 입력할 것입니다.
        #(2,1)셀에 ga55_margin[0][0]의 값이 들어가고, (2,2)셀에 ga55_margin[0][1]값이 들어가는 방식입니다.
        #(3,1)셀에는 ga55_margin[1][0]이 들어가는 것입니다.
        #23열까지는 값을 그대로 넣어주고 24열부터는 float형으로 소수점 2자리 까지 표현한 값을 넣어줘야하기 때문에 이렇게 나눠서 for문을 작성했습니다.
        for j in range(1, 24):
            ws0.cell(row=i+1, column=j).value = ga55_margin[i-1][j-1]  
        for j in range(24, 31):
            ws0.cell(row=i+1, column=j).value = "{:.2f}".format(float(ga55_margin[i-1][j-1]))  
        ws0.cell(row=i+1, column=31).value = "{:.2f}".format(((float(ga55_margin[i-1][26])-float(ga55_margin[i-1][25]))/float(ga55_margin[i-1][26])) * 100)
    

```

이렇게 해주면 GA55 파일의 MARGIN 시트가 완성됩니다.
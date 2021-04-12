## 공공데이터를 이용한 카페 상권분석(2020 Ver.)


**들어가며**

- 공공데이터를 통해 대한민국을 이해해봅시다(?).
- 공공데이터포털(data.go.kr)에 다양한 데이터가 공개되어 있습니다.
- 그 중에 카페(라는 업종분류)들에 대해서 현황을 조사하려고 합니다.

**명세사항**
1. 전국 카페 데이터를 모두 수집해야합니다.
2. 지역별 or 브랜드별 점포 현황을 확인합니다.
3. 분석 결과를 시각화합니다. 



[데이터] https://www.data.go.kr/data/15012005/fileData.do


```python
# 라이브러리를 불러옵니다.
import pandas as pd
```

## 1. 데이터 불러오기


```python
# 다운로드 받은 데이터중 일부를 열어봅니다.
temp = pd.read_csv("data/소상공인시장진흥공단_상가(상권)정보_서울_202012.csv",sep='|',encoding='utf_8')
temp
#window -> encoding='utf_8'써줘야함
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>상가업소번호</th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류코드</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류코드</th>
      <th>상권업종중분류명</th>
      <th>상권업종소분류코드</th>
      <th>상권업종소분류명</th>
      <th>표준산업분류코드</th>
      <th>...</th>
      <th>건물관리번호</th>
      <th>건물명</th>
      <th>도로명주소</th>
      <th>구우편번호</th>
      <th>신우편번호</th>
      <th>동정보</th>
      <th>층정보</th>
      <th>호정보</th>
      <th>경도</th>
      <th>위도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>17163092</td>
      <td>도전최강달인왕만두</td>
      <td>NaN</td>
      <td>Q</td>
      <td>음식</td>
      <td>Q01</td>
      <td>한식</td>
      <td>Q01A01</td>
      <td>한식/백반/한정식</td>
      <td>I56111</td>
      <td>...</td>
      <td>1174010200102170000018014</td>
      <td>고덕그라시움</td>
      <td>서울특별시 강동구 고덕로 333</td>
      <td>134082</td>
      <td>5224.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>127.159471</td>
      <td>37.556197</td>
    </tr>
    <tr>
      <th>1</th>
      <td>17120456</td>
      <td>이때</td>
      <td>NaN</td>
      <td>Q</td>
      <td>음식</td>
      <td>Q01</td>
      <td>한식</td>
      <td>Q01A01</td>
      <td>한식/백반/한정식</td>
      <td>I56111</td>
      <td>...</td>
      <td>1144012400103900067027687</td>
      <td>NaN</td>
      <td>서울특별시 마포구 동교로38안길 7</td>
      <td>121867</td>
      <td>3982.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>126.924660</td>
      <td>37.562176</td>
    </tr>
    <tr>
      <th>2</th>
      <td>17175350</td>
      <td>L.A.D</td>
      <td>NaN</td>
      <td>Q</td>
      <td>음식</td>
      <td>Q01</td>
      <td>한식</td>
      <td>Q01A01</td>
      <td>한식/백반/한정식</td>
      <td>I56111</td>
      <td>...</td>
      <td>1144012000103950112010755</td>
      <td>NaN</td>
      <td>서울특별시 마포구 잔다리로3안길 23</td>
      <td>121840</td>
      <td>4043.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>126.919845</td>
      <td>37.550689</td>
    </tr>
    <tr>
      <th>3</th>
      <td>17175311</td>
      <td>제이씨에스푸드</td>
      <td>NaN</td>
      <td>Q</td>
      <td>음식</td>
      <td>Q01</td>
      <td>한식</td>
      <td>Q01A01</td>
      <td>한식/백반/한정식</td>
      <td>I56111</td>
      <td>...</td>
      <td>1162010200101180033018722</td>
      <td>NaN</td>
      <td>서울특별시 관악구 신림로14길 3</td>
      <td>151856</td>
      <td>8839.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>126.937790</td>
      <td>37.471190</td>
    </tr>
    <tr>
      <th>4</th>
      <td>22767534</td>
      <td>BYC상신점</td>
      <td>상신점</td>
      <td>D</td>
      <td>소매</td>
      <td>D05</td>
      <td>의복의류</td>
      <td>D05A07</td>
      <td>셔츠/내의/속옷</td>
      <td>NaN</td>
      <td>...</td>
      <td>1171011300100360000022458</td>
      <td>성암빌딩</td>
      <td>서울특별시 송파구 오금로 527</td>
      <td>138110</td>
      <td>5768.0</td>
      <td>NaN</td>
      <td>1</td>
      <td>NaN</td>
      <td>127.147321</td>
      <td>37.493054</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>346572</th>
      <td>17222727</td>
      <td>현대기림</td>
      <td>NaN</td>
      <td>D</td>
      <td>소매</td>
      <td>D10</td>
      <td>건강/미용식품</td>
      <td>D10A04</td>
      <td>건강식품판매</td>
      <td>G47216</td>
      <td>...</td>
      <td>1120011400102760017009013</td>
      <td>NaN</td>
      <td>서울특별시 성동구 뚝섬로 366-72</td>
      <td>133819</td>
      <td>4775.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>127.051154</td>
      <td>37.539464</td>
    </tr>
    <tr>
      <th>346573</th>
      <td>17222723</td>
      <td>본현대미아점</td>
      <td>NaN</td>
      <td>D</td>
      <td>소매</td>
      <td>D10</td>
      <td>건강/미용식품</td>
      <td>D10A04</td>
      <td>건강식품판매</td>
      <td>G47216</td>
      <td>...</td>
      <td>1129013400100200001025204</td>
      <td>현대백화점미아점</td>
      <td>서울특별시 성북구 동소문로 315</td>
      <td>136719</td>
      <td>2730.0</td>
      <td>NaN</td>
      <td>5</td>
      <td>NaN</td>
      <td>127.028726</td>
      <td>37.608392</td>
    </tr>
    <tr>
      <th>346574</th>
      <td>17219564</td>
      <td>오피스알파</td>
      <td>약장수</td>
      <td>D</td>
      <td>소매</td>
      <td>D10</td>
      <td>건강/미용식품</td>
      <td>D10A04</td>
      <td>건강식품판매</td>
      <td>G47216</td>
      <td>...</td>
      <td>1130510300100360037035171</td>
      <td>NaN</td>
      <td>서울특별시 강북구 노해로17길 62-1</td>
      <td>142872</td>
      <td>1075.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>101</td>
      <td>127.018733</td>
      <td>37.640485</td>
    </tr>
    <tr>
      <th>346575</th>
      <td>17219761</td>
      <td>앤클라인뉴욕핸드백</td>
      <td>NaN</td>
      <td>D</td>
      <td>소매</td>
      <td>D06</td>
      <td>가방/신발/액세서리</td>
      <td>D06A10</td>
      <td>가방/가죽제품소매</td>
      <td>G47430</td>
      <td>...</td>
      <td>1153010200105730000020642</td>
      <td>NC백화점</td>
      <td>서울특별시 구로구 구로중앙로 152</td>
      <td>152715</td>
      <td>8292.0</td>
      <td>NaN</td>
      <td>1</td>
      <td>NaN</td>
      <td>126.882409</td>
      <td>37.501378</td>
    </tr>
    <tr>
      <th>346576</th>
      <td>17207617</td>
      <td>피움테라피</td>
      <td>NaN</td>
      <td>F</td>
      <td>생활서비스</td>
      <td>F01</td>
      <td>이/미용/건강</td>
      <td>F01A03</td>
      <td>비만/피부관리</td>
      <td>NaN</td>
      <td>...</td>
      <td>1135010600105040001010530</td>
      <td>중계그린아파트</td>
      <td>서울특별시 노원구 섬밭로 285</td>
      <td>139863</td>
      <td>1773.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>207</td>
      <td>127.062556</td>
      <td>37.640913</td>
    </tr>
  </tbody>
</table>
<p>346577 rows × 39 columns</p>
</div>




```python
# data 폴더에 있는 모든 csv 파일을 읽어오기 위해 glob을 사용합니다.
from glob import glob

# csv 목록 불러오기
file_names = glob("data/*.csv")
total = pd.DataFrame()
# 모든 csv 병합하기
for file_name in file_names:
    temp = pd.read_csv(file_name, sep='|', encoding = 'utf-8')
    total = pd.concat([total,temp])
# reset index
total.reset_index(inplace=True,drop = True)
total
```

    C:\ProgramData\Anaconda3\lib\site-packages\IPython\core\interactiveshell.py:3146: DtypeWarning: Columns (35) have mixed types.Specify dtype option on import or set low_memory=False.
      has_raised = await self.run_ast_nodes(code_ast.body, cell_name,
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>상가업소번호</th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류코드</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류코드</th>
      <th>상권업종중분류명</th>
      <th>상권업종소분류코드</th>
      <th>상권업종소분류명</th>
      <th>표준산업분류코드</th>
      <th>...</th>
      <th>건물관리번호</th>
      <th>건물명</th>
      <th>도로명주소</th>
      <th>구우편번호</th>
      <th>신우편번호</th>
      <th>동정보</th>
      <th>층정보</th>
      <th>호정보</th>
      <th>경도</th>
      <th>위도</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>17175358</td>
      <td>국수나루</td>
      <td>NaN</td>
      <td>Q</td>
      <td>음식</td>
      <td>Q01</td>
      <td>한식</td>
      <td>Q01A01</td>
      <td>한식/백반/한정식</td>
      <td>I56111</td>
      <td>...</td>
      <td>4215010900101730002015569</td>
      <td>NaN</td>
      <td>강원도 강릉시 토성로 193</td>
      <td>210934.0</td>
      <td>25531.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>128.896783</td>
      <td>37.757642</td>
    </tr>
    <tr>
      <th>1</th>
      <td>25033300</td>
      <td>동그라미중고타이어</td>
      <td>NaN</td>
      <td>D</td>
      <td>소매</td>
      <td>D23</td>
      <td>자동차/자동차용품</td>
      <td>D23A04</td>
      <td>타이어판매</td>
      <td>G45211</td>
      <td>...</td>
      <td>4215011100110960006010791</td>
      <td>NaN</td>
      <td>강원도 강릉시 가작로 270</td>
      <td>210110.0</td>
      <td>25488.0</td>
      <td>1</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>128.904472</td>
      <td>37.770252</td>
    </tr>
    <tr>
      <th>2</th>
      <td>17174549</td>
      <td>세인트존스호텔Ohcrab</td>
      <td>NaN</td>
      <td>O</td>
      <td>숙박</td>
      <td>O01</td>
      <td>호텔/콘도</td>
      <td>O01A01</td>
      <td>호텔/콘도</td>
      <td>NaN</td>
      <td>...</td>
      <td>4215011300100010001017124</td>
      <td>세인트존스호텔</td>
      <td>강원도 강릉시 창해로 307</td>
      <td>210120.0</td>
      <td>25467.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>128.920908</td>
      <td>37.791299</td>
    </tr>
    <tr>
      <th>3</th>
      <td>17174079</td>
      <td>평창라마다호텔</td>
      <td>NaN</td>
      <td>O</td>
      <td>숙박</td>
      <td>O01</td>
      <td>호텔/콘도</td>
      <td>O01A01</td>
      <td>호텔/콘도</td>
      <td>NaN</td>
      <td>...</td>
      <td>4276038024102450036000001</td>
      <td>NaN</td>
      <td>강원도 평창군 대관령면 오목길 107</td>
      <td>232954.0</td>
      <td>25342.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>128.717971</td>
      <td>37.660051</td>
    </tr>
    <tr>
      <th>4</th>
      <td>17173904</td>
      <td>호텔탑스텐스카이라운지</td>
      <td>NaN</td>
      <td>O</td>
      <td>숙박</td>
      <td>O01</td>
      <td>호텔/콘도</td>
      <td>O01A01</td>
      <td>호텔/콘도</td>
      <td>NaN</td>
      <td>...</td>
      <td>4215035029100920001000002</td>
      <td>NaN</td>
      <td>강원도 강릉시 옥계면 헌화로 455-34</td>
      <td>210831.0</td>
      <td>25633.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>129.052902</td>
      <td>37.654680</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2416247</th>
      <td>17215242</td>
      <td>해성국제결혼</td>
      <td>NaN</td>
      <td>F</td>
      <td>생활서비스</td>
      <td>F17</td>
      <td>예식/의례/관혼상제</td>
      <td>F17A05</td>
      <td>결혼상담소</td>
      <td>S96994</td>
      <td>...</td>
      <td>4311311100104460002018922</td>
      <td>NaN</td>
      <td>충청북도 청주시 서원구 1순환로 785</td>
      <td>362747.0</td>
      <td>28668.0</td>
      <td>상가</td>
      <td>NaN</td>
      <td>201</td>
      <td>127.464755</td>
      <td>36.624187</td>
    </tr>
    <tr>
      <th>2416248</th>
      <td>17221668</td>
      <td>CU</td>
      <td>청주우암공원점</td>
      <td>D</td>
      <td>소매</td>
      <td>D03</td>
      <td>종합소매점</td>
      <td>D03A01</td>
      <td>편의점</td>
      <td>G47122</td>
      <td>...</td>
      <td>4311111300103410001046095</td>
      <td>NaN</td>
      <td>충청북도 청주시 청원구 향군로15번길 27</td>
      <td>363814.0</td>
      <td>28538.0</td>
      <td>NaN</td>
      <td>1</td>
      <td>NaN</td>
      <td>127.486439</td>
      <td>36.647056</td>
    </tr>
    <tr>
      <th>2416249</th>
      <td>17222225</td>
      <td>슬기로운상회</td>
      <td>NaN</td>
      <td>D</td>
      <td>소매</td>
      <td>D03</td>
      <td>종합소매점</td>
      <td>D03A06</td>
      <td>종합소매</td>
      <td>G47190</td>
      <td>...</td>
      <td>4311111600104370000038563</td>
      <td>사천동아아파트</td>
      <td>충청북도 청주시 청원구 율봉로 31</td>
      <td>363710.0</td>
      <td>28341.0</td>
      <td>104</td>
      <td>NaN</td>
      <td>203</td>
      <td>127.473439</td>
      <td>36.664050</td>
    </tr>
    <tr>
      <th>2416250</th>
      <td>17222106</td>
      <td>김씨상회</td>
      <td>NaN</td>
      <td>D</td>
      <td>소매</td>
      <td>D03</td>
      <td>종합소매점</td>
      <td>D03A06</td>
      <td>종합소매</td>
      <td>G47190</td>
      <td>...</td>
      <td>4311410300108540001000001</td>
      <td>NaN</td>
      <td>충청북도 청주시 청원구 충청대로139번길 39</td>
      <td>363818.0</td>
      <td>28326.0</td>
      <td>NaN</td>
      <td>1</td>
      <td>102</td>
      <td>127.492232</td>
      <td>36.669295</td>
    </tr>
    <tr>
      <th>2416251</th>
      <td>17207629</td>
      <td>삼화씨에스</td>
      <td>NaN</td>
      <td>F</td>
      <td>생활서비스</td>
      <td>F02</td>
      <td>세탁/가사서비스</td>
      <td>F02A05</td>
      <td>청소/소독</td>
      <td>NaN</td>
      <td>...</td>
      <td>4313011600110760000000001</td>
      <td>NaN</td>
      <td>충청북도 충주시 칠금중앙1길 4-9</td>
      <td>380965.0</td>
      <td>27362.0</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>NaN</td>
      <td>127.920122</td>
      <td>36.980037</td>
    </tr>
  </tbody>
</table>
<p>2416252 rows × 39 columns</p>
</div>




```python
# 분석에 필요한 column을 고릅니다. ## 자유롭게 하셔도 상관없습니다.
data=total[['상호명','지점명','상권업종대분류명','상권업종중분류명','시도명','시군구명','행정동명']]
data
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>국수나루</td>
      <td>NaN</td>
      <td>음식</td>
      <td>한식</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>옥천동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>동그라미중고타이어</td>
      <td>NaN</td>
      <td>소매</td>
      <td>자동차/자동차용품</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>포남1동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>세인트존스호텔Ohcrab</td>
      <td>NaN</td>
      <td>숙박</td>
      <td>호텔/콘도</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>초당동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>평창라마다호텔</td>
      <td>NaN</td>
      <td>숙박</td>
      <td>호텔/콘도</td>
      <td>강원도</td>
      <td>평창군</td>
      <td>대관령면</td>
    </tr>
    <tr>
      <th>4</th>
      <td>호텔탑스텐스카이라운지</td>
      <td>NaN</td>
      <td>숙박</td>
      <td>호텔/콘도</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>옥계면</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2416247</th>
      <td>해성국제결혼</td>
      <td>NaN</td>
      <td>생활서비스</td>
      <td>예식/의례/관혼상제</td>
      <td>충청북도</td>
      <td>청주시 서원구</td>
      <td>성화.개신.죽림동</td>
    </tr>
    <tr>
      <th>2416248</th>
      <td>CU</td>
      <td>청주우암공원점</td>
      <td>소매</td>
      <td>종합소매점</td>
      <td>충청북도</td>
      <td>청주시 청원구</td>
      <td>우암동</td>
    </tr>
    <tr>
      <th>2416249</th>
      <td>슬기로운상회</td>
      <td>NaN</td>
      <td>소매</td>
      <td>종합소매점</td>
      <td>충청북도</td>
      <td>청주시 청원구</td>
      <td>율량.사천동</td>
    </tr>
    <tr>
      <th>2416250</th>
      <td>김씨상회</td>
      <td>NaN</td>
      <td>소매</td>
      <td>종합소매점</td>
      <td>충청북도</td>
      <td>청주시 청원구</td>
      <td>율량.사천동</td>
    </tr>
    <tr>
      <th>2416251</th>
      <td>삼화씨에스</td>
      <td>NaN</td>
      <td>생활서비스</td>
      <td>세탁/가사서비스</td>
      <td>충청북도</td>
      <td>충주시</td>
      <td>칠금.금릉동</td>
    </tr>
  </tbody>
</table>
<p>2416252 rows × 7 columns</p>
</div>




```python
data.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 2416252 entries, 0 to 2416251
    Data columns (total 7 columns):
     #   Column    Dtype 
    ---  ------    ----- 
     0   상호명       object
     1   지점명       object
     2   상권업종대분류명  object
     3   상권업종중분류명  object
     4   시도명       object
     5   시군구명      object
     6   행정동명      object
    dtypes: object(7)
    memory usage: 129.0+ MB
    


```python
total.info()
```

    <class 'pandas.core.frame.DataFrame'>
    RangeIndex: 2416252 entries, 0 to 2416251
    Data columns (total 39 columns):
     #   Column     Dtype  
    ---  ------     -----  
     0   상가업소번호     int64  
     1   상호명        object 
     2   지점명        object 
     3   상권업종대분류코드  object 
     4   상권업종대분류명   object 
     5   상권업종중분류코드  object 
     6   상권업종중분류명   object 
     7   상권업종소분류코드  object 
     8   상권업종소분류명   object 
     9   표준산업분류코드   object 
     10  표준산업분류명    object 
     11  시도코드       int64  
     12  시도명        object 
     13  시군구코드      int64  
     14  시군구명       object 
     15  행정동코드      int64  
     16  행정동명       object 
     17  법정동코드      float64
     18  법정동명       object 
     19  지번코드       int64  
     20  대지구분코드     int64  
     21  대지구분명      object 
     22  지번본번지      int64  
     23  지번부번지      float64
     24  지번주소       object 
     25  도로명코드      int64  
     26  도로명        object 
     27  건물본번지      int64  
     28  건물부번지      float64
     29  건물관리번호     object 
     30  건물명        object 
     31  도로명주소      object 
     32  구우편번호      float64
     33  신우편번호      float64
     34  동정보        object 
     35  층정보        object 
     36  호정보        object 
     37  경도         float64
     38  위도         float64
    dtypes: float64(7), int64(9), object(23)
    memory usage: 718.9+ MB
    


```python
# 메모리 낭비를 막기 위해 필요없는 변수는 제거합니다.
del total
```


```python

```


    ---------------------------------------------------------------------------

    NameError                                 Traceback (most recent call last)

    <ipython-input-14-675a0c330fec> in <module>
    ----> 1 total.head()
    

    NameError: name 'total' is not defined


## 2. 데이터 구경하기

#### 전국 커피 전문점 


```python
#set(data["상권업종대분류명"])
set(data["상권업종중분류명"])
```




    {'PC/오락/당구/볼링등',
     '가구소매',
     '가방/신발/액세서리',
     '가전제품소매',
     '가정/주방/인테리어',
     '개인/가정용품수리',
     '개인서비스',
     '건강/미용식품',
     '경마/경륜/성인오락',
     '광고/인쇄',
     '기타교육기관',
     '기타서비스업',
     '기타음식업',
     '기타판매업',
     '놀이/여가/취미',
     '닭/오리요리',
     '대중목욕탕/휴게',
     '대행업',
     '도서관/독서실',
     '모텔/여관/여인숙',
     '무도/유흥/가무',
     '물품기기대여',
     '민박/하숙',
     '법무세무회계',
     '별식/퓨전요리',
     '부동산관련서비스',
     '부동산임대',
     '부동산중개',
     '부페',
     '분식',
     '분양',
     '사무/문구/컴퓨터',
     '사진',
     '사진/광학/정밀기기소매',
     '선물/팬시/기념품',
     '세탁/가사서비스',
     '스포츠/운동',
     '시계/귀금속소매',
     '실내운동시설',
     '실외운동시설',
     '애견/애완/동물',
     '양식',
     '연구소',
     '연극/영화/극장',
     '예술품/골동품/수석/분재',
     '예식/의례/관혼상제',
     '요가/단전/마사지',
     '운동/경기용품소매',
     '운송/배달/택배',
     '운영관리시설',
     '유스호스텔',
     '유아교육',
     '유아용품',
     '유흥주점',
     '음/식료품소매',
     '음식배달서비스',
     '의복의류',
     '의약/의료품소매',
     '이/미용/건강',
     '인력/고용/용역알선',
     '일식/수산물',
     '자동차/이륜차',
     '자동차/자동차용품',
     '장례/묘지',
     '전시/관람',
     '제과제빵떡케익',
     '종교용품판매',
     '종합소매점',
     '주유소/충전소',
     '주택수리',
     '중고품소매/교환',
     '중식',
     '책/서적/도서',
     '철물/난방/건설자재소매',
     '취미/오락관련소매',
     '캠프/별장/펜션',
     '커피점/카페',
     '특수교육기관',
     '패스트푸드',
     '페인트/유리제품소매',
     '평가/개발/관리',
     '학교',
     '학문교육기타',
     '학원-보습교습입시',
     '학원-어학',
     '학원-예능취미체육',
     '학원-음악미술무용',
     '학원-자격/국가고시',
     '학원-창업취업취미',
     '학원-컴퓨터',
     '학원기타',
     '한식',
     '행사/이벤트',
     '호텔/콘도',
     '화장품소매'}




```python
# 카페만 뽑아냅니다.
df_coffee = data[data["상권업종중분류명"]=="커피점/카페"]
# index를 다시 세팅합니다.
df_coffee.index = range(len(df_coffee))
print("전국 커피 전문점 점포 수 : ", len(df_coffee))
df_coffee.head()
```

    전국 커피 전문점 점포 수 :  113705
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>키즈까페아이사랑</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>성덕동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>카페마실</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>원주시</td>
      <td>단계동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>힐링</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>원주시</td>
      <td>단구동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>드롭탑</td>
      <td>속초엑스포점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>속초시</td>
      <td>조양동</td>
    </tr>
    <tr>
      <th>4</th>
      <td>SHIMS</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>원주시</td>
      <td>행구동</td>
    </tr>
  </tbody>
</table>
</div>



#### 서울내 커피 전문점 


```python
set(data["시도명"])
```




    {'강원도',
     '경기도',
     '경상남도',
     '경상북도',
     '광주광역시',
     '대구광역시',
     '대전광역시',
     '부산광역시',
     '서울특별시',
     '세종특별자치시',
     '울산광역시',
     '인천광역시',
     '전라남도',
     '전라북도',
     '제주특별자치도',
     '충청남도',
     '충청북도'}




```python
# 카페 중에 "서울"에 위치하고 있는 점포만 뽑아냅니다.
df_seoul_coffee = data[(data["상권업종중분류명"]=="커피점/카페")&
                       (data["시도명"]=="서울특별시")]
df_seoul_coffee.index = range(len(df_seoul_coffee))
print('서울시 내 커피 전문점 점포 수 :', len(df_seoul_coffee))
df_seoul_coffee.head()
```

    서울시 내 커피 전문점 점포 수 : 22239
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>커피빈</td>
      <td>코리아대학로대명거리점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>종로구</td>
      <td>혜화동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>요거프레소</td>
      <td>쌍문점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>도봉구</td>
      <td>쌍문2동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>406번째스토브</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>양천구</td>
      <td>목1동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>로얄커피숍</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강동구</td>
      <td>성내2동</td>
    </tr>
    <tr>
      <th>4</th>
      <td>빈트리망원점</td>
      <td>망원점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>마포구</td>
      <td>망원1동</td>
    </tr>
  </tbody>
</table>
</div>



#### 전국 스타벅스


```python
# 이번엔 전국에 있는 스타벅스를 뽑아냅니다.
df_starbucks = df_coffee[df_coffee["상호명"].str.contains("스타벅스")]
df_starbucks.index = range(len(df_starbucks))
print('전국 스타벅스 점포 수 :', len(df_starbucks))
df_starbucks
```

    전국 스타벅스 점포 수 : 1613
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>스타벅스</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>원주시</td>
      <td>단계동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>스타벅스강릉안목항점</td>
      <td>강릉안목항점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>송정동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>스타벅스</td>
      <td>대명델피노리조트점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>고성군</td>
      <td>토성면</td>
    </tr>
    <tr>
      <th>3</th>
      <td>스타벅스춘천후평DT점</td>
      <td>춘천후평DT점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>춘천시</td>
      <td>후평3동</td>
    </tr>
    <tr>
      <th>4</th>
      <td>스타벅스</td>
      <td>춘천명동점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>춘천시</td>
      <td>약사명동</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1608</th>
      <td>스타벅스</td>
      <td>청주율량DT점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>청주시 청원구</td>
      <td>율량.사천동</td>
    </tr>
    <tr>
      <th>1609</th>
      <td>스타벅스</td>
      <td>충주성서점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>충주시</td>
      <td>성내.충인동</td>
    </tr>
    <tr>
      <th>1610</th>
      <td>스타벅스</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>음성군</td>
      <td>원남면</td>
    </tr>
    <tr>
      <th>1611</th>
      <td>스타벅스</td>
      <td>충주연수점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>충주시</td>
      <td>연수동</td>
    </tr>
    <tr>
      <th>1612</th>
      <td>스타벅스</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>음성군</td>
      <td>맹동면</td>
    </tr>
  </tbody>
</table>
<p>1613 rows × 7 columns</p>
</div>



#### 서울 스타벅스


```python
# 이번엔 서울에 있는 스타벅스를 뽑아냅니다.
df_seoul_starbucks = df_starbucks[df_starbucks["시도명"]=="서울특별시"]
df_seoul_starbucks.index = range(len(df_seoul_starbucks))
print('서울시 내 스타벅스 점포 수 :', len(df_seoul_starbucks))
df_seoul_starbucks.head()
```

    서울시 내 스타벅스 점포 수 : 509
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>스타벅스</td>
      <td>대학로점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>종로구</td>
      <td>이화동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>스타벅스</td>
      <td>한티점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강남구</td>
      <td>대치4동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>스타벅스</td>
      <td>동숭로아트점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>종로구</td>
      <td>이화동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>스타벅스남부터미널2점</td>
      <td>남부터미널2점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>서초구</td>
      <td>서초3동</td>
    </tr>
    <tr>
      <th>4</th>
      <td>스타벅스</td>
      <td>가로수길점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강남구</td>
      <td>신사동</td>
    </tr>
  </tbody>
</table>
</div>



#### 전국 이디야


```python
df_ediya = df_coffee[df_coffee["상호명"].str.contains("이디야")]
df_ediya.index = range(len(df_ediya))
print('전국 이디야 점포 수 :', len(df_ediya))
df_ediya
```

    전국 이디야 점포 수 : 2238
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>이디야커피</td>
      <td>원주반곡동점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>원주시</td>
      <td>반곡관설동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>이디야커피</td>
      <td>춘천제일점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>춘천시</td>
      <td>강남동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>이디야커피</td>
      <td>흥업점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>원주시</td>
      <td>흥업면</td>
    </tr>
    <tr>
      <th>3</th>
      <td>이디야커피</td>
      <td>정동진역점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>강동면</td>
    </tr>
    <tr>
      <th>4</th>
      <td>이디야커피</td>
      <td>속초동명항점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>속초시</td>
      <td>동명동</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>2233</th>
      <td>이디야커피</td>
      <td>오창2산업단지점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>청주시 청원구</td>
      <td>오창읍</td>
    </tr>
    <tr>
      <th>2234</th>
      <td>이디야커피</td>
      <td>가경중앙점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>청주시 흥덕구</td>
      <td>가경동</td>
    </tr>
    <tr>
      <th>2235</th>
      <td>이디야커피</td>
      <td>제천하소점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>제천시</td>
      <td>용두동</td>
    </tr>
    <tr>
      <th>2236</th>
      <td>이디야커피</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>증평군</td>
      <td>증평읍</td>
    </tr>
    <tr>
      <th>2237</th>
      <td>이디야커피</td>
      <td>충북대점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>청주시 서원구</td>
      <td>사창동</td>
    </tr>
  </tbody>
</table>
<p>2238 rows × 7 columns</p>
</div>



#### 서울 이디야


```python
df_seoul_ediya = df_ediya[df_ediya["시도명"]=="서울특별시"]
df_seoul_ediya.index = range(len(df_seoul_ediya))
print('서울시 내 이디야 점포 수 :', len(df_seoul_ediya))
df_seoul_ediya
```

    서울시 내 이디야 점포 수 : 474
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>이디야커피</td>
      <td>신길역점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>영등포구</td>
      <td>신길1동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>이디야커피</td>
      <td>북창동점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>중구</td>
      <td>소공동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>이디야커피</td>
      <td>라이프점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>영등포구</td>
      <td>여의동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>이디야커피</td>
      <td>시흥점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>금천구</td>
      <td>시흥2동</td>
    </tr>
    <tr>
      <th>4</th>
      <td>이디야커피양재AT점</td>
      <td>양재AT점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>서초구</td>
      <td>양재2동</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>469</th>
      <td>이디야커피</td>
      <td>이마트용산역</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>용산구</td>
      <td>한강로동</td>
    </tr>
    <tr>
      <th>470</th>
      <td>이디야잠실</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>송파구</td>
      <td>잠실3동</td>
    </tr>
    <tr>
      <th>471</th>
      <td>이디야커피</td>
      <td>신창점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>도봉구</td>
      <td>창2동</td>
    </tr>
    <tr>
      <th>472</th>
      <td>이디야커피</td>
      <td>대청역점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강남구</td>
      <td>일원2동</td>
    </tr>
    <tr>
      <th>473</th>
      <td>이디야커피</td>
      <td>전농사거리점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>동대문구</td>
      <td>전농2동</td>
    </tr>
  </tbody>
</table>
<p>474 rows × 7 columns</p>
</div>



#### 전국 커피빈 


```python
df_coffeebean = df_coffee[df_coffee["상호명"].str.contains("커피빈")]
df_coffeebean.index = range(len(df_coffeebean))
print('전국 커피빈 점포 수 :', len(df_coffeebean))
df_coffeebean
```

    전국 커피빈 점포 수 : 324
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>커피빈</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>동해시</td>
      <td>묵호동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>커피빈</td>
      <td>코리아원주AK플라자점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>원주시</td>
      <td>단계동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>커피빈</td>
      <td>현대프리미엄아울렛김포점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>경기도</td>
      <td>김포시</td>
      <td>고촌읍</td>
    </tr>
    <tr>
      <th>3</th>
      <td>커피빈</td>
      <td>중동현대백화점U-PLEX점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>경기도</td>
      <td>부천시</td>
      <td>중1동</td>
    </tr>
    <tr>
      <th>4</th>
      <td>커피빈</td>
      <td>코리아중동위브더스테이트점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>경기도</td>
      <td>부천시</td>
      <td>중2동</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>319</th>
      <td>커피빈</td>
      <td>코리아청주점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>청주시 상당구</td>
      <td>성안동</td>
    </tr>
    <tr>
      <th>320</th>
      <td>커피빈</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>제천시</td>
      <td>교동</td>
    </tr>
    <tr>
      <th>321</th>
      <td>커피빈</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>제천시</td>
      <td>중앙동</td>
    </tr>
    <tr>
      <th>322</th>
      <td>커피빈</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>제천시</td>
      <td>영서동</td>
    </tr>
    <tr>
      <th>323</th>
      <td>커피빈</td>
      <td>파비뇽점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>청주시 흥덕구</td>
      <td>봉명2.송정동</td>
    </tr>
  </tbody>
</table>
<p>324 rows × 7 columns</p>
</div>



#### 서울 커피빈 


```python
df_seoul_coffeebean = df_coffeebean[df_coffeebean["시도명"]=="서울특별시"]
df_seoul_coffeebean.index = range(len(df_seoul_coffeebean))
print('서울시 내 커피빈 점포 수 :', len(df_seoul_coffeebean))
df_seoul_coffeebean
```

    서울시 내 커피빈 점포 수 : 191
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>커피빈</td>
      <td>코리아대학로대명거리점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>종로구</td>
      <td>혜화동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>커피빈</td>
      <td>코리아선릉역점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강남구</td>
      <td>역삼1동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>커피빈코리아낙성대역점</td>
      <td>코리아낙성대역점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>관악구</td>
      <td>행운동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>커피빈</td>
      <td>코리아청담에스점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강남구</td>
      <td>청담동</td>
    </tr>
    <tr>
      <th>4</th>
      <td>커피빈</td>
      <td>코리아청담성당점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강남구</td>
      <td>청담동</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>186</th>
      <td>커피빈</td>
      <td>코리아서초동점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>서초구</td>
      <td>서초3동</td>
    </tr>
    <tr>
      <th>187</th>
      <td>커피빈코리아이태원점</td>
      <td>코리아이태원점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>용산구</td>
      <td>이태원1동</td>
    </tr>
    <tr>
      <th>188</th>
      <td>커피빈</td>
      <td>코리아을지로입구역점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>중구</td>
      <td>명동</td>
    </tr>
    <tr>
      <th>189</th>
      <td>커피빈코리아올림픽공원</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>송파구</td>
      <td>오륜동</td>
    </tr>
    <tr>
      <th>190</th>
      <td>커피빈코리아CBTL서울</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>중구</td>
      <td>회현동</td>
    </tr>
  </tbody>
</table>
<p>191 rows × 7 columns</p>
</div>



#### 전국 투썸 


```python
df_2some = df_coffee[df_coffee["상호명"].str.contains("투썸플레이스")]
df_2some.index = range(len(df_2some))
print('전국 투썸 점포 수 :', len(df_2some))
df_2some
```

    전국 투썸 점포 수 : 1101
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>투썸플레이스</td>
      <td>춘천명동점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>춘천시</td>
      <td>조운동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>투썸플레이스</td>
      <td>강릉포남점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>포남1동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>투썸플레이스</td>
      <td>춘천퇴계점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>춘천시</td>
      <td>퇴계동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>투썸플레이스</td>
      <td>소양강댐점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>춘천시</td>
      <td>신북읍</td>
    </tr>
    <tr>
      <th>4</th>
      <td>투썸플레이스</td>
      <td>용평리조트점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>평창군</td>
      <td>대관령면</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>1096</th>
      <td>투썸플레이스</td>
      <td>진천터미널점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>진천군</td>
      <td>진천읍</td>
    </tr>
    <tr>
      <th>1097</th>
      <td>투썸플레이스</td>
      <td>신봉네거리점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>청주시 흥덕구</td>
      <td>운천.신봉동</td>
    </tr>
    <tr>
      <th>1098</th>
      <td>투썸플레이스</td>
      <td>비하점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>청주시 흥덕구</td>
      <td>강서1동</td>
    </tr>
    <tr>
      <th>1099</th>
      <td>투썸플레이스</td>
      <td>청주예술의전당DT점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>청주시 흥덕구</td>
      <td>봉명1동</td>
    </tr>
    <tr>
      <th>1100</th>
      <td>투썸플레이스</td>
      <td>씨제이비센터점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>청주시 서원구</td>
      <td>산남동</td>
    </tr>
  </tbody>
</table>
<p>1101 rows × 7 columns</p>
</div>



#### 서울 투썸 


```python
df_seoul_2some = df_2some[df_2some["시도명"]=="서울특별시"]
df_seoul_2some.index = range(len(df_seoul_2some))
print('서울시 내 투썸 점포 수 :', len(df_seoul_2some))
df_seoul_2some
```

    서울시 내 투썸 점포 수 : 272
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>투썸플레이스</td>
      <td>서울타워점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>용산구</td>
      <td>용산2가동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>투썸플레이스서울대역중앙점</td>
      <td>서울대역중앙점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>관악구</td>
      <td>중앙동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>투썸플레이스</td>
      <td>씨제이프레시웨이강남세브란스병원점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강남구</td>
      <td>도곡1동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>투썸플레이스</td>
      <td>LG광화문빌딩점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>종로구</td>
      <td>사직동</td>
    </tr>
    <tr>
      <th>4</th>
      <td>투썸플레이스</td>
      <td>가락시장역점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>송파구</td>
      <td>가락본동</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>267</th>
      <td>투썸플레이스</td>
      <td>양천신목로점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>양천구</td>
      <td>신정2동</td>
    </tr>
    <tr>
      <th>268</th>
      <td>투썸플레이스</td>
      <td>삼성도심공항점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강남구</td>
      <td>삼성1동</td>
    </tr>
    <tr>
      <th>269</th>
      <td>투썸플레이스</td>
      <td>가산하이엔드클래식점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>금천구</td>
      <td>가산동</td>
    </tr>
    <tr>
      <th>270</th>
      <td>투썸플레이스</td>
      <td>목동하이페리온점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>양천구</td>
      <td>목1동</td>
    </tr>
    <tr>
      <th>271</th>
      <td>투썸플레이스</td>
      <td>강동명일점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강동구</td>
      <td>천호1동</td>
    </tr>
  </tbody>
</table>
<p>272 rows × 7 columns</p>
</div>



#### 전국 빽다방 


```python
df_bbaek = df_coffee[df_coffee["상호명"].str.contains("빽다방")]
df_bbaek.index = range(len(df_bbaek))
print('전국 빽다방 점포 수 :', len(df_bbaek))
df_bbaek
```

    전국 빽다방 점포 수 : 567
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>빽다방</td>
      <td>춘천명동씨지브이점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>춘천시</td>
      <td>조운동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>빽다방</td>
      <td>춘천석사CGV점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>춘천시</td>
      <td>석사동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>빽다방동해천곡점</td>
      <td>동해천곡점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>동해시</td>
      <td>천곡동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>빽다방</td>
      <td>원주중앙1호점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>원주시</td>
      <td>일산동</td>
    </tr>
    <tr>
      <th>4</th>
      <td>빽다방</td>
      <td>삼척대학로점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>삼척시</td>
      <td>성내동</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>562</th>
      <td>빽다방</td>
      <td>청주분평중앙점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>청주시 서원구</td>
      <td>분평동</td>
    </tr>
    <tr>
      <th>563</th>
      <td>빽다방</td>
      <td>충주신연수리첼점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>충주시</td>
      <td>연수동</td>
    </tr>
    <tr>
      <th>564</th>
      <td>빽다방</td>
      <td>청주율량중앙점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>청주시 청원구</td>
      <td>오근장동</td>
    </tr>
    <tr>
      <th>565</th>
      <td>빽다방</td>
      <td>충북대중문점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>청주시 서원구</td>
      <td>사창동</td>
    </tr>
    <tr>
      <th>566</th>
      <td>빽다방</td>
      <td>서원대점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>청주시 서원구</td>
      <td>모충동</td>
    </tr>
  </tbody>
</table>
<p>567 rows × 7 columns</p>
</div>



#### 서울 빽다방 


```python
df_seoul_bbaek = df_bbaek[df_bbaek["시도명"]=="서울특별시"]
df_seoul_bbaek.index = range(len(df_seoul_bbaek))
print('서울시 내 빽다방 점포 수 :', len(df_seoul_bbaek))
df_seoul_bbaek
```

    서울시 내 빽다방 점포 수 : 129
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>더본코리아_빽다방등촌</td>
      <td>성당점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강서구</td>
      <td>등촌3동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>스시마이우강남역빽다방</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>서초구</td>
      <td>서초4동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>빽다방공덕새창로점</td>
      <td>공덕새창로점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>마포구</td>
      <td>도화동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>빽다방서초우성점</td>
      <td>서초우성점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>서초구</td>
      <td>서초2동</td>
    </tr>
    <tr>
      <th>4</th>
      <td>빽다방잠실장미상가</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>송파구</td>
      <td>잠실6동</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>124</th>
      <td>빽다방PAIKSCOFFEE</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>마포구</td>
      <td>도화동</td>
    </tr>
    <tr>
      <th>125</th>
      <td>빽다방</td>
      <td>강남역지하도점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강남구</td>
      <td>역삼1동</td>
    </tr>
    <tr>
      <th>126</th>
      <td>빽다방강남소망점</td>
      <td>강남소망점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강남구</td>
      <td>압구정동</td>
    </tr>
    <tr>
      <th>127</th>
      <td>빽다방</td>
      <td>원효용문시장점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>용산구</td>
      <td>원효로1동</td>
    </tr>
    <tr>
      <th>128</th>
      <td>빽다방</td>
      <td>방이시장점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>송파구</td>
      <td>방이2동</td>
    </tr>
  </tbody>
</table>
<p>129 rows × 7 columns</p>
</div>



#### 전국 할리스 


```python
df_hollys = df_coffee[df_coffee["상호명"].str.contains("할리스")]
df_hollys.index = range(len(df_hollys))
print('전국 할리스 점포 수 :', len(df_hollys))
df_hollys
```

    전국 할리스 점포 수 : 707
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>할리스커피전문점</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>강릉시</td>
      <td>중앙동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>할리스커피</td>
      <td>원주점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>원주시</td>
      <td>단구동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>할리스</td>
      <td>에프엔비원주점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>원주시</td>
      <td>단구동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>할리스커피</td>
      <td>중앙점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>원주시</td>
      <td>중앙동</td>
    </tr>
    <tr>
      <th>4</th>
      <td>할리스커피동해묵호점</td>
      <td>동해묵호점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>동해시</td>
      <td>천곡동</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>702</th>
      <td>할리스커피</td>
      <td>괴산휴게소창원방면</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>괴산군</td>
      <td>장연면</td>
    </tr>
    <tr>
      <th>703</th>
      <td>할리스</td>
      <td>명암레이크</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>청주시 상당구</td>
      <td>용담.명암.산성동</td>
    </tr>
    <tr>
      <th>704</th>
      <td>할리스커피</td>
      <td>청주NC백화점CGV점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>청주시 흥덕구</td>
      <td>가경동</td>
    </tr>
    <tr>
      <th>705</th>
      <td>할리스커피</td>
      <td>청주성모병원점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>청주시 청원구</td>
      <td>오근장동</td>
    </tr>
    <tr>
      <th>706</th>
      <td>할리스</td>
      <td>충북진천점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>진천군</td>
      <td>진천읍</td>
    </tr>
  </tbody>
</table>
<p>707 rows × 7 columns</p>
</div>



#### 서울 할리스 


```python
df_seoul_hollys = df_hollys[df_hollys["시도명"]=="서울특별시"]
df_seoul_hollys.index = range(len(df_seoul_hollys))
print('서울시 내 할리스 점포 수 :', len(df_seoul_hollys))
df_seoul_hollys
```

    서울시 내 할리스 점포 수 : 200
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>할리스커피남부터미널</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>서초구</td>
      <td>서초1동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>할리스</td>
      <td>백석예술대점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>서초구</td>
      <td>방배3동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>할리스</td>
      <td>국기원점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강남구</td>
      <td>역삼1동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>할리스커피KBS본관점</td>
      <td>KBS본관점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>영등포구</td>
      <td>여의동</td>
    </tr>
    <tr>
      <th>4</th>
      <td>할리스커피</td>
      <td>선릉점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강남구</td>
      <td>대치4동</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>195</th>
      <td>할리스에프앤비</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>용산구</td>
      <td>원효로1동</td>
    </tr>
    <tr>
      <th>196</th>
      <td>인디스에프앤디할리스커피</td>
      <td>광화문점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>중구</td>
      <td>명동</td>
    </tr>
    <tr>
      <th>197</th>
      <td>할리스</td>
      <td>응암점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>은평구</td>
      <td>신사1동</td>
    </tr>
    <tr>
      <th>198</th>
      <td>할리스커피</td>
      <td>반포점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>서초구</td>
      <td>반포4동</td>
    </tr>
    <tr>
      <th>199</th>
      <td>할리스남영점</td>
      <td>남영점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>용산구</td>
      <td>이촌1동</td>
    </tr>
  </tbody>
</table>
<p>200 rows × 7 columns</p>
</div>



#### 전국 메가커피


```python
df_mega = df_coffee[df_coffee["상호명"].str.contains("메가커피")]
df_mega.index = range(len(df_mega))
print('전국 메가커피 점포 수 :', len(df_mega))
df_mega
```

    전국 메가커피 점포 수 : 481
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>메가커피인제점</td>
      <td>인제점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>인제군</td>
      <td>인제읍</td>
    </tr>
    <tr>
      <th>1</th>
      <td>메가커피</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>춘천시</td>
      <td>후평3동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>메가커피</td>
      <td>와수리점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>철원군</td>
      <td>서면</td>
    </tr>
    <tr>
      <th>3</th>
      <td>메가커피</td>
      <td>화천사창리점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>화천군</td>
      <td>사내면</td>
    </tr>
    <tr>
      <th>4</th>
      <td>메가커피철원문혜리점</td>
      <td>철원문혜리점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>강원도</td>
      <td>철원군</td>
      <td>갈말읍</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>476</th>
      <td>메가커피</td>
      <td>옥천점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>옥천군</td>
      <td>옥천읍</td>
    </tr>
    <tr>
      <th>477</th>
      <td>메가커피</td>
      <td>청주오창2산업단지점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>청주시 청원구</td>
      <td>오창읍</td>
    </tr>
    <tr>
      <th>478</th>
      <td>메가커피</td>
      <td>청주테크노폴리스점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>청주시 흥덕구</td>
      <td>강서2동</td>
    </tr>
    <tr>
      <th>479</th>
      <td>메가커피</td>
      <td>청주지웰시티점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>청주시 흥덕구</td>
      <td>복대1동</td>
    </tr>
    <tr>
      <th>480</th>
      <td>메가커피</td>
      <td>금곡중앙점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>충청북도</td>
      <td>진천군</td>
      <td>진천읍</td>
    </tr>
  </tbody>
</table>
<p>481 rows × 7 columns</p>
</div>



#### 서울 메가커피 


```python
df_seoul_mega = df_mega[df_mega["시도명"]=="서울특별시"]
df_seoul_mega.index = range(len(df_seoul_mega))
print('서울시 내 메가커피 점포 수 :', len(df_seoul_mega))
df_seoul_mega
```

    서울시 내 메가커피 점포 수 : 86
    




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>상호명</th>
      <th>지점명</th>
      <th>상권업종대분류명</th>
      <th>상권업종중분류명</th>
      <th>시도명</th>
      <th>시군구명</th>
      <th>행정동명</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>0</th>
      <td>메가커피미아</td>
      <td>수유시장점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강북구</td>
      <td>미아동</td>
    </tr>
    <tr>
      <th>1</th>
      <td>메가커피</td>
      <td>홍대점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>마포구</td>
      <td>서교동</td>
    </tr>
    <tr>
      <th>2</th>
      <td>메가커피</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>동대문구</td>
      <td>용신동</td>
    </tr>
    <tr>
      <th>3</th>
      <td>메가커피</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>금천구</td>
      <td>가산동</td>
    </tr>
    <tr>
      <th>4</th>
      <td>메가커피</td>
      <td>숙대입구역점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>용산구</td>
      <td>남영동</td>
    </tr>
    <tr>
      <th>...</th>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
      <td>...</td>
    </tr>
    <tr>
      <th>81</th>
      <td>메가커피등촌</td>
      <td>성당점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>강서구</td>
      <td>등촌3동</td>
    </tr>
    <tr>
      <th>82</th>
      <td>메가커피</td>
      <td>신대방삼거리역점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>동작구</td>
      <td>대방동</td>
    </tr>
    <tr>
      <th>83</th>
      <td>메가커피</td>
      <td>NaN</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>용산구</td>
      <td>한강로동</td>
    </tr>
    <tr>
      <th>84</th>
      <td>메가커피</td>
      <td>은평뉴타운점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>은평구</td>
      <td>진관동</td>
    </tr>
    <tr>
      <th>85</th>
      <td>메가커피</td>
      <td>목동파라곤점</td>
      <td>음식</td>
      <td>커피점/카페</td>
      <td>서울특별시</td>
      <td>양천구</td>
      <td>목1동</td>
    </tr>
  </tbody>
</table>
<p>86 rows × 7 columns</p>
</div>



## 3. 커피전문점 별 비율 비교하기 (주요 브랜드 위주로)

**2020년 12월 기준 커피전문점 평판 순위**


(source : https://www.futurekorea.co.kr/news/articleView.html?idxno=125637)

1. 스타벅스
2. 투썸플레이스
3. 이디야
4. 메가커피
5. 커피빈

**변수**

- 전체 점포 : data
- 전체/서울 커피전문점 : df_coffee / df_seoul_starbucks



- 전체/서울 스타벅스 : df_starbucks / df_seoul_starbucks
- 전체/서울 이디야 : df_ediya / df_seoul_ediya
- 전체/서울 커피빈 : df_coffeebean / df_seoul_coffeebean
- 전체/서울 투썸플레이스 : df_2some / df_seoul_2some
- 전체/서울 빽다방 : df_bbaek / df_seoul_bbaek
- 전체/서울 할리스 : df_hollys / df_seoul_hollys
- 전체/서울 메가커피 : df_mega / df_seoul_mega

### 1) 전체 커피전문점 내 주요 커피브랜드 입점 비율 


```python
print("**** 전국 커피전문점중 주요 5대 커피브랜드 입점 비율 ****")
print("주요 5대 커피브랜드 전국 입점 비율 : %.3f%%"\
     % ((len(df_starbucks)+len(df_2some)+len(df_ediya)+len(df_mega)+len(df_coffeebean))
      / len(df_coffee)*100))
print("1. 스타벅스 : %.3f%%" % (len(df_starbucks)/len(df_coffee)*100))
print("2. 투썸플레이스 : %.3f%%" % (len(df_2some)/len(df_coffee)*100))
print("3. 이디야 : %.3f%%" % (len(df_ediya)/len(df_coffee)*100))
print("4. 메가커피 : %.3f%%" % (len(df_mega)/len(df_coffee)*100))
print("5. 커피빈 : %.3f%%" % (len(df_coffeebean)/len(df_coffee)*100))
```

    **** 전국 커피전문점중 주요 5대 커피브랜드 입점 비율 ****
    주요 5대 커피브랜드 전국 입점 비율 : 5.063%
    1. 스타벅스 : 1.419%
    2. 투썸플레이스 : 0.968%
    3. 이디야 : 1.968%
    4. 메가커피 : 0.423%
    5. 커피빈 : 0.285%
    

### 2) 서울 커피전문점 내 주요 커피브랜드 입점 비율 


```python
print("스타벅스 : %.3f%%" % (len(df_seoul_starbucks)/len(df_seoul_coffee)*100))
print("이디야 : %.3f%%" % (len(df_seoul_ediya)/len(df_seoul_coffee)*100))
print("커피빈 : %.3f%%" % (len(df_seoul_coffeebean)/len(df_seoul_coffee)*100))
print("투썸플레이스 : %.3f%%" % (len(df_seoul_2some)/len(df_seoul_coffee)*100))
print("빽다방 : %.3f%%" % (len(df_seoul_bbaek)/len(df_seoul_coffee)*100))
print("할리스 : %.3f%%" % (len(df_seoul_hollys)/len(df_seoul_coffee)*100))
print("메가커피 : %.3f%%" % (len(df_seoul_mega)/len(df_seoul_coffee)*100))
```

    스타벅스 : 2.289%
    이디야 : 2.131%
    커피빈 : 0.859%
    투썸플레이스 : 1.223%
    빽다방 : 0.580%
    할리스 : 0.899%
    메가커피 : 0.387%
    

### 3) 각 커피브랜드별 서울 입점 비율 


```python
print("**** 주요 5대 커피브랜드별 서울 입점 비율 ****")
print("1. 스타벅스 : %.3f%%" % (len(df_seoul_starbucks)/len(df_starbucks)*100))
print("2. 투썸플레이스 : %.3f%%" % (len(df_seoul_2some)/len(df_2some)*100))
print("3. 이디야 : %.3f%%" % (len(df_seoul_ediya)/len(df_ediya)*100))
print("4. 메가커피 : %.3f%%" % (len(df_seoul_mega)/len(df_mega)*100))
print("5. 커피빈 : %.3f%%" % (len(df_seoul_coffeebean)/len(df_coffeebean)*100))
```

    **** 주요 5대 커피브랜드별 서울 입점 비율 ****
    1. 스타벅스 : 31.556%
    2. 투썸플레이스 : 24.705%
    3. 이디야 : 21.180%
    4. 메가커피 : 17.879%
    5. 커피빈 : 58.951%
    


```python
# 각 구별로 스타벅스가 얼마나 있는지 확인합니다.
starbucks_gu = df_seoul_starbucks.groupby('시군구명')['상호명'].count().to_frame().sort_values(by='상호명', ascending=False)
starbucks_gu = starbucks_gu.reset_index()
starbucks_gu = starbucks_gu.set_index('시군구명')
starbucks_gu
```




<div>
<style scoped>
    .dataframe tbody tr th:only-of-type {
        vertical-align: middle;
    }

    .dataframe tbody tr th {
        vertical-align: top;
    }

    .dataframe thead th {
        text-align: right;
    }
</style>
<table border="1" class="dataframe">
  <thead>
    <tr style="text-align: right;">
      <th></th>
      <th>상호명</th>
    </tr>
    <tr>
      <th>시군구명</th>
      <th></th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <th>강남구</th>
      <td>87</td>
    </tr>
    <tr>
      <th>중구</th>
      <td>48</td>
    </tr>
    <tr>
      <th>서초구</th>
      <td>44</td>
    </tr>
    <tr>
      <th>송파구</th>
      <td>33</td>
    </tr>
    <tr>
      <th>종로구</th>
      <td>31</td>
    </tr>
    <tr>
      <th>마포구</th>
      <td>28</td>
    </tr>
    <tr>
      <th>영등포구</th>
      <td>26</td>
    </tr>
    <tr>
      <th>용산구</th>
      <td>21</td>
    </tr>
    <tr>
      <th>서대문구</th>
      <td>21</td>
    </tr>
    <tr>
      <th>강서구</th>
      <td>19</td>
    </tr>
    <tr>
      <th>광진구</th>
      <td>17</td>
    </tr>
    <tr>
      <th>강동구</th>
      <td>15</td>
    </tr>
    <tr>
      <th>성북구</th>
      <td>15</td>
    </tr>
    <tr>
      <th>금천구</th>
      <td>12</td>
    </tr>
    <tr>
      <th>양천구</th>
      <td>12</td>
    </tr>
    <tr>
      <th>노원구</th>
      <td>11</td>
    </tr>
    <tr>
      <th>동작구</th>
      <td>11</td>
    </tr>
    <tr>
      <th>관악구</th>
      <td>11</td>
    </tr>
    <tr>
      <th>구로구</th>
      <td>9</td>
    </tr>
    <tr>
      <th>은평구</th>
      <td>9</td>
    </tr>
    <tr>
      <th>성동구</th>
      <td>8</td>
    </tr>
    <tr>
      <th>동대문구</th>
      <td>7</td>
    </tr>
    <tr>
      <th>중랑구</th>
      <td>7</td>
    </tr>
    <tr>
      <th>강북구</th>
      <td>5</td>
    </tr>
    <tr>
      <th>도봉구</th>
      <td>2</td>
    </tr>
  </tbody>
</table>
</div>




```python
# 시각화를 위한 라이브러리를 불러옵니다.
import seaborn as sns
import matplotlib.pyplot as plt
import platform

from matplotlib import font_manager, rc
%matplotlib inline
```


```python
#### Windows10 사용자는 해당 코드가 필요없습니다!

# macos에서 사용가능한 한글 글꼴 확인 코드
[f.name for f in font_manager.fontManager.ttflist if 'Neo' in f.name]
```




    []




```python
## 운영체제별 글꼴 세팅

path = "c:/Windows/Fonts/malgun.ttf"
if platform.system() == 'Darwin':
    font_name = 'Apple SD Gothic Neo'
    rc('font', family='Apple SD Gothic Neo')
elif platform.system() == 'Windows':
    font_name = font_manager.FontProperties(fname=path).get_name()
    rc('font', family=font_name)
else:
    font_name = font_manager.FontProperties(fname="/usr/share/fonts/nanumfont/NanumGothic.ttf")
    rc('font', family="NanumGothic")
```


```python
# 주요 5대 커피브랜드 서울 입점 비율을 시각화합니다.
starbucks_rate = (len(df_seoul_starbucks)/len(df_starbucks)*100)
twosome_rate = (len(df_seoul_2some)/len(df_2some)*100)
ediya_rate = (len(df_seoul_ediya)/len(df_ediya)*100)
mega_rate = (len(df_seoul_mega)/len(df_mega)*100)
coffeebean_rate = (len(df_seoul_coffeebean)/len(df_coffeebean)*100)

X = ["스타벅스","투썸","이디야","메가커피","커피빈"]
Y = [starbucks_rate,twosome_rate,ediya_rate,mega_rate,coffeebean_rate]

plt.figure(figsize=(12,12))
plt.title("주요 5대 커피브랜드 서울 입점 비율", fontdict={"fontsize":20})
sns.barplot(x=X,y=Y)
plt.savefig("coffee_barplot.png")
plt.show()
```


    
![png](output_59_0.png)
    



```python
starbucks_rate2 = (len(df_seoul_starbucks)/len(df_seoul_coffee)*100)
twosome_rate2 = (len(df_seoul_2some)/len(df_seoul_coffee)*100)
ediya_rate2 = (len(df_seoul_ediya)/len(df_seoul_coffee)*100)
mega_rate2 = (len(df_seoul_mega)/len(df_seoul_coffee)*100)
coffeebean_rate2 = (len(df_seoul_coffeebean)/len(df_seoul_coffee)*100)

X = ["스타벅스","투썸","이디야","메가커피","커피빈"]
Y = [starbucks_rate2,twosome_rate2,ediya_rate2,mega_rate2,coffeebean_rate2]

plt.figure(figsize=(12,12))
plt.title("주요 5대 커피브랜드 서울 입점 비율", fontdict={"fontsize":20})
sns.barplot(x=X,y=Y)
plt.savefig("coffee_barplot2.png")
plt.show()
```


    
![png](output_60_0.png)
    


### (Challenge) More Prettier!


```python
# 위의 barplot을 seaborn을 이용하여 더욱 멋지게 시각화해보세요!
```

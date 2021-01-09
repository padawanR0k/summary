## MySQL에서의 공간데이터

### GIS (Geographical Information System) 개념
- 지표면에 존재하는 각종 자연물과 인공물에 대한 위치, 속성정보를 컴퓨터에 입력 후, 이를 연계시켜 각종 의사결정, 산업활동에 효율적으로 지원할 수 있도록 만든 첨단 정보시스템
	- 흔히 사용하는 네이버지도, 네비게이션, 구글어스등이 있다.
- MySQL은 5.0버전 이후 부터 공간 데이터(Geometry)저장을 지원하기 시작했다.
- 공간데이터의 표현
	- 점 (POINT)
		- 1개의 x, y 좌표
	- 선 (LINESTRING)
		- N개의 x, y 좌표
	- 면 (POLYGON)
		- N개의 x, y 좌표 (단 첫번째 점이 마지막 점과 일치)
- GEOMETRY 데이터 타입
	- MySQL에서는 공간 데이터 정보를 저장할 수 있는 GEOMETRY타입을 지원한다.

#### 공간 데이터 형식의 함수
|함수명|설명|비고|
|--|--|--|
|ST_GeoFromText()|문자열을 공간 데이터 형식으로 변환||
|ST_AsText()|공간 데이터 형식을 문자열로 변환||
|ST_Length()|LineString의 길이를 구한다.||
|ST_Area()|Polygon의 면적을 구한다.||
|ST_Intersects()|두 도형의 교차 여부를 확인한다.|0 False 1 True|
|ST_Buffer()|도형에서부터 주어진 거리만큼 떨어진 좌표 집합을 구한다.||
|ST_Contains()|한 도형이 다른 도형에 들어있는지 확인한다|0 False 1 True|
|ST_Distance()|두 도형 사이의 거리를 구한다|0 |
|ST_Union | 두 도형을 합한 결과 좌표 집합을 구한다.||
|ST_Union | 두 도형이 교차하는 좌표집합을 구한다.||



### mysql workbench Tip
- 쿼리 결과에 GEOMETRY 형식이 포함된 경우, ui하단 우측에서 [Spatial View]를 클릭하면 시각화하여 보여준다.
-
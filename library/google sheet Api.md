## 따라하기

---

[구글코드랩]( https://codelabs.developers.google.com/codelabs/sheets-api/#0)

## api 생성

---

https://console.developers.google.com/apis

1. sheet 라이브러리 검색 

2. 사용자인증정보 추가 (sheet소유계정)

3. 사용하려는 api KEY 생성

4. 프로젝트에 연결

5. index.html에 스크립트 추가

   `<script type="text/javascript" src="https://apis.google.com/js/api.js"></script>`

CRA-ts를 사용했음



`gga.ts`

```tsx

const CLIENT_ID = '본인 클라이언트 아이디';

const DISCOVERY_DOCS = ['https://www.googleapis.com/discovery/v1/apis/sheets/v4/rest']; // 사용하는 api

const SCOPES = 'https://www.googleapis.com/auth/spreadsheets'; // 내 계정의 auth가 사용될 api 범위

const APIKEY = '내가 생성한 app key';

declare var gapi: any;

class Gga {

    constructor() {

        console.log(gapi);
        // this.initClient();
        gapi.load('client:auth2', this.initClient);
    }

    public initClient = () => {

        gapi.client.init({
            apiKey: APIKEY,

            discoveryDocs: DISCOVERY_DOCS,
            clientId: CLIENT_ID,
            scope: SCOPES,

        }).then(() => {
            console.log(gapi.auth2);
            gapi.auth2.getAuthInstance().isSignedIn.listen(this.updateSigninStatus);
            this.updateSigninStatus(gapi.auth2.getAuthInstance().isSignedIn.get());
            console.log('abcd');
        });
    }

    public getGses = () => {
        return gapi.auth2.getAuthInstance().isSignedIn.get();
    }

    public updateSigninStatus = (isSignedIn: any) => {
        console.log(isSignedIn);
        let mauth = false;
        if (isSignedIn === true) {
            mauth = true;
        } else {
            mauth = false;
        }
        console.log(mauth);
        return mauth;
    }

    public login = () => {
        console.log(gapi.auth2);
        gapi.auth2.getAuthInstance().signIn();
    }

    public logout = () => {
        gapi.auth2.getAuthInstance().signOut();
    }

    public makeSpreadSheet = () => {
        const spreadsheetBody = {
            // TODO: Add desired properties to the request body.
        };

        const request = gapi.client.sheets.spreadsheets.create({}, spreadsheetBody);
        request.then((response: any) => {
            // TODO: Change code below to process the `response` object:
            console.log(response);

            if (response.status === 200) {
                alert('스프레드 시트가 생성되었습니다.');
                console.log(response.result.spreadsheetId);
                console.log(response.result.spreadsheetUrl);
            }

        }, (reason: any) => {
            console.error('error: ' + reason.result.error.message);
        });
    }

    public insertSpreadSheet = (spreadsheetId: any, value: any) => {
        console.log(value);
        const params = {
            spreadsheetId,  // TODO: Update placeholder value.
            range: 'a3',
            responseValueRenderOption: 'FORMATTED_VALUE',
            insertDataOption: 'OVERWRITE',
            valueInputOption: 'RAW',
            responseDateTimeRenderOption: 'FORMATTED_STRING',
            includeValuesInResponse: true
        };

        const valueRangeBody = {
            // TODO: Add desired properties to the request body.
            'values': value
        };

        const request = gapi.client.sheets.spreadsheets.values.append(params, valueRangeBody);
        request.then((response: any) => {
            // TODO: Change code below to process the `response` object:
            console.log(response.result);
        }, (reason: any) => {
            console.error('error: ' + reason.result.error.message);
        });
    }

    public getSpreadSheet = (spreadsheetId: any) => {

        const params = {
            spreadsheetId,
        };

        // var valueRangeBody = {
        //                   // TODO: Add desired properties to the request body.
        //                   "values": value
        //                 };

        const request = gapi.client.sheets.spreadsheets.get(params);
        request.then((response: any) => {
            // TODO: Change code below to process the `response` object:
            console.log(response.result);
            return response.result;
        }, (reason: any) => {
            console.error('error: ' + reason.result.error.message);
            return reason.result;
        });
    }

    /**
     * @param spreadsheetId 시트pk
     * @param sheet 시트명:범위
     */
    public getSheet = (spreadsheetId: string, sheet: string) => {

        const params = {
            spreadsheetId,
            ranges: sheet
        };

        // batchGet	GET / v4 / spreadsheets / { spreadsheetId } / values: batchGet
        // Returns one or more ranges of values from a spreadsheet.
        // 특정 스프레드시트에서 하나 이상의 범위를 긁어서 가져온다.
        const request = gapi.client.sheets.spreadsheets.values.batchGet(params);
        request.then((response: any) => {
            // TODO: Change code below to process the `response` object:
            console.log(response);

            return response.result;
        }, (reason: any) => {
            console.error('error: ' + reason.result.error.message);
            return reason.result;
        });
    }

}

export default Gga;
```



`googleSheet.tsx`

```tsx
import * as React from 'react';
import { Card, CardHeader, CardBody, Container, Table, ButtonGroup, Input } from 'reactstrap';
import './GoogleSheet.scss';
import axios from 'axios';
import Mtable from 'src/component/Table/Mtable';
import Gga from 'src/func/gga';
import { Button, Select, MenuItem } from '@material-ui/core';

declare var google: any;
interface DefaultProps {
    location: any;
}

interface DefaultState {
    elist: any[];
    open: boolean;
    month: any[];
    orderTo: number;
    orderdList: any[];
    valueRanges: any;
}

class GoogleSheet extends React.Component<DefaultProps, DefaultState> {
    public sheetPK = '1cZ7P4rMa1CkT2OxNN4Alvsh19IVxrihiMKSHJj8BaIE'; // 시트의 pk
    public op = false;
    public textInput: any;
    public gga: Gga;

    public constructor(props: DefaultProps) {
        super(props);

        this.state = {
            valueRanges: null,
            elist: [],
            orderdList: [],
            month: [1, 2, 3, 4, 5, 6, 7, 8, 9, 10],
            orderTo: 0,
            open: false
        };

        this.open = this.open.bind(this);
        this.close = this.close.bind(this);
    }

    public componentDidMount() {
        google.charts.load('current', { 'packages': ['corechart'] });

        (async () => {
        })();
        this.gga = new Gga();
    }

    public open = (e: any) => {
        console.log('abc');
        console.log(this.gga);
        this.gga.login();
    }

    public confirm = (e: any) => {
        const a = this.gga.getGses();
        console.log(a);
    }

    public loadData = (e: any) => {
        const a = this.gga.getGses();
        console.log(a);
        if (a) {
            // 해당 고유값을 가진 스프레드시트 프로젝트 정보를 가져온다.
            const b = this.gga.getSpreadSheet(this.sheetPK);
            console.log(b);
        }
    }

    public getData = async () => {
        const data: any = await this.gga.getSheet(this.sheetPK, `'시트1'!C2:M25`, 0);
        console.log(data);

        data.values = data.values.map((d: any) => {
            const arr = d.map((a: any, i: number) => {
                if (i !== 0) {
                    a = parseInt(a, 10);
                }
                return a;
            });
            return arr;
        });
        await this.setState({
            valueRanges: data
        });
    }

    public close = (e: any) => {

    }

    public typeToString = (type: string) => {
        const obj = {
            a: 탑승신청',
            b: '탑승중',
            c: '탑승취소',
            d: '결제대기',
            e: '탑승대기',
        };
        return obj[type];
    }

    public handleChange = ({ target: {value} }: React.ChangeEvent<HTMLSelectElement>) => {
        this.setState({ orderTo: parseInt(value, 10) });
    }

    // 월 탑승자수로 정렬
    public orderList = () => {
        if (this.state.valueRanges.values.length) {
            const Arr = [...this.state.valueRanges.values];
            Arr.sort((a, b) => {
                const month = this.state.orderTo;
                return b[month] - a[month];
            });
            this.setState({
                valueRanges: Object.assign(this.state.valueRanges, {values: Arr})
            });
        } else {
            alert('먼저 가져와주세요.');
        }
    }

    // 지정범위 업데이트
    public update = () => {
        this.gga.updateValue(this.sheetPK, `'시트1'!C2:M25`, 0, this.state.valueRanges);
    }

    // 새 파일 생성
    public create = () => {
        return this.gga.makeSpreadSheet();
    }

    // 새 파일 생성 후  복사
    public createAndPaste = async () => {
        this.create().then((res: any) => {
            console.log(res);

            this.gga.insertSpreadSheet(res.spreadsheetId, this.state.valueRanges, `'시트1'!C2:M25`);
        });
    }

    public drawChart = () => {
        google.charts.load('current', { 'packages': ['corechart'] });
        google.charts.setOnLoadCallback(drawChart);

        const chartData = [['호선', '1월', '2월', '3월', '4월', '5월', '6월', '7월', '8월', '9월', '10월'], ...this.state.valueRanges.values];

        function drawChart() {
            console.log(chartData);
            const data = google.visualization.arrayToDataTable(chartData);

            const options = {
                title: '호선',
                hAxis: { minValue: 0 },
                vAxis: { title: '호선', titleTextStyle: { color: '#333' } },
            };

            const chart = new google.visualization.AreaChart(document.getElementById('chart_div'));
            chart.draw(data, options);
        }

    }

    public render() {
        console.log(this.op);
        return (
            <Container fluid>
                <ButtonGroup>
                    <Button color="secondary" onClick={this.open}>꾸욱</Button>
                    <Button color="secondary" onClick={this.confirm}>확인</Button>
                    <Button color="secondary" onClick={this.loadData}>확인</Button>
                    <Button color="primary" onClick={this.getData}>가져오기!</Button>
                    <Select value={this.state.orderTo}
                        onChange={this.handleChange}
                        inputProps={{
                            name: 'age',
                            id: 'age-simple',
                        }}>
                        {this.state.month.map((m: number, i: number) => <MenuItem value={m} key={i}>{m}월</MenuItem>)}
                    </Select>
                    <Button color="primary" onClick={this.orderList}>기준으로 정렬하기</Button>
                    <Button color="primary" onClick={this.update}>해당 시트 내보내기</Button>
                    <Button color="primary" onClick={this.create}>새 시트 만들기</Button>
                    <Button color="primary" onClick={this.createAndPaste}>새 시트 만들고 복사하기</Button>
                    <Button color="primary" onClick={this.drawChart}>Google Chart</Button>

                </ButtonGroup>
                    <Table striped>
                        <thead key={0}>
                            <tr>
                                <th>호선</th>
                                {this.state.month.map((m: number, i: number) => <th key={i}>{m}월</th>)}
                            </tr>
                        </thead>
                        {this.state.valueRanges ? this.state.valueRanges.values.map((c: any, k: number) => {
                            return <tbody key={k}>
                                    <tr>
                                        {c.map((tuple: string, i: number) => <th key={i}>{tuple}</th>)}
                                    </tr>
                                </tbody>;
                        }) : null}
                    </Table>
                <div id="chart_div" style={{width: '100%', height: '1000px'}}></div>
            </Container>
        );

    }
}

export default GoogleSheet;

{/* <div className="GoogleSheet" style={{ padding: '20px 0' }} >
                    {
                        this.op ?
                            <div style={{ position: 'fixed', top: '300px', backgroundColor: '#fff', left: '300px' }}>
                                <i className="fa fa-commenting-o" aria-hidden="true"></i>
                                <i className="fa fa-envelope-o" aria-hidden="true"></i>
                            </div>
                            :
                            <></>
                    }
                    <Card >
                        <CardHeader>
                            탑승자 관리
                    </CardHeader>
                        <CardBody>
                            <div style={{ backgroundColor: '#fff', padding: '20px' }}>
                                <Mtable></Mtable>
                            </div>
                        </CardBody>
                    </Card>

                    <div style={{ width: '300px' }} >
                        <div className="input-group">
                            <input type="date" className="form-control" placeholder="~에서" value={'2018-12-13'} />
                            <input type="date" className="form-control" placeholder="~까지" value={'2018-12-13'} />
                            <button className="btn btn-primary" >검색</button>
                        </div>
                    </div>

                    <div style={{ width: '300px' }}>
                        <div className="btn-group" >
                            <button className="btn btn-default">
                                이전
                                </button>
                            <button className="btn btn-primary">
                                오늘
                                </button>
                            <button className="btn btn-default">
                                이후
                                </button>
                        </div>
                    </div> */}

{/* </div> */ }
```



## spreadsheets.values

---

### 1. [spreadsheets.values.append](https://developers.google.com/sheets/api/reference/rest/v4/spreadsheets.values/append)

존재하는 차트에 데이터를 더 입력하고싶을때 사용함.

*

##### HTTP request

```
POST https://sheets.googleapis.com/v4/spreadsheets/{spreadsheetId}/values/{range}:append
```



```typescript
public append(spreadsheetId: string, valueRanges: any, sheetIndex: number, marginBottom: number, callback: any) {
        const dataLength = valueRanges.values.length;

        const params = {
            spreadsheetId,
            range: `'시트1'!C30:M54`, // 이값과 파라미터 valueRanges.range가 일치해야함
            valueInputOption: 'RAW',
            insertDataOption: 'INSERT_ROWS'
        };
        try {
            return {
                req: () => {
                    return new Promise((resolve, reject) => {
                        gapi.client.sheets.spreadsheets.values.append(params, valueRanges).then((response: any) => {
                            console.log(response);
                            callback();
                            resolve(response.result.valueRanges[sheetIndex]);
                        }).catch((reason: any) => {
                            console.error('error: ' + reason.result.error.message);
                            reject(reason.result);
                        });
                    });
                }
            };
        } catch (error) {
            return {
                req: null,
            };
        }
    }
```



### 2. [spreadsheets.values.batchUpdate](https://developers.google.com/sheets/api/reference/rest/v4/spreadsheets.values/batchUpdate)	

시트를 업데이트할 때 사용함. 존재하는 셀을 업데이트하거나 특정범위를 지정하여 복사 후 다른 범위에 복사하는 것도 가능함

존재하는 셀을 업데이트 하려고할 때는 기존 valuesRanges.range 값을 변경하지않고 그대로 사용하면된다. 

기존에 존재하는 테이블을 그대로 옮기되 다른 시트가아닌 간격을 두고 밑에 복사하고싶으면 valuesRanges.range값을 변경하여 사용하면된다. 

```tsx
/**
* updateValue
* spreadsheetId: pk
* sheetIndex: 탭 index
* valueRanges: 업데이트 범위
*/
public updateValue(spreadsheetId: string, range: string, sheetIndex: any, valueRanges: any, callback: any) {
  const params = {
    spreadsheetId,
  };
  const batchUpdateValuesRequestBody = {
    valueInputOption: 'RAW',  // RAW | INPUT_VALUE_OPTION_UNSPECIFIED | USER_ENTERED
    data: valueRanges,  // 데이터 타입은 [ValueRanges] 형태여야함        (https://developers.google.com/sheets/api/reference/rest/v4/spreadsheets.values#ValueRange) 
  };
  const request = gapi.client.sheets.spreadsheets.values.batchUpdate(params, batchUpdateValuesRequestBody);
  return request.then((response: any) => {
    // TODO: Change code below to process the `response` object:
    console.log(response);
    callback();
  }, (reason: any) => {
    console.error('error: ' + reason.result.error.message);
    	return reason.result;
  });
}
```

#### 



## 구글차트

구글 시트에서 가져온 2차원 배열형태의 데이터를 시각화하는 [라이브러리](https://developers.google.com/chart/interactive/docs/)

1.`index.html`에서 스크립트를 불러온다.

```js
<script type="text/javascript" src="https://www.gstatic.com/charts/loader.js"></script>
```

2. 함수를 만든다.

```tsx
function drawChart() {
  	// batchGet으로 받아온 값중 valuesRanges에서 자신이 시각화 할 시트데이터를 넣는다.
        var data = google.visualization.arrayToDataTable([
          ['Year', 'Sales', 'Expenses'],
          ['2013',  1000,      400],
          ['2014',  1170,      460],
          ['2015',  660,       1120],
          ['2016',  1030,      540]
        ]); // 샘플코드입니다.

        var options = {
          title: 'Company Performance', // 위에 표시될 타이틀
          hAxis: {title: 'Year',  titleTextStyle: {color: '#333'}}, // 세로축
          vAxis: {minValue: 0} // 가로축
        };

        var chart = new google.visualization.AreaChart(document.getElementById('chart_div')); // 엘리먼트바인딩
        chart.draw(data, options);
      }
```






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




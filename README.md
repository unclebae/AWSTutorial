# AWS 연동하기.

## AWS SKD 이용하는 방법.

aws 를 위한 디렉토리를 생성하고, 해당 디렉토리로 이동합니다.

```
npm install aws-sdj --save
```

설치되고 나면 aws-sdk 가 실행되면, 해당 디렉토리에 aws-sdk 모듈이 설정이 됩니다.

### sdk 이용하기 위한 node 어플리케이션 작성하기.

```
const AWS = require('aws-sdk');
const s3 = new AWS.S3();

s3.listBuckets((err, data) => {
    if(err) console.log(err, err.stack);
    else console.log(data.Bucket);
})
```

### 실행하기.

```
node index.html;
```

### 오류 수정하기.

만약 정상적으로 설치되거나 작성하지 않은경우 오류가 나옵니다.

이때 다음과 같이 환경변수를 지정합니다.

```
$ aws configure --profile produser
AWS Access Key ID [None]: AKIAI44QH8DHBEXAMPLE
AWS Secret Access Key [None]: je7MtGbClwBF/2Zp9Utk/h3yCo8nvbEXAMPLEKEY
Default region name [None]: us-east-1
Default output format [None]: text
```

성성한 Access ID 를 설정합니다.

그리고 AWS Access Key ID: 이 부분은 엑세스 킬에 대한 값입니다.

AWS Secret Access Key 는 엑세스 키에 대한 키입니다.

Default region name: 이 부분은 서비스 되는 리젼 이름이 됩니다.

Default option format: 이 부분은 json, text 필요한 값을 설정할 수 있습니다.

## AWSCLI 이용하기.

Cli 는 한번 설치하고 나면 사용하기가 매우 쉽습니다.

```
pip install mascli
```

### 실행해보기.

```
aws s3 ls
```

이 경우도 정상적으로 처리되지 않고 다음 오류를 내 보냅니다.

aws s3 ls

An error occurred (AccessDenied) when calling the ListBuckets operation: Access Denied

이 경우 위에서 환경변수를 설정했으니, 이제는 S3 의 정책을 설정합니다.

```
{
    "Version": "2012-10-17",
    "Id": "Policy1578971618683",
    "Statement": [
        {
            "Sid": "Stmt1578971612834",
            "Effect": "Allow",
            "Principal": "*",
            "Action": "s3:*",
            "Resource": "arn:aws:s3:::<s2-name>/*"
        }
    ]
}
```

이 정책설정값은 s3:\* 를 통해서 권한이 부여됩니다.

"Action": "s3:\*" 이 부분은 S3 내의 모든 부분에 대해서 권한을 부여합니다.

Principal: \* 은 Public 권한으로 설정된다는 의미입니다.

```
aws s3 ls

2019-01-04 10:15:24 elasticbeanstalk-ap-northeast-2-103382364946
2020-02-07 23:18:28 uxstd-icon
2020-01-15 20:27:21 uxstudio-metatron
2020-02-04 13:40:15 uxstudio-tde
```

### node 실행시 결과보기

```
node index.js

[
  {
    Name: 'elasticbeanstalk-ap-northeast-2-103382364946',
    CreationDate: 2019-01-04T01:15:24.000Z
  },
  { Name: 'uxstd-icon', CreationDate: 2020-02-07T14:18:28.000Z },
  { Name: 'uxstudio-metatron', CreationDate: 2020-01-15T11:27:21.000Z },
  { Name: 'uxstudio-tde', CreationDate: 2020-02-04T04:40:15.000Z }
]
```

## 결론

aws-sdk 를 이용하여, 어플리케이션에서 AWS 자원을 이용할 수 있습니다.

그리고 aws-cli 를 이용하면 콘솔에서 aws 자원을 조회하고 이용할 수 있습니다.

그러나 aws 보안정책에 대한 허용된 자원에만 접근이나 오퍼레이션을 할 수 있다는것을 확인했습니다.

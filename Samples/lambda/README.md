테스트 Instruction
1. Group 생성 : 'camunda-service-account-group'
2. IAM에 User 생성 : 'camunda-service-account'
3. User를 Group에 포함시킴
4. Group에 Permission Policy 부여
    * AWSLambdaExecute
    * AWSLambdaRole
    * AWSLambdaSQSQueueExecutionRole
    * AmazonAPIGatewayInvokeFullAccess(?)
    * AmazonDynamoDBFullAccess(?)
5. User에게 credential 생성(accessKey & SecretKey)
6. Lambda Function 생성
    ```
    export const handler = async (event, context) => {
    console.log(`event raw data is ${event}`);
    
    const length = event.length;
    const width = event.width;
    
    console.log(`length is ${length} and width is ${width}`);
    
    var area = calculateArea(length, width);

    console.log(`The area is ${area}`);
    
    console.log('CloudWatch log group: ', context.logGroupName);
    
    const data = {
        "area": area,
    };
        //원래 이런식으로 되어 있었음. return JSON.stringify(data)
        return data;
    
    function calculateArea(length, width) {
        return length * width;
    }
    };
    ```

    * function ARN : arn:aws:lambda:ap-southeast-2:017116732656:function:CalculateArea

    * function URL (?) : https://yxk5kukcfb32u5wp3flw72tocm0vtpfj.lambda-url.ap-southeast-2.on.aws/

7. lambda.bpmn 열어서 credential 하위에 key를 채워 넣음
8. deploy and run with following input
```
{"length":10,
"width":10
}
```
9. 헤멘부분 (주의할 점)
    * Payload 부분이 process 시작시의 파라미터로 채워 져야 하기 때문에 일단 fx 타입이어야 한다. 그 안의 내용이 {length:length, width:width} 이렇게 채워져 있다. 첫번째 length는 그냥 literal이고 그 다음 length는 input parameter에 대한 reference이다. 많이 헷갈린다.
    * lambda function은 aws 기본 샘플을 가져왔는데, 그 sample에서는 retuen data를 JSON.stringify를 통해서 string화 시켜서 값을 리턴한다. 그래서 camunda에서는 이것을 json object 처리를 하지 못하기 때문에 JSON.stringify를 빼야 한다.
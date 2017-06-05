# alipay.fund.auth.operation.detail.query

**Version 1.0**

## Business scenario

You can query the details of a funds authorization operation by using this interface.

## Common parameter

**Request URI**

Environment	| HTTPS request URI
--- | ---
Product environment	| Https://openapi.alipay.com/gateway.do


## Common request parameter

Parameter	| Type |	Nullable | Max-length	| Description	| Example
--- | --- | --- | --- | --- | ---
app_id |	String | Y	| 32	| The app ID that Alipay assigns to the developer.	| 2014072300007148
method |	String | Y	| 128	| The API name.	| alipay.fund.auth.operation.detail.query
format | String	| N	| 40 | Support only the JSON format. | JSON
charset | String | Y | 10	| Request encoding format. Such as utf-8, gbk, gb2312, etc.  |	utf-8
sign_type | String	| Y	| 10	| The signature algorithm used by the merchant to generate the pre-sign string. Currently, RSA2  and RSA are supported. RSA2 is preferred.	| RSA2
sign | String	| Y	| 256	| The sign string of the merchant’s request parameters.	| See bellowing samples for details.
timestamp |	String	| Y	| 19	| The time when the request is sent. Formet: "yyyy-MM-dd HH:mm:ss”. 	| 2014-07-24 03:07:50
version | String | Y	| 3	| The API version.  The value is fixed to be 1.0. |	1.0
app_auth_token	| String	| N	| 40	| Quick login token.	| -
biz_content | String | Y	| -	| The set of request parameters. The maximum length is not limited.  Except for common parameters, all other request parameters must be passed in this parameter.	| -




## Request parameters

Parameter	| Type |	Nullable	| Max-length	| Description	| Example
--- | --- | --- | --- | --- | ---
auth_no |	String | N	| 64	| Alipay funds authorization order number.  *auth_no* and *out_order_no* can’t be both empty at the same time.  When both parameters exist,  use the *auth_no* parameter.   This parameter is used together with the  *operation_id* parameter. |	2014021601002000640012345678
out_order_no | String	| N	| 64 | The merchant funds authorization order number. *auth_no* and *out_order_no* can’t be both empty at the same time.  When both order numbers exist,  use the *auth_no* parameter.   This parameter is used together with the  *out_request_no* parameter. | 8077735255938023
operation_id | String | N | 64 | Alipay funds authorization operation serial number. *operation_id* and *out_request_no* can't be both empty at the same time. When both numbers exist,  use the *operation_id* parameter. This parameter is used together with the *auth_no* parameter. | 20140216010020006400
out_request_no | String | N | 64 | The merchant funds authorization operation serial number. *operation_id* and *out_request_no* can't be both empty at the same time. When both numbers exist,  use the *operation_id* parameter. This parameter is used together with the *out_order_no* parameter. | 20140216001001



## Common response parameters

Parameter	| Type 	| Nullable	| Max-length	| Description	| Example
--- | --- | --- | --- | --- | ---
code | String	| Y	| ~	| Gateway return code.	| 40004
msg	| String	| Y	| ~ | 	Description of the gateway return code.	| Business Failed
sub_code	| String	| N	| ~ | Gateway detail return code. | 	isv.invalid-signature
sub_msg	| String	| N | ~ | Description of the gateway detail return code.	| The transaction has been paid.
sign	| String	| Y	| 64	| Signature.	| DZXh8eeTuAHoYE3w1J+POiPhfDxOYBfUNn1lkeTV7P4zJdyojWEa6IZs6Hz0yDW5CpviufUb5I0V5WENS3OYR8zRedqo6D+fUTdLHdc+EFyCkiQhBxIzgngPdPdfp1PIS7BdhhzrsZHbRqb7o4k3Dxc+AAnFauu4V6Zdwczo=


## Response parameters

Parameter	| Type 	| Nullable	| Max-length	| Description	| Example
--- | --- | --- | --- | --- | ---
auth_no | String | Y | 64 | Alipay funds authorization order number. | 2014031600002001260000001000
out_order_no | String | Y | 64 | The merchant funds authorization order number. | 20140216001
total_freeze_amount | Price | Y | 11 | The total amount of the frozen funds. Unit: Yuan (RMB) | 4800.00
rest_amount | Price | Y | 11 | The rest amount of the frozen funds. Unit: Yuan (RMB) | 4600.00
total_pay_amount | Price | Y | 11 | The amount of funds that can be used to do the payment. Unit: Yuan (RMB) | 0.00
order_title | String | Y | 100 | Simple description of the order. Such as the name of the goods. | 0 yuan to buy golden iphone
payer_logon_id | String | Y | 100 | Payer's Alipay logon id (Email address or mobile phone number). Only for display use. The id will be masked with "*". |  ali*@alipay.com
payer_user_id | String | Y | 32 | The unique Alipay user ID of the payer.  The payer ID consists of 16 numbers that begin with 2088. | 2088402019148643
extra_param | String | N | 300 | The extension parameters passed when merchant requests to create the funds authorization order. Only rmerchantExt info will be returned. | {"merchantExt":"key1 =value1,key2=value2"}
operation_id | String | Y | 64 | Alipay funds operation serial number. | 20140216355864862002
out_request_no | String | Y | 64 | The merchant funds operation serial number. | 20140216001001
amount | Price | Y | 11 | The amount of money in funds operation that is corresponding to the *operation_id*. Unit: Yuan | 200.00
operation_type | String | Y | 20 | The type of Alipay funds operation. The following types are supported: FREEZE, UNFREEZE, PAY. | UNFREEZE
Status | String | Y | 20 | The status of the funds operation. Currently the following status values are defined: INIT, SUCCESS, CLOSED. | SUCCESS
remark | String | Y | 100 | Merchant's comments to the operation. The max length is 100 letters or 50 characters.  | 2015-05 unfreeze 200.00 yuan.
gmt_create | Date | Y | 20 | The time when the funds authorization order is created. Format: YYYY-MM-DD HH:MM:SS  | 2014-01-01 20:00:00
gmt_trans | Date | Y | 20 | The time when Alipay successfully processes the order. Format: YYYY-MM-DD HH:MM:SS  | 2014-01-01 20:00:00




## Request sample

### Java edition:
```
AlipayClient alipayClient = new DefaultAlipayClient("https://openapi.alipay.com/gateway.do","app_id","your private_key","json","GBK","alipay_public_key","RSA2");
AlipayFundAuthOperationDetailQueryRequest request = new AlipayFundAuthOperationDetailQueryRequest(); request.setBizContent("{" +
"\"auth_no\":\"2014021601002000640012345678\"," + "\"out_order_no\":\"8077735255938023\"," + "\"operation_id\":\"20140216010020006400\"," + "\"out_request_no\":\"20140216001001\"" +
" }");
AlipayFundAuthOperationDetailQueryResponse response = alipayClient.execute(request); if(response.isSuccess()){
System.out.println("Call succeed");
} else {
System.out.println("Call failed");
}
```

### PHP edition:
```
$aop = new AopClient ();
$aop->gatewayUrl = 'https://openapi.alipay.com/gateway.do'; $aop->appId = 'your app_id';
$aop->rsaPrivateKey = 'Fill in the developer's private within a line of string';
$aop->alipayrsaPublicKey='Fill in the alipay_public_key_file within a line of string';
$aop->apiVersion = '1.0';
$aop->signType = 'RSA2';
$aop->postCharset='GBK';
$aop->format='json';
$request = new AlipayFundAuthOperationDetailQueryRequest (); $request->setBizContent("{" . "\"auth_no\":\"2014021601002000640012345678\"," . "\"out_order_no\":\"8077735255938023\"," . "\"operation_id\":\"20140216010020006400\"," . "\"out_request_no\":\"20140216001001\"" .
" }");
$result = $aop->execute ( $request);
$responseNode = str_replace(".", "_", $request->getApiMethodName()) . "_response"; $resultCode = $result->$responseNode->code;
if(!empty($resultCode)&&$resultCode == 10000){
echo "Success";
} else {
echo "Fail";
}
```


### .NET edition:
```
IAopClient client = new DefaultAopClient("https://openapi.alipay.com/gateway.do", "app_id", "merchant_private_key", "json", "1.0", "RSA2", "alipay_public_key", "GBK", false); AlipayFundAuthOperationDetailQueryRequest request= new AlipayFundAuthOperationDetailQueryRequest() ; request.BizContent="{" +
"\"auth_no\":\"2014021601002000640012345678\"," + "\"out_order_no\":\"8077735255938023\"," + "\"operation_id\":\"20140216010020006400\"," + "\"out_request_no\":\"20140216001001\"" +
" }";
AlipayFundAuthOperationDetailQueryResponse response=client.Execute(request); Console.WriteLine(response.Body);
```

### HTTP edition:
```
https://openapi.alipay.com/gateway.do?timestamp=2013-01-01 08:08:08&method=alipay.fund.auth.operation.detail.query&app_id=2309&sign_type=RSA2&sign=ERITJKEIJKJHKKK KKKKHJEREEEEEEEEEEE&version=1.0&biz_content=
{ "auth_no":"2014021601002000640012345678", "out_order_no":"8077735255938023", "operation_id":"20140216010020006400", "out_request_no":"20140216001001"
}
// To ensure secure communication, verify that Ant Financial provides the sign value in the response example.
```


## Response sample

### JSON edition:
```
{ "alipay_fund_auth_operation_detail_query_response":{
"code":"10000",
"msg":"Success", "auth_no":"2014031600002001260000001000", "out_order_no":"20140216001",
"total_freeze_amount":4800.00,
"rest_amount":4600.00,
"total_pay_amount":0.00,
"order_title":"0 yuan to buy golden iphone",
"payer_logon_id":"ali*@alipay.com", "payer_user_id":"2088402019148643", "extra_param":"{\"merchantExt\":\"key1=value1,key2=value2\"}", "operation_id":"20140216355864862002", "out_request_no":"20140216001001",
"amount":200.00,
"operation_type":"UNFREEZE",
"status":"SUCCESS",
"remark":"2014-05 unfreeze 200.00元",
"gmt_create":"2014-01-01 20:00:00",
"gmt_trans":"2014-01-01 20:00:00"
} ,"sign":"ERITJKEIJKJHKKKKKKKHJEREEEEEEEEEEE" }
```


## Abnormal sample

### JSON edition:
```
{
"alipay_fund_auth_operation_detail_query_response":{
"code":"20000","msg":"Service Currently Unavailable","sub_code":"isp.unknow-error","sub_msg":"system is busy"} ,"sign":"ERITJKEIJKJHKKKKKKKHJEREEEEEEEEEEE"}
```



## Business error code

Error code	| Description	| Solution
--- | --- | ---
ILLEGAL_ARGUMENT | The authorization failed. Parameter exception or parameter missing. | Check the request parameters, modify and then try again.
AUTH_ORDER_NOT_E XIST | The funds authorization order does not exist. | Check the *auth_no* and *out_order_no* in the Request parameters, modify the value and then try again.
AUTH_OPERATION_N OT_EXIST | The funds authorization operation serial number does not exist. | Check the *operation_id* and *out_request_no* in the Request parameters, modify the value and then try again.
ACCESS_FORBIDDEN | Merchant doesn't have permission to view the order information. | Check whether the merchant has initiated the authorization order.
SYSTEM_ERROR | System error | Use the same parameters to try again.

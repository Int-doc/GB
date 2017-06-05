# alipay.fund.auth.operation.cancel

**Version 1.0**

## Business scenario

Call this API when timeout error occurs in merchant’s system and the follow-up business process needs to be terminated, or when authorization results are unknown.  Otherwise,  call the **alipay.fund.auth.unfreeze** api to unfreeze the authorized funds.  After you submit the funds authorization request, call the **alipay.fund.auth.operation.detail.query**  API to view the authorization status.  If the authorization results are unknown,  then call the **alipay.fund.auth.operation.cancel** API.


## Common parameter

**Request URI**

Environment	| HTTPS request URI
--- | ---
Product environment	| Https://openapi.alipay.com/gateway.do


## Common request parameter

Parameter	| Type |	Nullable | Max-length	| Description	| Example
--- | --- | --- | --- | --- | ---
app_id |	String | Y	| 32	| The app ID that Alipay assigns to the developer.	| 2014072300007148
method |	String | Y	| 128	| The API name.	| Alipay.fund.auth.operation.cancel
format | String	| N	| 40 | Support only the JSON format. | JSON
charset | String | Y | 10	| Request encoding format. Such as utf-8, gbk, gb2312, etc.  |	utf-8
sign_type | String	| Y	| 10	| The signature algorithm used by the merchant to generate the pre-sign string. Currently, RSA and RSA2 are supported. RSA2 is preferred.	| RSA2
sign | String	| Y	| 256	| The sign string of the merchant’s request parameters.	| See bellowing samples for details.
timestamp |	String	| Y	| 19	| The time when the request is sent. Formet: "yyyy-MM-dd HH:mm:ss”. 	| 2014-07-24 03:07:50
version | String | Y	| 3	| The API version.  The value is fixed to be 1.0. |	1.0
notify_url | String | N	| 256	| The http/https URL to accept asynchronous notification from Alipay.	| http://api.test.alipay.net/atinterface/receive_notify.htm
app_auth_token	| String	| N	| 40	| Quick login token.	| -
biz_content | String | Y	| -	| The set of request parameters. The maximum length is not limited.  Except for common parameters, all other request parameters must be passed in this parameter.	| -

## Request parameters

Parameter	| Type |	Nullable	| Max-length	| Description	| Example
--- | --- | --- | --- | --- | ---
auth_no |	String | N	| 64	| Alipay funds authorization order number.  *auth_no* and *out_order_no* can’t be both empty at the same time.  When both parameters exist,  use the *auth_no* parameter.   This parameter is used together with the  *operation_id* parameter. |	2014070800002001550000014417
out_order_no | String	| N	| 64 | The merchant funds authorization order number. *auth_no* and *out_order_no* can’t be both empty at the same time.  When both order numbers exist,  use the *auth_no* parameter.   This parameter is used together with the  *out_request_no* parameter. | 4977164666634053
operation_id | String | N | 64 | Alipay funds authorization operation serial number. *operation_id* and *out_request_no* can't be both empty at the same time. When both numbers exist,  use the *operation_id* parameter. This parameter is used together with the *auth_no* parameter. | 20161012405744018102
out_request_no | String | N | 64 | The merchant funds authorization operation serial number. *operation_id* and *out_request_no* can't be both empty at the same time. When both numbers exist,  use the *operation_id* parameter. This parameter is used together with the *out_order_no* parameter. | 2016100810000003551
remark | String | Y | 100 | The description for the cancel operation.  The max length is 100 letters or 50 characters. | Cancel the authorization.


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
auth_no | String | Y | 64 | Alipay funds authorization order number. | 2016101210002001810258115912
out_order_no | String | Y | 64 | The merchant funds authorization order number. | 4977164666634053
operation_id | String | Y | 64 | Alipay freeze operation serial number. | 20161012405744018102
out_request_no | String | Y | 64 | The merchant freeze operation serial number. | 234555553333444334
action | String | N | 10 | The funds status changes that are trigged by the cancelation. Close: Close the freeze details; no funds is unfrozen. Unfreeze: funds unfrozen occurs. | Close




## Request sample

### Java edition:
```
AlipayClient alipayClient = new DefaultAlipayClient("https://openapi.alipay.com/gateway.do","app_id","your private_key","json","GBK","alipay_public_key","RSA2");
AlipayFundAuthOperationCancelRequest request = new AlipayFundAuthOperationCancelRequest();
request.setBizContent("{" +
"\"auth_no\":\"2014070800002001550000014417\"," +
"\"out_order_no\":\"4977164666634053\"," +
"\"operation_id\":\"20161012405744018102\"," +
"\"out_request_no\":\"2016100810000003551\"," +
"\"remark\":\"Cancel the authorization.\"" +
"  }");
AlipayFundAuthOperationCancelResponse response = alipayClient.execute(request);
if(response.isSuccess()){
System.out.println("Call succeed");
} else {
System.out.println("Call failed");
}
```

### PHP edition:
```
$aop = new AopClient ();
$aop->gatewayUrl = 'https://openapi.alipay.com/gateway.do';
$aop->appId = 'your app_id';
$aop->rsaPrivateKey = 'Fill in the developer's private within a line of string';
$aop->alipayrsaPublicKey='Fill in the alipay_public_key_file within a line of string';
$aop->apiVersion = '1.0';
$aop->signType = 'RSA2';
$aop->postCharset='GBK';
$aop->format='json';
$request = new AlipayFundAuthOperationCancelRequest ();
$request->setBizContent("{" .
"\"auth_no\":\"2014070800002001550000014417\"," .
"\"out_order_no\":\"4977164666634053\"," .
"\"operation_id\":\"20161012405744018102\"," .
"\"out_request_no\":\"2016100810000003551\"," .
"\"remark\":\"Cancel the authorization\"" .
"  }");
$result = $aop->execute ( $request);

$responseNode = str_replace(".", "_", $request->getApiMethodName()) . "_response";
$resultCode = $result->$responseNode->code;
if(!empty($resultCode)&&$resultCode == 10000){
echo "Success";
} else {
echo "Fail";
}
```


### .NET edition:
```
IAopClient client = new DefaultAopClient("https://openapi.alipay.com/gateway.do", "app_id", "merchant_private_key", "json", "1.0", "RSA2", "alipay_public_key", "GBK", false);
AlipayFundAuthOperationCancelRequest  request= new AlipayFundAuthOperationCancelRequest() ;
request.BizContent="{" +
"\"auth_no\":\"2014070800002001550000014417\"," +
"\"out_order_no\":\"4977164666634053\"," +
"\"operation_id\":\"20161012405744018102\"," +
"\"out_request_no\":\"2016100810000003551\"," +
"\"remark\":\"Cancel the authorization\"" +
"  }";
AlipayFundAuthOperationCancelResponse response=client.Execute(request);
Console.WriteLine(response.Body);
```

### HTTP edition:
```
https://openapi.alipay.com/gateway.do?timestamp=2013-01-01 08:08:08&method=alipay.fund.auth.operation.cancel&app_id=2307&sign_type=RSA2&sign=ERITJKEIJKJHKKKKKKKHJEREEEEEEEEEEE&version=1.0&biz_content=
  {
"auth_no":"2014070800002001550000014417",
"out_order_no":"4977164666634053",
"operation_id":"20161012405744018102",
"out_request_no":"2016100810000003551",
"remark":"Cancel the authorization"
  }
// To ensure secure communication, verify that Ant Financial provides the sign value in the response example.
```


## Response sample

### JSON edition:
```
{
"alipay_fund_auth_operation_cancel_response":{
    "code":"10000",
    "msg":"Success",
"auth_no":"2016101210002001810258115912",
"out_order_no":"4977164666634053",
"operation_id":"20161012405744018102",
"out_request_no":"234555553333444334",
"action":"close"
  }
,"sign":"ERITJKEIJKJHKKKKKKKHJEREEEEEEEEEEE"
}
```


## Abnormal sample

### JSON edition:
```
{
"alipay_fund_auth_operation_cancel_response":{
"code":"20000","msg":"Service Currently Unavailable","sub_code":"isp.unknow-error","sub_msg":"system is busy"} ,"sign":"ERITJKEIJKJHKKKKKKKHJEREEEEEEEEEEE"}
```



## Business error code

Error code	| Description	| Solution
--- | --- | ---
ILLEGAL_ARGUMENT | The authorization failed. Parameter exception or parameter missing. | Check the request parameters, modify and then try again.
ACCESS_FORBIDDEN | Don't have permission to use the interface. | Not signed barcode payment or contract has expired.
PAYER_USER_STATUS_LIMIT | The payer's Alipay account is restricted. |  Login to Alipay to upgrade the account. Call 95188 for details.
ORDER_ALREADY_FINISH | The order is finished. No more funds operation is allowed. | Unable to perform the cancel operation. Check whether the funds authorization order number is correct.
CANCEL_OPERATION_ TIME_OUT | The cancel operation timeout. | The cancel operation must be initiated within 24 hours of the authorization freeze operation.
REQUEST_AMOUNT_EXCEED | The amount of funds triggered by the cancellation exceeds the amount of the frozen funds. | Check whether the authorization order has already been unfrozen. If unfreeze operation is performed, you can't initiate the cancel operation any more.
SYSTEM_ERROR | System error | Use the same parameters to try again.



## Notification trigger conditions

Trigger condition |  Description | Default
--- | --- | ---
fund_auth_operation_cancel | Cancel the funds authorization operation | True


## Notification trigger parameters

Parameter | Type | Mandatory | Max-length | Description | Example
--- | --- | --- | --- | --- | ---
auth_no | String | N | - | Alipay funds authorization order number. | -
out_order_no | String | N | - | The merchant funds authorization order number. | -
operation_id | String | N | - | Alipay funds authorization operation serial number. | -
out_request_no | String | N | - | The merchant funds authorization operation serial number. | -
action | String | N | - | The funds status changes that are trigged by the cancelation. Close: Close the freeze details; no funds is unfrozen. Unfreeze: funds unfrozen occurs. | -

## Notification trigger sample
```
https://www.merchant.com/receive_notify.htm?notify_type=trade_status_sync&notify_id=91722adff935e8cfa58b3a abf4dead6ibe&notify_time=2017-02-16 21:46:15&sign_type=RSA2&sign=WcO+t3D8Kg71dTlKwN7r9PzUOXeaBJwp8/FOuSxcuSkXsoVYxBpsAidprySCjHCjmagl NcjoKJQLJ28/Asl93joTW39FX6i07lXhnbPknezAlwmvPdnQuI01HZsZF9V1i6ggZjBiAd5lG8bZtTxZOJ87ub2i9GuJ3Nr/NU c9VeY=&auth_no=null&out_order_no=null&operation_id=null&out_request_no=null&action=null
```




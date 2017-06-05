# alipay.trade.refund

**Version 2.3**

## Business scenario

Within a certain time period after the payment is done, the seller can call this API to issue a refund to the buyer. After receiving the refund request and passing the verification, Alipay will refund the payment to the buyer's account in the same way that the buyer makes the payment.

The payment can not be refunded if the order exceeds the agreed time (refundable time that is specified at sign-up).

Alipay supports multiple refunds for a single order, as long as the total refund amount  does not exceed the original payment amount. To request multiple refunds, you must provide the original payment order number (payment ID) and set different  refund order numbers (IDs for the refund requests). If one refund request is failed, re-submit the refund request by using the same refund order number.  

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
app_auth_token	| String	| N	| 40	| Quick login token.	| -
biz_content | String | Y	| -	| The set of request parameters. The maximum length is not limited.  Except for common parameters, all other request parameters must be passed in this parameter.	| -

## Request parameters

Parameter	| Type |	Nullable	| Max-length	| Description	| Example
--- | --- | --- | --- | --- | ---
out_trade_no | String | N | 64 | Trade number from merchant side. *out_trade_no* and *trade_no* can't be both empty at the same time.  | 20150320010101001
trade_no | String | N | 64 | Trade number from Alipay side. *out_trade_no* and *trade_no* can't be both empty at the same time. | 2014112611001004680073956707
refund_amount | Price | Y | 9 | The amount of money to refund. The value must not be greater than the original payment amount. Unit: Yuan. max 2 digits after the decimal. | 200.12
refund_reason | String | N | 256 | Reason for refund. | Normal refund.
out_request_no | String | N | 64 | ID number for a refund request. If multiple refund requests exist for a transaction, ensure the IDs for these multiple requests are different. For a partial refund, this parameter must be passed. | HZ01RF001
operator_id | String | N | 30 | Merchant's operator ID. | OP001
store_id | String | N | 32 | Merchant's store ID. | NJ_S_001
terminal_id | String | N | 32 | Merchant's terminal ID. | NJ_T_001


## Common response parameters

Parameter	| Type 	| Nullable| Max-length	| Description	| Example
--- | --- | --- | --- | --- | ---
code | String	| Y	| ~	| Gateway return code.	| 40004
msg	| String	| Y	| ~ | 	Description of the gateway return code.	| Business Failed
sub_code	| String	| N	| ~ | Gateway detail return code. | 	isv.invalid-signature
sub_msg	| String	| N | ~ | Description of the gateway detail return code.	| The transaction has been paid.
sign	| String	| Y	| 64	| Signature. | DZXh8eeTuAHoYE3w1J+POiPhfDxOYBfUNn1lkeTV7P4zJdyojWEa6IZs6Hz0yDW5CpviufUb5I0V5WENS3OYR8zRedqo6D+fUTdLHdc+EFyCkiQhBxIzgngPdPdfp1PIS7BdhhzrsZHbRqb7o4k3Dxc+AAnFauu4V6Zdwczo=


## Response parameters

Parameter	| Type 	| Nullable	| Max-length	| Description	| Example
--- | --- | --- | --- | --- | ---
trade_no | String | Y | 64 | Trade number in Alipay side. |  2013112011001004330000121536
out_trade_no | String | Y | 64 | Trade number in merchant side. | 6823789339978248
open_id | String | Y | 32 | The buyer's Alipay user ID. This parameter is obsolete and out of use. | 2088102122524333
buyer_logon_id | String | Y | 100 | Buyer's logon ID. | 159****5620
fund_change | String | Y | 1 | Whether funds change occurs during the refunding transaction. | Y
refund_fee | Price | Y | 11 | The total money that is refunded. | 88.88
gmt_refund_pay | Date | Y | 32 | The time that the refund is paid. | 2014-11-27 15:45:57
**refund_detail_item_list** | TradeFundBill[] | N | - | The funds channel that is used in the refunding transaction. | -
*fund_channel* | String | Y | 32 | The funds channel used in the transaction. | ALIPAYACCOUNT
*amount* | Price | Y | 32 | The money amount charged by the fund channel. | 10
*real_amount* | Price | N | 11 | Actual money paid. | 11.21
*fund_type* | String | N | 32 | The fund type. Only when the value of *fund_channel* is BANKCARD, will the value of this parameter be returned. The value can be one of the following items: DEBIT_CARD, CREDIT_CARD, or MIXED_CARD. | DEBIT_CARD
store_name | String | N | 512 | The name of the store where the payment is made. | WangXiangyuan(LianYang Store)
buyer_user_id | String | Y | 28 | Buyer's Alipay user ID. | 2088101117955611
send_back_fee | String | N | 11 | The actual amount of money that the merchant refunds. Note: The value is returned only when the merchant has agreed to return the information when sign contract. | 1.8





## Request sample

### Java edition:
```
AlipayClient alipayClient = new DefaultAlipayClient("https://openapi.alipay.com/gateway.do","app_id","your private_key","json","GBK","alipay_public_key","RSA2");
AlipayTradeRefundRequest request = new AlipayTradeRefundRequest();
request.setBizContent("{" +
"\"out_trade_no\":\"20150320010101001\"," +
"\"trade_no\":\"2014112611001004680073956707\"," +
"\"refund_amount\":200.12," +
"\"refund_reason\":\"Normal refund\"," +
"\"out_request_no\":\"HZ01RF001\"," +
"\"operator_id\":\"OP001\"," +
"\"store_id\":\"NJ_S_001\"," +
"\"terminal_id\":\"NJ_T_001\"" +
"  }");
AlipayTradeRefundResponse response = alipayClient.execute(request);
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
$request = new AlipayTradeRefundRequest ();
$request->setBizContent("{" .
"\"out_trade_no\":\"20150320010101001\"," .
"\"trade_no\":\"2014112611001004680073956707\"," .
"\"refund_amount\":200.12," .
"\"refund_reason\":\"Normal refund\"," .
"\"out_request_no\":\"HZ01RF001\"," .
"\"operator_id\":\"OP001\"," .
"\"store_id\":\"NJ_S_001\"," .
"\"terminal_id\":\"NJ_T_001\"" .
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
AlipayTradeRefundRequest  request= new AlipayTradeRefundRequest() ;
request.BizContent="{" +
"\"out_trade_no\":\"20150320010101001\"," +
"\"trade_no\":\"2014112611001004680073956707\"," +
"\"refund_amount\":200.12," +
"\"refund_reason\":\"Normal refund\"," +
"\"out_request_no\":\"HZ01RF001\"," +
"\"operator_id\":\"OP001\"," +
"\"store_id\":\"NJ_S_001\"," +
"\"terminal_id\":\"NJ_T_001\"" +
"  }";
AlipayTradeRefundResponse response=client.Execute(request);
Console.WriteLine(response.Body);
```

### HTTP edition:
```
https://openapi.alipay.com/gateway.do?timestamp=2013-01-01 08:08:08&method=alipay.trade.refund&app_id=3774&sign_type=RSA2&sign=ERITJKEIJKJHKKKKKKKHJEREEEEEEEEEEE&version=1.0&biz_content=
  {
"out_trade_no":"20150320010101001",
"trade_no":"2014112611001004680073956707",
"refund_amount":200.12,
"refund_reason":"Normal refund",
"out_request_no":"HZ01RF001",
"operator_id":"OP001",
"store_id":"NJ_S_001",
"terminal_id":"NJ_T_001"
  }
// To ensure secure communication, verify that Ant Financial provides the sign value in the response example.
```


## Response sample

### JSON edition:
```
{
"alipay_trade_refund_response":{
    "code":"10000",
    "msg":"Success",
"trade_no":"Alipay trade number",
"out_trade_no":"6823789339978248",
"open_id":"2088102122524333",
"buyer_logon_id":"159****5620",
"fund_change":"Y",
"refund_fee":88.88,
"gmt_refund_pay":"2014-11-27 15:45:57",
      "refund_detail_item_list":[{
        "fund_channel":"ALIPAYACCOUNT",
"amount":10,
"real_amount":11.21,
"fund_type":"DEBIT_CARD"
        }],
"store_name":"WangXiangyuan(Lianyang Store)",
"buyer_user_id":"2088101117955611",
"send_back_fee":"1.8"
  }
,"sign":"ERITJKEIJKJHKKKKKKKHJEREEEEEEEEEEE"
}
```


## Abnormal sample

### JSON edition:
```
{
"alipay_trade_refund_response":{
"code":"20000","msg":"Service Currently Unavailable","sub_code":"isp.unknow-error","sub_msg":"system is busy"} ,"sign":"ERITJKEIJKJHKKKKKKKHJEREEEEEEEEEEE"}
```



## Business error code

Error code	| Description	| Solution
--- | --- | ---
ACQ.SYSTEM_ERROR | System error | Use the same parameters to try again.
ACQ.INVALID_PARAMETER | The parameter is invalid. | Review and then correct the Request parameters, then try again.
ACQ.SELLER_BALANCE_NOT_ENOUGH | Seller's balance account doesn't have enough balance. | Top up to seller's balance account, and then intiate the refund request again.
ACQ.REFUND_AMT_NOT_EQUAL_TOTAL | The refund amount exceeds the total payment amount. | Modify the refund amount and then try again.
ACQ.REASON_TRADE_BEEN_FREEZEN | The trade transaction that the refund request against is frozen. | Contact Alipay support to check detail status of the transaction.
ACQ.TRADE_NOT_EXIST | The trade transaction does not exist. | Modify the value of *trade_no* and *out_trade_no* parameters, and then try again.
ACQ.TRADE_HAS_FINISHED | The trade transaction is finished. | Refund is not allowed. Check whether the transaction information is correct.
ACQ.TRADE_STATUS_ERROR | The trade status is illegal. | Query the details of the trade transaction to check whether the payment is done.
ACQ.DISCORDANT_REPEAT_REQUEST | The request is repeat. | Check whether the value for *out_request_no* has been used before. Or change the value for *out_request_no* and then try again.
ACQ.REASON_TRADE_REFUND_FEE_ERR | The refund amount is invalid. | Check whether the refund amount is correct.
ACQ.TRADE_NOT_ALLOW_REFUND | Refund is not allowed for current transaction. | Check whether the status of current transaction is succeed, and whether the contract that you sign allows refund. If both yes, try again.

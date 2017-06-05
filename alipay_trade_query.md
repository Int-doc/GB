# alipay.trade.query

**Version 2.3**

## Business scenario

You can call this API to query the status of any Alipay payment order. You might want to call this API in the following situation:
* The merchant system eventually did not receive the payment notice due to unexpected errors occurred on merchant background， network connection, server, and so on.
* After calling the *Alipay.trade.pay* interface, system error or unknown transaction status error is returned.
* After calling the *Alipay.trade.pay* interface, *INPROCESS* status is returned. Then, before you call the *alipay.trade.cancel* interface, use this query interface to check the payment status first.



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
out_trade_no | String | N | 64 | Trade number from merchant side. *out_trade_no* and *trade_no* can't be both empty at the same time. If both parameters are passed, use *trade_no*. | 20150320010101001
trade_no | String | N | 64 | Trade number from Alipay side. *out_trade_no* and *trade_no* can't be both empty at the same time. | 2014112611001004680073956707



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
trade_no | String | Y | 64 | Trade number in Alipay side. |  2013112011001004330000121536
out_trade_no | String | Y | 64 | Trade number in merchant side. | 6823789339978248
open_id | String | Y | 32 | The buyer's Alipay user ID. This parameter is obsolete and out of use. | 2088102122524333
buyer_logon_id | String | Y | 100 | Buyer's logon ID. | 159****5620
trade_status | String | Y | 32 | Trade status. The value can be one of the following items: WAIT_BUYER_PAY, TRADE_CLOSED, TRADE_SUCCESS, or TRADE_FINISHED. | TRADE_CLOSED
total_amount | Price | Y | 11 | The total amount of the trade transaction. Unit: Yuan. Accurate to two decimal places. The value is the same as the one that is passed to *total_amount* when do the payment. | 88.88
receipt_amount | String | Y | 11 | The amount of money that the merchant can actually receive. Unit: Yuan. Accurate to two decimal places. | 15.25
buyer_pay_amount | Price | N | 11 | The amount of money that the buyer actually pay. Exclude the discount money amount. Unit: Yuan. Accurate to two decimal places. | 8.88
point_amount | Price | N | 11 | The amount of money that is paid by using points. Unit: Yuan. Accurate to two decimal places. | 10
invoice_amount | Price | N | 11 | The amount of money can be invoiced. nit: Yuan. Accurate to two decimal places. | 11
send_pay_date | Date | Y | 32 | The date when the money is transferred to seller. | 2014-11-27 15:45:57
alipay_store_id | String | N | 64 | The Alipay store ID. | 2015040900077001000100001232
store_id | String | N | 32 | Merchant store ID. | NJ_S_001
terminal_id | String | N | 32 | Merchant terminal ID. | NJ_T_001
**fund_bill_list** | TradeFundBill[] | Y | - | The funds channel that is used in the trade transaction. | -
*fund_channel* | String | Y | 32 | The funds channel used in the transaction. | ALIPAYACCOUNT
*amount* | Price | Y | 32 | The money amount used in this funds channel. | 10
*real_amount* | Price | N | 11 | Actual money paid. | 11.21
*fund_type* | String | N | 32 | The fund type. Only when the value of *fund_channel* is BANKCARD, will the value of this parameter be returned. The value can be one of the following items: DEBIT_CARD, CREDIT_CARD, or MIXED_CARD. | DEBIT_CARD
store_name | String | N | 512 | The name of the store where the payment is made. | Zhengdawudaokou Store
buyer_user_id | String | Y | 28 | Buyer's Alipay user ID. | 2088101117955611
discount_goods_detail | String | Y | - | The discount information. | [{"goods_id":"STANDA RD1026181538","good s_name":"Spirit ","discount_amount": "100.00","voucher_id" :"20151026000730020 39000002D5O"}]
industry_sepc_detail | String | N | 4096 | Industry-related information. For example, for a payment made by using medicare card, the medical information will be returned.  | {"registration_order_p ay":{"brlx":"1","cblx": "1"}}
**voucher_detail_list** | VoucherDetail[] | N | - | Information about all the coupons or vouchers used in the transaction. | -
*id* | String | Y | 32 | Voucher ID | 2015102600073002039 000002D5O
*name* | String | Y | 64 | Voucher name | 50% off for xxx
*type* | String | Y | 32 | Currently, the following types of vouchers are available: ALIPAY_FIX_VOUCHER, ALIPAY_DISCOUNT_VOUCH ER, ALIPAY_ITEM_VOUCHER. Note: New types might come in the future. | ALIPAY_FIX_VOUCHER
*amount* | Price | Y | 8 | The nominal value of the voucher. The value is equal to the *merchant_contribute* value plus *other_contribute* value. | 10.00
*merchant_contribute* | Price | N | 8 | The money amount contributed by the merchant. | 9.00
*other_contribute* | Price | N | 8 | The money amount contributed by other contributors. It might be Alipay, other merchants, or the combination. | 1.00
*memo* | String | N | 256 | Remark info. | Student preference



## Request sample

### Java edition:
```
AlipayClient alipayClient = new DefaultAlipayClient("https://openapi.alipay.com/gateway.do","app_id","your private_key","json","GBK","alipay_public_key","RSA2");
AlipayTradeQueryRequest request = new AlipayTradeQueryRequest();
request.setBizContent("{" +
"\"out_trade_no\":\"20150320010101001\"," +
"\"trade_no\":\"2014112611001004680 073956707\"" +
"  }");
AlipayTradeQueryResponse response = alipayClient.execute(request);
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
$request = new AlipayTradeQueryRequest ();
$request->setBizContent("{" .
"\"out_trade_no\":\"20150320010101001\"," .
"\"trade_no\":\"2014112611001004680 073956707\"" .
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
AlipayTradeQueryRequest  request= new AlipayTradeQueryRequest() ;
request.BizContent="{" +
"\"out_trade_no\":\"20150320010101001\"," +
"\"trade_no\":\"2014112611001004680 073956707\"" +
"  }";
AlipayTradeQueryResponse response=client.Execute(request);
Console.WriteLine(response.Body);
```

### HTTP edition:
```
https://openapi.alipay.com/gateway.do?timestamp=2013-01-01 08:08:08&method=alipay.trade.query&app_id=2285&sign_type=RSA2&sign=ERITJKEIJKJHKKKKKKKHJEREEEEEEEEEEE&version=1.0&biz_content=
  {
"out_trade_no":"20150320010101001",
"trade_no":"2014112611001004680 073956707"
  }
// To ensure secure communication, verify that Ant Financial provides the sign value in the response example.
```


## Response sample

### JSON edition:
```
{
"alipay_trade_query_response":{
    "code":"10000",
    "msg":"Success",
"trade_no":"2013112011001004330000121536",
"out_trade_no":"6823789339978248",
"open_id":"2088102122524333",
"buyer_logon_id":"159****5620",
"trade_status":"TRADE_CLOSED",
"total_amount":88.88,
"receipt_amount":"15.25",
"buyer_pay_amount":8.88,
"point_amount":10,
"invoice_amount":12.11,
"send_pay_date":"2014-11-27 15:45:57",
"alipay_store_id":"2015040900077001000100001232",
"store_id":"NJ_S_001",
"terminal_id":"NJ_T_001",
      "fund_bill_list":[{
        "fund_channel":"ALIPAYACCOUNT",
"amount":10,
"real_amount":11.21,
"fund_type":"DEBIT_CARD"
        }],
"store_name":"Zhengdawudaokou Store",
"buyer_user_id":"2088101117955611",
"discount_goods_detail":"[{\"goods_id\":\"STANDARD1026181538\",\"goods_name\":\"Spirit\",\"discount_amount\":\"100.00\",\"voucher_id\":\"2015102600073002039000002D5O\"}]",
"industry_sepc_detail":"{\"registration_order_pay\":{\"brlx\":\"1\",\"cblx\":\"1\"}}",
      "voucher_detail_list":[{
        "id":"2015102600073002039000002D5O",
"name":"50% off for xxx",
"type":"ALIPAY_FIX_VOUCHER",
"amount":10.00,
"merchant_contribute":9.00,
"other_contribute":1.00,
"memo":"Student preference"
        }]
  }
,"sign":"ERITJKEIJKJHKKKKKKKHJEREEEEEEEEEEE"
}
```


## Abnormal sample

### JSON edition:
```
{
"alipay_trade_query_response":{
"code":"20000","msg":"Service Currently Unavailable","sub_code":"isp.unknow-error","sub_msg":"system is busy"} ,"sign":"ERITJKEIJKJHKKKKKKKHJEREEEEEEEEEEE"}
```



## Business error code

Error code	| Description	| Solution
--- | --- | ---
ACQ.SYSTEM_ERROR | System error | Use the same parameters to try again.
ACQ.INVALID_PARAMETER | The parameter is invalid. | Review and then correct the Request parameters, then try again.
ACQ.TRADE_NOT_EXIST | The trade transaction does not exist. | Check whether the value of *trade_no* and *out_trade_no* is correct, and then try again.

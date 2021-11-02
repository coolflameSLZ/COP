# COSCO Syncon Hub API Specification for Reefer Booking

[toc]


## Update Logs

v1.0.0 (2021-08-30)

1. **`A `**  Added the API Reefer Booking interface. 
2. **`A `**  Added the API Reefer Order Query interface. 
3. **`A `**  Added the API Reefer Order Details interface. 

## Http Header

HTTP header information description

| HTTP Header             | Type   | Required | Description             | Remarks |
| ----------------------- | ------ | -------- | ----------------------- | ------- |
| X-Consumer-Forwarder-ID | String | no       | Forwarder company sapId |         |

- X-Consumer-Forwarder-ID

  ```
  # If you want to submit an order/booking for your subsidiary,add this header to the request.The value is sapid of the subsidiary
  
  X-Consumer-Forwarder-ID: 1234567890
  ```

  


## Booking Interface

Customer can place booking throught this interface.  The interface will generate the order with the charge details of the product and related discounts. While customer can apply the coupons to deduct the ocean freight of the Product as well. The avaible counpons infomation can be obtain through the Sport Rate Detail interface. Please note that the minimum amount of the ocean freight for one single container after deduction shall be 1 US dollar. No change in coupon amount. For the details of the Booking Rules, please refer to the Syncon Hub online.



### 1. Informations

* **Path**: /service/synconhub/shipment/reefer/booking
* **Method**: POST

### 2. Request parameters

| Name                        |   Type    | Required |                         Description                          |                  **Remarks**                  |
| :-------------------------- | :-------: | :------: | :----------------------------------------------------------: | :-------------------------------------------: |
| productId                   |  string   |   yes    |                          product id                          |                                               |
| preferPaymentTerms          |  String   |   yes    |                  prefer payment terms type                   |              optional value: P,C              |
| specificPaymentTerms        | Object[]  |    no    |                 charge payment terms option,                 |                                               |
| \|--- chargeType            |  String   |   yes    | chargeType from surcharge query interface. For ocean charges, chargeType is 'OCEAN_FEE', loadingService is 'LOADING_TRANSFER', and dischargeservice is 'DISCHARGE_TRANSFER' |                                               |
| \|--- chargeName            |  String   |    no    | chargeName from surcharge query interface.Chargename is not required for ocean fee and intermodal services |                                               |
| \|--- paymentTerms          |  String   |   yes    |                                                              |              optional value: P,C              |
| loadingServiceNo            |  String   |    no    |  intermodalServiceNo from intermodal service query results   |                                               |
| dischargeServiceNo          |  String   |    no    |  intermodalServiceNo from intermodal service query results   |                                               |
| containerInfos              | object [] |   yes    |                        container info                        |                                               |
| \|---containerType          |  string   |   yes    |                                                              |         **optional value:** 20RF,40RQ         |
| \|---quantity               |  integer  |   yes    |                                                              |                  **min**: 1                   |
| \|---estimateWeight         |  decimal  |    no    |                estimate cargo weight per case                |                 **unit**: TON                 |
| \|---cargoValue             |  decimal  |    no    | The cargoValue is mandatory when reefer value-added service "MED" is selected and it's currency type is USD |                **max**: 500000                |
| reeferValueAddedServices    | string [] |    no    | reefer value-added services, the specific type can be obtained from product details, like ["MED", "IOT"]. |                                               |
| blQuantity                  |  integer  |    no    |                         bl quantity                          |          **default**: 1, **min**: 1           |
| couponsInfo                 |  object   |    no    |                       coupon use info                        |                                               |
| \|---amount                 |  integer  |   yes    |                                                              |                  **min**: 1                   |
| \|---couponId               |  string   |   yes    |                                                              |                                               |
| includeInsurance            |  boolean  |    no    |                  whether include insurance                   |              **default**: false               |
| shipperInfo                 |  object   |    no    |                         shipper info                         |                                               |
| \|---name                   |  string   |   yes    |                                                              |  **length**: [1, 70], **English characters**  |
| \|---addressLine1           |  string   |   yes    |              country / region / province / city              |  **length**: [1, 35], **English characters**  |
| \|---addressLine2           |  string   |   yes    |                         street name                          |  **length**: [1, 35], **English characters**  |
| \|---addressText            |  string   |    no    |                      additional address                      | **length**: [1, 1200], **English characters** |
| \|---phone                  |  string   |   yes    |                                                              |              **length**: [1, 22]              |
| \|---email                  |  string   |    no    |                                                              |             **length**: [1, 400]              |
| \|---carrierCustomerCode    |  string   |    no    |                    carrier customer code                     |                                               |
| consigneeInfo               |  object   |    no    |                        consignee info                        |                                               |
| \|---name                   |  string   |   yes    |                                                              |  **length**: [1, 70], **English characters**  |
| \|---addressLine1           |  string   |   yes    |              country / region / province / city              |  **length**: [1, 35], **English characters**  |
| \|---addressLine2           |  string   |   yes    |                         street name                          |  **length**: [1, 35], **English characters**  |
| \|---addressText            |  string   |    no    |                      additional address                      | **length**: [1, 1200], **English characters** |
| \|---phone                  |  string   |   yes    |                                                              |              **length**: [1, 22]              |
| \|---email                  |  string   |    no    |                                                              |             **length**: [1, 400]              |
| \|---carrierCustomerCode    |  string   |    no    |                    carrier customer code                     |                                               |
| notifyPartyInfo             |  object   |    no    |                      notify party info                       |                                               |
| \|---name                   |  string   |   yes    |                                                              |  **length**: [1, 70], **English characters**  |
| \|---addressLine1           |  string   |   yes    |              country /region / province / city               |  **length**: [1, 35], **English characters**  |
| \|---addressLine2           |  string   |   yes    |                         street name                          |  **length**: [1, 35], **English characters**  |
| \|---addressText            |  string   |    no    |                      additional address                      | **length**: [1, 1200], **English characters** |
| \|---phone                  |  string   |   yes    |                                                              |              **length**: [1, 22]              |
| \|---email                  |  string   |    no    |                                                              |             **length**: [1, 400]              |
| emergencyContactInfo        |  object   |   yes    |                                                              |                                               |
| \|---name                   |  string   |    no    |                                                              |  **length**: [1, 70], **English characters**  |
| \|---email                  |  string   |   yes    |                                                              |             **length**: [1, 400]              |
| \|---mobile                 |  string   |    no    |                                                              |              **length**: [1, 15]              |
| \|---phone                  |  string   |    no    |                                                              |              **length**: [1, 22]              |
| \|---address                |  string   |    no    |                                                              | **length**: [1, 245], **English characters**  |
| cargoInfo                   |  object   |   yes    |                          cargo info                          |                                               |
| \|---desc                   |  string   |   yes    |                          cargo name                          |  **length**: [1, 60], **English characters**  |
| \|---packageType            |  string   |   yes    |                                                              |                                               |
| \|---quantity               |    int    |   yes    |                                                              |                                               |
| \|---weight                 |  decimal  |   yes    |                                                              |                 **unit**: KG                  |
| \|---volume                 |  decimal  |   yes    |                                                              |            **unit**: m<sup>3</sup>            |
| \|---remarks                |  string   |    no    |                                                              |            **English characters**             |
| \|---reeferTemperatureType  |  string   |    no    |                Optional: CELSIUS, FAHRENHEIT                 |                                               |
| \|---reeferTemperatureValue |  decimal  |    no    |                                                              |      **max**: 999.99, **digits**: [3, 2]      |
| \|---reeferVentilationType  |  string   |    no    | Optional: CUBIC_FEET_PER_MINUTE, CUBIC_METER_PER_HOUR, DEGREES, PERCENTAGE |                                               |
| \|---reeferVentilationValue |  decimal  |    no    |                                                              |       **max**: 999, **digits**: [3, 0]        |
| \|---reeferGenSetType       |  string   |    no    |         Optional: NOT_REQUIRED, UNDER_SLUNG, CLIP_ON         |                                               |
| \|---reeferHumidityValue    |  decimal  |    no    |                                                              | **min**: 1, **max**: 100, **digits**: [3, 0]  |
| remarks                     |  string   |    no    |                                                              |                                               |


### 3. Request sample

```javascript
{
    "productId": "8a5e11157351b2df01735562622a0000",
    "loadingServiceNo":"8a5e112478e8ac470178e90d79cc0025",
    "dischargeServiceNo":"8a5e112478e8ac470178e8bbe0d90001",
    "preferPaymentTerms": "P",
    "specificPaymentTerms":[
      {
          "chargeType":"OCEAN_FEE",
          "chargeName":null,
          "paymentTerms":"C"
      },
      {
          "chargeType":"LOADING_TRANSFER",
          "chargeName":null,
          "paymentTerms":"C"
      },
      {
          "chargeType":"DISCHARGE_TRANSFER",
          "chargeName":null,
          "paymentTerms":"C"
      },  
      {
          "chargeType":"THC",
          "chargeName":"THC",
          "paymentTerms":"C"
      }
    ],
    "containerInfos": [
        {
            "containerType": "20RF",
            "quantity": 1,
            "estimateWeight": 22,
            "cargoValue": 1
        }
    ],
    "reeferValueAddedServices": [
    	"MED", "IOT"
    ],
    "blQuantity": 1,
    "couponsInfo": {
        "amount": 1,
        "couponId": "8a5e11227152917a017152b6049f0002"
    },
    "includeInsurance": true,
    "shipperInfo": {
        "name": "shipper name",
        "addressLine1": "CN",
        "addressLine2": "ren min lu",
        "phone": "12345678910",
        "email": "test@test.com",
        //if we are going to book cosplus product,we must pass sapid of bond here
        //For NVO customer the carrierCustomerCode is FMC sapId,For other customer the carrierCustomerCode is company sapId
        "carrierCustomerCode": "1234567890"  
    },
    "consigneeInfo": {
        "name": "consignee name",
        "addressLine1": "CN",
        "addressLine2": "jian she lu",
        "phone": "12345678910",
        "email": "test@test.com",
        //if we are going to book cosplus product,we must pass sapid of bond here
        //For NVO customer the carrierCustomerCode is FMC sapId,For other customer the carrierCustomerCode is company sapId
        "carrierCustomerCode": "1234567890"
    },
    "notifyPartyInfo": {
        "name": "notify party name",
        "addressLine1": "CN",
        "addressLine2": "jian she lu",
        "phone": "12345678910",
        "email": "test@test.com"
    },
    "emergencyContactInfo": {
        "name": "contacter name",
        "email": "test@test.com",
        "mobile": "123456",
        "phone": "12345678910",
        "address": "ren min lu"
    },
    "cargoInfo": {
        "desc": "desk",
        "packageType": "IV",
        "quantity": 1000,
        "weight": 1000.50,
        "volume": 1000.50,
        "remarks": "remarks...",
        "reeferTemperatureType": "CELSIUS",
        "reeferTemperatureValue": 10,
        "reeferVentilationType": "CUBIC_FEET_PER_MINUTE",
        "reeferVentilationValue": 10,
        "reeferGenSetType": "CLIP_ON",
        "reeferHumidityValue": 1
    },
    "remarks": "remarks..."
}
```

### 4. Response parameters

| Name    | Description    |
| ------- | -------------- |
| code    | status code    |
| message | detail message |
| data    | response data  |

### 5. Response sample

```javascript
{ 
    "code": 0,
    "message": "",
    "data": {
	  "orderNo": "AR00000048",
	  "brNo": "4880315040"
    }
}
```

### 6. Error sample

```javascript
{
    "code": 20003,
    "message": "the inventory is not enough"
}

{
    "code": 20073,
    "message": "Charge Type:XXX,ChargeName:XXX cannot be set to Collect"
}
```

### 7. Error Code Specification

Please refer to the [General Error Codes](#General Error Codes) .



## Order Query Interface

Query brief information of eligible orders based on conditions

### 1. Informations


- **Path**: /service/synconhub/order/reefer/search
- **Method**: POST

### 2. Request parameters

|      Name      |  Type   | Required |         Description         |              **Remarks**               |
| :------------: | :-----: | :------: | :-------------------------: | :------------------------------------: |
|      brNo      | string  |    no    |   booking request number    |                                        |
|    orderNo     | string  |    no    |          order no           |                                        |
|  orderStatus   | string  |    no    |        order status         | **optional value:** CONFIRMED,CANCELED |
|      page      | integer |   yes    |          page size          |      **default:**1，**minimum:**1      |
|      size      | integer |   yes    |         record size         |   **default:**20, **range:**[1, 30]    |
| updateDateFrom | ISODate |    no    | Order update time from(GMT) |                                        |
|  updateDateTo  | ISODate |    no    |  Order update time to(GMT)  |                                        |

### 3. Request sample

```javascript
{
  "brNo": "4880315040",
  "orderNo": "AR00000048",
  "orderStatus": "CONFIRMED",
  "page": 1,
  "size": 20,
  "updateDateFrom": "2020-07-20T03:14:55.200Z",
  "updateDateTo": "2020-07-25T03:14:55.200Z"
}
```

### 4. Response parameters

| Name    | Description          |
| ------- | -------------------- |
| code    | status code          |
| message | detail message       |
| data    | paging query results |

### 5. Response sample

```java
{
    "code": 0,
    "message": "",
    "data": {
        "content": [
            {
                "orderNo": "AR00000048", // order number
                "vesselName": "COSCO YANTIAN",
                "voyageNo": "020", // voyage number
                "porCityName": "Shekou",
                "fndCityName": "Durban",
                "etd": "2020-08-04 00:00",
                "carrier": "集运", 
                "orderStatus": "CONFIRMED",
                "offlineCharges": {
                    "ORIGINAL": {
                        "onlineTotalAmount": 7443.37,
                        "onlinePaidAmount": 0,
                        "onlineUnpaidAmount": 7443.37, 
                        "minimumUnpaidAmount": 7443.37, // unpaid amount of down payment in installments
                        "balanceUnpaidAmount": 0.00,
                        "currencyType": "CNY",
                        "deadFreightAmount": 0,
                        "remissionAmount": 0
                    }
                },
                "effectiveStartDate": "2020-07-05T00:00:00.000Z",
                "effectiveEndDate": "2020-12-23T23:59:59.000Z",
                "orderUser": "testuser",
                "direction": "W",
                "brNoInfos": [ // booking number info
                    {
                        "isBrNoInternal": true, // is our platform booking request
                        "brNo": "4880315040" // booking request number
                    }
                ],
                "orderTime": "2020-07-06T08:57:05.460Z",
                "orderContainerSummaryDTOList": [ // container info
                    {
                        "cntrType": "20RF" , // container type
                        "avaCount": 0, // returnable container count
                        "validTotalCount": 1,
                        "bookedCount": 1
                    }
                ]
            }
        ],
        "number": 1,
        "size": 20,
        "totalPages": 1,
        "totalElements": 1,
        "first": true, // is first page
        "last": true, // is last page
        "empty": false
    }
}
```

### 6. Error sample

```javascript
{
    "code": 20000,
    "message": "cannot identify the user, please contact support for help"
}
```

### 7. Error Code Specification

Please refer to the [General Error Codes](#General Error Codes) .



## Order Details Query Interface

Query order details by order number

### 1. Informations

- **Path**: /service/synconhub/order/detail/{orderNo}
- **Method**: GET

### 2. Path parameters

|  Name   |  Type  | Required | Description  |
| :-----: | :----: | :------: | :----------: |
| orderNo | String |   yes    | order number |

### 3. Request sample

```javascript
GET /service/synconhub/order/detail/AR00000048
```

### 4. Response parameters

| Name    | Description       |
| ------- | ----------------- |
| code    | status code       |
| message | detail message    |
| data    | order detail info |

### 5. Response sample

```javascript
{
    "code": 0,
    "message": "",
    "data": {
        "orderNo": "AR00000048", // order number
        "orderStatus": "CONFIRMED",
        "orderTime": "2020-07-06T08:57:05.460Z",
        "offlineCharges": {
            "ORIGINAL":[ // charge type:[ORIGINAL，EXTRA_CHARGE，REFUND，WITHDRAW_COMMISSION]
                {
                    "currencyType": "CNY",
                    "charge": 1622.17, 
                    "deadFreightAmount": 0,
                    "remissionAmount": 0
                }
            ]
        }, 
        "sailingProduct": {...}, // product info，for the specific structure, please refer to "Spot Rate Query"
        "orderContainerSummaries": [ // container info
            {
                "cntrType": "20RF",
                "avaCount": 0, // returnable container 
                "validTotalCount": 1,
                "bookedCount": 1
            }
        ],
        "bookingContainers": [
            {
                "bookingNo": "4880315040", // booking number
                "status": "Confirmed", // status values : ['New','Confirmed','Cancelled']
                "cntrType": "20RF", // container type
                "count": 1
            }
        ],
       "transferServices": [
            {
                "cityId": "738872886233881",
                "cityName": "Singapore",
                "ctryCode": "SG",
                "cityFullNameEn": "Singapore",
                "cityFullNameCn": "新加坡",
                "transferType": "DISCHARGE",
                "transportMode": "RAIL",
                "transportTerms": "CY",
                "transitTime": 2,
                "facilityCode": null,
                "facilityName": null
            },
            {
                "cityId": "738872886232879",
                "cityName": "Ningbo",
                "ctryCode": "CN",
                "cityFullNameEn": "Ningbo",
                "cityFullNameCn": "宁波",
                "transferType": "LOADING",
                "transportMode": "RAIL",
                "transportTerms": "CY",
                "transitTime": 2,
                "facilityCode": null,
                "facilityName": null
            }
        ]
    }
}
```

### 6. Error sample

```javascript
{
    "code": 20006,
    "message": "The order does not exist"
}
```

### 7. Error Code Specification

Please refer to the [General Error Codes](#General Error Codes) .

## General Error Codes

 - 20000: "cannot identify the user, please contact support for help"
 - 20001: "The system cannot process the request,please try again or contact support for help"
 - 20002: "The product cannot be found"
 - 20003: "Inventory not enough"
 - 20004: "Out of time"
 - 20005: "Invalid box type"
 - 20006: "Order does not exist"
 - 20007: "Inventory of this product could not be found"
 - 20008: "Promotion inventory in short supply"
 - 20009: "Coupon for this order could not be found"
 - 20010: "The number of coupons available is smaller than the number of coupons available for the order"
 - 20011: "Discount plan inventory shortage or price change, please place a new order"
 - 20012: "Invalid parameter, promotion plan verification failed"
 - 20013: "Insured service does not exist or has expired, please re-order"
 - 20014: "Some charges not allowed to change currency"
 - 20015: "Down payment is not allowed"
 - 20016: "All charges of Instance Booking have to be paid online"
 - 20017: "No currency conversion is allowed for the order fee"
 - 20018: "Only customers in Mainland China are allowed to purchase Instance Booking"
 - 20019: "Wrong order product type"
 - 20020: "Some fees only support CNY or USD"
 - 20021: "Company country/region is different from country/region of POR or FND, purchase is not allowed"
 - 20022: "The down payment ratio must be greater than 0"
 - 20023: "Does not meet the minimum down payment percentage range"
 - 20024: "Does not meet the maximum down payment percentage range"
 - 20025: "The order is missing some necessary cost items"
 - 20026: "The product type of the order can only be I or P"
 - 20027: "The payment method for part of the order is wrong"
 - 20028: "Some cost items are missing to prepaid attributes"
 - 20029: "Online collect payment not support partial payment for now"
 - 20030: "There are invalid charges, please place a new order"
 - 20031: "Coupon overuse"
 - 20032: "System charge item pre-paid configuration error, please contact customer service for processing"
 - 20033: "Due to the system configuration, some fees are not allowed to be paid in USD or to switch to pre-paid, please contact the customer service for processing"
 - 20034: "Dear user, this product is no longer available, please contact customer service"
 - 20035: "Online payment cannot include both prepaid and collect-paid, please contact customer service for processing"
 - 20036: "The quantity of BL should be greater that 1"
 - 20037: "Unable to purchase this product for now, please contact customer service for processing"
 - 20038: "Incorrect product information, please contact customer service for processing"
 - 20039: "This payment method is not currently supported, please contact customer service for processing"
 - 20040: "The page size must be greater than 0"
 - 20041: "The page size should be between 0 and 30."
 - 20042: "Parameter verification error"
 - 20043: "Cross Booking invalid inventory"
 - 20044: "Cross Booking wrong shipping date information"
 - 20045: "Cross Booking insufficient stock"
 - 20046: "Cross Booking cargo container not found"
 - 20047: "This cargo nature type is not supported at the moment, please contact customer service"
 - 20048:"Failed to get enterprise information"
 - 20049:''Order query only supports data query within two months"
 - 20051:"Sorry for system busy, please try again later"
 - 20053:"CrossBooking schedule information LTD is wrong"
 - 20054: "Address cannot be empty"
 - 20055: "Sub-company carrier customer code out of scope."
 - 20056: "The TEU purchase limit for the week in which the product is sold is insufficient"
 - 20057: "CrossBooking contract number does not exist"
 - 20058: "CrossBooking start week cannot be earlier than the current four weeks"
 - 20059: "API booking cannot purchase FMC products"
 - 20060: "Permission denied"
 - 20061: "CosPlus | product booking must fill in all shipping related party information"
 - 20062: "Wrong import and export type of intermodal service"
 - 20063: "The intermodal service does not exist"
 - 20064: "Can't find the city"
 - 20066: "Some container types do not support intermodal service"
 - 20067: "The intermodal service fee is charged by weight, and the estimated cargo weight of the box type is required"
 - 20068: "The estimated weight of the container is not in the optional range, please re-order"
 - 20070: "CosPlus | Shipper or consignee of shipping related party filled in the product booking is illegal."
 - 20072: "API cannot purchase this type of product"
 - 20073: "Charge Type:xxx,ChargeName:xxx cannot be set to Collect"
 - 20074: "The long-term product does not exist"
 - 20075: "The long-term product is off-shelf"
 - 20076: "Insufficient amount of available containers"
 - 20077: "It is not the time allowed for booking"
 - 20078: "Ocean charge cannot do prepaid-collect switch"
 - 20079: "The reefer value-added service does not exist or has expired"
 - 20080: "The reefer value-added service type is incorrect"
 - 20081: "The value of a single box of goods is more than US $500,000, please buy offline, the online process is not supported"
 - 20082: "When MED are selected for reefer value-added services, the cargoValue is required"
 - 20083: "You cannot choose more than one cold box value-added service other than IOT"
 - 20084: "cargoInfo.reeferGenSetType cannot be empty"
 - 20085: "cargoInfo.reeferTemperatureType and cargoInfo.reeferTemperatureValue  cannot be empty"
 - 20086: "Ventilation Settings cannot be added when the reefer temperature is less than 0 C or 32 F"
 - 20087: "Ventilation setting must be added when the reefer temperature is ≥0°C or ≥32°F"
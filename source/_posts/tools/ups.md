---
title: UPS Tracking API
tags: [Tool]
date: 2019-09-21 13:58:55
---

Request Url: 

JSON Request Sample:
```
{
    "UPSSecurity": {
        "UsernameToken": {
            "Username": " userHere",
            "Password": " passHere"
        }
    },
    "ServiceAccessToken": {
        "AccessLicenseNumber": "licNoHere"
    },
    "Request": {
        "RequestOption": "15"
    },
    "InquiryNumber": "1Z12345E0291980793",
    "TrackingOption": "02"
}
```

UPS SurePost rates:
92: UPS SurePost Less than 1LB - USL
93: UPS SurePost 1LB or greater - USG
94: UPS SurePost BPM - USB
95: UPS SurePost Media Mail - USM

参考：
https://www.ups.com/upsdeveloperkit

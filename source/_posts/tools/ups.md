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
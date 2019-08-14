---
title: Postman
date: 2019-08-10 15:09:21
tags: [TODO, Java, Tool]
---

## Request
headers
body

### Presets

## Collection

## Export & Import

## Environment

## Mock

## Test
```javascript
pm.test("Response is Success", function () {
    pm.response.to.have.status(200);
    var jsonData = pm.response.json();
    pm.expect(jsonData.success).to.eql(true);
    pm.expect(jsonData.messageId).to.eql("10000");
});
```

## Runner

## Workspace

### Team
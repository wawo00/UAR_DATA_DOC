Guides of syncing UAR data to custom target
===========================================

1. Get started
--------------

First of all, please turn on the sync status of the Apps and set your
target URL:

1. Login UPLTV, find "Advertising ROI Analysis" on page "Advanced
   Features" - "User-level Data".
2. Check to tab "Custom Target".
3. Click the "edit" button of the App that you want to sync.
4. Turning the status switch on, filling your target URL and Apple
   ID(for iOS App only). Then, click the "check" button to save it.
5. For the App that enabled sync, there will be a "Send testing data"
   button. Please click the button to verify that the sync is working
   normally.

**Important**: For security reasons, we strongly recommend that you use
a "https" URL as target URL!

2. Content of sync request
--------------------------

**Request Address**\ ：Your target URL

**Request Method**\ ：POST

**Request content**\ ：

The content of sync request a json array, and each element in the array
is an user-level ad revenue data record. These records will consist of
the following parameters:

+----------------------+------------+------------+--------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------+
| Parameters           | Type       | Required   | Description                                                                                                                                      | Example                                |
+======================+============+============+==================================================================================================================================================+========================================+
| upltv\_product\_id   | string     | Yes        | Product ID in UPLTV, a 7 digits string.                                                                                                          | 1001786                                |
+----------------------+------------+------------+--------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------+
| product\_mark        | string     | Yes        | Product identification info, including the App platform(android or ios) and App identifier(package name of Android App & Apple ID of iOS App).   | android:com.testing.new                |
+----------------------+------------+------------+--------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------+
| device\_id           | string     | Yes        | Device identification info, GAID of Android App & IDFA of iOS App.                                                                               | 1E2DFA89-496A-47FD-9941-DF1FC4E6484A   |
+----------------------+------------+------------+--------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------+
| event\_name          | string     | Yes        | Event name that generated the revenue.                                                                                                           | UPLTV\_adrev                           |
+----------------------+------------+------------+--------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------+
| currency             | float      | Yes        | Currency of the revenue.                                                                                                                         | usd                                    |
+----------------------+------------+------------+--------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------+
| revenue              | detail     | Yes        | Daily total revenue amount of special user.                                                                                                      | 17.19                                  |
+----------------------+------------+------------+--------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------+
| event\_time          | datetime   | Yes        | Record generation time(UTC). Ususlly, it will be 0am on the next day after the revenue generation, in format: yyyy-mm-dd hh:mm:ss                | 2019-07-07 00:00:00                    |
+----------------------+------------+------------+--------------------------------------------------------------------------------------------------------------------------------------------------+----------------------------------------+

Sample of request content：

::

    [{
        "upltv_product_id":"1001786",
        "product_mark":"android:com.testing.new",
        "device_id":"38400000-8cf0-11bd-b23e-10b96e40000d",
        "event_name":"UPLTV_adrev",
        "revenue": 0.88,
        "currency": "usd",
        "event_time":"2019-07-17 00:00:00"
    },
    {
        "upltv_product_id":"1001785",
        "product_mark":"ios:1759002391",
        "device_id":"1E2DFA89-496A-47FD-9941-DF1FC4E6484A",
        "event_name":"UPLTV_adrev",
        "revenue": 0.89,
        "currency": "usd",
        "event_time":"2019-07-17 00:00:00"
    }]

3. Content of return
--------------------

Please provide the following content in the return message after
receiving the push request:

+--------------+----------+------------+-----------------------------------------------------------------------+-------------+
| Parameters   | Type     | Required   | Description                                                           | Example     |
+==============+==========+============+=======================================================================+=============+
| code         | number   | Yes        | Status code, **return 200** when processing the request successful.   | 200         |
+--------------+----------+------------+-----------------------------------------------------------------------+-------------+
| message      | string   | Yes        | Brief description of the request processing result.                   | Succeeded   |
+--------------+----------+------------+-----------------------------------------------------------------------+-------------+

Sample of return content

::

    {
        "code":200,
        "message":"Succeeded"
    }

4. Whitelist the IP of UPLTV
----------------------------

Please add the following IP which UPLTV may use for sync requests to
your whitelist: **120.132.23.7**

5. Important remarks
--------------------

1. UPLTV will start to push the data of previous day at UTC 7pm. Sync
   tasks of different Apps will be postponded after that time according
   to the actual tasks queue.
2. The daily data will be splited into multiple requests, and up to 20
   records in each request.
3. The request will be considered to be failed if the request timed
   out(beyond 15000ms), or the code in the returned content is not 200.
4. For the failed request, UPLTV will attempt to re-post and retry up to
   3 times.
5. Please contact your Account Manager, if you need to obtain the data
   which is synced failed after retrying 3 times.
6. Due to the retry mechanism, UPLTV recommend that to update your local
   data by taking the parameters "upltv\_product\_id" &
   "device\_id\_combination" together as unique indentifier when
   processing the request content.


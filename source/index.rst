Guides of the Seeds Acquisition API
===================================

This is an operational documentation of Germinative Audience data
(hereafter called “Seeds”) acquisition API which is provided by UPLTV.

1. Keys & IP whitelist
----------------------

Get Keys
^^^^^^^^

First of all, you need get your secret Keys include “Client ID” &
“Secret”

1. Login UPLTV, find “Get the Seeds” part on page “Advanced Features -
   User-level Data”.
2. Click “Send the Keys to secure mailbox”.
3. Check the mail with Keys in your secure mailbox.

//todo 示意截图

IP whitelist
^^^^^^^^^^^^

Then, you should put your IPs into the whitelist, only requests that
from IPs in the whitelist are approved. 1. Login UPLTV, find “Get the
Seeds” part on page “Advanced Features - User-level Data”. 2. Click the
edit button next to “IP whitelist” box. 3. Input your IPs, separated
multiple IP records by line break, and up to 10 records.

//todo 示意截图

2. Get access_token
-------------------

**Request Address:**\ https://open-api.upltv.com/oauth/access_token
**Request Method:**\ POST **Request Parameters:**

============= ====== ======== ================================================
Parameters    Type   Required Description
============= ====== ======== ================================================
grant_type    String Yes      Fixed as: **client_credentials**
client_id     String Yes      **Client ID** in the mail to your secure mailbox
client_secret String Yes      **Secret** in the mail to your secure mailbox
============= ====== ======== ================================================

**Return Parameters:**

============ ====== ===============================================
Parameters   Type   Description
============ ====== ===============================================
expires_in   int    The effective duration of the Token, in seconds
access_token String Token of the Seeds Acquisition API
============ ====== ===============================================

**Return Sample:**

::

   {
    "expires_in": 3600,
    "access_token": "xxxxxxxxxx"
   }

3. The Seeds Acquisition API
----------------------------

**Request Address:**\ https://open-api.upltv.com/api/v.1.0.0/app/
**<upltv_pid>**/core/**<page>**/**<page_size>** **Request Method:**\ GET
**header 信息：**\ 需设置 header 信息 “Authorization:Bearer
**<access_token>**” **Related Parameters:**

+------------------------+--------------+--------------+--------------+
| Parameters             | Type         | Required     | Description  |
+========================+==============+==============+==============+
| access_token           | String       | Yes          | Token of the |
|                        |              |              | Seeds        |
|                        |              |              | Acquisition  |
|                        |              |              | API          |
+------------------------+--------------+--------------+--------------+
| upltv_id               | -            | Yes          | **App ID**   |
|                        |              |              | of your App  |
|                        |              |              | on UPLTV,    |
|                        |              |              | find it on   |
|                        |              |              | page “App    |
|                        |              |              | Management - |
|                        |              |              | App List” on |
|                        |              |              | UPLTV        |
|                        |              |              | dashboard    |
+------------------------+--------------+--------------+--------------+
| page                   | -            | Yes          | Page number  |
|                        |              |              | (default as  |
|                        |              |              | 1)           |
+------------------------+--------------+--------------+--------------+
| page_size              | -            | Yes          | Number of    |
|                        |              |              | records on   |
|                        |              |              | each page    |
|                        |              |              | (Up to 1000, |
|                        |              |              | default as   |
|                        |              |              | 20)          |
+------------------------+--------------+--------------+--------------+

**Return Parameters:**

========== ====== =============================================
Parameters Type   Description
========== ====== =============================================
country    String iso code of the country that the user located
device     String user’s device id
page       int    current page number
pageCount  int    count of pages
========== ====== =============================================

**Return Sample:**

::

   {
       "code": 200,
       "data": {
           "list": [
               {
                   "country": "cn"
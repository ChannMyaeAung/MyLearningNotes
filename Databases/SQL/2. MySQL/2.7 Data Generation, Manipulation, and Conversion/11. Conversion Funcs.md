# Conversion Functions

- To use `case()`, we provide a value or expression the `as` keyword and the type to which we want the value converted.

```bash
mysql> SELECT CAST('1456328' AS SIGNED INTEGER);
+-----------------------------------+
| CAST('1456328' AS SIGNED INTEGER) |
+-----------------------------------+
|                           1456328 |
+-----------------------------------+
1 row in set (0.00 sec)
```

- When converting a string to a number, the `cast()` function will attempt to convert the entire string from left to right.
- If any non-numeric characters are found, the conversion halts without an error.

```bash
mysql> SELECT CAST('999ABC111' AS UNSIGNED INTEGER);
+---------------------------------------+
| CAST('999ABC111' AS UNSIGNED INTEGER) |
+---------------------------------------+
|                                   999 |
+---------------------------------------+
1 row in set, 1 warning (0.00 sec)

mysql> show warnings;
+---------+------+------------------------------------------------+
| Level   | Code | Message                                        |
+---------+------+------------------------------------------------+
| Warning | 1292 | Truncated incorrect INTEGER value: '999ABC111' |
+---------+------+------------------------------------------------+
1 row in set (0.00 sec)
```


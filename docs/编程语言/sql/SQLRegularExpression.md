## MySQL正则表达式

| 模式           | 描述                                                         | 示例                                                  |
| :------------- | :----------------------------------------------------------- | ----------------------------------------------------- |
| `^`            | **匹配输入字符串的开始位置。**如果设置了 RegExp 对象的 Multiline 属性，^ 也匹配 '\n' 或 '\r' 之后的位置。 | SELECT name FROM person_tbl WHERE name REGEXP '^st';  |
| `$`            | **匹配输入字符串的结束位置。**如果设置了 RegExp 对象的 Multiline 属性，$ 也匹配 '\n' 或 '\r' 之前的位置。 | SELECT name FROM person_tbl WHERE name REGEXP 'ok$';  |
| `.`            | **匹配除 '\n' 之外的任何单个字符。**要匹配包括 '\n' 在内的任何字符，请使用像 '[.\n]' 的模式。 | SELECT name FROM person_tbl WHERE name REGEXP '.0';   |
| `[...]`        | **字符集合**。匹配所包含的任意一个字符。例如，'[abc]' 可以匹配 'plain' 中的 'a'。 | SELECT name FROM person_tbl WHERE name REGEXP 'mar';  |
| `[^...]`       | **负值字符集合。**匹配未包含的任意字符。例如，'[^abc]' 可以匹配 'plain' 中的 'p'。 | SELECT name FROM person_tbl WHERE name REGEXP '^mar'; |
| `p1 | p2 | p3` |                                                              |                                                       |
| `*`            | **匹配前面的子表达式零次或多次。**例如，zo* 能匹配 'z' 或 'zoo'。* 等价于 {0,}。 |                                                       |
| `+`            | **匹配前面的子表达式一次或多次（即至少要有一次）。**例如，'zo+' 能匹配 'zo' 或 'zoo'，但不能匹配 "z"。+ 等价于 {1,}。 |                                                       |
| `n`            | **n 是一个非负整数，表示匹配前面的表达式出现 n 次。**例如，'o{2}' 不能匹配 'Bob' 中的 'o'，但是能匹配 "food" 中的两个 o。 |                                                       |
| `{n,m}`        | **n 和 m 均为非负整数，并且 n <= m，表示最少匹配 n 次且最多匹配 m 次。** |                                                       |


## Nối chuỗi ##
- Oracle: `'foo'||'bar'`
- Microsoft: `'foo'+'bar'`
- PostgreSQL: `'foo'||'bar'`
- MySQL: `'foo' 'bar'`, `CONCAT('foo','bar')`
## Substring ##
Chỉ số bắt đầu là 1
- Oracle: `SUBSTR('foobar', 4, 2)`
- Microsoft: `SUBSTRING('foobar', 4, 2)`
- PostgreSQL: `SUBSTRING('foobar', 4, 2)`
- MySQL: `SUBSTRING('foobar', 4, 2)`
## Comment ##
- Oracle: `--comment`
- Microsoft: `--comment`, `/*comment*/`
- PostgreSQL: `--comment`, `/*comment*/`
- MySQL: `#comment`, `-- comment`(có dấu cách sau --), `/*comment*/`
## Xác định phiên bản database ##
- Oracle: `SELECT banner FROM v$version`, `SELECT version FROM v$instance`
- Microsoft: `SELECT @@version`
- PostgreSQL: `SELECT version()`
- MySQL: `SELECT @@version`

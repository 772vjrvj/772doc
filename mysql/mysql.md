

root

772vjrvj

testuser

772vjrvj



(보기 파일 확장명 클릭 활성화)

(아래 시작전에 ) C:\Program Files\MySQL\MySQL Server 8.0\bin 경로 복사 

바탕화면 윈도우 표시 우클릭 -> Windows PowerShell(관리자)(A) -> 팝업창 "예" 클릭 -> 

C:\WINDOWS\stsyem32>  SETX PATH "C:\Program Files\MySQL\MySQL Server 8.0\bin;%PATH%"

경로 설정 시스템 고급 경로 설정과 같다.





cmd 창에서

mysql -u root -p 엔터 (환경변수를 잡아줘야 cmd dptj > mysql먹는다.)

패스워드 입력





source employee.sql;  -> 엔터

show database; 데이터베이스 보기

exit 나가기





create user 'testuser'@'localhost' identified by '772vjrvj'

GRANT ALL PRIVILEGES ON *.* TO 'root'@'localhost' IDENTIFIED BY '%password%' WITH GRANT OPTION;





https://linuxize.com/post/how-to-create-mysql-user-accounts-and-grant-privileges/

- Grand all privileges to a user account over a specific database:

  ```
  GRANT ALL PRIVILEGES ON database_name.* TO 'database_user'@'localhost';
  ```

- Grand all privileges to a user account on all databases:

  ```
  GRANT ALL PRIVILEGES ON *.* TO 'database_user'@'localhost';
  ```

- Grand all privileges to a user account over a specific table from a database:

  ```
  GRANT ALL PRIVILEGES ON database_name.table_name TO 'database_user'@'localhost';
  ```

- Grant multiple privileges to a user account over a specific database:

  ```
  GRANT SELECT, INSERT, DELETE ON database_name.* TO database_user@'localhost';
  ```

WITH GRANT OPTION

자신이 가진 권한을 남에게 줄 수 있음
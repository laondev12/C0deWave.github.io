---
layout: default
title: PHP언어로 MariaDB 연동하기
parent: Linux
nav_order: 3
---

# PHP언어로 MariaDB 연동하기

전체 언어 내용이다.

JSON의 형태로 값을 불러주는 방법이다.

```php
<?php
    $mysql_hostname = 'localhost';
    $mysql_username = 'jj';
    $mysql_password = '1234';
    $mysql_port = '3306';
    $mysql_charset = 'utf8';
    $mysql_db = 'appdata';

    //연동 부분이다.
    $connect = mysqli_connect($mysql_hostname, $mysql_username, $mysql_password);
    mysqli_select_db($connect,$mysql_db);

    if($connect->connect_errno){
        echo '[연결실패] : '.$connect->connect_error.'';
    }else{
        //echo '[연결성공!]';
    }

   // 세션 시작
   session_start();

   // 쿼리문 생성
   $sql = "select * from urltext";

   // 쿼리 실행 결과를 $result에 저장
   $result = mysqli_query($connect, $sql);
   // 반환된 전체 레코드 수 저장.
   $total_record = mysqli_num_rows($result);

   // JSONArray 형식으로 만들기 위해서...
   echo "{\"status\":\"OK\",\"num_results\":\"$total_record\",\"results\":[";

   // 반환된 각 레코드별로 JSONArray 형식으로 만들기.
   for ($i=0; $i < $total_record; $i++)
   {
      // 가져올 레코드로 위치(포인터) 이동
      mysqli_data_seek($result, $i);


      $row = mysqli_fetch_array($result);
   echo "{\"imgurl\":$row[imgurl],\"txt1\":\"$row[txt1]\",\"txt2\":\"$row[txt2]\"}";

   // 마지막 레코드 이전엔 ,를 붙인다. 그래야 데이터 구분이 되니깐.
   if($i<$total_record-1){
      echo ",";
   }

   }
   // JSONArray의 마지막 닫기
   echo "]}";
?>
```

정상적으로 잘 작동하는 것을 볼 수 있다.

![스크린샷 2020-06-11 오전 11 52 38](https://user-images.githubusercontent.com/16849874/84339758-19113600-abda-11ea-8c86-491b9997030c.png)

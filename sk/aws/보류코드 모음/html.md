```
awsS3Down.html

<html xmlns:th="http://www.thymeleaf.org">

<head>
   <title>Item</title>
</head>
<script type="text/javascript">

   document.addEventListener("DOMContentLoaded", function(){
      var formObj = document.getElementById("item");
      document.getElementById("btnRegister").onclick = function (){
         console.log("down");
         formObj.setAttribute("action", "/aws/downObject");
         formObj.setAttribute("method", "get");
         formObj.submit();
      }
   });

</script>

<body>
   <h2>aws파일 저장</h2>
   <form id="item" action="success.html" th:action="@{down}" method="GET" enctype="multipart/form-data">
      <table>
         <tr>
            <td>파일</td>
            <td><input type="input" name="filename" /></td>
            <td></td>
         </tr>
      </table>   
   </form>
   <div>
      <button type="submit" id="btnRegister">조회</button>
   </div>
</body>
</html>
```



```
<html xmlns:th="http://www.thymeleaf.org">

<head>
   <title>Item</title>
</head>
<script type="text/javascript">

   document.addEventListener("DOMContentLoaded", function(){
      var formObj = document.getElementById("item");
      document.getElementById("btnRegister").onclick = function (){
         console.log("down");
         formObj.setAttribute("action", "/aws/downObject");
         formObj.setAttribute("method", "get");
         formObj.submit();
      }
   });

</script>

<body>
   <h2>aws파일 다운</h2>
   <form id="item" action="success.html" th:action="@{down}" method="GET" enctype="multipart/form-data">
      <input type="hidden" name="bucket" th:value="${bucket}">
      <table>
         <tr>
            <td>다운받을 파일이름 : </td>
            <td><input type="input" name="filename" /></td>
            <td></td>
         </tr>
         <tr th:each="item : ${resultList}">
            <td align="left" th:text="${item}">${item}</td>
         </tr>
      </table>   
   </form>
   <div>
      <button type="submit" id="btnRegister">조회</button>
   </div>
</body>
</html>
```




## login-with-facebook

1. ขั้นแรกไปที่ https://developers.facebook.com/ หน้านักพัฒนา Facebook และเข้าสู่ระบบด้วยบัญชี Facebook ของคุณ

2. หลังจากเข้าสู่บัญชี Facebook ตอนนี้คลิกที่เมนูแอพของฉันและคลิกที่ลิงค์สร้างแอพ

3. หลังจากคลิกที่ลิงค์สร้างแอพโมดอลจะปรากฏขึ้นบนหน้าเว็บที่นี่คุณต้องกำหนดรายละเอียดชื่อที่แสดงแอพและคลิกที่ปุ่มสร้างรหัสแอพ

4. ไปที่การตั้งค่า » ลิงค์พื้นฐาน

5. ที่นี่คุณต้องระบุโดเมนของแอป URL นโยบายความเป็นส่วนตัว URL ข้อกำหนดในการให้บริการและเลือกหมวดหมู่  สุดท้ายคุณต้องคลิกที่ปุ่มบันทึกการเปลี่ยนแปลง

6. ตอนนี้คุณต้องไปที่หน้าเพิ่มผลิตภัณฑ์โดยคลิกที่ปุ่มบวก  ดังนั้นโมดอลจะปรากฏขึ้นและที่นี่คุณต้องเลือกเว็บไซต์

7. หลังจากเลือกตัวเลือกเว็บไซต์แล้วฟิลด์เว็บไซต์จะปรากฏบนหน้าเว็บและที่นี่คุณต้องกำหนด URL ไซต์แอปของคุณ

8. ตอนนี้คุณต้องเพิ่มผลิตภัณฑ์ดังนั้นไปที่ลิงค์แดชบอร์ดและคุณต้องตั้งค่าผลิตภัณฑ์เข้าสู่ระบบ Facebook

9. เมื่อคุณตั้งค่าผลิตภัณฑ์เข้าสู่ระบบ Facebook แล้วก็โหลดหน้าเว็บใหม่ที่นี่คุณจะต้องกำหนด URI การเปลี่ยนเส้นทาง OAuth ที่ถูกต้องและคลิกที่ปุ่มบันทึกการเปลี่ยนแปลง

10. คุณได้เปลี่ยนสถานะของคุณจาก In Development mode เป็น Live mode สำหรับสิ่งนี้คุณต้องคลิกที่ปุ่ม Off หลังจากคลิกที่ปุ่ม Off จากนั้นโมดอลจะปรากฏขึ้นและที่นี่คุณต้องเลือกหมวดหมู่และคลิกที่ปุ่ม Switch Mode  หลังจากคลิกที่ปุ่ม Switch Mode แอพของคุณจะเป็น Live ซึ่งจำเป็นสำหรับการรับข้อมูลโปรไฟล์จาก Facebook API


### ดาวน์โหลด Facebook SDK สำหรับ PHP
```
composer require facebook/graph-sdk
```

#### config.php
ในไฟล์นี้ก่อนอื่นเราต้องเพิ่มไฟล์ autoload.php ของไลบรารี Facebook SDK สำหรับ PHP โดยใช้คำสั่ง require_once

หลังจากนี้เราต้องตรวจสอบตัวแปรเซสชันเริ่มต้นหรือไม่โดยใช้ฟังก์ชัน PHP session_id ()


ตอนนี้เราต้องเรียกว่า Facebook API ที่นี่เราต้องกำหนด app_id และ app_secret คีย์ที่เราต้องได้รับเมื่อสร้างแอพ Facebook  ที่นี่เราต้องกำหนด default_graph_version ด้วย

```php
<?php
//config.php

require_once 'vendor/autoload.php';

if (!session_id())
{
    session_start();
}

// Call Facebook API

$facebook = new \Facebook\Facebook([
  'app_id'      => '{app_id}',
  'app_secret'     => '{app_secret}',
  'default_graph_version'  => 'v2.10'
]);

?>
```

#### index.php

 ในไฟล์นี้ก่อนอื่นเราต้องเพิ่มไฟล์ config.php โดยใช้คำสั่ง include

 หลังจากนี้เราต้องการรับตัวช่วยการเปลี่ยนเส้นทางการเข้าสู่ระบบสำหรับสิ่งนี้เราได้ใช้เมธอด getRedirectLoginHelper ()

 ตอนนี้เราต้องการตรวจสอบว่าได้รับค่าตัวแปร $ _GET ['code'] แล้วหรือไม่หากได้รับหมายความว่าผู้ใช้มีการเข้าสู่ระบบโดยใช้บัญชี Facebook และแสดงข้อมูลโปรไฟล์บนหน้าเว็บมิฉะนั้นจะแสดงลิงก์สำหรับเข้าสู่ระบบโดยใช้บัญชี Facebook

 

 สมมติว่าได้รับค่าตัวแปร $ _GET ['code'] แล้วเราต้องการตรวจสอบว่าโทเค็นการเข้าถึงถูกเก็บไว้ในตัวแปร $ _SESSION หรือไม่หากเก็บไว้ในตัวแปร $ _SESSION แล้วค่านั้นจะถูกเก็บไว้ในตัวแปรภายในเครื่อง

 แต่สมมติว่าโทเค็นการเข้าถึงไม่ได้รับจากตัวแปร $ _SESSION เราจำเป็นต้องได้รับโดยใช้เมธอด getAccessToken () และจัดเก็บในตัวแปรภายในจากนั้นหลังจากจัดเก็บในตัวแปร $ _SESSION

 หลังจากได้รับโทเค็นการเข้าถึงตอนนี้เราต้องการตั้งค่าโทเค็นการเข้าถึงเริ่มต้นเพื่อใช้กับคำขอโดยใช้เมธอด setDefaultAccessToken ()

 ตอนนี้เราได้ดำเนินการเพื่อรับข้อมูลโปรไฟล์สำหรับที่นี่เราได้ใช้ get () วิธีการและภายใต้วิธีนี้เราต้องกำหนดฟิลด์โปรไฟล์เช่นชื่ออีเมล ฯลฯ และในอาร์กิวเมนต์ที่สองเราต้องกำหนดค่าโทเค็นการเข้าถึง

 สุดท้ายสำหรับการรับข้อมูลโปรไฟล์ผู้ใช้ที่นี่เราใช้ getGraphUser () วิธีนี้จะสร้างคอลเล็กชันผู้ใช้กราฟ

 วิธีนี้ทำให้เราสามารถรับข้อมูลจาก Facebook API และเก็บไว้ในตัวแปร $ _SESSION

 สำหรับการสร้างลิงค์สำหรับเข้าสู่ระบบโดยใช้บัญชี Facebook สำหรับที่นี่เราใช้เมธอด getLoginUrl () วิธีนี้จะส่งผู้ใช้เพื่อเข้าสู่ระบบ Facebook  ในวิธีนี้คุณต้องกำหนด URL การเปลี่ยนเส้นทางและอาร์เรย์สิทธิ์

 


 ด้านล่างนี้คุณสามารถซอร์สโค้ดเพื่อเข้าสู่ระบบด้วยบัญชี Facebook โดยใช้ PHP และแสดงข้อมูลโปรไฟล์บนหน้าเว็บ

```php
<?php

//index.php

include('config.php');

$facebook_output = '';

$facebook_helper = $facebook->getRedirectLoginHelper();

if(isset($_GET['code']))
{
 if(isset($_SESSION['access_token']))
 {
  $access_token = $_SESSION['access_token'];
 }
 else
 {
  $access_token = $facebook_helper->getAccessToken();

  $_SESSION['access_token'] = $access_token;

  $facebook->setDefaultAccessToken($_SESSION['access_token']);
 }

 $_SESSION['user_id'] = '';
 $_SESSION['user_name'] = '';
 $_SESSION['user_email_address'] = '';
 $_SESSION['user_image'] = '';

 $graph_response = $facebook->get("/me?fields=name,email", $access_token);

 $facebook_user_info = $graph_response->getGraphUser();

 if(!empty($facebook_user_info['id']))
 {
  $_SESSION['user_image'] = 'http://graph.facebook.com/'.$facebook_user_info['id'].'/picture';
 }

 if(!empty($facebook_user_info['name']))
 {
  $_SESSION['user_name'] = $facebook_user_info['name'];
 }

 if(!empty($facebook_user_info['email']))
 {
  $_SESSION['user_email_address'] = $facebook_user_info['email'];
 }
 
}
else
{
 // Get login url
    $facebook_permissions = ['email']; // Optional permissions

    $facebook_login_url = $facebook_helper->getLoginUrl('https://greenhouses-pro.co.uk/demo/', $facebook_permissions);
    
    // Render Facebook login button
    $facebook_login_url = '<div align="center"><a href="'.$facebook_login_url.'"><img src="php-login-with-facebook.gif" alt="เข้าสู้ระบบด้วย Facebook" /></a></div>';
}



?>
<html>
 <head>
  <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
  <title>PHP Login using Facebook Account</title>
  <meta content='width=device-width, initial-scale=1, maximum-scale=1' name='viewport'/>
  <script src="https://ajax.googleapis.com/ajax/libs/jquery/2.1.3/jquery.min.js"></script>
  <script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/js/bootstrap.min.js"></script>
  <link href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.6/css/bootstrap.min.css" rel="stylesheet" />
  
 </head>
 <body>
  <div class="container">
   <br />
   <h2 align="center">PHP Login using Facebook Account</h2>
   <br />
   <div class="panel panel-default">
    <?php 
    if(isset($facebook_login_url))
    {
     echo $facebook_login_url;
    }
    else
    {
     echo '<div class="panel-heading">สวัสดีผู้ใช้</div><div class="panel-body">';
     echo '<img src="'.$_SESSION["user_image"].'" class="img-responsive img-circle img-thumbnail" />';
     echo '<h3><b>Name :</b> '.$_SESSION['user_name'].'</h3>';
     echo '<h3><b>Email :</b> '.$_SESSION['user_email_address'].'</h3>';
     echo '<h3><a href="logout.php">Logout</h3></div>';
    }
    ?>
   </div>
  </div>
 </body>
</html>
```

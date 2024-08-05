input[type=text], input[type=password] 
{ width: 100%; 
padding: 12px 20px; 
margin: 8px 0; display: 
inline-block; border: 
1px solid #ccc; boxsizing: border-box; 
position: relative; 
} 
/* Set a style for all buttons */ button 
{ 
 background-color: #4CAF50; 
color: white; padding: 14px 
20px; margin: 8px 0; 
border: none; cursor: pointer; 
width: 60%; position: 
relative; 
} button:hover 
{ opacity: 
0.8;
} 
/* Extra styles for the cancel button */ 
.cancelbtn { width: auto; 
padding: 10px 18px; 
background-color: #f44336; 
} 
/* Center the image and position the close button */ 
.imgcontainer { textalign: center; margin: 
24px 0 12px 0; 
position: relative; 
} img.avatar { 
width: 40%; 
border-radius: 50%; 
} .container { 
padding: 16px; 
} span.psw 
{ 
 float: right; paddingtop: 16px; } 
/* The Modal (background) */ 
.modal { 
 display: none; /* Hidden by default */ 
position: fixed; /* Stay in place */ zindex: 1; /* Sit on top */ left: 0;
top: 0; width: 100%; /* Full width */ 
height: 100%; /* Full height */ 
 overflow: auto; /* Enable scroll if needed */ background￾color: rgb(0,0,0); /* Fallback color */ background-color: 
rgba(0,0,0,0.4); /* Black w/ opacity */ padding-top: 60px; 
} 
/* Modal Content/Box */ .modal￾content { background-color: 
#fefefe; 
 margin: 5% auto 15% auto; /* 5% from the top, 15% from the bottom and centered 
*/ 
 border: 1px solid #888; 
 width: 50%; /* Could be more or less, depending on screen size */ } 
/* The Close Button (x) */ 
.close { 
 position: absolute; 
right: 25px; 
 top: 0; color: 
#000; font-size: 
35px; font￾weight: bold; 
} 
.close:hover, 
.close:focus { 
color: red; 
cursor: pointer; 
} 
/* Add Zoom Animation */ 
.animate { 
 -webkit-animation: animatezoom 0.6s; 
animation: animatezoom 0.6s 
} 
@-webkit-keyframes animatezoom { 
from {-webkit-transform: scale(0)} to 
{-webkit-transform: scale(1)} 
{ 
@keyframes animatezoom { 
 from {transform: scale(0)} 
to {transform: scale(1)} 
} 
/* Change styles for span and cancel button on extra small screens */ 
@media screen and (max-width: 300px) { 
span.psw { 
 display: block; 
float: none; 
 } 
 .cancelbtn { 
width: 100%; 
 }
 PHP CODE login <?php 
session_start();
$user = dataFilter($_POST['uname']); 
$pass = $_POST['pass']; 
$category = dataFilter($_POST['category']); require 
'../db.php' if($category == 1) 
{ 
 $sql = "SELECT * FROM farmer WHERE username='$user'"; // Update column 
name 
 $result = mysqli_query($conn, $sql); 
$num_rows = mysqli_num_rows($result) 
if($num_rows == 0) 
 { 
 $_SESSION['message'] = "Invalid User Credentials!"; 
header("location: error.php"); 
} 
else 
 { 
 $User = $result->fetch_assoc(); 
 if (password_verify($pass, $User['password'])) // Update column name 
 { 
 $_SESSION['id'] = $User['fid']; 
 $_SESSION['Hash'] = $User['hash']; 
 $_SESSION['Password'] = $User['password']; 
 $_SESSION['Email'] = $User['email']; 
 $_SESSION['Name'] = $User['name']; 
 $_SESSION['Username'] = $User['username']; 
 $_SESSION['Mobile'] = $User['mobile'];
 $_SESSION['Addr'] = $User['address']; 
 $_SESSION['Active'] = $User['active']; 
 $_SESSION['picStatus'] = $User['picStatus']; 
 $_SESSION['picExt'] = $User['picExt']; 
 $_SESSION['logged_in'] = true; 
 $_SESSION['Category'] = 1; 
 $_SESSION['Rating'] = 0; 
 // Set picId and picName based on picStatus 
if($_SESSION['picStatus'] == 0) 
 { 
 $_SESSION['picId'] = 0; 
 $_SESSION['picName'] = "profile0.png"; 
 } 
 else 
 { 
 $_SESSION['picId'] = $_SESSION['id']; 
 $_SESSION['picName'] = 
"profile".$_SESSION['picId'].".".$_SESSION['picExt']; 
 } 
 header("location: profile.php"); 
 } 
else 
 { 
 $_SESSION['message'] = "Invalid User Credentials!"; 
header("location: error.php"); 
 } } 
} else 
{ 
 $sql = "SELECT * FROM buyer WHERE busername='$user'"; 
 $result = mysqli_query($conn, $sql); 
$num_rows = mysqli_num_rows($result); 
if($num_rows == 0) 
 { 
 $_SESSION['message'] = "Invalid User Credentials!"; 
header("location: error.php"); 
 } 
else { 
 $User = $result->fetch_assoc(); 
 if (password_verify($pass, $User['bpassword'])) // Update column name 
 { 
 // Set session variables 
 $_SESSION['id'] = $User['bid']; 
 $_SESSION['Hash'] = $User['bhash']; 
 $_SESSION['Password'] = $User['bpassword']; 
 $_SESSION['Email'] = $User['bemail']; 
 $_SESSION['Name'] = $User['bname']; 
 $_SESSION['Username'] = $User['busername']; 
 $_SESSION['Mobile'] = $User['bmobile']; 
 $_SESSION['Addr'] = $User['baddress']; 
$_SESSION['Active'] = $User['bactive']; 
 $_SESSION['logged_in'] = true; 
$_SESSION['Category'] = 0; 
header("location: profile.php"); 
 } 
else 
 { 
 $_SESSION['message'] = "Invalid User Credentials!"; 
header("location: error.php"); 
 } 
 } 
} 
function dataFilter($data) 
{ 
 $data = trim($data); 
 $data = stripslashes($data); 
$data = htmlspecialchars($data); 
return $data; 
} 
?> 
LOGOUT CODE 
<?php 
session_start(); 
$_SESSION['logged_in'] = false; 
session_unset(); session_destroy();?> 
<!DOCTYPE html> 
<html> 
<head> 
 <title>AgroCulture: LogOut</title> 
 <meta charset="utf-8" /> 
 <meta http-equiv="X-UA-Compatible" content="IE=edge"> 
 <meta name="viewport" content="width=device-width, initial-scale=1" /> 
<link href="../bootstrap\css\bootstrap.min.css" rel="stylesheet"> 
<script 
src="https://ajax.googleapis.com/ajax/libs/jquery/1.12.4/jquery.min.js"></script> 
<script src="../bootstrap\js\bootstrap.min.js"></script> 
<meta name="description" content="" /> 
<meta name="keywords" content="" /> 
<!--[if lte IE 8]><script 
src="css/ie/html5shiv.js"></script><![endif]--> 
<script src="../js/jquery.min.js"></script> 
<script src="../js/skel.min.js"></script> 
<script src="../js/skel-layers.min.js"></script> 
<script src="../js/init.js"></script> 
<link rel="stylesheet" href="../css/skel.css" /> 
<link rel="stylesheet" href="../css/style.css" /> 
<link rel="stylesheet" href="../css/style-xlarge.css" /> 
 </head> 
<body> <?php 
require 'menu.php'; 
 ?> 
 <section id="banner"> 
<div class="container"> 
 <header class="major"> 
 <h2>Thanks for visiting !!!</h2> 
<center> 
 <p>You have been succesfully logged out !!!</p> 
 <div class="6u 12u$(xsmall)"> 
<br /> 
 <a href="../index.php" class="button special">HOME</a> 
 </div>
 </center> 
 </header> 
 </div> 
 </div> 
 </section> 
<script src="../assets/js/jquery.min.js"></script> 
 <script src="../assets/js/jquery.scrolly.min.js"></script> 
 <script src="../assets/js/jquery.scrollex.min.js"></script> 
 <script src="../assets/js/skel.min.js"></script> 
 <script src="../assets/js/util.js"></script> 
 <script src="../assets/js/main.js"></script> 
</body> 
</html> 
MENU CODE 
<?php 
if(isset($_SESSION['logged_in']) AND 
$_SESSION['logged_in'] == 1) 
{ 
$loginProfile = "My Profile: ". $_SESSION['Username']; 
$logo = "glyphicon glyphicon-user"; if($_SESSION['Category']!= 
1) 
{ 
$link = "profile.php"; 
} 
else { 
$link = "../profileView.php"; 
} } 
else 
{ 
$loginProfile = "Login"; 
$link = "../index.php"; 
$logo = "glyphicon glyphicon-log-in"; 
} 
?> 
<!DOCTYPE html> 
<header id="header"> 
<h1><a 
href="index.php">AgroCulture</a></h1> 
<nav id="nav"> 
<ul> 
<li><a href="../index.php"><span 
class="glyphicon glyphicon-home"></span> Home</a></li> 
<li><a 
href="../myCart.php"><span class="glyphicon glyphicon-shopping-cart"> 
MyCart</a></li> 
<li><a href="<?= $link; 
?>"><span class="<?php echo $logo; ?>"></span><?php echo" ". $loginProfile; 
?></a></li> 
<li><a href="../market.php"><span 
class="glyphicon glyphicon-grain"> Digital-Market</a></li> 
<li><a 
href="../blogView.php"><span class="glyphicon glyphicon-comment"> 
BLOG</a></li> 
</ul> 
</nav> 
</header> 
</body> 
</html> 
DATA BASE CONNECTION 
<?php 
 $serverName = "localhost"; 
 $userName = "root"; 
 $password = "1012"; 
 $dbName = "agroculture"; 
 $conn = mysqli_connect($serverName, $userName, $password, $dbName); 
if (!$conn) 
 { 
 die("Connection failed: " . mysqli_connect_error()); 
 } 
?> 
SQL 
-- phpMyAdmin SQL Dump 
-- version 4.6.4 
-- https://www.phpmyadmin.net 
-- Host: 127.0.0.1 
-- Generation Time: Feb 26, 2018 at 07:52 AM 
-- Server version: 5.7.14 
-- PHP Version: 5.6.25 
SET SQL_MODE = "NO_AUTO_VALUE_ON_ZERO"; 
SET time_zone = "+00:00"; 
-- Database: `agroculture` 
-- Table structure for table `blogdata` 
CREATE TABLE `blogdata` ( 
 `blogId` int(10) NOT NULL, 
 `blogUser` varchar(256) NOT NULL, 
 `blogTitle` varchar(256) NOT NULL, 
 `blogContent` longtext NOT NULL, 
 `blogTime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP, 
 `likes` int(10) NOT NULL DEFAULT '0' 
) ENGINE=InnoDB DEFAULT CHARSET=latin1; 
INSERT INTO `blogdata` (`blogId`, `blogUser`, `blogTitle`, 
`blogContent`, `blogTime`, `likes`) VALUES 
CREATE TABLE `blogfeedback` ( 
 `blogId` int(10) NOT NULL, 
 `comment` varchar(256) NOT NULL, 
 `commentUser` varchar(256) NOT NULL, 
 `commentPic` varchar(256) NOT NULL DEFAULT 'profile0.png', 
 `commentTime` timestamp NOT NULL DEFAULT CURRENT_TIMESTAMP 
) ENGINE=InnoDB DEFAULT CHARSET=latin1; 
INSERT INTO `blogfeedback` (`blogId`, `comment`, `commentUser`, `commentPic`, 
`commentTime`) VALUES 
(19, 'Mast yarr', 'ThePhenom', 'profile0.png', '2018-02-25 13:09:54');CREATE TABLE 
`buyer` ( 
 `bid` int(100) NOT NULL, 
 `bname` varchar(100) NOT NULL, 
 `busername` varchar(100) NOT NULL, 
 `bpassword` varchar(100) NOT NULL, 
 `bhash` varchar(100) NOT NULL, 
 `bemail` varchar(100) NOT NULL, 
 `bmobile` varchar(100) NOT NULL, 
 `baddress` text NOT NULL, 
 `bactive` int(100) NOT NULL DEFAULT '0' 
) ENGINE=InnoDB DEFAULT CHARSET=latin1; 
CREATE TABLE `farmer` ( 
 `fid` int(255) NOT NULL, 
 `fname` varchar(255) NOT NULL, 
 `fusername` varchar(255) NOT NULL, 
 `fpassword` varchar(255) NOT NULL, 
 `fhash` varchar(255) NOT NULL, 
 `femail` varchar(255) NOT NULL, 
 `fmobile` varchar(255) NOT NULL, 
 `faddress` text NOT NULL, 
 `factive` int(255) NOT NULL DEFAULT '0', 
 `frating` int(11) NOT NULL DEFAULT '0', 
 `picExt` varchar(255) NOT NULL DEFAULT 'png', 
 `picStatus` int(10) NOT NULL DEFAULT '0' 
) ENGINE=InnoDB DEFAULT CHARSET=latin1; 
INSERT INTO `farmer` (`fid`, `fname`, `fusername`, `fpassword`, `fhash`, `femail`, 
`fmobile`, `faddress`, `factive`, `frating`, `picExt`, `picStatus`) VALUES 
(3, 'Kaivalya Hemant Mendki', 'ThePhenom', 
'$2y$10$22ezmzHRa9c5ycHmVm5RpOnlT4LwFaDZar1XhmLRJQKGrcVRhPgti', 
'61b4a64be663682e8cb037d9719ad8cd', 'kmendki98@gmail.com', '8600611198', 
'abcde', 0, 0, 'png', 0); 
CREATE TABLE `fproduct` ( 
 `fid` int(255) NOT NULL, 
 `pid` int(255) NOT NULL, 
 `product` varchar(255) NOT NULL, 
 `pcat` varchar(255) NOT NULL, 
 `pinfo` varchar(255) NOT NULL, 
 `price` float NOT NULL, 
 `pimage` varchar(255) NOT NULL DEFAULT 'blank.png', 
 `picStatus` int(10) NOT NULL DEFAULT '0' 
) ENGINE=InnoDB DEFAULT CHARSET=latin1; 
INSERT INTO `fproduct` (`fid`, `pid`, `product`, `pcat`, `pinfo`, `price`, `pimage`, 
`picStatus`) VALUES 
(3, 27, 'Mango', 'Fruit', '<p>Mango raseela</p>\r\n', 500, 'Mango3.jpeg', 1), 
(3, 28, 'Ladyfinger', 'Vegetable', '<p>Its veggie</p>\r\n', 1000, 'Ladyfinger3.jpg', 1), 
(3, 29, 'Bajra', 'Grains', '<p>bajre di rti</p>\r\n', 400, 'Bajra3.jpg', 1), 
(3, 30, 'Banana', 'Fruit', '<p>Jalgaon banana</p>\r\n', 400, 'Banana3.jpg', 1); 
CREATE TABLE `likedata` ( 
 `blogId` int(10) NOT NULL, 
 `blogUserId` int(10) NOT NULL 
) ENGINE=InnoDB DEFAULT CHARSET=latin1; 
INSERT INTO `likedata` (`blogId`, `blogUserId`) VALUES 
(19, 3); 
CREATE TABLE `mycart` ( 
 `bid` int(10) NOT NULL, 
 `pid` int(10) NOT NULL 
) ENGINE=InnoDB DEFAULT CHARSET=latin1; 
INSERT INTO `mycart` (`bid`, `pid`) VALUES 
(3, 27), 
(3, 30); 
CREATE TABLE `review` ( 
 `pid` int(10) NOT NULL, 
 `name` varchar(255) NOT NULL, 
 `rating` int(10) NOT NULL, 
 `comment` text NOT NULL 
) ENGINE=InnoDB DEFAULT CHARSET=latin1; 
CREATE TABLE `transaction` ( 
 `tid` int(10) NOT NULL, 
 `bid` int(10) NOT NULL, 
 `pid` int(10) NOT NULL, 
 `name` varchar(255) NOT NULL, 
 `city` varchar(255) NOT NULL, 
 `mobile` varchar(255) NOT NULL, 
 `email` varchar(255) NOT NULL, 
 `pincode` varchar(255) NOT NULL, 
 `addr` varchar(255) NOT NULL 
) ENGINE=InnoDB DEFAULT CHARSET=latin1; 
INSERT INTO `transaction` (`tid`, `bid`, `pid`, `name`, `city`, `mobile`, `email`, 
`pincode`, `addr`) VALUES 
(1, 3, 28, 'sa,j,cns', 'sajc', 'sajch', 'kmendki98@gmail.com', 'sacu', 'ckaskjc'); 
ALTER TABLE `blogdata` 
 ADD PRIMARY KEY (`blogId`); 
ALTER TABLE `buyer` 
 ADD PRIMARY KEY (`bid`), 
 ADD UNIQUE KEY `bid` (`bid`); 
ALTER TABLE `farmer` 
 ADD PRIMARY KEY (`fid`), 
 ADD UNIQUE KEY `fid` (`fid`); 
ALTER TABLE `fproduct` 
 ADD PRIMARY KEY (`pid`); 
ALTER TABLE `likedata` 
 ADD KEY `blogId` (`blogId`), 
 ADD KEY `blogUserId` (`blogUserId`); 
ALTER TABLE `transaction` 
 ADD PRIMARY KEY (`tid`); 
ALTER TABLE `blogdata` 
 MODIFY `blogId` int(10) NOT NULL AUTO_INCREMENT, 
AUTO_INCREMENT=20; 
ALTER TABLE `buyer` 
 MODIFY `bid` int(100) NOT NULL AUTO_INCREMENT;` 
ALTER TABLE `farmer` 
 MODIFY `fid` int(255) NOT NULL AUTO_INCREMENT, 
AUTO_INCREMENT=4; 
ALTER TABLE `fproduct` 
 MODIFY `pid` int(255) NOT NULL AUTO_INCREMENT, 
AUTO_INCREMENT=31; 
ALTER TABLE `transaction` 
ALTER TABLE `buyer` 
 ADD CONSTRAINT `buyer_ibfk_1` FOREIGN KEY (`bid`) REFERENCES 
`farmer` (`fid`) 
ALTER TABLE `likedata` 
 ADD CONSTRAINT `likedata_ibfk_1` FOREIGN KEY (`blogId`) REFERENCES 
`blogdata` (`blogId`);

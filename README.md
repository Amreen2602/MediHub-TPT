# MediHub-TPT1
//login.php
<?php
    $email = $_POST['email'];
    $password = $_POST['pswd'];
    //database connection
   $conn = new mysqli('localhost','root','','project');
   if (!$conn)
   { die("Connection failed: " . mysqli_connect_error());

   }
   else{
    $stmt = $conn->prepare("Select * from registration where email = ?");
    $stmt->bind_param("s",$email);
    $stmt->execute();
    $stmt_result = $stmt->get_result();
    if($stmt_result->num_rows > 0)
    {
        $data = $stmt_result->fetch_assoc();
        if($data['Password']==$password)
        {
            //echo "<h2> Login Successfull</h2>";
            echo '<script>window.location.href = "http://127.0.0.1:5500/homepage1.html";</script>';
        exit();

        }
        else{
            echo "<h2> Invalid Email or Password</h2>";
        }
    }
    else{
        echo "<h2> Invalid Email or Password</h2>";
    }
   }
   
?>

//connect1.php
<?php
    $name = $_POST['txt'];
    $number = $_POST['phn'];
    $email = $_POST['email'];
    $password = $_POST['pswd'];
   //database connection
   $conn = new mysqli('localhost','root','','project');
   if (!$conn)
   { die("Connection failed: ". mysqli_connect_error());

   }
   else{
    $stmt = $conn->prepare("insert into registration(Name,Phone,Email,Password)
    values(?,?,?,?)");
    $stmt->bind_param("siss",$name,$number,$email,$password);
    $stmt->execute();
    if ($stmt->affected_rows > 0) {
        $message = "Registration successful!";
    } else {
        $message = "Registration failed. Please try again.";
    }
    $stmt->close();
    $conn->close();
    header("Location: loginpage1.php?message=" . urlencode($message));
    exit;
   }
   
?>

//loginpage1.php
<!DOCTYPE html>
<html>

<head>
    <title>Hospital Login Page</title>
    <link rel="stylesheet" type="text/css" href="loginstyles1.css">
    <link rel="icon" href="https://us.123rf.com/450wm/mantinov/mantinov2004/mantinov200400007/143789285-help-for-health-icon-logo-vector-graphic-design-helping-hands-inside-medical-cross-sign.jpg?ver=6" type="image/x-icon">
</head>
<body>
    <div class="container" id="main">
        <div class="Register">
            <form action="connect1.php" method="post">
                <h1>Create Account</h1>
                <input type="text" name="txt" placeholder="Name" required="">
                <input type="tel" name="phn" placeholder="Phone" required="">
                <input type="email" name="email" placeholder="Email" required="">
                <input type="password" name="pswd" placeholder="Password" required="">
                <button>Sign Up</button>
            </form>       
        </div>
        <div class="registration-message">
            <?php
            if (isset($_GET['message'])) {
                echo $_GET['message'];
            }
            ?>
        </div>
        <?php
    if (isset($error_msg)) {
        echo "<h2>$error_msg</h2>";
    }
    ?>
        <div class="log-in">
            <form action="login1.php" method="post">
                <h1>LOGIN</h1>
                <input type="email" name="email" placeholder="ID" required="">
                <input type="password" name="pswd" placeholder="Password" required="">
                <a href="./frogpass.html">Forgot Password?</a>
                <button href="http://127.0.0.1:5500/homepage1.html">Login</button>
                <!-- <a href="homepage1.html"></a> -->
            </form>
        </div>

        <div class="overlay-container">
            <div class="overlay">
                <div class="overlay-left">
                    <h1>Welcome Back</h1>
                    <img class="img1" src="https://media.istockphoto.com/id/1139549801/vector/stethoscope-heart-icon.jpg?s=612x612&w=0&k=20&c=qEJ7fFxWkok8j7FYYj4NwAlHSgqsw-azZz7c3IQJ4KI=" width="150px" height="120px" style="border: none;">
                    <p>Kindly Login to Experince Our Service</p>
                    <button id="signIn">LOGIN</button>
                </div>
                <div class="overlay-right">
                    <h1>Medi Hub-TPT</h1>
                    <img class="imgs" src="https://us.123rf.com/450wm/mantinov/mantinov2004/mantinov200400007/143789285-help-for-health-icon-logo-vector-graphic-design-helping-hands-inside-medical-cross-sign.jpg?ver=6">
                    <p>Start a Healthy Journey With Us </p>
                    <button id="signUp">Sign Up</button>
                </div>
            </div>
        </div>
    </div>
    <script type="text/javascript">
        const signUpButton = document.getElementById('signUp');
        const signInButton = document.getElementById('signIn');
        const main = document.getElementById('main');

        signUpButton.addEventListener('click', () => {
            main.classList.add("right-panel-active");
        });
        signInButton.addEventListener('click', () => {
            main.classList.remove("right-panel-active");
        });
    </script>
</body>
</html> 

//logout1.php
<?php
session_start();

// Clear the session variables
session_unset();
session_destroy();

// Redirect to the login page
header("Location: loginpage1.php");
exit();
?>
//appointmentpage.php
<?php
    $name = $_POST['name'];
    $spname = $_POST['spname'];
    $Fname = $_POST['Fname'];
    $phone= $_POST['phone'];
    $email = $_POST['email'];
    $date= $_POST['date'];
   //database connection
   $conn = new mysqli('localhost','root','','project');
   if (!$conn)
   { die("Connection failed: ". mysqli_connect_error());

   }
   else{
    $stmt = $conn->prepare("insert into appointment(Doctor,Specialist,Name,Phone,Email,Date)
    values(?,?,?,?,?,?)");
    $stmt->bind_param("sssiss",$name,$spname,$Fname,$phone,$email,$date);
    $stmt->execute();
    $stmt->close();
    $conn->close();
    echo '<!DOCTYPE html>
    <html lang="en">
    <head>
        <meta charset="UTF-8">
        <meta http-equiv="X-UA-Compatible" content="IE=edge">
        <meta name="viewport" content="width=device-width, initial-scale=1.0">
        <link rel="icon" href="https://us.123rf.com/450wm/mantinov/mantinov2004/mantinov200400007/143789285-help-for-health-icon-logo-vector-graphic-design-helping-hands-inside-medical-cross-sign.jpg?ver=6" type="image/x-icon">
        <title>Appointment Receipt</title>
        <link rel="stylesheet" type="text/css" href="appointmentpage1.css">
        <style>
            #receipt {
                border: 1px solid #ccc;
                padding: 20px;
                margin-top: 20px;
                background-color: #f9f9f9;
            }

            #receipt h2 {
                font-size: 24px;
                margin-bottom: 10px;
            }

            #receipt p {
                margin: 0;
            }

            #receipt strong {
                font-weight: bold;
            }

            #receipt .success-message {
                font-size: 18px;
                color: green;
                margin-bottom: 20px;
            }

            #receipt .contact-info {
                font-size: 14px;
                color: #777;
                margin-top: 20px;
            }
        </style>
    </head>
    <body>
        <div id="receipt">
        <h1>appointment Successful</h1>
            <h2>Appointment Receipt</h2>
            <p><strong>Doctor:</strong> ' . $name . '</p>
            <p><strong>Specialist:</strong> ' . $spname . '</p>
            <p><strong>Name:</strong> ' . $Fname . '</p>
            <p><strong>Phone:</strong> ' . $phone . '</p>
            <p><strong>Email:</strong> ' . $email . '</p>
            <p><strong>Date:</strong> ' . $date . '</p>
            <div class="success-message">Congratulations! Your appointment has been booked.</div>
            <p><strong>If you have any queries, please contact: 987653436</strong></p>
        </div>
    </body>
    </html>';
   }
   
?>

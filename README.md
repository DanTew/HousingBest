<?php

//did files get sent 
if(isset($_FILES) && (bool) $_FILES) {

    $allowedExtensions = array ("gif", "jpeg", "jpg"," png");

    Foreach($_FILES as $name=>$file) {


    $FILE_name = $file['name'];
    $temp_name = $file['tmp_name'];

    $path_parts = pathinfo($file_name);
    $ext = $path_parts['extension'];

    if (!in_array($ext,$allowedExtensions)) {

    die("extension not allowed");

    }


    $server_file = "public_html/housingbestco/testing/$path_parts[basename]";
    move_upliaded_file($temp_name,$server_file);

    array_push($files,$server_file);
    }

    $to = "dantewo@hotmail.com";
    //$to = "info@housingbest.co.uk";
    $subject = $_POST["name"];

    $name = $_POST["name"];
    $address = $_POST["address"];
    $message = $_POST["message"];
    $tel = $_POST["usrtel"];

    $email = $_POST["email"];
    $postcode = $_POST["postcode"];
    $date = $_POST["date"];
    $HL = $_POST["HL"];
    $headers = "From: $email";
    $send = " TOPIC: REPAIR

    Name:	$name
    Email:	$email
    Tel:    $tel
    Address:  $address
    Post Code: $postcode

    Date: $date 
    How long this has been happening $HL



    Message:

    $message
    $image ";



    $semi_rand = md5(time());
    $mime_boundary = " ==Multipart_Boundary_x{$semi_rand}x ";


    $headers .= "\nMIME-Version:1.0\n";
    $headers .= "Content-Type: multipart/mixed;\n";
    $headers .= "boundary=\"{$mime_boundary}\"";

    $messgae ="\n\n--{$mime_boundary} \n";
    $messgae .= "Content-Type: text/plain; charset=\"iso-8859-1\"\n";
    $messgae .= "Content-Transfer-Encoding: 7bit\n\n" . $msg . "\n\n";
    $messgae .= "--{$mime_boundary} \n";


    foreach( $files as $file) { 
        $aFile = fopen($file,"rb");
        $data = fread($aFile,filesize($file));
        fclose($aFile);
        $data = chuck_split(base64_encode($data));
     $messgae .= "Content-Type: text/plain; charset=\"iso-8859-1\"\n";
     $messgae .= " name=\"file\"\n";
    $messgae .= "Content-Disposition: attachment:\n";
        $messgae .= "filename=\"$file\"\n";
        $messgae .= "Content-Transfer-Encoding: base64\n\n" . data . "\n\n";
        $message .= "--{$mime_boundary}\n";




    }

                       $ok = mail($to,$send,$subject, $message, $headers);

}
                       ?>


 print $send  ;


<?php

?>

#### Build a user interface to convert voice to text (speech to text) :
```
<!DOCTYPE html>
<html>
<head>
  <title>Speech to Text📋</title>
  <style>
    body {
      font-family: Arial, sans-serif;
      text-align: center;
      padding: 20px;
      color: black;
    }
    #robot__control {
      width: 80%;
      height: 200px;
      font-size: 16px;
      padding: 10px;
      display: block;
      margin: 20px auto;
      background-color: #f2f2f2;
      border: 1px solid #ddd;
      border-radius: 5px;
    }
    button {
      font-size: 16px;
      padding: 10px 20px;
      background-color: green;
      border-radius: 5px;
      cursor: pointer;
    }
  </style>
</head>
<body>
  <h1>📋Speech to Text📋</h1>
  <textarea id="robot__control" placeholder="Converted text will appear here..."></textarea>
  <button onclick="convertSpeechToText()">Convert Speech to Text</button>

  <script>
    function convertSpeechToText() {
      // Check if the browser supports the Web Speech API
      if ('webkitSpeechRecognition' in window) {
        var recognition = new webkitSpeechRecognition();
        recognition.lang = 'ar-SA'; // Set the language to Arabic
        recognition.onresult = function(event) {
          var text = event.results[0][0].transcript;
          document.getElementById('robot__control').value = text;
          
          // Save the text to a database
          saveTextToDatabase(text);
        };
        recognition.start();
      } else {
        alert('Speech recognition is not supported in this browser.');
      }
    }

    function saveTextToDatabase(text) {
      var xhr = new XMLHttpRequest();
      xhr.open("POST", "save.php", true);
      xhr.setRequestHeader("Content-Type", "application/x-www-form-urlencoded");
      xhr.onreadystatechange = function () {
        if (xhr.readyState === 4 && xhr.status === 200) {
          alert("Text saved to database.");
        }
      };
      xhr.send("text=" + encodeURIComponent(text));
    }
  </script>
</body>
</html>
```
![لقطة شاشة 2024-07-14 220008](https://github.com/user-attachments/assets/560715b7-8082-4279-82e2-b818c67c0f03)

![لقطة شاشة 2024-07-14 215949](https://github.com/user-attachments/assets/d4b773d5-8812-4a6e-b599-83f807f5cfc3)

#### Save the output to database :
```
<?php
$servername = "localhost";
$username = "root";
$password = ""; // Add your database password if you have one
$dbname = "robot__control";

// Create connection
$conn = new mysqli($servername, $username, $password, $dbname);

// Check connection
if ($conn->connect_error) {
    die("Connection failed: " . $conn->connect_error);
}

if ($_SERVER["REQUEST_METHOD"] == "POST") {
    $text = $_POST['text'];
    $sql = "INSERT INTO saund_movements (text) VALUES ('$text')";
    
    if ($conn->query($sql) === TRUE) {
        echo "Text saved successfully👏🏻.";
    } else {
        echo "Error: " . $sql . "<br>" . $conn->error;
    }
    } else {
    echo "No 'direction' value submitted in the POST request.";
    }
  
  $conn->close();
  
?>
```
![لقطة شاشة 2024-07-14 220033](https://github.com/user-attachments/assets/81d0d405-3016-4e78-a14d-2bf55ac0c670)


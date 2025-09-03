Francis Samuel Enya
23/csc/119
csc 282
student registration system
first create html 
action="process.php" method="POST">
  <input type="text" name="fullname" placeholder="Full Name" required>
  <input type="email" name="email" placeholder="Email" required>
  <input type="text" name="department" placeholder="Department" required>
  <input type="text" name="matric" placeholder="Matric Number" required>
  <button type="submit">Register</button>
</form>
<?php
$conn = new mysqli("localhost", "root", "", "your_database");

if ($_SERVER["REQUEST_METHOD"] == "POST") {
  $fullname = $_POST["fullname"];
  $email = $_POST["email"];
  $department = $_POST["department"];
  $matric = $_POST["matric"];

  if (!filter_var($email, FILTER_VALIDATE_EMAIL)) {
    die("Invalid email format");
  }

  $stmt = $conn->prepare("INSERT INTO student_records (fullname, email, department, matric) VALUES (?, ?, ?, ?)");
  $stmt->bind_param("ssss", $fullname, $email, $department, $matric);
  $stmt->execute();

  echo "Student registered successfully!";
}
?>
<?php
$conn = new mysqli("localhost", "root", "", "your_database");
$result = $conn->query("SELECT * FROM student_records");

echo "<table border='1'>
<tr><th>Name</th><th>Email</th><th>Department</th><th>Matric</th><th>Action</th></tr>";

while ($row = $result->fetch_assoc()) {
  echo "<tr>
    <td>{$row['fullname']}</td>
    <td>{$row['email']}</td>
    <td>{$row['department']}</td>
    <td>{$row['matric']}</td>
    <td><a href='delete.php?id={$row['id']}'>Delete</a></td>
  </tr>";
}
echo "</table>";
?>
<?php
$conn = new mysqli("localhost", "root", "", "your_database");
$id = $_GET['id'];
$conn->query("DELETE FROM student_records WHERE id=$id");
header("Location: view.php");
?>

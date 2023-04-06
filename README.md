<?php
include('db.php');
session_start();

class Account{
	private $db;
	public function __construct($db){
		$this->db = $db;
	}

	public function login($username,$password){
		$sql = "SELECT username,password FROM users WHERE username='$username'AND password='$password'";
		$result = mysqli_query($this->db,$sql);
		if(mysqli_num_rows($result)>0){
			$_SESSION['username'] = $_POST['username'];
			header("location:result.php");
		}
	}
}

 <?php
include('db.php');
class Tag{
	private $db;
	public function __construct($db ){
		$this->db = $db;
	}

	public function getAllTags(){
		$sql = "SELECT * FROM tags";
		$result = mysqli_query($this->db,$sql);
		return $result;
	}

	public function getTags($slug){
		$data = [];
		//$sql = "SELECT * FROM tags";
		$sql = "SELECT  *
				FROM   posts
   				INNER JOIN post_tags
      			ON post_tags.post_id = posts.id
   				INNER JOIN tags
    			ON tags.id = post_tags.tag_id
   				WHERE tags.tag='$slug'";
   		$result = mysqli_query($this->db,$sql);
   		//return $result;
		//$result = mysqli_query($this->db,$sql);
		foreach($result as $res){
			array_push($data, $res['tag']);
		}
		return $data;
	}


}

 <?php
  include('session.php');
 ?>
<?php
    include('header.php');
?>
<?php
    include('post.php');
?>
<?php
    include('image_upload_function.php');
?>
<?php
  include('Tag.php');
 ?>
 <?php
     $tags = new Tag($db);
     $post = new Post($db);
     ?>
<?php
    if(isset($_POST['btnSubmit'])){

       	$data = date('Y-m-d');

    	if(!empty($_POST['title'])&&!empty($_POST['description'])){
    		$title = strip_tags($_POST['title']);
    		$description = $_POST['description'];
        $slug = createSlug($title);
    		$record = $post->addPost($title,$description,uploadImage(),$data,$slug);
    		if($record==True){
    			echo"<div class='text-center alert alert-success'>Post added Successfully!</div>";
    		}
    	}else{
    		echo"<div class='text-center alert alert-danger'>Every field is required</div>";
    	}
    }
?>

<div class="container">
	<div class="row justify-content-center">
		<div class="col-md-8">
			<form action="add.php" method="POST" enctype="multipart/form-data">
			<div class="card">
				<div class="card-header">Add post</div>
				<div class="card-body">
					<div class="form-group">
						<label for="title">Title</label>
						<input type="text" name="title" class="form-control">
					</div>

					<div class="form-group">
						<label for="description">Description</label>
						<textarea cols="10" id="editor" name="description" class="form-control"></textarea>
					</div>

					<div class="form-group">
						<label for="image">Image</label>
						<input type="file" name="image" class="form-control">
					</div>
          <div class="form-group form-check-inline">
            <label for="image"><b>Choose tags</b>&nbsp;&nbsp;</label>
            <?php  foreach($tags->getAllTags() as $tag){ ?>
              <input type="checkbox" name="tags[]" class="form-check-input" value="<?php echo $tag['id'] ?>"><?php echo $tag['tag']; ?>
            <?php } ?>
          </div>

					<div class="form-group">
						<button type="submit" name="btnSubmit" class="btn btn-primary">Submit</button>
					</div>
				</div>
			</div>
		</form>

		</div>
	</div>

</div>

<script>
    ClassicEditor
        .create( document.querySelector( '#editor' ) )
        .catch( error => {
            console.error( error );
        } );
</script>
</script>

-- phpMyAdmin SQL Dump
-- version 5.0.2
-- https://www.phpmyadmin.net/
--
-- Host: localhost
-- Generation Time: Jun 05, 2020 at 11:56 AM
-- Server version: 10.4.11-MariaDB
-- PHP Version: 7.4.5

SET SQL_MODE = "NO_AUTO_VALUE_ON_ZERO";
START TRANSACTION;
SET time_zone = "+00:00";


/*!40101 SET @OLD_CHARACTER_SET_CLIENT=@@CHARACTER_SET_CLIENT */;
/*!40101 SET @OLD_CHARACTER_SET_RESULTS=@@CHARACTER_SET_RESULTS */;
/*!40101 SET @OLD_COLLATION_CONNECTION=@@COLLATION_CONNECTION */;
/*!40101 SET NAMES utf8mb4 */;

--
-- Database: `blog`
--

-- --------------------------------------------------------

--
-- Table structure for table `posts`
--

CREATE TABLE `posts` (
  `id` int(11) NOT NULL,
  `title` varchar(200) NOT NULL,
  `description` text NOT NULL,
  `image` varchar(80) NOT NULL,
  `created_at` datetime NOT NULL,
  `slug` varchar(200) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

--
-- Dumping data for table `posts`
--

INSERT INTO `posts` (`id`, `title`, `description`, `image`, `created_at`, `slug`) VALUES
(13, 'Node Js a Awesome Language', '<p>Node.js is an open source, cross-platform runtime environment for developing server-side and networking applications. Node.js applications are written in JavaScript, and can be run within the Node.js runtime on OS X, Microsoft Windows, and Linux.</p>', 'node.png', '2020-06-05 00:00:00', 'Node-Js-a-Awesome-Language'),
(14, 'React is a Frontend Library', '<p>React is a JavaScript library created for building fast and interactive user interfaces for web and mobile applications. It is an open-source, component-based, front-end library responsible only for the application’s view layer. In Model View Controller (MVC) architecture, the view layer is responsible for how the app looks and feels. React was created by Jordan Walke, &nbsp;at Facebook.&nbsp;</p><p>Let’s take a look at an Instagram webpage example, entirely built using React, to get a better understanding of how React works. As the illustration shows, React divides the UI into multiple components, which makes the code easier to debug. This way, each component has its property and function.</p><p><strong>Easy creation of dynamic applications:</strong> React makes it easier to create dynamic web applications because it requires less coding and offers more functionality, as opposed to JavaScript, where coding often gets complex very quickly.</p>', 'react.png', '2020-06-05 00:00:00', 'React'),
(15, 'Android a great thing to learn', '<p>Android is a powerful operating system and it supports a large number of applications in Smartphones. These applications are more comfortable and advanced for users. The hardware that supports android software is based on the ARM architecture platform. The android is an open-source operating system that means that it’s free and anyone can use it. The android has got millions of apps available that can help you manage your life one or another way and it is available to low cost in the market for that reason android is very popular.</p>', 'man.png', '2020-06-05 00:00:00', 'Android-a-great-thing-to-learn'),
(16, 'Java is a Programming Language', '<p>Today the Java platform is a commonly used foundation for developing and delivering content on the web. According to Oracle, there are more than 9 million Java developers worldwide and more than 3 billion mobile phones run Java.</p><p>In 2014 one of the most significant changes to the Java language was launched with Java SE 8. Changes included additional functional programming features, parallel processing using streams and improved integration with JavaScript. The 20th anniversary of commercial Java was celebrated in 2015.</p>', 'service_img2.png', '2020-06-05 00:00:00', 'Java-is-a-Programming-Language');

-- --------------------------------------------------------

--
-- Table structure for table `post_tags`
--

CREATE TABLE `post_tags` (
  `tag_id` int(11) NOT NULL,
  `post_id` int(11) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

--
-- Dumping data for table `post_tags`
--

INSERT INTO `post_tags` (`tag_id`, `post_id`) VALUES
(1, 7),
(2, 7),
(1, 8),
(2, 8),
(4, 9),
(2, 10),
(2, 13),
(1, 14),
(4, 15),
(5, 15),
(5, 16);

-- --------------------------------------------------------

--
-- Table structure for table `tags`
--

CREATE TABLE `tags` (
  `id` int(11) NOT NULL,
  `tag` varchar(60) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

--
-- Dumping data for table `tags`
--

INSERT INTO `tags` (`id`, `tag`) VALUES
(1, 'react'),
(2, 'node.js'),
(3, 'android'),
(4, 'html'),
(5, 'java');

-- --------------------------------------------------------

--
-- Table structure for table `users`
--

CREATE TABLE `users` (
  `id` int(11) NOT NULL,
  `name` varchar(60) NOT NULL,
  `username` varchar(60) NOT NULL,
  `password` varchar(60) NOT NULL
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4;

--
-- Dumping data for table `users`
--

INSERT INTO `users` (`id`, `name`, `username`, `password`) VALUES
(2, 'Raghav', 'hello', '5d41402abc4b2a76b9719d911017c592');

--
-- Indexes for dumped tables
--

--
-- Indexes for table `posts`
--
ALTER TABLE `posts`
  ADD PRIMARY KEY (`id`);

--
-- Indexes for table `tags`
--
ALTER TABLE `tags`
  ADD PRIMARY KEY (`id`);

--
-- Indexes for table `users`
--
ALTER TABLE `users`
  ADD PRIMARY KEY (`id`);

--
-- AUTO_INCREMENT for dumped tables
--

--
-- AUTO_INCREMENT for table `posts`
--
ALTER TABLE `posts`
  MODIFY `id` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=17;

--
-- AUTO_INCREMENT for table `tags`
--
ALTER TABLE `tags`
  MODIFY `id` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=6;

--
-- AUTO_INCREMENT for table `users`
--
ALTER TABLE `users`
  MODIFY `id` int(11) NOT NULL AUTO_INCREMENT, AUTO_INCREMENT=3;
COMMIT;

/*!40101 SET CHARACTER_SET_CLIENT=@OLD_CHARACTER_SET_CLIENT */;
/*!40101 SET CHARACTER_SET_RESULTS=@OLD_CHARACTER_SET_RESULTS */;
/*!40101 SET COLLATION_CONNECTION=@OLD_COLLATION_CONNECTION */;

 <?php
    $db = mysqli_connect("localhost","root","","blog");
    if(mysqli_connect_error()){
        echo "failed to connect".mysqli_connect_error();
    }
?>

<?php
  // include('session.php');
 ?>
<?php include('header.php'); ?>
<?php include('post.php');
	$post = new Post($db);
?>
<?php
$post->deletePostBySlug($_GET['slug']);
header('Location:result.php');
?>

 <?php
  // include('session.php');
 ?>
<?php
	include('header.php');
?>
<?php
include('session.php');
?>
<?php
include('image_upload_function.php');
?>
<?php
include('post.php');
$posts = new Post($db);
?>
<?php
if(isset($_POST['btnUpdate'])){
	$result = $posts->updatePost($_POST['title'],$_POST['description'],$_GET['slug']);
	if($result==true){
		echo"<div class='text-center alert alert-success'>Post updated Successfully!</div>";
	}
}
?>

<div class="container">
	<div class="row justify-content-center">
		<?php foreach($posts->getSinglePost($_GET['slug'])as $post){ ?>
		<div class="col-md-8">
			<form action="#" method="POST" enctype="multipart/form-data">
			<div class="card">
				<div class="card-header">Edit post</div>
				<div class="card-body">
					<div class="form-group">
						<label for="title">Title</label>
						<input type="text" name="title" class="form-control" value="<?php echo $post['title']; ?>">
					</div>

					<div class="form-group">
						<label for="description">Description</label>
						<textarea cols="10" id="editor" name="description" class="form-control"><?php echo $post['description'] ;?></textarea>
					</div>

					<div class="form-group">
						<label for="image">Image</label>
						<input type="file" name="image" class="form-control">
						<img style="width: 180px;" src="images/<?php echo $post['image'] ?>">
					</div>



					<div class="form-group">
						<button type="submit" name="btnUpdate" class="btn btn-primary">Update </button>
					</div>


				</div>
			</div>
		</form>

		</div>
	<?php }?>
	</div>

</div>
<script>
    ClassicEditor
        .create( document.querySelector( '#editor' ) )
        .catch( error => {
            console.error( error );
        } );
</script>
<style type="text/css">
	.card{
		margin-top: 10px;
	}
</style>

<!DOCTYPE html>
<html>
<head>
	<title>PHP Assignment 1</title>
	<link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.2.1/css/bootstrap.min.css" integrity="sha384-GJzZqFGwb1QTTN6wy59ffF1BuGJpLSa9DkKMp0DgiMDm4iYMj70gZWKYbI706tWS" crossorigin="anonymous">

	<script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
<script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.6/umd/popper.min.js" integrity="sha384-wHAiFfRlMFy6i5SRaxvfOCifBUQy1xHdJ/yoi7FRNXMRBu5WHdZYu1hA6ZOblgut" crossorigin="anonymous"></script>
<script src="https://stackpath.bootstrapcdn.com/bootstrap/4.2.1/js/bootstrap.min.js" integrity="sha384-B0UglyR+jN6CkvvICOB2joaf5I4l3gm9GU6Hc1og6Ls7i6U/mkkaduKaBhlAXv9k" crossorigin="anonymous"></script>


<script src="https://cdn.ckeditor.com/ckeditor5/11.2.0/classic/ckeditor.js"></script>


</head>
<body>
<nav class="navbar navbar-expand-lg navbar-light bg-light">
  <a class="navbar-brand" href="index.php">Php Assignment 1</a>
  <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>
  <div class="collapse navbar-collapse" id="navbarNav">
    <ul class="navbar-nav ml-auto">
      <li class="nav-item active">
        <a class="nav-link" href="index.php">Home <span class="sr-only">(current)</span></a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="add.php">Add Post</a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="result.php">Dashboard</a>
      </li>
    <?php if(!empty($_SESSION['username'])){ ?>
      <li class="nav-item">
        <a class="nav-link" href="logout.php">Logout</a>
      </li>
    <?php }else{  ?>
      <li class="nav-item">
        <a class="nav-link" href="login.php">Login</a>
      </li>
<?php  } ?>


    </ul>
  </div>
</nav>
</body>
</html>

 <?php
function uploadImage(){
	$imagename= $_FILES['image']['name'];
	$imagetmp = $_FILES['image']['tmp_name'];
	
	$allowed = array('jpeg','png','jpg');

	$ext = pathinfo($imagename,PATHINFO_EXTENSION);

	if(in_array($ext,$allowed)){
		move_uploaded_file($imagetmp, "images/".$imagename);
	}else{
		echo"only png,jpg and jpeg image format are allowed";
	}
	return $imagename;
}

function createSlug($string){
	$slug = preg_replace('/[^A-Za-z0-9]+/','-',$string);
	return $slug;
	
}

?>

<?php
include('header.php');
include('post.php');
include('Tag.php')
?>
<?php
$post = new Post($db);
$tags = new Tag($db);
?>

<div class="container">
    <div class="row">
        <div class="col-md-8">
        <?php foreach($post->getPost() as $post) { ?>
            <div class="media">
                <div class="media-left media-top">
                    <img src="images/<?php echo $post['image']; ?>" class="media-object" style="width:200px; margin-right: 10px;"/>
                    <p>
                      Author:Admin<br>
          						Created:<?php echo date('Y-m-d',strtotime($post['created_at'])); ?>
                    </p>
                </div>
                <div class="media-body">
                  <h4 class="media-heading"><a href="view.php?slug=<?php echo $post['slug'];?>"><?php echo $post['title']; ?></a></h4>
                  <?php echo htmlspecialchars_decode(substr($post['description'],0,100));?>                 </div>
            </div>

            <?php } ?>

            <div class="col-md-4">
            	<h4>Browse by Tags</h4>
            	<p>
            	<?php
            	foreach($tags->getAllTags() as $tag){?>
            		<a href="index.php?tag=<?php  echo $tag['tag'];?>"><button type="button" class="btn btn-outline-warning btn-sm">
            			<?php  echo $tag['tag'];?>
            		</button></a>

            	<?php  } ?>

            	</p>

        </div>
    </div>
</div>
</div>
</div>

<style type="text/css">
    body{
        text-align: justify;
    }
    img{
        margin-right: 10px;
    }
    .media{
        margin-top: 20px;
    }
</style>

<?php include('header.php'); ?>
<?php include('Account.php');  ?>

<?php
 $user = new Account($db);
?>
<?php
if(isset($_POST['btnLogin'])){
	$user->login($_POST['username'],md5($_POST['password']));


}

?>


<form action="login.php" method="POST">
	<div class="container">
		<h4>Admin Login</h4>
		<div class="col-md-4">
			<div class="form-group">
				<label for="username">Username</label>
				<input type="text" name="username" class="form-control" placeholder="username...">
			</div>
			<div class="form-group">
				<label for="password">password</label>
				<input type="password" name="password" class="form-control" placeholder="password...">
			</div>
			<div class="form-group">
				<button type="submit" name="btnLogin" class="btn btn-success">Login</button>
			</div>

		</div>
	</div>
</form>

 <?php
session_start();
session_destroy();
header("Location:login.php");

 <?php
include('db.php');
    class Post
    {
        private $db;
        public function __construct($db){
            $this->db = $db;
        }
        public function addPost($title,$description,$image,$date,$slug){


      		$sql = "INSERT INTO posts(title,description,image,created_at,slug)VALUES('$title','$description','$image','$date','$slug')";
      		$result = mysqli_query($this->db,$sql);

      		if($result){
      			if($_POST['tags']){
      				$tags = $_POST['tags'];
      				$lastInsertedId = mysqli_insert_id($this->db);
      				foreach($tags as $tag){
      					$sql="INSERT INTO post_tags(post_id,tag_id)VALUES('$lastInsertedId',$tag)";
      					$result = mysqli_query($this->db,$sql);
      				}
      			}
      		}
      		return $result;
      	}

        public function getPost(){
          if(isset($_GET['tag'])){
            $tag = $_GET['tag'];
            $sql = "SELECT *
                    FROM posts
                    INNER JOIN post_tags ON posts.id = post_tags.post_id
                    INNER JOIN tags ON tags.id = post_tags.tag_id
                    WHERE tags.tag='$tag'";
          $result = mysqli_query($this->db,$sql);
          return $result;

        }
            $sql = "SELECT * from posts";
            $result = mysqli_query($this->db, $sql);
            return $result;
        }

        public function updatePost($title,$description,$slug){
      		$newImage = $_FILES['image']['name'];
      		if(!empty($newImage)){
      			$image = uploadImage();
      			$sql = "UPDATE posts SET title ='$title',description='$description',image = '$image' WHERE slug = '$slug'";
      			$result = mysqli_query($this->db,$sql);
      			return $result;

      		}else{
      			$sql = "UPDATE posts SET title ='$title',description='$description' WHERE slug = '$slug'";
      			$result = mysqli_query($this->db,$sql);
      			return $result;
      		}
      	}

        public function deletePostBySlug($slug){
          $sql = "DELETE FROM posts WHERE slug='$slug'";
          $result = mysqli_query($this->db,$sql);
          return $result;
        }

        public function getSinglePost($slug){
          $sql = "SELECT * FROM posts WHERE slug='$slug'";
          $result = mysqli_query($this->db, $sql);
          return $result;
      }
    }

?>

<?php
include('session.php');
include('header.php'); ?>
<?php
include('post.php');
$post = new Post($db);

?>

<div class="container">
	<h2>All Posts</h2>


	<?php
		if(!empty($_SESSION['username'])){
			echo "Hello, {$_SESSION['username']}";
		}else{
			echo"You're not logged in!";
		}
	?>

	</h2>
	<table class="table table-striped">
		<thead>
			<tr>
				<th>Title</th>
				<th>Description</th>
				<th>Created at</th>
				<th>Action</th>
			</tr>
		</thead>
		<tbody>
			<?php foreach($post->getPost() as $post){ ?>
			<tr>
				<td><?php echo $post['title']; ?></td>
				<td><?php echo $post['description'] ?></td>
				<td><?php echo date('Y-m-d',strtotime($post['created_at'])); ?></td>
				<td>
					<a href="view.php?slug=<?php echo $post['slug'];?>"><button type="submit" class="btn btn-outline-success btn-sm">View</button></a>
					<a href="edit.php?slug=<?php echo $post['slug'];?>"><button type="submit" class="btn btn-outline-primary btn-sm">Edit</button></a>
					<a href="delete.php?slug=<?php echo $post['slug'];?>"><button type="submit" class="btn btn-outline-danger btn-sm">Delete</button></a>
				</td>
			<?php }?>
			</tr>
		</tbody>
	</table>
</div>

 <?php
session_start();
if(empty($_SESSION['username'])){
	header("location:login.php");

}
?>

<?php include('header.php');
	  include('post.php');

$posts = new Post($db);

 ?>

 <div class="container">
 	<div class="row">
 		<?php foreach($posts->getSinglePost($_GET['slug']) as $post){ ?>
 		<div class="card">
 			<img  class="card-img-top"src="images/<?php echo $post['image']; ?>" >
 		</div>
 		<div class="card-body">
 			<h4 class="card-title"><?php echo $post['title']; ?></h4>
 			<p class="card-text"><?php echo $post['description']; ?></p>
 		</div>
 	<?php } ?>
</div>
</div>

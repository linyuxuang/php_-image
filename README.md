# php_-image
php_画图
              
               imagecreatetruecolor — 新建一个真彩色图像
               imagecolorallocate — 为一幅图像分配颜色
               imagefill — 区域填充
               
               

        //step 1创建图片资源
            $img=imagecreatetruecolor(200, 200);

          $red=imagecolorallocate($img, 255, 0, 0);
          $yellow=imagecolorallocate($img, 255, 255, 0);
          $green=imagecolorallocate($img, 0, 255, 0);
          $blue=imagecolorallocate($img, 0, 0, 255);

          imagefill($img, 0, 0, $yellow);


          //step 2画各种图形
            //画一个矩形并填充
            imagefilledrectangle — 画一矩形并填充
          imagefilledrectangle($img, 10, 10, 80, 80, $green);
            //画一个矩形
            imagerectangle — 画一个矩形
          imagerectangle($img, 90, 10, 190, 80, $green);

          //线段
            imageline — 画一条线段
          imageline($img,0, 0, 200, 200 ,$blue);
          imageline($img,200, 0, 0, 200, $blue);

          //点
             imagesetpixel — 画一个单一像素
          imagesetpixel($img,50, 50 ,$red);
          imagesetpixel($img,55, 50 ,$red);
          imagesetpixel($img,59, 50 ,$red);
          imagesetpixel($img,64, 50 ,$red);
          imagesetpixel($img,72, 50 ,$red);

          //圆
            imageellipse — 画一个椭圆
          imageellipse($img, 100, 100, 100, 100,$green);
          //圆
             imagefilledellipse — 画一椭圆并填充
          imagefilledellipse($img, 100, 100, 10, 10,$blue);
          
          
          //输出或保存图像
          header("Content-Type:image/gif");
            imagegif — 输出图象到浏览器或文件。
          imagegif($img);

          //释放资源
          imagedestroy($img);



 在图片上添加画图
 
             
              
                imagecreatefromjpeg — 由文件或 URL 创建一个新图象。
                imagecolorallocate为一幅图像分配颜色
                imageline — 画一条线段
                imageellipse — 画一个椭圆
                imagegif — 输出图象到浏览器或文件。
                imagedestroy — 销毁一图像
                $img=imagecreatefromjpeg("img/1.jpg");

                $red= imagecolorallocate($img, 255, 0, 0);

                imageline($img, 0, 0, 100, 100, $red);

                imageellipse($img, 250, 160, 100, 100, $red);

                imagegif($img, "img/2.jpg");

                imagedestroy($img);


获取图片的属性


                    imagesx — 取得图像宽度
                    imagesy — 取得图像高度

                    getimagesize — 取得图像大小(他返回来的是一个数组)
                    
                    
                    //这是一种方式
                    $img=imagecreatefromjpeg("./img/1.jpg");

                        echo 'width:'.imagesx($img)."<br>";
                      echo 'height:'.imagesy($img)."<br>";

                    //第二种方式
                      $arr=getimagesize("./img/1.jpg");
                      echo 'width:'.$arr[0]."<br>";
                      echo 'height:'.$arr[1]."<br>";

                    imagedestroy($img);



放大图片大小

     例子一：
 
    	   getimagesize(图片名称);  //返回数组， 0==width 1==height 2==type
         imagecopyresized — 拷贝部分图像并调整大小

            
            $filename="./img/1.jpg";

              $per=2;

              list($width, $height)=getimagesize($filename);

              $n_w=$width*$per;
              $n_h=$width*$per;

              $new=imagecreatetruecolor($n_w, $n_h);

              $img=imagecreatefromjpeg($filename);

              imagecopyresized($new, $img,0, 0,0, 0,$n_w, $n_h, $width, $height);


              imagejpeg($new, "./img/4.jpg");

              imagedestroy($new);
              imagedestroy($img);



      例子二：

                 imagecreatetruecolor — 新建一个真彩色图像
                 imagecreatefromjpeg — 由文件或 URL 创建一个新图象。
                 imagecopyresampled — 重采样拷贝部分图像并调整大小
                 imagejpeg — 输出图象到浏览器或文件。
                 
                 
                function thumn($background, $width, $height, $newfile) {
                  list($s_w, $s_h)=getimagesize($background);

                  if ($width && ($s_w < $s_h)) {
                     $width = ($height / $s_h) * $s_w;
                  } else {
                     $height = ($width / $s_w) * $s_h;
                  }

                  $new=imagecreatetruecolor($width, $height);

                  $img=imagecreatefromjpeg($background);

                  imagecopyresampled($new, $img, 0, 0, 0, 0, $width, $height, $s_w, $s_h);

                  imagejpeg($new, $newfile);

                  imagedestroy($new);
                  imagedestroy($img);
                }

                thumn("img/1.jpg", 200, 200, "./img/6.jpg");

图片透明处理
                  
              png jpeg透明色都正常， 只有gif不正常
              
             imagecolortransparent — 将某个颜色定义为透明色
             
             imagecolorstotal — 取得一幅图像的调色板中颜色的数目
                   
             imagecolorsforindex — 取得某索引的颜色
              本函数返回一个具有 red，green，blue 和 alpha 的键名的关联数组，包含了指定颜色索引的相应的值。 


                  function thumn($background, $width, $height, $newfile) {
                      list($s_w, $s_h)=getimagesize($background);

                      if ($width && ($s_w < $s_h)) {
                        $width = ($height / $s_h) * $s_w;
                      } else {
                        $height = ($width / $s_w) * $s_h;
                      }

                    $new=imagecreatetruecolor($width, $height);

                    $img=imagecreatefromjpeg($background);

                    $otsc=imagecolortransparent($img);

                    if($otsc >=0 && $otst < imagecolorstotal($img)){
                      $tran=imagecolorsforindex($img, $otsc);

                      $newt=imagecolorallocate($new, $tran["red"], $tran["green"], $tran["blue"]);

                      imagefill($new, 0, 0, $newt);

                      imagecolortransparent($new, $newt);
                    }


                    imagecopyresized($new, $img, 0, 0, 0, 0, $width, $height, $s_w, $s_h);

                    imagegif($new, $newfile);

                    imagedestroy($new);
                    imagedestroy($img);
                  }

                  thumn("img/1.jpg", 200, 200, "./img/7.jpg");

图片的裁剪


         imagecopyresized — 拷贝部分图像并调整大小
         imagecopyresampled — 重采样拷贝部分图像并调整大小
         

         function cut($background, $cut_x, $cut_y, $cut_width, $cut_height, $location){

                  $back=imagecreatefromjpeg($background);

                  $new=imagecreatetruecolor($cut_width, $cut_height);

                  imagecopyresampled($new, $back, 0, 0, $cut_x, $cut_y, $cut_width,                                    $cut_height,$cut_width,$cut_height);

                  imagejpeg($new, $location);

                  imagedestroy($new);
                  imagedestroy($back);
                }

                cut("./img/1.jpg", 440, 140, 117, 112, "./img/8.jpg");


加水印（文字， 图片）

               imagettftext — 用 TrueType 字体向图像写入文本
               imagecopy — 拷贝图像的一部分


          给图片上添加文字

         function mark_text($background, $text, $x, $y){
            $back=imagecreatefromjpeg($background);

            $color=imagecolorallocate($back, 0, 255, 0);

            imagettftext($back, 20, 0, $x, $y, $color, "simkai.ttf", $text);

            imagejpeg($back, "./images/hee8.jpg");

            imagedestroy($back);
          }

          mark_text("./images/hee.jpg", "细说PHP", 150, 250);


	 给图片上添加一个图片         
     
             	function mark_pic($background, $waterpic, $x, $y){
                  $back=imagecreatefromjpeg($background);
                  $water=imagecreatefromgif($waterpic);


                  $w_w=imagesx($water);
                  $w_h=imagesy($water);

                  imagecopy($back, $water, $x, $y, 0, 0, $w_w, $w_h);

                  imagejpeg($back,"./images/hee9.jpg");

                  imagedestroy($back);
                  imagedestroy($water);
                }

                mark_pic("./images/hee.jpg", "./images/gaolf.gif", 50, 200);


图片旋转

                imagerotate -- 用给定角度旋转图像
		
		

                $back=imagecreatefromjpeg("./images/hee.jpg");

	        $new=imagerotate($back, 45, 0);

	         imagejpeg($new, "./images/hee9.jpg");



图片翻转
    	
          沿Y轴  沿X轴

             沿Y轴 
            function turn_y($background, $newfile){
		$back=imagecreatefromjpeg($background);

		$width=imagesx($back);
		$height=imagesy($back);

		$new=imagecreatetruecolor($width, $height);

		for($x=0; $x < $width; $x++){
			imagecopy($new, $back, $width-$x-1, 0, $x, 0, 1, $height);
		}

		imagejpeg($new, $newfile);

		imagedestroy($back);
		imagedestroy($new);
	}

    
      沿X轴
	function turn_x($background, $newfile){
		$back=imagecreatefromjpeg($background);

		$width=imagesx($back);
		$height=imagesy($back);

		$new=imagecreatetruecolor($width, $height);

		for($y=0; $y < $height; $y++){
			imagecopy($new, $back,0, $height-$y-1, 0, $y, $width, 1);
		}

		imagejpeg($new, $newfile);

		imagedestroy($back);
		imagedestroy($new);
	}

	turn_y("./images/hee.jpg", "./images/hee11.jpg");
	turn_x("./images/hee.jpg", "./images/hee12.jpg");












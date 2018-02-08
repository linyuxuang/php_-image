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













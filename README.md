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



  实现   验证码

            validationcode.class文件
            
             
             <?php
            class ValidationCode {
              private $width;
              private $height;
              private $codeNum;
              private $image;   //图像资源
              private $disturbColorNum;
              private $checkCode;

              function __construct($width=80, $height=20, $codeNum=4){
                $this->width=$width;
                $this->height=$height;
                $this->codeNum=$codeNum;
                $this->checkCode=$this->createCheckCode();
                $number=floor($width*$height/15);

                if($number > 240-$codeNum){
                  $this->disturbColorNum=	240-$codeNum;
                }else{
                  $this->disturbColorNum=$number;
                }


              }
              //通过访问该方法向浏览器中输出图像
              function showImage($fontFace=""){
                //第一步：创建图像背景
                $this->createImage();
                //第二步：设置干扰元素
                $this->setDisturbColor();
                //第三步：向图像中随机画出文本
                $this->outputText($fontFace);
                //第四步：输出图像
                $this->outputImage();
              }

              //通过调用该方法获取随机创建的验证码字符串
              function getCheckCode(){
                return $this->checkCode;
              }

              private function createImage(){
                //创建图像资源
                $this->image=imagecreatetruecolor($this->width, $this->height);
                //随机背景色
                $backColor=imagecolorallocate($this->image, rand(225, 255),
                rand(225,255), rand(225, 255));
                
                //为背景添充颜色
                imagefill($this->image, 0, 0, $backColor);
                //设置边框颜色
                $border=imagecolorallocate($this->image, 0, 0, 0);
                //画出矩形边框
                imagerectangle($this->image, 0, 0, $this->width-1, $this->height-1, $border);
              }

              private function  setDisturbColor(){
                for($i=0; $i<$this->disturbColorNum; $i++){
                
                  $color=imagecolorallocate($this->image, rand(0, 255),
                  rand(0, 255), rand(0, 255));
                  
                  imagesetpixel($this->image, rand(1, $this->width-2), 
                  rand(1, $this->height-2), $color); 
                }

                for($i=0; $i<10; $i++){
                  $color=imagecolorallocate($this->image, rand(200, 255), 
                  rand(200, 255), rand(200, 255));
                  
                  imagearc($this->image, rand(-10, $this->width), 
                  rand(-10, $this->height), rand(30, 300), rand(20, 200), 55, 44, $color);
                }
              }

              private function createCheckCode(){
                $code="23456789abcdefghijkmnpqrstuvwxyzABCDEFGHIJKMNPQRSTUVWXYZ";
                $string='';
                for($i=0; $i < $this->codeNum; $i++){
                  $char=$code{rand(0, strlen($code)-1)};
                  $string.=$char;
                }

                return $string;
              }

              private function outputText($fontFace=""){
                for($i=0; $i<$this->codeNum; $i++){
                
                  $fontcolor=imagecolorallocate($this->image, rand(0, 128),
                  rand(0, 128), rand(0, 128));
                  
                  if($fontFace==""){
                    $fontsize=rand(3, 5);
                    $x=floor($this->width/$this->codeNum)*$i+3;
                    $y=rand(0, $this->height-15);
                    imagechar($this->image,$fontsize, $x, $y, $this->checkCode{$i},$fontcolor);
                  }else{
                    $fontsize=rand(12, 16);
                    $x=floor(($this->width-8)/$this->codeNum)*$i+8;
                    $y=rand($fontSize+5, $this->height);
                    imagettftext($this->image,$fontsize,
                    rand(-30, 30),$x,$y ,$fontcolor, $fontFace, $this->checkCode{$i});
                  }
                }
              }

              private function outputImage() {
                if(imagetypes() & IMG_GIF){
                  header("Content-Type:image/gif");
                  imagepng($this->image);
                }else if(imagetypes() & IMG_JPG){
                  header("Content-Type:image/jpeg");
                  imagepng($this->image);
                }else if(imagetypes() & IMG_PNG){
                  header("Content-Type:image/png");
                  imagepng($this->image);
                }else if(imagetypes() & IMG_WBMP){
                  header("Content-Type:image/vnd.wap.wbmp");
                  imagepng($this->image);
                }else{
                  die("PHP不支持图像创建");
                }
              }

              function __destruct(){
                imagedestroy($this->image);
              }
            }
            
            

        code.php 文件
          
         <?php
          session_start();
          include "validationcode.class.php";

          $code=new ValidationCode(80, 20, 4);

          $code->showImage();   //输出到页面中供 注册或登录使用

          $_SESSION["code"]=$code->getCheckCode();  //将验证码保存到服务器中



        login.php文件
        
             <?php
              session_start();
              echo $_POST["code"]."<br>";
              echo $_SESSION["code"]."<br>";

              if(strtoupper($_POST["code"])==strtoupper($_SESSION["code"])){
                echo "ok";
              }else{
                echo "error";
              }
            ?>
            <body>
              <form action="login.php" method="post">
                user:<input type="text" name="usenrame"><br>
                pass:<input type="passowrd" name="pass"><br>

                code: <input type="text" name="code" 
                onkeyup="if(this.value!=this.value.toUpperCase())
                this.value=this.value.toUpperCase()">
                
                <img src="code.php" onclick="this.src='code.php?'+Math.random()">
                <br>

                <input type="submit" name="sub" value="login"><br>
              </form>
            </body>
 
             













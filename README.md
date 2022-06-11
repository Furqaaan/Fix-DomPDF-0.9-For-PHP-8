Fix-DomPDF-For-PHP-8-Without-Updating

Fix DomPDF 0.8 or 0.9 For PHP 8

**1) vendor\dompdf\dompdf\src\Adapter\CPDF**

Change
```
 public function __construct($paper = "letter", $orientation = "portrait", Dompdf $dompdf)
 ```
 
To
```
 public function __construct($paper = "letter", $orientation = "portrait", Dompdf $dompdf = null )
```
 
Change
 ```
    $this->_dompdf = $dompdf;
 ```
  
 To
 ```
     if ($dompdf === null) {
          $this->_dompdf = new Dompdf();
      } else {
          $this->_dompdf = $dompdf;
      }
 ```
      
 **2) vendor\dompdf\dompdf\src\Canvas.php**
 
 Change 
 ```
  function __construct($paper = "letter", $orientation = "portrait", Dompdf $dompdf);
 ```
 
 To
 ```
  function __construct($paper = "letter", $orientation = "portrait", Dompdf $dompdf = null);
 ```
  
 **3) vendor\dompdf\dompdf\lib\Cpdf.php**
 
 Change
 ```
  function addImagePng($file, $x, $y, $w = 0.0, $h = 0.0, &$img, $is_mask = false, $mask = null)
 ```
 
 To
 ```
  function addImagePng(&$img, $file, $x, $y, $w = 0.0, $h = 0.0, $is_mask = false, $mask = null)
 ```
  
  
  Change
  ```
    $this->addImagePng($tempfile_alpha, $x, $y, $w, $h, $imgalpha, true);
 ```
 
 To
 ```
    $this->addImagePng($imgalpha, $tempfile_alpha, $x, $y, $w, $h, true);
```
  
Change
```
     $this->addImagePng($tempfile_plain, $x, $y, $w, $h, $imgplain, false, ($tempfile_alpha !== null));
```

To
```
    $this->addImagePng($imgplain, $tempfile_plain, $x, $y, $w, $h, false, ($tempfile_alpha !== null));
```
    
Change
  ```
     $this->addImagePng($file, $x, $y, $w, $h, $img);
 ```
 
To
```
     $this->addImagePng($img, $file, $x, $y, $w, $h);
```
     
Change
```
    function addPngFromBuf($file, $x, $y, $w = 0.0, $h = 0.0, &$data, $is_mask = false, $mask = null)
```

To
```
    function addPngFromBuf(&$data, $file, $x, $y, $w = 0.0, $h = 0.0, $is_mask = false, $mask = null)
```
    
Change
 ```
    $this->addPngFromBuf($file, $x, $y, $w, $h, $data, $is_mask, $mask);
```
To
```
    $this->addPngFromBuf($data, $file, $x, $y, $w, $h, $is_mask, $mask);
```

Change 
```
   private function addJpegImage_common(
        &$data,
        $x,
        $y,
        $w = 0,
        $h = 0,
        $imageWidth,
        $imageHeight,
        $channels = 3,
        $imgname
    )
 ```
 To
```
     private function addJpegImage_common(
        &$data,
        $imgname,
        $imageWidth,
        $imageHeight,
            $x,
            $y,
            $w = 0,
            $h = 0,
            $channels = 3,
       )
 ```
  
Change
```
        $this->addJpegImage_common($data, $x, $y, $w, $h, $imageWidth, $imageHeight, $channels, $img);
 ```
 
To
```
        $this->addJpegImage_common($data, $img, $imageWidth, $imageHeight, $x, $y, $w, $h, $channels);
 ```
        
        
  **4) vendor\dompdf\dompdf\src\Renderer\AbstractRenderer.php**

Change
```
      $this->_canvas->get_cpdf()->addImagePng($filedummy, $x, $this->_canvas->get_height() - $y - $height, $width, $height, $bg);
```

To
```
      $this->_canvas->get_cpdf()->addImagePng($bg, $filedummy, $x, $this->_canvas->get_height() - $y - $height, $width, $height);
```
      
Change 
```
        protected function _border_line($x, $y, $length, $color, $widths, $side, $corner_style = "bevel", $pattern_name, $r1 = 0, $r2 = 0)
```
  
To
```
        protected function _border_line($x, $y, $length, $color, $widths, $side, $corner_style = "bevel", $pattern_name = "none", $r1 = 0, $r2 = 0)
```
  
      
  **5) vendor\dompdf\dompdf\src\Css\Style.php**
  
Change
```
        if (round($val) != $val && $val !== "auto")
```
 
 To
```
        if ($val !== "auto" && round($val) != $val)
 ```
 
   **6) vendor\dompdf\dompdf\src\Renderer\Inline**
   
Change
``` 
          $w += (float)$widths[1] + (float)$widths[3];
          $h += (float)$widths[0] + (float)$widths[2];
```     

To
```  
       		$w = (float)$w;
		      $h = (float)$h;
          $w += (float)$widths[1] + (float)$widths[3];
          $h += (float)$widths[0] + (float)$widths[2];
```     
       
   **7) vendor\dompdf\dompdf\src\Adapter\GD.php**
    
Change
```
         public function __construct($size = 'letter', $orientation = "portrait", Dompdf $dompdf, $aa_factor = 1.0, $bg_color = [1, 1, 1, 0])
```

 To
```
        public function __construct($size = 'letter', $orientation = "portrait", Dompdf $dompdf = null, $aa_factor = 1.0, $bg_color = [1, 1, 1, 0])
```

Change
```
        $this->_dompdf = $dompdf;
```
 
To
```
      if ($dompdf === null) {
          $this->_dompdf = new Dompdf();
      } else {
          $this->_dompdf = $dompdf;
      }
```

   **8) vendor\dompdf\dompdf\src\Adapter\PDFLib.php**
    
Change
```
          public function __construct($paper = "letter", $orientation = "portrait", Dompdf $dompdf)
```
          
To 
```
          public function __construct($paper = "letter", $orientation = "portrait", Dompdf $dompdf = null)
```
          
Change
```
          $this->_dompdf = $dompdf;
```     

To
```
         if ($dompdf === null) {
            $this->_dompdf = new Dompdf();
          } else {
              $this->_dompdf = $dompdf;
          }
```       
          
   **9) vendor\dompdf\dompdf\src\FrameReflower\Block.php**
     
Change
```        
          $height = $style->length_in_pt($style->height, $cb["h"]);
```
          
To
```        
          $height = $style->length_in_pt($style->height, $cb["h"]) === 'auto' ? $content_height : $style->length_in_pt($style->height, $cb["h"]);
```
          
          
       

Sources :
https://github.com/dompdf/dompdf/commit/6e48707d7b39140c4846477d5b7d26f486d1da06#diff-9cc2b6d318b74bc8bf9723ddf80e5150712c5fcebb8e9d30ebba54b0139f3db4
https://github.com/dompdf/dompdf/commit/6c82f65912f777ab8a9c76149cb64c5a933308c2
https://github.com/dompdf/dompdf/commit/df57bbd29e5e08a8aa76ce321e9cf109aad6fc8f
https://github.com/dompdf/dompdf/issues/2242

https://github.com/dompdf/dompdf/pull/2313/commits/04753b44abd693022d2c1dc1e4279a777a3948d7
https://github.com/dompdf/dompdf/pull/2313/commits/5f1f790751c6429433e27a262d753a3e8da4ff07
https://github.com/dompdf/dompdf/pull/2313/commits/5c11bbe75725e6a21c1f8c14c276e6136d37eb7d

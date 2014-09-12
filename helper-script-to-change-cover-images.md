#Helper script to change cover images

    <script type="text/javascript" src='https://ajax.googleapis.com/ajax/libs/jquery/1.8.3/jquery.min.js'></script>
    <script type="text/javascript">
      var idx = 0;
      var existingLikes =  [122, 132, 160, 187, 187, 187, 216, 256, 256, 314, 360];
      var likes = [];
      var changeBg = (function(idx){ $('body').css('background-image', 'url(http://localhost:8080/' + existingLikes[idx] + '.jpg)'); })
      $(document).on('keyup', function(e){
          var inc = e.keyCode == 37 ? -1 : (e.keyCode == 39 ? 1 : 0)
          idx += inc;
          changeBg(idx);
          if(e.keyCode == 13){
            likes.push(existingLikes[idx]);
            //likes.push(idx);
            console.log("LIKES", likes.join(","))
          }
      });
    </script>

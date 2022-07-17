# usefull-code

## lazy load images js
```js
$(document).ready(function() {
  let lazyloadImages;

//if window object has an intersectio observer
  if ("IntersectionObserver" in window) {
 //get all items marked for lazy load class
    lazyloadImages = document.querySelectorAll(".lazy");

//create new observer with IntersectionObserver(callbackfunc(),options{}) basic syntax
    let imageObserver = new IntersectionObserver(function(entries, observer) {
      console.log(observer);
      entries.forEach(function(entry) {
        if (entry.isIntersecting) {
          var image = entry.target;
          image.src = image.dataset.src;
          image.classList.remove("lazy");
          imageObserver.unobserve(image);
        }
      });
    }, 
    
  //options {root:, rootMargin:, threshold:} for intersectionobserver
    {
//what elements acts as viewport can be element or window if root:null
      root: document.querySelector("#container"),

//margin around view port element to be used to calculate intersection default 0px
      rootMargin: "0px",

//what percentage of element should be visible before loading in lazy load element
      threshold:0,
    });

    lazyloadImages.forEach(function(image) {
      imageObserver.observe(image);
    });
  } 
  else {
    let lazyloadThrottleTimeout;
    lazyloadImages = $(".lazy");

    function lazyload () {
      if(lazyloadThrottleTimeout) {
        clearTimeout(lazyloadThrottleTimeout);
      }

      lazyloadThrottleTimeout = setTimeout(function() {
          let scrollTop = $(window).scrollTop();
          lazyloadImages.each(function() {
              let el = $(this);
              if(el.offset().top < window.innerHeight + scrollTop + 500) {
                let url = el.attr("data-src");
                el.attr("src", url);
                el.removeClass("lazy");
                lazyloadImages = $(".lazy");
              }
          });
          if(lazyloadImages.length == 0) {
            $(document).off("scroll");
            $(window).off("resize");
          }
      }, 20);
    }

    $(document).on("scroll", lazyload);
    $(window).on("resize", lazyload);
  }
})```

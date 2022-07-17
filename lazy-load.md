## Lazy load

## vscode preview markdown Ctrl + shift + V
## lazy load images js
[mdn instersection observer](https://developer.mozilla.org/en-US/docs/Web/API/Intersection_Observer_API#creating_an_intersection_observer)


#### mark images to be lazy loaded with class="lazy" and a data-img="www.image.com" property with url 
```js
   window.onload = (function() {
  let lazyloadImages; //will hold all lazy elements in nodelist

//if window object has an intersectio observer
if ("IntersectionObserver" in window) {
//get all items marked for lazy load class
console.log(`instersect in window object`)
  lazyloadImages = document.querySelectorAll(".lazy");
  let options={
    root:null,
    rootMargin:'0px',
    threshold:0,

  }


//callback inside observer auto recieves entries and observer arguments
  let callback=function(entries, observer){
    entries.forEach((entry)=>{
        console.log(`observer callback`)
      //element in in view port and has class of lazyload
      if(entry.isIntersecting && entry.target.className==='lazy'){

        //get elements hidden image url data-img
        let imageUrl = entry.target.getAttribute('data-img')
        console.log(imageUrl)

        //if it had a url in data-img
        if(imageUrl){
          //swap it to src attribute
          entry.target.src = imageUrl;

          //remove observer since it no longer needed
          observer.unobserve(entry.target)
        }
      }
    })
  }

//observer instance
  let observer = new IntersectionObserver(callback,options);

  //if anything to observe
    if(lazyloadImages){
      lazyloadImages.forEach(image=>{
        console.log(`observer on kitten`)
        observer.observe(image)
      })
    }


} 

})
```
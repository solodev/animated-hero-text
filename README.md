# animated-hero-text
Oftentimes, hero text can be boring or perhaps you have more to say than just one fragment of a sentence. In some cases, you may provide multiple services or products that can't be summed up in one header. That's where animated hero text comes in. Using the example in the tutorial below, you can have your hero text alternate to describe different aspects of your business. In this article, [Solodev](https://www.solodev.com/blog/) will show you how to add animated hero text to your website using jQuery.

## Tutorial

For detailed instructions, view Solodev's [Adding Infinite Animated Hero Text with jQuery](https://www.solodev.com/blog/web-design/adding-infinite-animated-hero-text-with-jquery.stml) article.

## Demo

Try out a working example on [JSFiddle](https://jsfiddle.net/solodev/8vqk6n8r/).

## HTML

The animated hero text example contains the following basic HTML markup.

```
<div id="highlighted">
   <div id="search-form" class="container">
      <div class="row">
         <div class="col-sm-12">
            <div class="mod-search block">
               <h1 class="home-image">
                  <img alt="WebCorpCo" class="img-responsive" src="https://www.solodev.com/assets/hero-form/logo2.png">
               </h1>
               <h2 class="home-title">
                  <section class="cd-intro" style="user-select: auto;">
                     <h2 class="cd-headline clip is-full-width font-accident-two-normal uppercase" style="user-select: auto;">
                        <span style="user-select: auto;">The Company of Choice for </span>
                        <span class="cd-words-wrapper" style="width: 81.5142px; user-select: auto; overflow: hidden;">
                        <b class="is-visible" style="user-select: auto;">Marketing</b>
                        <b class="is-hidden" style="user-select: auto;">Design</b>
                        <b class="is-hidden" style="user-select: auto;">Programming</b>
                        <b class="is-hidden" style="user-select: auto;">Training</b>
                        </span>
                     </h2>
                  </section>
               </h2>
               <div class="block-content">
                  <form action="#" class="hero-form form-inline" id="hero-form" method="post" role="form">
                     <div class="form-group">
                        <label class="control-label sr-only" for="services-select">Choose Type of Service</label> 
                        <select>
                           <option value="">
                              Choose Type of Service
                           </option>
                           <option value="marketing">
                              Marketing
                           </option>
                           <option value="design">
                              Design
                           </option>
                           <option value="programming">
                              Programming
                           </option>
                           <option value="support">
                              Support
                           </option>
                           <option value="training">
                              Training
                           </option>
                        </select>
                        <label class="control-label sr-only" for="email">Enter Email</label>
                        <input id="email" class="form-control" name="email_search" placeholder="Enter Email" type="text">
                        <input id="Get Started" class="form-control btn btn-lg btn-default btn-warning" type="submit" value="Get Started">
                     </div>
                  </form>
                  <h3 class="home-title">
                     <a href="#">Learn more about WebCorpCo&nbsp;<i class="fa fa-angle-double-right fa-lg" aria-hidden="true"></i></a>
                  </h3>
               </div>
            </div>
         </div>
      </div>
   </div>
   <section class="heroimage-wrapper">
      <div class="heroimage">
      </div>
   </section>
</div>
```
## JavaScript
The animated hero text example utilizes the following JavaScript.
```
$(document).ready(function () {

        (function () {
            //set animation timing
            var animationDelay = 3500,
            //loading bar effect
                barAnimationDelay = 3800,
                barWaiting = barAnimationDelay - 3000, //3000 is the duration of the transition on the loading bar - set in the scss/css file
            //letters effect
                lettersDelay = 50,
            //type effect
                typeLettersDelay = 150,
                selectionDuration = 500,
                typeAnimationDelay = selectionDuration + 800,
            //clip effect
                revealDuration = 600,
                revealAnimationDelay = 2500;

            initHeadline();


            function initHeadline() {
                //insert <i> element for each letter of a changing word
                singleLetters($('.cd-headline.letters').find('b'));
                //initialise headline animation
                animateHeadline($('.cd-headline'));
                //set basic width on load
                $('.cd-words-wrapper').css('width', '200px');
            }

            function singleLetters($words) {
                $words.each(function(){
                    var word = $(this),
                        letters = word.text().split(''),
                        selected = word.hasClass('is-visible');
                    for (var i in letters) {
                        if(word.parents('.rotate-2').length > 0) letters[i] = '<em>' + letters[i] + '</em>';
                        letters[i] = (selected) ? '<i class="in">' + letters[i] + '</i>': '<i>' + letters[i] + '</i>';
                    }
                    var newLetters = letters.join('');
                    word.html(newLetters).css('opacity', 1);
                });
            }

            function animateHeadline($headlines) {
                var duration = animationDelay;
                $headlines.each(function(){
                    var headline = $(this);

                    if(headline.hasClass('loading-bar')) {
                        duration = barAnimationDelay;
                        setTimeout(function(){ headline.find('.cd-words-wrapper').addClass('is-loading') }, barWaiting);
                    } else if (headline.hasClass('clip')){
                        var spanWrapper = headline.find('.cd-words-wrapper'),
                            newWidth = spanWrapper.width() + 10
                        spanWrapper.css('width', newWidth);
                    } else if (!headline.hasClass('type') ) {
                        //assign to .cd-words-wrapper the width of its longest word
                        var words = headline.find('.cd-words-wrapper b'),
                            width = 0;
                        words.each(function(){
                            var wordWidth = $(this).width();
                            if (wordWidth > width) width = wordWidth;
                        });
                        headline.find('.cd-words-wrapper').css('width', width);
                    };

                    //trigger animation
                    setTimeout(function(){ hideWord( headline.find('.is-visible').eq(0) ) }, duration);
                });
            }

            function hideWord($word) {
                var nextWord = takeNext($word);

                if($word.parents('.cd-headline').hasClass('type')) {
                    var parentSpan = $word.parent('.cd-words-wrapper');
                    parentSpan.addClass('selected').removeClass('waiting');
                    setTimeout(function(){
                        parentSpan.removeClass('selected');
                        $word.removeClass('is-visible').addClass('is-hidden').children('i').removeClass('in').addClass('out');
                    }, selectionDuration);
                    setTimeout(function(){ showWord(nextWord, typeLettersDelay) }, typeAnimationDelay);

                } else if($word.parents('.cd-headline').hasClass('letters')) {
                    var bool = ($word.children('i').length >= nextWord.children('i').length) ? true : false;
                    hideLetter($word.find('i').eq(0), $word, bool, lettersDelay);
                    showLetter(nextWord.find('i').eq(0), nextWord, bool, lettersDelay);

                }  else if($word.parents('.cd-headline').hasClass('clip')) {
                    $word.parents('.cd-words-wrapper').animate({ width : '2px' }, revealDuration, function(){
                        switchWord($word, nextWord);
                        showWord(nextWord);
                    });

                } else if ($word.parents('.cd-headline').hasClass('loading-bar')){
                    $word.parents('.cd-words-wrapper').removeClass('is-loading');
                    switchWord($word, nextWord);
                    setTimeout(function(){ hideWord(nextWord) }, barAnimationDelay);
                    setTimeout(function(){ $word.parents('.cd-words-wrapper').addClass('is-loading') }, barWaiting);

                } else {
                    switchWord($word, nextWord);
                    setTimeout(function(){ hideWord(nextWord) }, animationDelay);
                }
            }

            function showWord($word, $duration) {
                if($word.parents('.cd-headline').hasClass('type')) {
                    showLetter($word.find('i').eq(0), $word, false, $duration);
                    $word.addClass('is-visible').removeClass('is-hidden');

                }  else if($word.parents('.cd-headline').hasClass('clip')) {
                    $word.parents('.cd-words-wrapper').animate({ 'width' : $word.width() + 10 }, revealDuration, function(){
                        setTimeout(function(){ hideWord($word) }, revealAnimationDelay);
                    });
                }
            }

            function hideLetter($letter, $word, $bool, $duration) {
                $letter.removeClass('in').addClass('out');

                if(!$letter.is(':last-child')) {
                    setTimeout(function(){ hideLetter($letter.next(), $word, $bool, $duration); }, $duration);
                } else if($bool) {
                    setTimeout(function(){ hideWord(takeNext($word)) }, animationDelay);
                }

                if($letter.is(':last-child') && $('html').hasClass('no-csstransitions')) {
                    var nextWord = takeNext($word);
                    switchWord($word, nextWord);
                }
            }

            function showLetter($letter, $word, $bool, $duration) {
                $letter.addClass('in').removeClass('out');

                if(!$letter.is(':last-child')) {
                    setTimeout(function(){ showLetter($letter.next(), $word, $bool, $duration); }, $duration);
                } else {
                    if($word.parents('.cd-headline').hasClass('type')) { setTimeout(function(){ $word.parents('.cd-words-wrapper').addClass('waiting'); }, 200);}
                    if(!$bool) { setTimeout(function(){ hideWord($word) }, animationDelay) }
                }
            }

            function takeNext($word) {
                return (!$word.is(':last-child')) ? $word.next() : $word.parent().children().eq(0);
            }

            function takePrev($word) {
                return (!$word.is(':first-child')) ? $word.prev() : $word.parent().children().last();
            }

            function switchWord($oldWord, $newWord) {
                $oldWord.removeClass('is-visible').addClass('is-hidden');
                $newWord.removeClass('is-hidden').addClass('is-visible');
            }

        })();
        
});
```
## CSS

All required CSS is contained in animated-hero.css

## External Includes

This tutorial contains the following third party resources.
```
<link rel="stylesheet" href="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/css/bootstrap.min.css">
<link rel="stylesheet" href="animated-hero.css">
<script src="https://ajax.googleapis.com/ajax/libs/jquery/2.2.0/jquery.min.js"></script>
<script src="https://maxcdn.bootstrapcdn.com/bootstrap/3.3.7/js/bootstrap.min.js"></script>
<script src="animated-hero.js"></script>
```

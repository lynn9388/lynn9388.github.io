---
layout: page
title: About
description: 要有最柔软温和的心
header_img: /assets/img/about.jpeg
---

Hi there! I'm Lynn. This website is my personal blog powered by [Jekyll](https://jekyllrb.com/) and [Huxpro](https://github.com/lynn9388/huxpro).

### Hobbies

I like photography, baking, running, and of course programming. 🙂

<!-- Support for photo gallery https://github.com/blueimp/Gallery -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/blueimp-gallery/3.3.0/css/blueimp-gallery.min.css" integrity="sha512-ZpixWcgC4iZJV/pBJcyuoyD9sUsW0jRVBBTDge61Fj99r1XQNv0LtVIrCwHcy61iVTM+/1cXXtak8ywIbyvOdw==" crossorigin="anonymous" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/blueimp-gallery/3.3.0/js/blueimp-gallery.min.js" integrity="sha512-ou+HAocCH7k3ASCmn1jxK14HyDl7Ff0jci4skAEWcGKoLx32MPNOnLWaLh08XodPcaG59N9YsDyYN5+qPrR7Ag==" crossorigin="anonymous"></script>

<div id="blueimp-gallery" class="blueimp-gallery" aria-label="image gallery" aria-modal="true" role="dialog">
    <div class="slides" aria-live="polite"></div>
    <h3 class="title"></h3>
    <a class="prev" aria-controls="blueimp-gallery" aria-label="previous slide" aria-keyshortcuts="ArrowLeft"></a>
    <a class="next" aria-controls="blueimp-gallery" aria-label="next slide" aria-keyshortcuts="ArrowRight"></a>
    <a class="close" aria-controls="blueimp-gallery" aria-label="close" aria-keyshortcuts="Escape"></a>
    <a class="play-pause" aria-controls="blueimp-gallery" aria-label="play slideshow" aria-keyshortcuts="Space" aria-pressed="false role="button"></a>
    <ol class="indicator"></ol>
</div>

<div id="photography-links">
    {%- for image in site.static_files reversed -%}
        {%- if image.path contains "images/photography" -%}
            {%- unless image.path contains "thumbnails" -%}
                <a href="{{ image.path }}"><img src="{{ site.baseurl }}/assets/images/photography/thumbnails/{{ image.name }}"></a>
            {%- endunless -%}
        {%- endif -%}

        {%- if image.path contains "images/baking" -%}
            {%- unless image.path contains "thumbnails" -%}
                <a href="{{ image.path }}"><img src="{{ site.baseurl }}/assets/images/baking/thumbnails/{{ image.name }}"></a>
            {%- endunless -%}
        {%- endif -%}
    {%- endfor -%}
</div>

<script>
    document.getElementById('photography-links').onclick = function (event) {
        event = event || window.event
        var target = event.target || event.srcElement
        var link = target.src ? target.parentNode : target
        var options = { index: link, event: event }
        var links = this.getElementsByTagName('a')
        blueimp.Gallery(links, options)
    }
</script>

#### Running

<div class="row mt-3">
    {%- for image in site.static_files reversed -%}
        {%- if image.path contains "images/running" -%}
            {%- unless image.path contains "thumbnails" -%}
                <div class="col-4 col-sm-3 pb-3">
                    <a data-fancybox="running-images" data-caption="{{ image.basename }}" href="{{ image.path }}" >
                        <img class="img-fluid img-thumbnail img-responsive" src="{{ site.baseurl }}/assets/images/running/thumbnails/{{ image.name }}">
                    </a>
                </div>
            {%- endunless -%}
        {%- endif -%}
    {%- endfor -%}
</div>

<script>
    $('[data-fancybox="baking-images"], [data-fancybox="running-images"]').fancybox({
        transitionEffect: "slide",

        // Support for retina displays.
        afterLoad : function(instance, current) {
            var pixelRatio = window.devicePixelRatio || 1;

            if ( pixelRatio > 1.5 ) {
                current.width  = current.width  / pixelRatio;
                current.height = current.height / pixelRatio;
            }
        }
    });
</script>

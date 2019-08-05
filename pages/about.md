---
title: About
permalink: /about/
---

Hi there! I'm Lynn. This website is my personal blog powered by [Jekyll](https://jekyllrb.com/) and [light-blog](https://github.com/lynn9388/light-blog).

### Hobbies

I like photography, baking, running, and of course programming. 🙂

#### Photography

<!-- Support for photo gallery https://github.com/blueimp/Gallery -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/blueimp-gallery/2.34.1/css/blueimp-gallery.min.css" integrity="sha256-naDjnon+nzJq+z5LGT5dfwVi+u7YLvkdWwaUsxAgMxE=" crossorigin="anonymous" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/blueimp-gallery/2.34.1/js/blueimp-gallery.min.js" integrity="sha256-EcnKZNKRqT2zlkvfoLcGvm2fLPoXIltTuBL3buEbNsE=" crossorigin="anonymous"></script>

<div id="blueimp-gallery-carousel" class="blueimp-gallery blueimp-gallery-carousel my-3" style="color: white">
    <div class="slides"></div>
    <a class="prev">‹</a>
    <a class="next">›</a>
    <a class="play-pause"></a>
    <ol class="indicator"></ol>
</div>

<div id="photography-links" style="display: none;">
    {% for image in site.static_files reversed %}
        {% if image.path contains "images/photography" %}
            {% unless image.path contains "thumbnails" %}
                <a href="{{ image.path }}">
                    <img src="{{ site.baseurl }}/assets/images/photography/thumbnails/{{ image.name }}">
                </a>
            {% endunless %}
        {% endif %}
    {% endfor %}
</div>

<script>
    blueimp.Gallery(document.getElementById("photography-links").getElementsByTagName("a"), {
        container: '#blueimp-gallery-carousel',
        carousel: true
    })
</script>

#### Baking

<!-- Support for fancybox https://github.com/fancyapps/fancybox -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/fancybox/3.5.7/jquery.fancybox.min.css" integrity="sha256-Vzbj7sDDS/woiFS3uNKo8eIuni59rjyNGtXfstRzStA=" crossorigin="anonymous" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/fancybox/3.5.7/jquery.fancybox.min.js" integrity="sha256-yt2kYMy0w8AbtF89WXb2P1rfjcP/HTHLT7097U8Y5b8=" crossorigin="anonymous"></script>

<div class="row mt-3">
    {% for image in site.static_files reversed %}
        {% if image.path contains "images/baking" %}
            {% unless image.path contains "thumbnails" %}
                <div class="col-3 col-sm-2 pb-3">
                    <a data-fancybox="baking-images" data-caption="{{ image.basename }}" href="{{ image.path }}" >
                        <img class="img-fluid img-thumbnail img-responsive" src="{{ site.baseurl }}/assets/images/baking/thumbnails/{{ image.name }}">
                    </a>
                </div>
            {% endunless %}
        {% endif %}
    {% endfor %}
</div>

#### Running

<div class="row mt-3">
    {% for image in site.static_files reversed %}
        {% if image.path contains "images/running" %}
            {% unless image.path contains "thumbnails" %}
                <div class="col-4 col-sm-3 pb-3">
                    <a data-fancybox="running-images" data-caption="{{ image.basename }}" href="{{ image.path }}" >
                        <img class="img-fluid img-thumbnail img-responsive" src="{{ site.baseurl }}/assets/images/running/thumbnails/{{ image.name }}">
                    </a>
                </div>
            {% endunless %}
        {% endif %}
    {% endfor %}
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

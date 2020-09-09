---
layout: page
title: About
description: 要有最柔软温和的心
header_img: /assets/img/about.jpeg
---

Hi there! I'm Lynn. This website is my personal blog powered by [Jekyll](https://jekyllrb.com/) and [Huxpro](https://github.com/lynn9388/huxpro).

### Hobbies

I like photography, baking, running, and of course programming. 🙂

<div id="photography-container">
    {%- for image in site.static_files reversed -%}
        {%- if image.path contains "images/photography" -%}
            {%- unless image.path contains "thumbnails" -%}
    <a href="{{ image.path }}" data-fancybox="photography-gallery"><img src="{{ site.baseurl }}/assets/images/photography/thumbnails/{{ image.name }}"></a>
            {%- endunless -%}
        {%- endif -%}

        {%- if image.path contains "images/baking" -%}
            {%- unless image.path contains "thumbnails" -%}
                <a href="{{ image.path }}" data-fancybox="photography-gallery"><img src="{{ site.baseurl }}/assets/images/baking/thumbnails/{{ image.name }}"></a>
            {%- endunless -%}
        {%- endif -%}
    {%- endfor -%}
</div>

Here are some of my certificates of completion.

<div style="display: flex; flex-wrap: nowrap; overflow-x: auto">
    {%- for image in site.static_files reversed -%}
        {%- if image.path contains "images/running" -%}
            {%- unless image.path contains "thumbnails" -%}
    <div style="flex: 0 0 auto; padding-right: 20px">
        <a href="{{ image.path }}" data-fancybox="running-gallery"><img src="{{ site.baseurl }}/assets/images/running/thumbnails/{{ image.name }}" height="200"/></a>
    </div>
            {%- endunless -%}
        {%- endif -%}
    {%- endfor -%}
</div>

<!-- jQuery -->
<script src="https://cdnjs.cloudflare.com/ajax/libs/jquery/3.5.1/jquery.min.js" integrity="sha512-bLT0Qm9VnAYZDflyKcBaQ2gg0hSYNQrJ8RilYldYQ1FxQYoCLtUjuuRuZo+fjqhx/qtq/1itJ0C2ejDxltZVFg==" crossorigin="anonymous"></script>

<!-- Support for fancybox https://github.com/fancyapps/fancybox -->
<link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/fancybox/3.5.7/jquery.fancybox.min.css" integrity="sha512-H9jrZiiopUdsLpg94A333EfumgUBpO9MdbxStdeITo+KEIMaNfHNvwyjjDJb+ERPaRS6DpyRlKbvPUasNItRyw==" crossorigin="anonymous" />
<script src="https://cdnjs.cloudflare.com/ajax/libs/fancybox/3.5.7/jquery.fancybox.min.js" integrity="sha512-uURl+ZXMBrF4AwGaWmEetzrd+J5/8NRkWAvJx5sbPSSuOb0bZLqf+tOzniObO00BjHa/dD7gub9oCGMLPQHtQA==" crossorigin="anonymous"></script>

<script>
    window.onresize = function() {
        var containerWidth = $("#photography-container").width();
        var imageWidth = containerWidth / Math.ceil(containerWidth / 80);
        $("#photography-container").find("img").each(function(i, image) {
            image.width = imageWidth;
        });
    };

    $('[data-fancybox="photography-gallery"], [data-fancybox="running-gallery"]').fancybox({
        arrows: false,
        transitionEffect: "slide",
        buttons : [],
        thumbs : {}
    });
</script>

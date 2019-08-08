---
title: Reading List of Blockchain
---

The following are some related papers on blockchain, which can be used as a reading list for getting started. I recommend reading the early papers and choosing the papers you are interested in after you have a basic understanding of blockchain technology. Good luck!🤓

<!-- Support for BibTex https://github.com/pcooksey/bibtex-js -->
<script type="text/javascript" src="https://cdn.jsdelivr.net/gh/pcooksey/bibtex-js@a8026168951beef519cd771672c0dc09e6603cc9/src/bibtex_js.js"></script>

<bibtex src="{{ site.baseurl }}/assets/bibtex/reading-list-of-blockchain.bib"></bibtex>

<div class="bibtex_structure">
    <div class="group year" extra="DESC number">
        <div class="sort journal" extra="DESC string">
          <h2 class="title"></h2>
          <div class="templates"></div>
        </div>
    </div>
</div>

<style>
    .bibtex_display ul {
        font-family: "Times New Roman", Times, serif;
    }

    span.title {
        font-weight: bold;
    }

    span.booktitle, span.journal {
        font-style: italic;
    }
</style>

<div class="bibtex_display" callback="refresh()">
    <div class="bibtex_template">
        <ul><li>
            <div class="if url">
                <a class="bibtexVar" href="+URL+" extra="url"><span class="title"></span></a>
            </div>
            <div class="if !url">
                <div class="if doi">
                    <a class="bibtexVar" href="https://dx.doi.org/+DOI+" extra="doi"><span class="title"></span></a>
                </div>
                <div class="if !doi">
                    <span class="title"></span>
                </div>
            </div>
            <div class="if author">
                <span class="author" max="5"><span class="first"></span> <span class="last"></span></span>
            </div>
            <div>
                <span class="if booktitle">In <span class="booktitle"></span>,</span>
                <span class="if journal"><span class="journal"></span>,</span>
                <span class="if month"><span class="month"></span>,</span>
                <span class="if year"><span class="year"></span></span>
                <a class="bibtexVar" role="button" data-toggle="collapse" href="#bib+BIBTEXKEY+" aria-expanded="false" aria-controls="bib+BIBTEXKEY+" extra="BIBTEXKEY">[bib]</a>
            </div>
            <div class="bibtexVar collapse" id="bib+BIBTEXKEY+" extra="BIBTEXKEY">
                <pre><span class="bibtexraw noread"></span></pre>
            </div>
        </li></ul>
    </div>
</div>

<script>
    function refresh() {
        anchors.remove("h2");
        anchors.add("h2");
        tocbot.refresh();
    }
</script>

---
title: Reading List of Blockchain
sections:
  conference:
    filter: "@inproceedings"
    title: Conference Papers
  journal:
    filter: "@article"
    title: Journal Articles
  book:
    filter: "@book"
    title: Books
  other:
    filter: "@misc|@phdthesis|@mastersthesis|@bachelorsthesis|@techreport"
    title: Other Publications
---

The following are some related papers on blockchain, which can be used as a reading list for getting started. I recommend reading the early papers and choosing the papers you are interested in after you have a basic understanding of blockchain technology. Good luck!🤓

<!-- Support for BibTex https://github.com/pcooksey/bibtex-js -->
<script type="text/javascript" src="https://cdn.jsdelivr.net/gh/pcooksey/bibtex-js@5ccf967e3afb0e319b14b12a6e3a442be932bf0a/src/bibtex_js.js"></script>

<bibtex src="{{ site.baseurl }}/assets/bibtex/reading-list-of-blockchain.bib"></bibtex>

<div class="bibtex_structure">
    <div class="sections bibtextypekey">
        {% for section in page.sections %}
            <div class="section {{ section[1].filter }}">
                <h2>{{ section[1].title }}</h2>
                <div class="sort year" extra="DESC number">
                    <div class="templates"></div>
                </div>
            </div>
        {% endfor %}
    </div>
</div>

<style>
    .bibtex_display ul {
        font-family: "Times New Roman", Times, serif;
    }

    .title {
        font-weight: bold;
    }

    .booktitle, .journal {
        font-style: italic;
    }
</style>

<div class="bibtex_display">
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
    $(window).on("load", function () {
       tocbot.refresh();
    });
</script>

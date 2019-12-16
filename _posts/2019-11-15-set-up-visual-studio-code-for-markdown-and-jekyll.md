---
title: Set Up Visual Studio Code for Markdown and Jekyll
tags: [Markdown, Jekyll]
---

In the end, I switched from [Sublime Text]({{ site.baseurl }}{% post_url 2019-07-27-set-up-sublime-text-for-markdown-and-jekyll %}) back to Visual Studio Code 😂. I think I should not go back anymore. The reason I gave up the Visual Studio Code before was that it would make my computer overheat, but I found that I can solve this problem by [uninstalling or disabling unnecessary extensions](https://code.visualstudio.com/docs/editor/extension-gallery#_manage-extensions).

Visual Studio Code is very [friendly to Markdown](https://code.visualstudio.com/docs/languages/markdown) and has a very good experience even after installation without any customization. I mainly use Markdown to write my blog and README documents. The following settings are mainly based on my two needs.

## Markdown Linting

This is a feature that is not built into Visual Studio Code, which is the only I have to install an extension. The name of the extension is [markdownlint](https://marketplace.visualstudio.com/items?itemName=DavidAnson.vscode-markdownlint), the same as the [package for Sublime Text](https://github.com/markdownlint/markdownlint).

After installation, you only need to set the rules in the Visual Studio Code's `Settings` according to your writing habits. My settings are:

```json
"markdownlint.config": {
    "MD003": { "style": "atx" },
    "MD004": { "style": "dash" },
    "MD007": { "indent": 4 },
    "MD029": { "style": "one" },
    "MD046": { "style": "fenced" }
}
```

## Snippets for Jekyll

Visual Studio Code also supports [snippets](https://code.visualstudio.com/docs/editor/userdefinedsnippets). Since I put all my Jekyll projects under the `Blog` folder, I create a folder snippets through `Code -> Preferences -> User Snippets -> New Snippets file for 'Blog'...`, it's located in `/Blog/.vscode/markdown.code-snippets`.

```json
{% raw %}{
    "Markdown link": {
        "scope": "markdown",
        "prefix": "link",
        "body": "[$1]($2)"
    },
    "Markdown post link": {
        "scope": "markdown",
        "prefix": "post",
        "body": "[$1]({{ site.baseurl }}{% post_url $2 %})"
    },
    "Markdown post anchor link": {
        "scope": "markdown",
        "prefix": "anchor",
        "body": "[$1]({{ site.baseurl }}{% post_url $2 %}#$3)"
    },
    "Markdown image": {
        "scope": "markdown",
        "prefix": "image",
        "body": "![$1]({{ site.baseurl }}/assets/images/$2)"
    },
    "Markdown video": {
        "scope": "markdown",
        "prefix": "video",
        "body": "{% include youtube.html id=\"$1\" %}"
    }
}{% endraw %}
```

## Auxiliary Script

I haven't found an extension like [Jekyll](https://packagecontrol.io/packages/Jekyll) yet, so I wrote a few scripts to make my life easier.

### Create New Post

```bash
#!/bin/bash
set -euo pipefail

date=$(date +"%Y-%m-%d")
title=$(echo "$@" | tr "[:upper:]" "[:lower:]" | tr " " -)

cat >> "$date-$title.md" << EOF
---
title: $@
tags: []
---
EOF
```

It will create a file with the beginning of the current date, followed by the parameters of the script, where the parameters are the title. The file contains a simple template with the post's title. For example, you can use `newpost Hello World` (the script is saved as `newpost`) to create file `2019-11-15-hello-world.md`

### Build Site with Docker

I created a [Docker image](https://hub.docker.com/r/lynn9388/github-pages-docker) to make it easy for me to build Jekyll site based on [light-blog](https://github.com/lynn9388/light-blog). It's very convenient to use, as long as you install Docker, execute the following script in your site's root directory, Docker will automatically pull the image, and then build your site.

```bash
#!/bin/bash
set -euo pipefail

rm -f Gemfile.lock
docker run --rm -p 4000:4000 -v $(pwd):/site lynn9388/github-pages-docker
```

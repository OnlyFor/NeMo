# NeMo Website - Github Pages

This branch is the source of the content viewable in https://nvidia.github.io/NeMo/.
In order to create a blog post on the NeMo Github.io website (https://nvidia.github.io/NeMo/) you will need to write a 
markdown document and submit a PR to Github for this specific branch.

Preliminaries:
- It is preferred to use Visual Studio Code for writing markdown for mkdocs. You can use any ide to commit and manage your PR.
- You can setup autocomplete for mkdocs.yml by following the VS Code specific instructions here - https://squidfunk.github.io/mkdocs-material/creating-your-site/?h=vscode#minimal-configuration


# Workflow

- It is necessary to create a branch off of `NVIDIA/NeMo` and not a fork, so that the docs are built immediately on push.
	- If you use a fork, that will also work but you will need to wait till the next day for changes to show up.
- First, switch to the `gh-pages-src` branch
- Create a new branch using `gh-pages-src` as the base, call it something else.
- Make changes to this branch and push commits.
- Open Pull Request - **Make sure that **base** is `gh-pages-src` and **compare** is `<your branch name>`.
- Assign to reviewer and update PR with comments
- Merge PR. Changes should show up in the website in a few minutes after Github Actions builds the page.
- **Note**: If you submitted a PR using a branch from **NVIDIA/NeMo**, the docs should automatically build and overwrite the original docs.
   - This is ok - **in case you close the PR, the branch reverts to original version and the correct docs are shown again.**
- If you submitted using a `fork/branch`, then the PR will not auto build (though the checks will “pass”). 
  - This is because forked PR have Github Action workers which do not have write permission to the Github repo (even if author is part of the repo with write permissions. So the changes will show up only after merging the PR.



# Building the Docs (Docker)

- Simply call `bash build_docs.sh` to build your docs using Docker and then open `site/index.html` in your browser.
- If you want to serve the pages insead, `docker run --rm -it -p 8000:8000 -v ${PWD}:/docs squidfunk/mkdocs-material`
- To deploy the website - you should commit and push the changes to the new branch and let the Github Action handle it.


# Building the Docs (Local)

- Install requirements : `pip install mkdocs-material` 
- To serve the website locally (See changed automatically updated) - `mkdocs serve`
- To build the website locally - `mkdocs build` and then open `site/index.html` in your browser.
- To deploy the website - you should commit and push the changes to the new branch and let the Github Action handle it.

# Steps to create a post

1) Create a new branch from the gh-pages-src branch on NeMo. Note that you should not use a fork/branch to do this, for the changes to show 
up during the PR it must be a branch directly from NVIDIA/NeMo.
    - If you prefer to not showcase the post until merge, then and only then consider using the fork/branch method.

2) Open the directory docs/blogs/ folder. Here you will find template.md - copy the contents of this template file.

3) Go inside the required subfolder - it is organized by year 

4) Create a new file with the following format - YYYY-MM-{title with dashes}.md and paste all of the contents of template.md into it.

5) At the top of this file, there is a header section marked by  --- : Update the following:
    - title: The title required. Try to make it fit in one line.
    - author: List of author full names separated by commas, enclosed inside [ ]
    - author_gh_user: List of author Github ids separated by commas, enclosed inside [ ]
    - read_time: An approximate read time for your post, write as a string
    - publish_date: String date on which the post will be merged into NeMo. Do NOT update this date after it has been published, unless absolutely required. 
    - This date should be in expanded notation: 7th August, 2022

6) Read and follow the instructions written in the “Notes” section of the template:
    - These are the steps you will take to link the markdown file to the actual website in the final step. 
    - These steps will be noted below, so just note them and delete the template text 

7) Write down post content
    - This is extended markdown - all ordinary markdown rules apply.
    - Extensions are listed below :
    - References of Material for MkDocs has a great section about all the extension - https://squidfunk.github.io/mkdocs-material/reference/ 
    - These have mostly been enabled already, you can directly use them.
    - Very useful extensions:
    - MathJax - https://squidfunk.github.io/mkdocs-material/reference/mathjax/
    - Admonishments - https://squidfunk.github.io/mkdocs-material/reference/admonitions/#supported-types 
    - Buttons (for end of post, call to action etc) - https://squidfunk.github.io/mkdocs-material/reference/buttons/ 
    - Code blocks - https://squidfunk.github.io/mkdocs-material/reference/code-blocks/
    - Diagrams (via Mermaid.js) - https://squidfunk.github.io/mkdocs-material/reference/diagrams/
    - Footnotes - https://squidfunk.github.io/mkdocs-material/reference/footnotes/#footnotes
    - Expanded Text formatting - https://squidfunk.github.io/mkdocs-material/reference/formatting/
    - Images [**READ NOTE ABOUT IMAGES BELOW**](#note-about-images-) - https://squidfunk.github.io/mkdocs-material/reference/images/#image-alignment

# Steps to update the Index Page

1) Create a new branch from the gh-pages-src branch on NeMo. Note that you should not use a fork/branch to do this, for the changes to show 
up during the PR it must be a branch directly from NVIDIA/NeMo.
    - If you prefer to not showcase the post until merge, then and only then consider using the fork/branch method.

2) Open the directory docs/overrides/ folder. Here you will find `home.html`.

3) The Index Page is a custom website build without mkdocs. It is a simple HTML file with some custom CSS and JS. 
    - The CSS and JS are in the assets/stylesheets and assets/javascript folder respectively.
    - There is some inline CSS and JS in the `home.html` file itself.
    - The CSS and JS are not minified, so you can easily read and understand what is happening.
    - The HTML file is also not minified, so you can easily read and understand what is happening.

4) The `home.html` file is divided into a few sections - 
   - `Hero Banner`: Contains the text and buttons at the very top of the page.
   - `What is NeMo?`: Contains the text regarding what NeMo is.
   - `LLM with NeMo`: Contains the text regarding NeMo Megatron.
   - `OSS Community`: Contains the text regarding the NeMo integration with PTL and Hydra.
   - `RIVA`: Contains the text regarding NVIDIA RIVA.

# Publishing content

Once your post content is ready (blogpost, website), you can now begin to publish it on Github Pages.

1) Go to the `mkdocs.yml` file and scroll to the very bottom to the section called `Page Tree`.
2) There will be a nested list of articles.

```yaml

nav:
    - Home: index.md
    - 2023:
        - blogs/2023/2023-02-NeMo-101.md
        - blogs/2023/2023-01-NeMo-101.md
    - 2022:
        - blogs/2022/2022-02-NeMo-101.md
        - ...

```

3) First, select the **year** which matches your post.
4) Then, copy the relative path (inside of docs) that corresponds to your post.
      - Usually of the format `blogs/{YYYY}/{YYYY}-{MM}-{Title}.md`
5) Now, **we need to sort the post such that the first post is always the latest**.
6) IE: First item after Home should be {Current Year} and inside it should be the **latest blog**. **All other blogs should be pushed down the list**.
7) Build your docs locally to make sure it looks correct.
   - Make sure you have docker installed, then call `bash build_docs.sh`.
   - It should build most of the documentation for your page and then you can open the **site folder**.
   - Inside **site** folder, open the **index.md** file.
   - Then browse to your actual blogpost. **Note: You may need to reclick the page url if it doesnt auto show up.**
8) Submit your changes to the `gh-pages-src` branch following [instructions above](#workflow)



# Note about Images:
	
	Please DO NOT PUSH images or any type of non-text media to this folder via git push.

All media used in the post must be published elsewhere and then simple URL linked. A simple way to do this is to visit the 
current Released NeMo version (https://github.com/NVIDIA/NeMo/releases) and then Click on Edit release.

- In the release page, it shows sections **Attach binaries by dropping them here** towards the end. Add images here, then click **Update Release**.
- You can upload assets to this page for your blog post.
    - File name format : asset-post-{post-name}-{file-name}.{filetype}
- Click update release when done.
- Right click on the Asset and select “Copy Link Address” and use in your post.

# Note about additional Requirments:

- If you need to add any additional requirements, please add them to the `requirements.txt` file in `requirements` directory.
---
layout: links
# multilingual page pair id, this must pair with translations of this page. (This name must be unique)
lng_pair: id_links

# publish date (used for seo)
# if not specified, site.time will be used.
#date: 2022-03-03 12:32:00 +0000

# for override items in _data/lang/[language].yml
#title: My title
#button_name: "My button"
# for override side_and_top_nav_buttons in _data/conf/main.yml
#icon: "fa fa-bath"

# seo
# if not specified, date will be used.
#meta_modify_date: 2022-03-03 12:32:00 +0000
# check the meta_common_description in _data/owner/[language].yml
#meta_description: ""

# optional
# please use the "image_viewer_on" below to enable image viewer for individual pages or posts (_posts/ or [language]/_posts folders).
# image viewer can be enabled or disabled for all posts using the "image_viewer_posts: true" setting in _data/conf/main.yml.
#image_viewer_on: true
# please use the "image_lazy_loader_on" below to enable image lazy loader for individual pages or posts (_posts/ or [language]/_posts folders).
# image lazy loader can be enabled or disabled for all posts using the "image_lazy_loader_posts: true" setting in _data/conf/main.yml.
#image_lazy_loader_on: true
# exclude from on site search
#on_site_search_exclude: true
# exclude from search engines
#search_engine_exclude: true
# to disable this page, simply set published: false or delete this file
#published: false


# you can always move this content to _data/content/ folder
# just create new file at _data/content/links/[language].yml and move content below.
###########################################################
#                Links Page Data
###########################################################
page_data:
  main:
    header: "Links"
    info: "Your Links page description."

  # To change order of the Categories, simply change order. (you don't need to change list order.)
  category:
    - title: "JekyII / Liquid"
      type: id_jekyiiliquid
      color: "#141E26"
    - title: "Web Design"
      type: id_webdesign
      color: "#014040"
    - title: "Programming"
      type: id_programming
      color: "#59D9D9"
    - title: "Machine Learning"
      type: id_machinelearning
      color: "#25D9C7"
    - title: "Data Science"
      type: id_datascience
      color: "#79F297"

  list:
    -
    # programming
    - type: id_programming
      title: "Stack OverFlow"
      url: "https://stackoverflow.com/"
      info: "Stack Overflow is a question and answer website for professional and enthusiastic programmers."

    # jekyiiliquid
    - type: id_jekyiiliquid
      title: "Jekyll"
      url: "https://jekyllrb.com/"
      info: "Transform your plain text into static websites and blogs."
    - type: id_jekyiiliquid
      title: "Jekyll Cheat Sheet"
      url: "https://cloudcannon.com/community/jekyll-cheat-sheet/"
      info: "There are so many Jekyll variables and filters to remember and it can be tricky to keep it all in your head. This cheat sheet serves as a quick reference of everything Jekyll can do."
    - type: id_jekyiiliquid
      title: "Liquid for Designers"
      url: "https://github.com/Shopify/liquid/wiki/Liquid-for-Designers"
      info: "Liquid for Designers wiki on GitHub."
    - type: id_jekyiiliquid
      title: "Liquid for Programmers"
      url: "https://github.com/Shopify/liquid/wiki/Liquid-for-Programmers"
      info: "Liquid for Programmers wiki on GitHub."
    - type: id_jekyiiliquid
      title: "Liquid Reference"
      url: "https://shopify.dev/api/liquid/"
      info: "Liquid is a template language created by Shopify and written in Ruby. It is now available as an open source project on GitHub."

    # webdesign
    - type: id_webdesign
      title: "W3Schools"
      url: "https://www.w3schools.com/"
      info: "W3Schools offers free online tutorials, references and exercises in all the major languages of the web. Covering popular subjects like HTML, CSS, JavaScript, Python, SQL, Java, and many more."

    # machinelearning
    - type: id_machinelearning
      title: "Hugging Face"
      url: "https://huggingface.co/"
      info: "A platform for sharing and using pre-trained machine learning models."

    # datascience
    - type: id_datascience
      title: "Kaggle"
      url: "https://www.kaggle.com/"
      info: "An online community for data scientists and machine learning practitioners."
    - type: id_datascience
      title: "UCI Machine Learning Repository"
      url: "https://archive.ics.uci.edu/"
      info: "A free and public repository of machine learning datasets."
    - type: id_datascience
      title: "Papers With Code"
      url: "https://paperswithcode.com/"
      info: "A free platform for researchers and practitioners to find and track the latest papers in Machine Learning, Deep Learning, and AI, along with their code and datasets."

---

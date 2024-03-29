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
    info: "É um prazer compartilhar com você uma lista de links que podem ajudá-lo(a) a aprimorar suas habilidades em machine learning, análise de dados e estatística. Sinta-se à vontade para explorar os recursos disponíveis e escolher aqueles que melhor se adequam às suas necessidades."

  # To change order of the Categories, simply change order. (you don't need to change list order.)
  category:
    - title: "JekyII / Liquid"
      type: id_jekyiiliquid
      color: "#141E26"
    - title: "Web Design"
      type: id_webdesign
      color: "#014040"
    - title: "Programação"
      type: id_programming
      color: "#59D9D9"
    - title: "Aprendizado de Máquina"
      type: id_machinelearning
      color: "#25D9C7"
    - title: "Ciência de Dados"
      type: id_datascience
      color: "#79F297"
    - title: "Educação"
      type: id_education
      color: "#595452"

  list:
    -
    # programming
    - type: id_programming
      title: "Stack OverFlow"
      url: "https://stackoverflow.com/"
      info: "Stack Overflow é um site voltado para perguntas e respostas para professionais e entusiastas da programação."

    # jekyiiliquid
    - type: id_jekyiiliquid
      title: "Jekyll"
      url: "https://jekyllrb.com/"
      info: "Transforme o seu simples texto em websites estáticos e blogs."
    - type: id_jekyiiliquid
      title: "Tabela rápida para Jekyll"
      url: "https://cloudcannon.com/community/jekyll-cheat-sheet/"
      info: "Existem tantos filtros e variáveis em Jekyll para se lembrar que pode ser difícil lembrar de todos. Essa tabela serve como uma rápida referência para tudo que Jekyll é capaz de fazer."
    - type: id_jekyiiliquid
      title: "Liquid para Designers"
      url: "https://github.com/Shopify/liquid/wiki/Liquid-for-Designers"
      info: "Wiki do Liquid para Designers no GitHub"
    - type: id_jekyiiliquid
      title: "Liquid para Programadores"
      url: "https://github.com/Shopify/liquid/wiki/Liquid-for-Programmers"
      info: "Wiki de Liquid para Programadores no GitHub"
    - type: id_jekyiiliquid
      title: "Referências de Liquid"
      url: "https://shopify.dev/api/liquid/"
      info: "Liquid é uma linguagem modelo create pelo Shopify e escrita em Ruby. Ela, agora, está disponível em código aberto no GitHub."

    # webdesign
    - type: id_webdesign
      title: "W3Schools"
      url: "https://www.w3schools.com/"
      info: "W3Schools oferece tutoriais gratuitos, referências e exercícios nas linguagens mais importantes da web, cobrindo a mais populares como HTML, CSS, JavaScript, Python, SQL, Java e mutio mais."

    # machinelearning
    - type: id_machinelearning
      title: "Hugging Face"
      url: "https://huggingface.co/"
      info: "A Hugging Face é uma plataforma fácil de usar para o desenvolvimento de aplicações de aprendizado de máquina. É uma ótima opção para desenvolvedores, pesquisadores e estudantes que desejam acessar os últimos modelos de aprendizado de máquina de forma rápida e eficiente."

    # datascience
    - type: id_datascience
      title: "Kaggle"
      url: "https://www.kaggle.com/"
      info: "Kaggle é uma plataforma completa para Cientistas de Dados. É uma oportunidade para aprender, colaborar e compartilhar conhecimento com outros Cientistas de Dados de todo o mundo."
    - type: id_datascience
      title: "Repositório da UCI"
      url: "https://archive.ics.uci.edu/"
      info: "O Repositório da UCI é um recurso fundamental para Cientistas de Dados, pois oferece uma ampla variedade de conjuntos de dados para diversos fins."
    - type: id_datascience
      title: "Papers With Code"
      url: "https://paperswithcode.com/"
      info: "O Papers With Code é uma ferramenta para pesquisadores, profissionais e estudantes de Machine Learning. É uma ótima maneira de encontrar soluções para seus problemas, aprender sobre novos métodos e técnicas e se manter atualizado com as últimas pesquisas na área."
    - type: id_datascience
      title: "KNIME"
      url: "https://www.knime.com/"
      info: "KNIME (Konstanz Information Miner) é uma plataforma completa e flexível para análise de dados, integração e geração de relatórios. A plataforma oferece uma interface intuitiva e fácil de usar, tornando-a acessível a usuários de todos os níveis de experiência."

    # education
    - type: id_education
      title: "Machine Learning Mastery"
      url: "https://machinelearningmastery.com/"
      info: "Machine Learning Mastery é um recurso completo para aprender sobre Machine Learning. É uma plataforma fácil de usar e oferece uma variedade de conteúdos para atender às suas necessidades."
    - type: id_education
      title: "W3Schools"
      url: "https://www.w3schools.com/"
      info: "W3Schools oferece tutoriais gratuitos, referências e exercícios nas linguagens mais importantes da web, cobrindo a mais populares como HTML, CSS, JavaScript, Python, SQL, Java e muito mais."

---

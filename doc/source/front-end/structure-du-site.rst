=========================
Structure globale du site
=========================

Le site est composé de plusieurs grandes parties.

Tous ces éléments sont dans le fichier ``templates/base.html``. Pour les utiliser dans les gabarits, il suffit
d'étendre ``base.html``, avec cette ligne :

.. sourcecode:: html

   {% extends "base.html" %}

Quand il s'agit d'un contenu comme un tutoriel ou un article, il faut étendre ``templates/base_content_page.html`` :

.. sourcecode:: html

   {% extends "base_content_page.html" %}

Il se peut aussi que les modules aient leur propre gabarit de base, auquel cas c'est ce dernier qu'il faut utiliser :

.. sourcecode:: html

   {% extends "module/base.html" %}

.. seealso::
   Pour en savoir plus, n'hésitez pas à la partie de la documentation de Django sur
   `l'héritage des gabarits <https://docs.djangoproject.com/fr/1.7/topics/templates/#template-inheritance>`_ !


L'en-tête
=========

.. figure:: ../images/design/en-tete.png
   :align: center

On peut découper l'en-tête du site en quatre.

Le logo
-------

Le logo est simplement un lien qui a pour contenu une image. L'image change en fonction de la taille de l'écran.

Le menu
-------

Le menu est composé soit d'un lien, soit d'un menu déroulant. Ces derniers contiennent des listes de liens.

.. figure:: ../images/design/en-tete_menu.png
   :align: center

La *logbox*
-----------

Pour un utilisateur connecté
~~~~~~~~~~~~~~~~~~~~~~~~~~~~

Pour un utilisateur connecté, la *logbox* contient trois menus déroulants :

- le premier affiche les messages privés ;
- le deuxième affiche les notifications ;
- et le dernier contient des liens vers les zones réservées à l'utilisateur.

Pour le Staff (les membres avec les droits de modération), il existe un autre menu pour les alertes de modération.

.. figure:: ../images/design/en-tete_logbox.png
   :align: center

Pour un anonyme
~~~~~~~~~~~~~~~

Pour un anonyme, il y a simplement deux boutons :

- un pour se connecter ;
- un pour s'inscrire.

Le sous en-tête
---------------

Le sous-entête est en deux partie :

- à gauche, une aide à la navigation ;
- à droite, un petit formulaire de recherche.

L'aide à la navigation
~~~~~~~~~~~~~~~~~~~~~~

Pour renseigner l'aide à la navigation, il faut utiliser le bloc ``{% block breadcrumb %}`` :

.. sourcecode:: html

   {% block breadcrumb %}<li>Le titre de ma page</li>{% endblock %}

Quand il s'agit d'un gabarit de base, comme ``templates/module/base.html``, il faut utiliser
``{% block breadcrumb_base %}`` :

.. sourcecode:: html

   {% block breadcrumb_base %}
       <li>
           <a href="{% url mon-module %}">
               Le titre de mon module
           </a>
       </li>
   {% endblock %}


Le contenu principal
====================

Le contenu principal change radicalement suivant les pages : tutoriels, articles, messages du forum, messages privés...
Il y a donc différentes façons de l'afficher suivant le contenu ! Dans les gabarits, le résultat est donc l'existence
de plusieurs blocs différents.

Du contenu accompagné de titre et sous-titre
--------------------------------------------

Il faut mettre le contenu entre ``{% block content %}`` et ``{% endblock %}``, comme ceci :

.. sourcecode:: html

   {% extends "base.html" %}

   {% block content %}
       <div>
           Super contenu !
       </div>
   {% endblock %}

Vous avez deux autres blocs, le premier pour le titre et le deuxième pour le sous-titre :

.. sourcecode:: html

   {% extends "base.html" %}

   {% block headline %}
       Mon super titre !
   {% endblock %}

   {% block headline_sub %}
       Avec mon tout aussi superbe sous-titre...
   {% endblock %}

   {% block content %}
       <div>
           Toujours avec mon super contenu !
       </div>
   {% endblock %}

Du contenu sans titre ni sous-titre
-----------------------------------

Dans ce cas là, il faut simplement utiliser ``{% block content_out %}`` :

.. sourcecode:: html

   {% extends "base.html" %}

   {% block content_out %}
       <div>
           Super contenu sans titre ni sous-titre !
       </div>
   {% endblock %}

Du contenu comme un tutoriel ou un article
------------------------------------------

Dans ce cas là, il faut utiliser ``{% block content %}`` mais avec ``{% extends "base_content_page.html" %}`` :

.. sourcecode:: html

   {% extends "base_content_page.html" %}

   {% block headline %}
       C'est un article !
   {% endblock %}

   {% block headline_sub %}
       A moins que ce soit un tutoriel...
   {% endblock %}

   {% block content %}
       <p>
           Bref, il y a encore et encore du contenu !
       </p>
   {% endblock %}

Il peut arriver de devoir afficher d'autres types de contenus, comme des commentaires, en bas de la page. Dans ce
cas là, on peut utiliser ``{% block content_after %}`` en plus :

.. sourcecode:: html

   {% extends "base_content_page.html" %}

   {% block content %}
       <p>
           Bref, il y a encore, encore et encore du contenu !
       </p>
   {% endblock %}

   {% block content_after %}
       <p>
           Et là, paf, un autre type de contenu.
       </p>
   {% endblock %}

Il existe une alternative à ``{% block content_after %}`` qui s'utilise de la même manière : ``{% block content_ext %}``.
La différence est minime, pour plus d'informations, regardez le fichier ``templates/base_content_page.html``.


La barre latérale
=================

La barre latérale contient des listes de liens, boutons ou formulaires permettant à l'utilisateur d'effectuer des actions.

.. figure:: ../images/design/barre-laterale.png
   :align: center

   Barre latérale de la page d'un profil

Pour la barre latérale, il faut étendre ``templates/base.html`` et utiliser ``{% block sidebar %}`` :

.. sourcecode:: html

   {% extends "base.html" %}

   {% block sidebar %}
       Ma superbe barre latérale !
   {% endblock %}

Pour les pages comme un tutoriel ou un article
----------------------------------------------

Il faut étendre ``templates/base_content_page.html`` et utiliser ``{% block sidebar_actions %}`` :

.. sourcecode:: html

   {% extends "base_content_page.html" %}

   {% block sidebar_actions %}
       La superbe barre latérale des articles ou tutoriels !
   {% endblock %}

Tout comme les modules peuvent avoir leur propre gabarit de base, il peuvent avoir leur propre bloc pour la barre latérale :

.. sourcecode:: html

   {% extends "module/base.html" %}

   {% block nom_du_bloc %}
       La superbe barre latérale de mon module !
   {% endblock %}

Pour trouver le nom de ce bloc, regardez ``templates/module/base.html`` !


Le bas de page
==============

Le bas de page est sûrement la partie la plus simple du site. Il contient trois *flexboxs* [#flexbox]_ :

- celui de gauche affiche le nom du site ;
- celui du milieu contient les liens vers les comptes des réseaux sociaux du site ;
- celui de droite contient des liens vers les pages annexes du site, tel que les CGUs par exemple.

.. figure:: ../images/design/bas-de-page.png
   :align: center

.. [#flexbox] modèle de boîte flexible en HTML/CSS (`voir le tutoriel sur Alsacréations <http://www.alsacreations.com/tuto/lire/1493-css3-flexbox-layout-module.html>`_)


Le menu pour mobile
===================

Le menu pour mobile est généré à partir de l'en-tête et de la barre latérale grâce à `ce code Javascript <https://github.com/zestedesavoir/zds-site/blob/dev/assets/js/mobile-menu.js>`_.

.. figure:: ../images/design/menu-mobile.png
   :align: center

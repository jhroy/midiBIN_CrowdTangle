# midiBIN_CrowdTangle (18 février 2021)

### Avant -> GraphAPI

- [GraphAPI](https://developers.facebook.com/tools/explorer/) (existe toujours, mais est plus difficile d'accès)

- Des chercheurs ont prévenu qu'il y avait des problèmes pour la vie privée

<img src="images/bodle.png" width=80%>

- [Bodle, R. (2011). Regimes of Sharing. Information, Communication & Society, 14(3), 320‑337](https://doi.org/10.1080/1369118X.2010.542825).

<img src="images/symeonidis.png" width=80%>

- [Symeonidis, I., Tsormpatzoudi, P. et Preneel, B. (2015). Collateral damage of Facebook Apps: an enhanced privacy scoring model. Dans IACR Cryptology EPrint Archive](https://eprint.iacr.org/2015/456.pdf).

- Résultat: le «meltdown» [**Cambridge Analytica**](https://www.economist.com/leaders/2018/03/22/facebook-faces-a-reputational-meltdown)

<img src="https://www.economist.com/img/b/1280/720/90/sites/default/files/images/print-edition/20180324_LDD001_0.jpg" width=25%>

### CrowdTangle

- Créé en 2015 ([brevet](https://patents.google.com/patent/US20150169587A1/en?inventor=Brandon+Ashley+Silverman&assignee=Openpage+Labs+Inc.+d%2fb%2fa+CrowdTangle))

- Acquis par Facebook en 2016.

- Accessible à chercheuses.eurs dans le cadre du partenariat [**Social Science One** de Harvard](https://socialscience.one/partnerships)

- Prend trois formes:

  1. [**Extension chrome**](https://chrome.google.com/webstore/detail/crowdtangle-link-checker/klakndphagmmfkpelfkgjbkimjihpmkh)

      - exemple avec [texte d'opinion dans *Le Devoir*](https://www.ledevoir.com/opinion/idees/594650/qu-est-ce-qu-une-universite)
  
  2. [**Tableau de bord**](https://www.crowdtangle.com/) (ou *dashboard*)

      - Se créer des listes et les *manage*. Exemple avec [Valnet](https://www.valnetinc.com/)
        - [Pages Facebook](fichiers/listePages.csv)
        - [Comptes Instagram](fichiers/comptesInsta.csv)
      
      - Explorer ces listes. Exemple avec IIJ:
        - [First Nations Pages](https://apps.crowdtangle.com/iij/lists/1347336)
        - [First Nations Groups](https://apps.crowdtangle.com/iij/lists/1388572)
      
      - *Historical search*

      - [*Search*](https://apps.crowdtangle.com/search/results)
        - Avec autres réseaux: Insta, Reddit, Twitter.
        - Changements dans les fonctionnalités:
          - En 2020, retournait **30 000** résultats
          - En janvier 2021, ne retournait plus que **10 000** résultats
          - Depuis quelques jours, retourne **300 000** résultats
     
        - Copier l'URL d'une requête si vous en faites plusieurs.

        - Exemple: tout le FB belge en janvier 2020 (pour avoir février, il suffirait de changer les champs `customStartDate`, `customEndDate`, `customChartStartDate` et `customChartEndDate`):

          [`https://apps.crowdtangle.com/search/results?includedCountries=BE&customStartDate=2020-01-01T00:00:00&customEndDate=2020-02-01T00:00:00&customChartStartDate=2020-01-01T00:00:00&customChartEndDate=2020-02-01T00:00:00&platform=facebook&postTypes=&producerTypes=3&q=&sortBy=score&sortOrder=desc&timeframe=custom`](https://apps.crowdtangle.com/search/results?includedCountries=BE&customStartDate=2020-01-01T00:00:00&customEndDate=2020-02-01T00:00:00&customChartStartDate=2020-01-01T00:00:00&customChartEndDate=2020-02-01T00:00:00&platform=facebook&postTypes=&producerTypes=3&q=&sortBy=score&sortOrder=desc&timeframe=custom)
          
        - Ça peut être long...
          
        <img src="images/oops.png" alt="Faut être patient en titi" width=80%>

  
  3. [API](https://github.com/CrowdTangle/API/wiki)

      - Pour une utilisation programmatique. Avec la fonction *search*, ci-dessus, qui permet de retourner 300 000 posts, ce n'est plus guère nécessaire, sinon pour mettre sur pied une veille ou pour aller chercher davantage de posts...

      - Nécessite l'utilisation d'un **jeton** (*API token*). Chaque jeton est associé à un tableau de bord.

        - Limites:

          - 100 posts à la fois (pallier avec la pagination)
          - 6 appels à la minute seulement! (mettre des *sleep* de 10 secondes dans vos scripts)

      - *endpoint* `posts`

        Permet d'aller chercher tous les posts d'une liste donnée, préalablement préparée dans un tableau de bord.
        
        Exemple avec l'une des listes vues plus haut, **First nations pages** qui regroupe plus de 800 pages. Son numéro est *1347336*. Avec le jeton, ce numéro permet de construire un appel à l'API de CrowdTangle. Voici un exemple. [Cette liste de posts mentionnant la Covid-19 dans les pages ou groupes Facebook autochtones](http://bit.ly/covidIIJ) est peuplée automatiquement par des appels simples comme celui-ci (pour les pages; un autre appel doit être fait pour les groupes):
        
        `https://api.crowdtangle.com/posts?token={votreToken}&listIds=1347336&count=100`

      - *endpoint* `posts/search`
       
        Accessible **sur demande seulement**! Pour des recherches sur l'ensemble du *contenu indexé par CrowdTangle* (nuance importante, plus bas), ou pour des recherches par pays.
        
        Nécessite l'utilisation d'une expression à rechercher. Pour maximiser la collecte, on peut s'inspirer de regex et faire une recherche pour «[a-b][0-9]».
        
        Exemple. Ici, on demande à CT de retourner des posts de pages administrées au Canada, publiés le 1er juillet 2020, des les ordonner par nombre d'interactions et de nous afficher du 3300e au 3399e post. 
        
        `https://api.crowdtangle.com/posts/search?token={votreToken}&startDate=2020-07-01&endDate=2020-07-02&sortBy=total_interactions&platforms=facebook&accountTypes=facebook_page&pageAdminTopCountry=CA&searchTerm=a,b,c,d,e,f,g,h,i,j,k,l,m,n,o,p,q,r,s,t,u,v,w,x,y,z,1,2,3,4,5,6,7,8,9,0&count=100&offset=3300`
          
          
### Autres limites

### Autres ressources

- [Academic FAQ](https://help.crowdtangle.com/en/articles/3323105-academics-researchers-faq)
- [Naomi Shiffman](nshiffman@fb.com), *Academic Lead*

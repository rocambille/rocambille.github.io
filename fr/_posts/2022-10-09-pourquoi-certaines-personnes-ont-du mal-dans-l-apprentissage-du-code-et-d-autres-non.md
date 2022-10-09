---
ref: 2022-10-09-pourquoi-certaines-personnes-ont-du mal-dans-l-apprentissage-du-code-et-d-autres-non
lang: fr
title: "Pourquoi certaines personnes ont du mal dans l'apprentissage du code et d'autres non"
tags: programming beginners
authors:
  - orwa-diraneyya
translators:
  - romain-guillemot
---

> **Avant-propos**
>
> Beinvenue à vous qui comemcnez la letcure de ce tetxe.<!--more-->
> Très probablement, avec plus ou moins de facilité, vous avez déchiffré cette première phrase.
> Peut-être n'avez-vous même pas remarqué qu'elle contenait des fautes de frappe sur 4 de ses mots.
> Et même si vous l'avez remarqué, votre cerveau a fait l'effort de les corriger pour rendre la lecture possible.
>
> Dans le cadre de mon travail, je suis régulièrement amené à parler anglais.
> Mes collègues bienveillants ne me l'ont jamais avoué, mais je ne suis pas dupe : j'ai un accent typiquement français, et mon vocabulaire comme ma grammaire sont très approximatives.
> Mais lorsque nous échangeons, ils font l'effort de comprendre mon anglais imparfait.
> Discuter avec un être humain est un jeu coopératif : nous gagnons ensemble en nous comprenant, et pour nous comprendre nous corrigeons en direct les messages que nous recevons pour leur donner un sens, pour faire avancer la conversation.
>
> Sans les accuser de malveillance, les machines ne jouent pas ce jeu.
> Elles ne font pas l'effort de comprendre le code écrit par les développeurs et les développeuses.
> C'est donc à nous de faire l'effort de leur présenter un code "parfait".
> Tout du moins sans faute de syntaxe, de grammaire, de vocabulaire.
> S'efforcer de construire un message en étant ainsi rigoureux pour 2 peut difficilement être qualifié de "naturel".
> Ce n'est généralement pas ainsi que nous apprenons à communiquer.
> Imaginez si des parents refusaient de répondre aux besoins de leurs enfants tant qu'ils ne sont pas exprimés dans un français irréprochable...
>
> Pour communiquer efficacement avec une machine (façon littéraire de dire "coder"), il est important de comprendre cette relation non-coopérative dans la communication.
> Une machine ne fera jamais l'effort de comprendre ce qu'on lui dit.
> Intégrer cette froide vérité est une étape cruciale pour des élèves dans l'apprentissage du code.
> J'en suis convaincu depuis longtemps.
> Mais avec le recul, je me rends compte que je n'ai jamais pris le temps de partager cette conviction.
> Jusqu'à une discussion avec Orwa.
> En parlant de nos élèves respectifs et de leurs difficultés, j'ai naturellement évoqué ce décalage entre la communication humaine et le codage.
> Quelques semaines plus tard, il me confia sur une terrasse que mon point de vue, associé à ses propres recherches, l'avait beaucoup aider à réfléchir.
> Un peu plus tard encore, il m'a partagé l'article dont vous trouverez ici une traduction.
>
> J'ai d'abord été très touché qu'Orwa ait trouvé quelque chose d'intéressant dans nos échanges.
> J'en ai été remercié par son texte, qui m'a aidé à mettre des mots sur des idées jusque là approximatives.
> Si vous êtes à l'aise avec l'anglais, je ne peux que vous inviter à lire [la version originale d'Orwa](https://medium.com/@orwa.diraneyya/why-some-people-struggle-in-a-technology-bootcamp-and-others-dont-a3a8d1c83683).
> Pour les personnes moins à l'aise, j'ai souhaité proposer une version en français, afin de vous partager ses réflexions sur l'apprentissage du code.
> Elles sont déjà pour moi une nouvelle source d'idées à tester avec mes élèves, et je remercie Orwa pour ça.
>
> R. G.

![image générée à l'aide du modèle de génération d'images de l'IA "Stable Diffusion"](/assets/2022-10-09-pourquoi-certaines-personnes-ont-du mal-dans-l-apprentissage-du-code-et-d-autres-non-cover-min.png){: width="768"}{: height="448"}

Après quelques mois d'enseignement dans un "bootcamp" en tant que formateur Fullstack (ou Développement Web), je pense que je commence à comprendre pourquoi certains d'entre nous ont du mal (_vraiment_ du mal !) à apprendre à écrire et à comprendre le code informatique.

Cet article est le résultat de ma réflexion personnelle, de conversations avec des collègues/élèves et des lectures que j'ai faites pendant mon temps libre (principalement le livre intitulé _"The Language Game"_ de _Morten Christiansen_ et _Nick Chater_).

Certes, cet article est écrit avec un fort parti pris d'enseignant. Pourtant, j'espère que l'empathie derrière cela et l'intention d'améliorer les choses en offrant une perspective significative finiront par apporter de la valeur au lecteur.

À savoir, j'espère que si vous enseignez, étudiez, postulez ou envisagez de postuler à un bootcamp technologique, ou même à un cours de codage en ligne, que cet article vous apportera un éclairage, ou du moins vous donnera quelque chose d'intéressant à considérer.

## Pédagogie différenciée en classe

Si vous avez déjà enseigné dans une école ou ailleurs, vous devez avoir entendu parler de la question de la _pédagogie différenciée_. À savoir le défi de garder tout le monde engagé et intéressé pendant les heures de classe. C'est une chose difficile à faire, surtout lorsque les élèves sont des adultes, d'âges divers (un grand écart de 18 à 45 ans n'est pas rare), et venant de tous les horizons : certains avec et d'autres sans bagage universitaire.

Ce niveau de diversité dans un groupe, combiné au degré d'implication souvent requis par les matières enseignées dans un bootcamp technologique, est la difficulté majeure à laquelle les enseignants dans un bootcamp sont confrontés au quotidien.

Mon avis est qu'**au cœur du problème de différenciation** dans un bootcamp technologique se trouvent les nombreuses façons dont **certains étudiants peuvent acquérir un avantage inné sur les autres, dans le domaine de l'apprentissage d'un langage logique.**

> Oh, attendez! Avantage inné ? Langage logique ? Qu'est-ce que tout cela signifie!

Voyons ça ensemble...

## Langages logiques

Les langages de programmation sont parfois décrits comme des _langages logiques_, pour les distinguer des langages que nous, humains, utilisons pour communiquer au quotidien, qui sont à leur tour appelés _langages naturels_.

Par conséquent, apprendre à coder, c'est essentiellement _essayer d'apprendre un langage logique_. Notez que les difficultés à faire cela viennent _naturellement_ (vous avez le jeu de mot 😛?) pour les personnes qui le font pour la première fois. En d'autres termes, celles qui n'ont utilisé que des langages naturels dans leur vie.

Il est donc _logique_ (désolé 😆), de relier une grande partie des difficultés rencontrées par ces "apprenants naturels" dans un bootcamp technologique, à la _transition à effectuer_ entre les langages naturels et les langages logiques.

> Alors, qu'est-ce qu'il y a de si particulier à propos des langages logiques ? et pourquoi l'utilisation d'un langage logique est-elle si différente de l'utilisation d'un langage naturel ?

Dans un langage logique comme JavaScript, différents symboles et séquences de caractères, tels que le _crochet ouvert_ `[` ou le _double point d'interrogation_ `??`, ont une signification technique précise et indiscutable.

Par exemple, non seulement l'ajout d'un espace entre les deux points d'interrogation ci-dessus impliquerait un sens complètement différent, mais cela "casserait" également le message logique, ou en termes de langage logique... Conduirait à une _erreur de syntaxe_.

Dans le monde des langages logiques, le « récepteur » du message logique ne peut pas se remettre d'une erreur de syntaxe et doit donc rejeter ou abandonner complètement le discours. Cela est dû à la raison d'être des langages logiques, qui est d'être le plus précis et le moins ambigu possible ! [[1](#footnote-1){: id="back-from-footnote-1"}]

## Langages naturels

Comparé à JavaScript, le "sens" dans un langage naturel (comme le français ou l'arabe) est constamment _en évolution_ et _négocié_ entre les parties communicantes. Ces dernières essaient toutes les deux constamment de deviner ou de prédire à quoi l'autre partie fait réellement référence dans le monde réel.

Dans ce domaine, la communication peut continuer à se produire même lorsqu'elle est effectivement rompue ou a perdu son sens. À savoir, même lorsque la prédiction des parties communicantes sur ce à quoi l'autre fait référence a divergé et est maintenant éloignée de ce que l'autre partie a l'intention de faire 😅 (quelque chose que nous avons tous vécu à un moment ou à un autre quand nous n'arrivons plus à nous comprendre avec une autre personne !).

## La situation de handicap logique

Cela m’amène à mon point principal dans cet article…

À mon avis, la plus grande situation de handicap des élèves dans un bootcamp technologique est créé par le degré variable d'aptitude des étudiants à _la production et à la consommation (ou à la compréhension)_ d'un langage logique.

Par exemple, les travailleurs sociaux, les parents au foyer, les professionnels de la communication et bien d'autres, pourraient (en raison de leurs adaptations de carrière/vie passées) avoir besoin de beaucoup plus de préparation, **_avant même de rêver de commencer à apprendre quelque chose comme JavaScript !_**

Une déclaration audacieuse, je sais... Mais attendez.

Ne vous méprenez pas. Ces personnes sont hautement qualifiées, mais leurs compétences sont conçues pour _comprendre ce que d'autres communicateurs humains, émotionnels et imprécis, essaient de leur dire_. Les parents au foyer sont doués pour discerner pourquoi leurs enfants pleurent, même en l'absence d'informations claires. Les professionnels de la communication sont des personnes expertes dans l'art de capter des indices non verbaux sur les raisons pour lesquelles des gens sont ennuyés, pourquoi ils sont intéressés, ce qu'il faudrait faire pour les faire avancer, et /ou répondre à leurs besoins inexprimés. Donc, comme vous pouvez le voir, ces personnes sont expertes du domaine de la communication naturelle ! mais comme elles ont évolué en réponse à _la nature humaine imprécise et imprévisible_, il peut être difficile pour ces personnes de se "déshumaniser" pour pouvoir s'engager dans un dialogue logique _sans contexte émotionnel_ et avec des _règles d'interprétation plutôt rigides et impartiales_.

...Cela a du sens, car cela va tout simplement à l'encontre de leur nature même et de tout ce qu'elles sont devenues.

À savoir, cette catégorie de personnes devra peut-être suivre une formation « visuelle » (et sans doute « émotionnelle ») juste pour pouvoir faire ce qui suit :

- Discerner la présence d'espaces autour des mots et des opérateurs dans le code écrit, puisque ces espaces ne jouent pas de rôle sémantique dans l'écriture des langues naturelles/humaines [[2](#footnote-2){: id="back-from-footnote-2"}].
- Discerner la différence entre des signes de ponctuation distincts dans le code informatique, lorsque ces ponctuations sont considérées comme "similaires" ou n'ont pas de signification claire/spéciale dans les systèmes d'écriture des langues naturelles/humaines [[3](#footnote-3){: id="back-from-footnote-3"}].
- Associer les glyphes d'ouverture aux glyphes de fermeture (par exemple, les crochets et autres paires de caractères symétriques) et/ou discerner la hiérarchie et la structure du discours logique lorsque ces ouvertures et fermetures sont utilisées pour indiquer des niveaux de portée ou de hiérarchie [[4](#footnote-4){: id="back-from-footnote-4"}].
- Établir une correspondance entre une _description de diagramme en utilisant un langage logique_ (par exemple, un langage comme _Mermaid_) et le diagramme lui-même (sous forme visuelle), même lorsqu'ils sont posés l'un à côté de l'autre, ou trouver une telle tâche très difficile.
- Reconnaître les blocs d'une langue logique (avec ses propres règles de grammaire) dans un autre et naviguer dans de nombreuses blocs imbriqués, ce qui, encore une fois, s'avère souvent incroyablement difficile pour cette catégorie de personnes [[5](#footnote-5){: id="back-from-footnote-5"}].
- Prédire la fonction globale d'un programme informatique, même lorsque la fonction des lignes individuelles est complètement comprise [[6](#footnote-6){: id="back-from-footnote-6"}].

(Remarque : des exemples inspirés de faits réels se trouvent dans les notes de bas de page associées aux points ci-dessous. Je recommanderais fortement de lire les notes de bas de page à la fin pour tirer le meilleur parti de cet article)

## L'avantage logique

Par rapport à la catégorie de personnes précédente, les personnes juristes, mathématiciennes et les philosophes pourraient avoir beaucoup plus de facilité à s'adapter au domaine des langages logiques.

Dans le cas des juristes, leur carrière et leurs études sont uniquement axées sur l'élimination autant que possible de l'ambiguïté du discours naturel (dans le but de protéger et de défendre les intérêts de leurs clients dans le contexte de l'interprétation partielle et vague des langues naturelles). Il n'est donc pas étonnant que les langages logiques semblent être le graal dont ces personnes rêvaient.

En effet, il est vrai qu'elles sont souvent aptes à comprendre les règles des langages logiques et ont tendance à assimiler très rapidement ses principes de fonctionnement. Je l'ai vu, maintes et maintes fois.

## Mais est-ce qu'il ne faut pas alors se former avant la formation ?

En plus de l'avantage dit inné que des élèves peuvent avoir sur les autres en raison de leur aptitude à apprendre un langage logique, un avantage supplémentaire (et plus évident) pourrait également être obtenu en ayant une exposition préalable au contenu d'une formation en développement web (avoir des notions des mots-clés, des règles de syntaxe et de la terminologie des technologies enseignées au bootcamp).

En effet, c'est ce point que la plupart d'entre nous soupçonnent d'être la raison pour laquelle certains souffrent dans une formation, tandis que d'autres non (ou du moins, c'est ce que nous avons tendance à croire).

Penser qu'une personne participant à une formation devrait déjà se "préformer" en amont pour prévenir ses difficultés est, à mon avis, une erreur.

En fait, s'il n'y avait pas les profondes difficultés d'apprentissage des langages logiques (pour les personnes qui n'ont pas le formatage pour cela), il suffirait pour apprendre à coder de s'inscrire à un cours en ligne.

Mais, comme vous le savez probablement déjà, apprendre à coder va bien plus loin que ça...

1. {: id="footnote-1"} Pensez à ce qui se passerait si le récepteur d'un langage logique était autorisé à rectifier le message logique à sa propre discrétion lorsqu'il n'a pas de sens. Alors, le sens associé au message ne serait ni précis ni univoque. Si un tel message était utilisé pour parler à ou contrôler une machine (c'est pourquoi ces langages ont été inventés), cela impliquerait que la machine se comporterait de manière imprévisible. [↑](#back-from-footnote-1)
2. {: id="footnote-2"} Par exemple, l'incapacité à faire naturellement la distinction entre la présence et l'absence d'espaces avant les virgules dans l'écriture, et l'incapacité à être cohérent avec l'utilisation ou la suppression de tels espaces dans un support numérique, est une observation courante parmi cette catégorie de personnes. De plus, étant donné que la notion de « caractère » ou de « glyphe » dans un support numérique peut parfois être moins concrète dans l'esprit de ces personnes, les élèves de cette catégorie pourraient complètement manquer qu'il existe parfois plusieurs représentations numériques pour le symbole, et que la signification sémantique est associée aux représentations, et non à la forme approximative du symbole. Ainsi, substituer une représentation à l'autre (résultant d'une erreur de frappe ou d'une opération de copier-coller) cassera certainement le code 🤯. [↑](#back-from-footnote-2)
3. {: id="footnote-3"} Par exemple, confondre les parenthèses `()` avec les crochets `[]` ou les utiliser de manière interchangeable, et être surpris d'apprendre qu'ils signifient différentes choses en JavaScript, entrent tous deux dans cette catégorie. Aussi, continuer à faire face à une difficulté à utiliser les bons symboles, même après avoir été informé de la différence entre les deux, indique (à mon avis) qu'une couche de "formation visuelle" pourrait manquer à cette catégorie d'élèves. [↑](#back-from-footnote-3)
4. {: id="footnote-4"} Par exemple, ne pas discerner la fin d'une instruction et le début d'une autre en JavaScript. De plus, face à la difficulté d'identifier l'endroit' dans le code où les instructions d'une fonction doivent aller, ou celui où la condition d'une instruction "if" doit être placée, tombent dans cette catégorie. [↑](#back-from-footnote-4)
5. {: id="footnote-5"} Par exemple, l'intégration de JSX à l'intérieur de JavaScript, et la "bascule" associée dans les règles de grammaire à cause d'une telle intégration. Cela devient encore plus difficile car la notation JSX est capable de "rebasculer" vers le contexte JavaScript sans aucun symbole, directement depuis le contexte du JSX lui-même 🤯 (permettant de nombreux niveaux d'imbrication). Ma théorie est que, puisque les langues naturelles n'offrent au mieux que deux niveaux d'imbrication (citation : "The Language Game", page 148), notre cerveau n'est pas naturellement doué pour suivre autant de niveaux d'imbrication sans entraînement spécial. Il est donc logique que les personnes issues de langues naturelles aient plus de mal à comprendre ces déclarations logiques alambiquées. [↑](#back-from-footnote-5)
6. {: id="footnote-6"} Je ne sais toujours pas pourquoi cela se produit. Je suppose que «l'apprenant naturel» a tendance à essayer de donner un sens global à toutes les phrases de son code (en une seule fois ou dans leur ensemble) plutôt que de les traiter comme des déclarations séquentielles strictement ordonnées, sans contexte émotionnel et avec des règles d'interprétation impartiales. Plus récemment, une conversation avec un de mes étudiants a révélé qu'il se pourrait, en effet, que souligner la nature non émotive des ordinateurs, via un agent sympathique (typiquement un de ses semblables humain) est une étape clé. Mettre en avant le manque d'émotion d'une machine devant l'apprenant et donc le soulager, "lui donner la permission" de ne pas tenir compte de l'aspect coopératif de la communication dans l'exercice de codage pourrait être une étape de déblocage nécessaire dans le processus d'apprentissage pour certaines personnes. Cela a pu être le cas pour cet élève en particulier, étant un expert en communication et une personnalité, présentateur radio en Ukraine. Il s'agit d'un domaine particulièrement absent de l'apprentissage à distance par rapport à l'apprentissage en présentiel sur un campus, et pourrait être l'un des rares ou des très rares domaines où l'apprentissage en présentiel pourrait être avantageux par rapport à l'apprentissage à distance dans les formations. Mais ceci, encore une fois, n'est qu'une hypothèse quant à la raison pour laquelle cette difficulté survient dans certains cas. [↑](#back-from-footnote-6)

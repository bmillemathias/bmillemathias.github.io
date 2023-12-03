---
layout: post
lang: fr
title: "Montage et decouverte du portable Framework 13' AMD Ryzen"
date: 2023-12-03 08:00:04 +0000
tags: technology
---
![](/assets/images/202312-fw-13-1.jpg)

Après 2 mois d'attente (on est encore dans la période des pré-commandes), j'ai enfin recu mon ordinateur portable Framework équipé d'un [AMD Ryzen 7840U][amd-ryzen-7840u]. Ce modèle vient juste d'arriver sur le marché, il s'agit du premier modèle sorti des usines avec un CPU AMD. Pour les specs complètes de la machine, voyez [cette page qui donne tous les détails][framework-13-amd-spec].

Visuellement il est très beau, tout en aluminium (en bonne partie recyclé) et semble très solide. Les touches du clavier de l'ordinateur - que j'utiliserai seulement en mode nomade - ont juste ce qui faut de resistance à l'appui. L'écran est dans un format 3/2 qui sort un peu du "standard" 16/9 ou 16/10 vus souvent. l'épaisseur de la machine est de 15mm ce qui reste à mon avis tres correct, vu les contraintes que necessitent de pouvoir remplacer des pièces. 

```
# MIAM MIAM
baptistemm@dsadsdhukd:~$ lscpu 
Architecture:            x86_64
  CPU op-mode(s):        32-bit, 64-bit
  Address sizes:         48 bits physical, 48 bits virtual
  Byte Order:            Little Endian
CPU(s):                  16
  On-line CPU(s) list:   0-15
Vendor ID:               AuthenticAMD
  Model name:            AMD Ryzen 7 7840U w/ Radeon 780M Graphics
...
```

### Mon utilisation

Je cherchais une machine personnelle portable (plus de grosse tour) fiable et facilement réparable, et possiblement en créant le moins de déchet possible, c'est le but des portables développés par Framework. La société ne cache pas que la [production d'ordinateurs n'est pas durable («sustainable»)][framework-not-sustainable] mais elle tente de construire des équipements pouvant être facilement mis à jour, donc être gardé plus longtemps. Elle publie sur son site sa [stratégie pour réduire la production de déchets][framework-sustainability-strategy]. Par contre pour le transport on n'évite pas l'envoi en 5 jours par avion.

Pas non plus d'exigences élevées de performances dans le modèle, je ne suis pas un geek, ni un expert du hardware et je n'ai pas fait de bench, mon avis se porte plutôt sur le montage dans cet article. Comme je n'utilise que Linux, et que je passe une partie de ma vie entre une console texte et un navigateur, pas besoin d'une énorme capacité graphique, mais juste de CPU et de mémoire pour faire tourner quelques VMs et des containers.

### Déballage

Mon modèle est un DIY («Do it yourself» dans la langue de JCVD), c'est à dire qu'il n'arrive pas entièrement monté, ca coûte moins cher (200e quand même) et on peut choisir de ne pas acheter certains composants (stockage, mémoire, alimentation) lors de la commande, si vous voulez acheter votre propre matériel (ou que vous voulez recycler des périphériques). C'est une des choses les plus frustantes lorsque j'avais acheté mon précédent portable, c'est le peu de personnalisation disponibles dans la taille des disques ou la mémoire, et à un coût prohibitif.

Tout arrive dans un seul carton (45cm x 28cm x 11cm), lui-même contenant le portable, l'alimentation, et les ports d'extension, un carton pour le «bezel» (le contour de l'écran, on peut choisir la couleur), un carton pour le clavier (choix extensible de «layout» (disposition ?): US, FR, sans lettre, ISO, ANSI, clavier transparent, et j'[en passe][keyboard-layout]) et un carton qui contient le seul outil dont vous avez besoin pour monter le portable: un tournevis (fiche [iFixit][ifixit]).

![](/assets/images/202312-fw-13-2.jpg)

<figure>
    <img src="/assets/images/202312-fw-13-3.jpg"
         alt="le tournevis fourni par Framework pour le montage">
    <figcaption>Le seul outil nécessaire est fourni</figcaption>
</figure>

Une fois ouvert tous les cartons on se retrouve avec pas mal de blister (plastique transparent rigide) utilisés pour protéger les pièces dans les cartons. 

![](/assets/images/202312-fw-13-4.jpg)

### Montage

**mise à jour:** Je viens de voir que Framework m'avait envoyé par mail il y a quelques jour la [procedure d'installation complète][framework-amd-ryzen-guide]. 🫠😅

Une fois les cartons ouverts, on se retrouve avec cela. 

![](/assets/images/202312-fw-13-5.jpg)

Ci-dessous, l'intérieur avec la carte-mère dans la partie supérieure et la batterie en dessous. Il y a plein de QR-codes qui vous envoient vers les instructions de (dé-)montage, par contre pas toujours facile à lire sur un téléphone portable.

![](/assets/images/202312-fw-13-6.jpg)

À l'emplacement **rose** on clippe nos deux barrettes de RAM (1 minute), à l'emplacement **bleu**, on dévisse, on enfiche le stockage sur le connecteur M2 et on revisse (3 minutes tranquillou). Ensuite on monte le clavier, il suffit de brancher le cable-ruban du clavier sur le connecteur **jaune**, on pose ensuite le clavier sur le chassis (tout est aimanté). On retourne l'ordinateur, et on fixe les 5 vis (5 minutes). Normalement en 10 minutes c'est fait.

![Dos de l'ordinateur pour voir les vis de fixation](/assets/images/202312-fw-13-7-dos.jpg)

Par contre parlons de la frustration de monter le bezel autour de l'écran... 

Il s'agit normalement de poser cette protection plastique, contenant aussi le micro et l'objectif de la webcam, sur le cadre aimanté de l'écran, donc rien de difficile à priori. Mais cela m'a demandé plusieurs tentatives pour bien le placer, avec la peur de casser **le cadre qui est très fragile**. Même après cela, à chaque ouverture de l'écran j'entendais un «**clac !**» pas très rassurant. Après avoir cherché un moment, le bas du cadre n'était pas bien pressé, à cause d'un câble sorti du cheminement prévu qui empêchait le cadre d'être bien fixé... Problème rencontré souvent d'après ce [post reddit][bezel-broken-reddit] et les commentaires que l'on peut trouver sous le [guide de montage du bezel][guide-montage-bezel].

![cable au niveau de la charnière de l'écran](/assets/images/202312-fw-13-8-cable-ecran.png)

Notons aussi que je n'avais pas vu qu'il y avait des caches d'autocollant dans le bezel à enlever avant de le placer (là aussi je ne suis pas le seul).

On y est presque, il ne reste plus qu'à brancher les **ports d'extensions**. C'est aussi **un des atouts de machines Framework**. Elles sont équipées d'emplacements qui permettent de brancher des sorties de different format interchangeables: USB-C, USB-A, port HDMI, DisplayPort, slot microSD, un slot de stockage, etc. Avis aux bidouilleurs, un trouve une communauté Framework pour créér ses propres ports.

![Quelques ports d'extension Framework](/assets/images/202312-fw-13-9-extensions.png)

Le modèle 13 pouce est équipé de 4 emplacements. J'ai pour ma part acheté 4 USB-C, 1 displayPort et 1 HDMI.

Ensuite on est prêt à installer un OS et tester le matériel, on verra peut-être cela dans un autre article.

Cout du portable: 1450e

Composants achetés en plus
 - Stockage: [Western Digital Black SN850X 1 TB][wd-black-sn850x] (95e)
 - Kit mémoire: [Crucial RAM 32GB Kit (2x16GB) DDR5 5600MHz][crucial-ram-32gb] (130e)

**Positif**
 - grande choix dans la configuration disponible (et possibilité d'acheter ses composants). Atout majeur dans l'achat d'un portable pour moi. 
 - chassis et charnière très solides
 - machine très belle (je suis un romantique)
 - montage (presque) simple
 - guide de montage / remplacement disponible

**À améliorer**
 - Le bezel de l'écran fragile et pas facile à bien monter sans documentation. Par contre plus de probleme après.

[amd-ryzen-7840u]: https://www.amd.com/en/products/apu/amd-ryzen-7-7840u
[framework-not-sustainable]: https://frame.work/nl/en/blog/we-are-not-sustainable
[framework-sustainability-strategy]: https://frame.work/sustainability
[keyboard-layout]: https://frame.work/fr/fr/products/keyboard?v=FRANBKEN01
[framework-13-amd-spec]: https://frame.work/products/laptop-13-gen-amd
[ifixit]: https://fr.ifixit.com/Device/Framework_Screwdriver
[framework-amd-ryzen-guide]: https://guides.frame.work/Guide/Framework+Laptop+13+(AMD+Ryzen%E2%84%A2+7040+Series)+DIY+Edition+Quick+Start+Guide/211?lang=en
[bezel-broken-reddit]:  https://www.reddit.com/r/framework/comments/187hna5/screen_cable_not_glued_bezel_damaged/
[guide-montage-bezel]: https://guides.frame.work/Guide/Bezel+Replacement+Guide/82
[wd-black-sn850x]: https://www.westerndigital.com/products/internal-drives/wd-black-sn850x-nvme-ssd?sku=WDS100T2X0E
[crucial-ram-32gb]: https://www.crucial.fr/memory/ddr5/ct2k16g56c46s5

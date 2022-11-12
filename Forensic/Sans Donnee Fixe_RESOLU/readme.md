# All your base are belong to us

|difficulté|type|
|:---:|:---:|
|Difficile|Forensic|

## Énoncé
Une clé USB a été trouvée dans une poubelle verte par notre service d'entretien des surfaces.



Comme la procédure nous y oblige, seuls les papiers sont autorisés dans la poubelle verte.



Notre unité d'analyse de premier niveau en a fait une image et vous l'a transmise....



On attend votre rapport avant de la jeter dans le recyclage de déchets électroniques....

SHA1 17f128bdec976f4e45f91a9e40093b844049e802

## Analyse

C'est parti pour ce grand classique, nous avons une image d'un système de fichier que nous allons devoir analyser.
Nous commençons par essayer d'identifier le file système utilisé:

```bash
file disk.dd
disk.dd: Linux rev 1.0 ext2 filesystem data, UUID=7de36af6-daec-4de1-85de-76827f551f68 (extents) (64bit) (large files) (huge files)
```

## Récupération

Nous avons une partition Linux, au moins la clef n'a pas été effacée et nous n'allons pas faire de post-mortem dessus, maintenant, nous allons chercher s'il reste des fichiers, toutes les commandes qui suivent viennent de Sleuthkit

```bash
fls -m / -l -p disk.dd
0|/lost+found|11|d/drwx------|0|0|16384|1657616487|1657616487|1657616487|0
0|/Confidentiel.vc|12|r/rrw-------|1000|1000|1048576|1657616607|1657616004|1657619259|0
0|/$OrphanFiles|257|V/V---------|0|0|0|0|0|0|0
```

Nous avons un répertoire lost+found à la racine, le $OrphanFiles et un fichier Confidentiel.vc, nous travaillons sur l'offset 0 (début de la seule partition), et il n'y a aucun fichier supprimé.

La solution est donc dans le fichier Confidentiel, nous allons l'extraire avec icat en règlant l'offset (-O 0) et le numéro d'index du fichier (12):

```bash
icat -o 0 disk.dd 12 > Confidentiel.vc
```

puis nous allons analyser le fichier :

```bash
file Confidentiel.vc
Confidentiel.vc: data
```

Bon, pas très parlant, si on regarde dedans que voyons nous ?

```bash
cat Confidentiel.vc | head -n 5
Þµn�ÖÃ²zY SNWÜÙçrÝ±7í¼d)÷Úl¥1fXÒWûâ-wêÖÑ6ûqIúòÅ9/\´Ö³úÃ)uùÝ)èÄÒ×½(O0~³!/5:Ô«4FÛßÃEôÈºàÙèÚ;ïY?D '½Q¡(å2>ú·	7	·fAßÉd´¬P}={Á$¥­ÚìE�
ô*sÓ5åÅ¦J©0ü;ýc6ª7â½ÉF¶Î5¾òÇ(Ë¨Ü¿¥nNêèF5{><KìCÝ±ûn7ê$OK0©"=Ëàdb³¸q÷gØDëä©I#6»}ë¶Pý¨OãY<TyÙþ{ÿêhÿÁ>û<àiªÛÒ¶¥ê=²Ì-Gpÿ§r½ßå#SBZ²¥R¹àOÔü¸ÑQà²çÆIC±%82³°b°jq³�WÿcÓ¢>#ê¡ùáÓ±²÷­Pcý$¨2N<Ñî//¡nÅ_õÁó#~Q©nÚ+:i)¾GéTpalhì-Õmuð-öZx#îKÃá½0Yº|Dic×Â_$òúæÔ;h¬,NãkÀ7²]Ñ-iÆ?µÝzqz?ãòéÉÁï*äÏ¬yÚ°/¨"%ÝÑ÷¦ÒÎÿÈä¿Ë529²9ÂÏ+æºëÏ[ÑÚ²ráô)^ÝÆãô]ïfâR¬ÕÍã�¿x9=ÃÈtÐp¹ñ/tbTÐÐÓâà5b¦?y3RÝ<8¤ãèYb¹çvã«Ço-A9`,uþ·¦Ð>Ys<Pt6«'mæ"üÙY±ÊIòýW±$/ÿim.¿¦òQë:YâèpÈò	ë­Þ5ðôr©Qóæ +¶#t©=ÑÄ½d<é<±aæddbhdnï8¸ÄR²5J¢{±Gì$h;ÀëÇR÷uzîA�¬±}¾fðødÃÞÞz.:ßÌä÷42¬}8¶Û2Mn¬íu
=´ªåPÙÂ?$ðZÐÕÅt×ÆÙcJ0q1ûéÓsº¼!¬ð·ó84Ã'´30ó­V,¯11cbÅp®¦ñ31»ÖçÑw!s2yi¡~dPiÒiLë¡ÄØú÷)|QdG9IË^°&áq%pbb´0e!vÝÒò\~oÚ/S©¯Ny¦9Í²"øáVm÷¡ÇgýdO!jègCó/ä3MÆ¤-x»z¶ºuBAT\ ÍÁæ§iÕå¸
L^Nmûaøº.ejmr¥oÝ]±ÿ«·!o:%«íP~H?ÁÐ ´EÎ1y¸YSNÇË=Ê§Õ ydî�×]KIa¨.ÜÓ&s¯¸êIã?|êÖºìà¡9Z¹ÒÇq)û{vTp`º­Ý'¶öêÒ¦ÊieÝ\@_%Å$Ù%ªàCÙ¡v²iÞrc!}Ô´kÃ*eº¾ó½PuÂÊ5òö{´lË%5\}ò÷ÛZü®zVöÀeØ'9ÑÄ=M.³¶ùÓ`ö{i^jEÛn*5@LdæIü¨½¯&²®_	x
M¡Psz¿¹ýÃïLù-6ÜqpC¦[aD­`$uÉê-PPLüÞØwËjü_a2\ýûO1Ý.¥YZî£û!$F`Øæò×GµñXD1ØVÞû&S"1Ð'{£
=lPxJÉÔç*õnÐ¡÷ u®¯DNµÿÇB]ÁÈ¬¸»ÝÊzmX=%©B¨ôpÅÚs:K3bB-E}vâ#CÒõõNyÁäû£Óù4övfÜ8P#N{ãÉIh¾rgýðî¡Bþ[ºµË;Zry­%!À
¬/¥=ñÓJÐDb8Dj)¤xR­Ð¦¬òW\Wà=q¶lÝ^bê_J±ùFvvõa:¶êuªèëcA¸xfë`Ö·Ûc «qëÀ½ÄÖ}U	ÛºöÅFdç!#}FÆ¢ÂÌÜBø¬Ìp¬Ísí¶Ö#ÃÄ8{¡ñ±¼2{}Þù¡Ø"°Ë¦²àIbÄ=·úÚb÷ÌòQ¥ìÍ¢tÓn_»ÊþhwOÓ>&%uÀ¼V÷É¾-°¼W_'%Tb'!Ï¼&%DØ)Ysø.´ØÇiSþh¿³$Øÿ¥êÊþ§ø­á2¬ó¯bowÚKWU·DRÁÌ×y[efzc^®ð[ô©}¢ÑÄëóQ
```

Bon, on a une entropy de malade, le fichier semble chiffré, l'extension **.vc** me fait penser à Veracrypt, mais sur le net il parle d'un logiciel pour créer des RPG. Une peu de recherche et de fails, et l'on retourne sur notre première idée, c'est un conteneur Veracrypt.

Seulement pour le monter, il va nous manquer le mot de passe…

## Hashcat on his way

Pas grave, nous avons la technologie

![bob](./IMG/bob.gif)

Nous allons espérer que le mot de passe et l'encryption soit suffisament faible pour être reverse suffisament rapidement. Nous regardons l'algo par défaut de Veracrypt, et l'on voit que le premier proposé est celui là:

> HMAC-SHA256

Nous allons espérer que notre cible aura été basique dans ses choix:

```bash
hashcat -m 13751 -a 0 Confidentiel.vc /opt/SecLists/Passwords/rockyou.txt
```

et la réponse arrive assez rapidement:

![hascat win](./IMG/hashcatpng.png)

Il nous reste plus qu'a monter le volume veracrypt:

```bash
mkdir /tmp/vera && veracrypt Confidentiel.vc /tmp/vera -p ricardo
```

On répond aux questions posée (pas de PIM, pas de keyfile, on entre le mdp superuser pour monter le volume dans le repertoire que l'on a créé)

![veramount](./IMG/veramount.png)

Nous avons trois fichier, nous allons tenter le note.txt, mon instinct félin me dit que c'est le bon:

```bash
cat /tmp/vera/Notes.txt
Qm9uam91ciBSaWNhcmRvLApQZW5zZXog4CBtZXR0cmUgY2UgY29kZSBlbiBz6WN1cml06To=
SU5OTntCNHMzNjRfM25jMGQzfQ==
```

Tiens, encore du base64, on va ajouter la commande magique pour le décoder:

```bash
cat /tmp/vera/Notes.txt | base64 -d
Bonjour Ricardo,
Pensez à mettre ce code en sécurité:INNN{B4s364_3nc0d3}
```

Merci Ricardo pour ce flag !

>INNN{B4s364_3nc0d3}

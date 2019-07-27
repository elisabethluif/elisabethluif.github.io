# Homework 6: Regular Expressions

## PART I

1. What regular expression matches each of the following?
	“eat”, “eats”, “ate”, “eaten”, “eating”, “eater”,
	“eatery”

   **Solution:** `\b(eat(s|en|ing|ery?)?|ate)\b`

2. Find all Qadhdhafis...
	... the name of the country's head of state [is]
	Colonel Gaddafi. Wait, no, that's Kaddafi. Or maybe it's
	Qadhafi. Tell you what, we'll just call him by his first
	name, which is, er ... hoo boy.
		(SRC: http://tinyurl.com/4839sks)
	The LOC lists 72 alternate spellings
		(SRC: http://tinyurl.com/3nnftpt)

	Maummar Gaddafi, Moamar AI Kadafi, Moamar al-Gaddafi,
	Moamar el Gaddafi, Moamar El Kadhafi, Moamar Gaddafi,
	Moamar Gadhafi, Moamer El Kazzafi, Moamer Gaddafi,
	Moamer Kadhafi, Moamma Gaddafi, Moammar el Gadhafi,
	Moammar El Kadhafi, Mo'ammar el-Gadhafi, Moammar Gaddafi,
	Moammar Gadhafi, Mo'ammar Gadhafi, Moammar Ghadafi,
	Moammar Kadhafi, Moammar Khadaffy, Moammar Khadafy

   **Solution:** `\b(M[ao][au']a?mm?+[ae]r?) ([AEae][Il][- ])?([GK]h?[zz|ad]+h?af+[iy])\b`

3. Find all variations of Iṣbahān
	(construct the shortest possible regular expression):
	
	EASY:
	Iṣbahān, Iṣfahān, Isbahan,
	Isfahan, Esfāhān‎, Esfahān,
	Esfahan, Hispahan,
	iṣbahān, iṣfahān, isbahan,
	isfahan, esfāhān‎, esfahān,
	esfahan, hispahan,

   **Solution:** `\b([IEHieh]i?[sṣ][bfp][aā]h[aā]n)\b`

## PART II (more practice)

1. Conversion: Convert “Askin, Leon” > “Leon Askin”

	Leon Askin
	Helmut Berger
	Berger, Senta
	Gold, Käthe
	Haid, Liane
	Hörbiger, Attila
	Hörbiger, Christiane
	Hörbiger, Paul
	Kogler, Melanie
	Lamarr, Hedy
	Merkatz, Karl
	Minichmayr, Birgit
	Moser, Hans
	Nalder, Reggie
	Schell, Maximilian
	Schneider, Romy
	Schwarzenegger, Arnold
	Waltz, Christoph
	Werner, Oskar
	Vader, Darth

   **Solution:** Search: `\t(\w+), (\w+)\n` Replace: `\t$2 $1\n`

2. Simple: Construct regular expressions that finds references all Austrian cities.	

	Major cities in Austria are as follows: Vienna, Graz, Linz,
	Salzburg, Innsbruck, Klagenfurt, Villach, Wels, Sankt Pölten,
	Dornbirn, Wiener Neustadt, Steyr, Feldkirch, Bregenz, Leonding,
	Klosterneuburg, Baden bei Wien, Wolfsberg, Leoben, Krems, Traun,
	Amstetten, Lustenau, Kapfenberg, Mödling, Hallein, Kufstein,
	Traiskirchen, Schwechat, Braunau am Inn, Stockerau, Saalfelden,
	Ansfelden, Tulln, Hohenems, Spittal an der Drau, Telfs, Ternitz,
	Perchtoldsdorf, Feldkirchen, Bludenz, Bad Ischl, Eisenstadt,
	Schwaz, Hall in Tirol, Gmunden, Wörgl, Wals-Siezenheim,
	Marchtrenk, Bruck an der Mur, Sankt Veit an der Glan,
	Korneuburg, Neunkirchen, Hard, Vöcklabruck, Lienz, Rankweil,
	Hollabrunn, Enns, Brunn am Gebirge, Ried im Innkreis,
	Bad Vöslau, Waidhofen, Knittelfeld, Trofaiach, Mistelbach,	
	Zwettl, Völkermarkt, Götzis, Sankt Johann im Pongau,
	Gänserndorf, Gerasdorf bei Wien, Ebreichsdorf, Bischofshofen,
	Groß-Enzersdorf, Seekirchen am Wallersee, Sankt Andrä

   **Solution:**
   
   `\b(Vienna|Graz|Linz|Salzburg|Innsbruck|Klagenfurt|Villach|Wels|Sankt Pölten|Dornbirn|Wiener Neustadt|Steyr|Feldkirch|Bregenz|Leonding|Klosterneuburg|Baden bei Wien|Wolfsberg|Leoben|Krems|Traun|Amstetten|Lustenau|Kapfenberg|Mödling|Hallein|Kufstein|Traiskirchen|Schwechat|Braunau am Inn|Stockerau|Saalfelden|Ansfelden|Tulln|Hohenems|Spittal an der Drau|Telfs|Ternitz|Perchtoldsdorf|Feldkirchen|Bludenz|Bad Ischl|Eisenstadt|Schwaz|Hall in Tirol|Gmunden|Wörgl|Wals-Siezenheim|Marchtrenk|Bruck an der Mur|Sankt Veit an der Glan|Korneuburg|Neunkirchen|Hard|Vöcklabruck|Lienz|Rankweil|Hollabrunn|Enns|Brunn am Gebirge|Ried im Innkreis|Bad Vöslau|Waidhofen|Knittelfeld|Trofaiach|Mistelbach|Zwettl|Völkermarkt|Götzis|Sankt Johann im Pongau|Gänserndorf|Gerasdorf bei Wien|Ebreichsdorf|Bischofshofen|Groß-Enzersdorf|Seekirchen am Wallersee|Sankt Andrä)\b`

3. More Difficult: Construct regular expression that finds only cities from 1) Lower Austria; 2) Salzburg.

	Vienna (Vienna), Graz (Styria), Linz (Upper Austria),
	Salzburg (Salzburg), Innsbruck (Tyrol), Klagenfurt (Carinthia),
	Villach (Carinthia), Wels (Upper Austria),
	Sankt Pölten (Lower Austria), Dornbirn (Vorarlberg),
	Wiener Neustadt (Lower Austria), Steyr (Upper Austria),
	Feldkirch (Vorarlberg), Bregenz (Vorarlberg),
	Leonding (Upper Austria), Klosterneuburg (Lower Austria),
	Baden bei Wien (Lower Austria), Wolfsberg (Carinthia),
	Leoben (Styria), Krems (Lower Austria), Traun (Upper Austria),
	Amstetten (Lower Austria), Lustenau (Vorarlberg),
	Kapfenberg (Styria), Mödling (Lower Austria),
	Hallein (Salzburg), Kufstein (Tyrol),
	Traiskirchen (Lower Austria), Schwechat (Lower Austria),
	Braunau am Inn (Upper Austria), Stockerau (Lower Austria),
	Saalfelden (Salzburg), Ansfelden (Upper Austria),
	Tulln (Lower Austria), Hohenems (Vorarlberg),
	Spittal an der Drau (Carinthia), Telfs (Tyrol),
	Ternitz (Lower Austria), Perchtoldsdorf (Lower Austria),
	Feldkirchen (Carinthia), Bludenz (Vorarlberg),
	Bad Ischl (Upper Austria), Eisenstadt (Burgenland),
	Schwaz (Tyrol), Hall in Tirol (Tyrol), Gmunden (Upper Austria),
	Wörgl (Tyrol), Wals-Siezenheim (Salzburg),
	Marchtrenk (Upper Austria), Bruck an der Mur (Styria),
	Sankt Veit an der Glan (Carinthia), Korneuburg (Lower Austria),
	Neunkirchen (Lower Austria), Hard (Vorarlberg),
	Vöcklabruck (Upper Austria), Lienz (Tyrol),
	Rankweil (Vorarlberg), Hollabrunn (Lower Austria),
	Enns (Upper Austria), Brunn am Gebirge (Lower Austria),
	Ried im Innkreis (Upper Austria), Bad Vöslau (Lower Austria),
	Waidhofen (Lower Austria), Knittelfeld (Styria),
	Trofaiach (Styria), Mistelbach (Lower Austria),
	Zwettl (Lower Austria), Völkermarkt (Carinthia),
	Götzis (Vorarlberg), Sankt Johann im Pongau (Salzburg),
	Gänserndorf (Lower Austria), Gerasdorf bei Wien (Lower Austria),
	Ebreichsdorf (Lower Austria), Bischofshofen (Salzburg),
	Groß-Enzersdorf (Lower Austria),
	Seekirchen am Wallersee (Salzburg), Sankt Andrä (Carinthia)

   **Solution:** `\b((?<=\,\s).+(?=\s\(Lower Austria\),)|(?<=\,\s).+(?=\s\(Salzburg\),))\b`

   *Note:* There's a problem with this solution: this code can't find those city names in the beginning of each new line (i.e after the tabulator)

****

_Back to [start](https://elisabethluif.github.io/)_

# JTCOP FPGA Clone of DEC0 Hardware by Jose Tejada (@topapate)

You can show your appreciation through
* Patreon: https://patreon.com/jotego
* Paypal: https://paypal.me/topapate

Yes, you always wanted to have a Robocop arcade board at home. First you couldn't get it because your parents somehow did not understand you. Then you grow up and your wife doesn't understand you either. Don't worry, MiST(er) is here to the rescue.

What you get with this is an extremely accurate (allegedly 100% accurate) clone of the original hardware.

I hope you will have as much fun with it as I had it while making it!

# The Cores

* jtcop, for Robocop and the other games using the HuC processor
* jtninja, for Bad Dudes and the rest using the i8751 MCU
* jtmidres, for Midnight Resistance
* jtslyspy, for Sly Spy
* jtkarnov, for Karnov and Chelnov

The correct core is selected by the MRA files (MiSTer) or ARC files (MiST)

# The Games

Game                            | Protection | Popularity
--------------------------------|------------|-------
Bad Dudes vs. Dragonninja       | i8751      | 700
Robocop                         | HuC6280    | 691
Midnight Resistance             | HuC6280    | 198
Hippodrome / Fighting Fantasy   | HuC6280    | 138
Chelnov                         | i8751      |  92
Karnov                          | i8751      |  66
Heavy Barrel                    | HuC6280    |  42
Sly Spy                         | HuC6280    |  34
Wonder Planet                   | i8751      |  10
Bandit                          | i8751      |   1
Boulder Dash                    | HuC6280    |   1
Birdie Try                      | i8751      |   0


# Schematics

The KiCAD schematics for Sly Spy developed by us are in the slyspy core folder. A link to a PDF version is available below.

- [Karnov's original schematics](https://www.gamesdatabase.org/Media/SYSTEM/Arcade/Manual/formated/Karnov_-_1987_-_Data_East.pdf)
- [JOTEGO's schematics for Sly Spy](https://github.com/jotego/jtbin/tree/master/sch/slyspy.pdf)

## Heavy Barrel

The player orientation is stored as a byte with the following encoding

```
        0
    1C     4
  18         8
    14     C
       10
```

Player | RAM address | SDRAM address
-------|-------------|---------------
 1P    | FF8066      | 10'0033
 2P    | FF80AA      | 10'0055

The MSB part of the 16-bit value is used. The JTFRAME cheat engine
is used to transfer the analogue right stick readings to the SDRAM
so the game changes the player orientation correctly. This is done
in [beta.s](cheat/beta.s)

# BAC06

Column scroll: 0x80 bytes -> 0x40 values
Row scroll: 0x400 bytes -> 0x200 values

BAC06 chip |   Region    | Total Size  |  MSFT   | MAME gfx
-----------|-------------|-------------|---------|----------
 3A, main  |   B0/       |   16kB      |  Yes    |  gfx1
 7A, main  |   B1/       |    4kB      |  Yes    |  gfx2
 6E, second|   B2/       |    4kB      |  No     |  gfx3

# FPGA Requirements

Block  |  kB  | Location | Use
-------|------|----------|-----------
B0     |  16  |  SDRAM   | VRAM + RAM?
B1     |   4  |  SDRAM   | VRAM
B2     |   4  |  SDRAM   | VRAM
OBJ    |   1  |   BRAM   | line buffer*
OBJ    |   2  |   BRAM   | obj RAM
OBJ    |   2  |   BRAM   | obj table buffer
Main   |  16  |  SDRAM   | M68000 RAM
6502   |   2  |   BRAM   | 6502A RAM
HuC6280|  10  |   BRAM   | protection
HuC6280|   1  |   BRAM   | firmware
i8751  |   4  |   BRAM   | firmware
i8751  |   1  |   BRAM   | on-chip RAM
JT03   |  12  |   BRAM   |
JTOPL  |   9  |   BRAM   |
FX68K  |   6  |   BRAM   | microcode
Scan2x |   2  |   BRAM   |
OSD    |   2  |   BRAM   |
Credits|   1  |   BRAM   |

Model   |  BRAM
--------|---------
Huc6280 |  53
I8751   |  47


* the line buffer is not in the schematics,
there is a signal changing each frame that
could be used for a frame buffer. But there
is no frame buffer memory either.
The MXC-06 chip must have had internal memory
for line buffers

# Misc

There is a video recording of the Midnight Resistance PCB game play [here](https://www.youtube.com/watch?v=k0V2VGGQ8Cg)

# Development Support

This core was possible thanks to the patrons listed
in the credits screen as well as the following
supporters. Thank you.

Supporters for JTCOP, JTNINJA, JTMIDRES and JTSLYSPY:

```
8bits4ever             Adam Leslie            Adam Small             Adrian Labastida
Alan McGrath           Alan Shurvinton        Alden                  Alex Mandic
Alexander Facchini     Alexander Lash         Alexander Upton        Alfonso Clemente
Allen Tipper           Allister Fiend         Alvaro Paniagua        Andrea Chiavazza
Andreas Micklei        Andrew Boudreau        Andrew Hannan          Andrew P Gibson
Andrew Schmidt         Angel Aguinaga         Angelfred              Angelo Kanaris
Anselmo Moreno         Anthony Monaco         Antoine Mariette       Anton Gale
Antwon                 Aquijacks (Flashjacks  Arend Pronk            Arkadiusz
Arthur Fung            Aurich Lawson          BRCDEvg                Banane
Bear S                 Ben                    Ben Sanborn            Ben Toman
Bitmap Bureau          Bitmaps Retro          Bliz 452               Blue Abs
Boris Pruessmann       Brandon Lennie         Brandon Peach          Brandon Smith
Brandon Thomas         Brent Fraser           Brian Peek             Bruce Fontaine
Bruno M                Bryan Adams            Bryan Evans            C
Cameron Berkenpas      Cameron Tinker         Carlos Bailleres       Carlos Gruberman
Carrboroman            Cesar Sandoval         Charles                Chris
Chris Babishoff        Chris D                Chris Hoff             Chris Maguire
Chris Mzhickteno       Chris Scully           Chris Tuckwell         Chris W Miller
Chris smith            Christian Bailey       Clinton Cronin         Cobra Clips
Colin Colehour         Colin Edwards          Cory Stargel           Dan Doyle
Daniel .               Daniel Casadevall      Daniel Dongil          Daniel Fowler
Daniel Page            Darren Newman          Darren Wootton         Daryll David
Dasutin                Dave Douglas           David Drury            David Filskov
David Fleetwood        David Jones            Denny Letourneau       Diana Carolina
Didgeridoo             Didier Touron          Dimitris Zongas        Dr Catjail
Dr. Octagon            DrMnike                Dre137                 Edward Mallett
Enthropy               Eren Kotan             Eric                   Eric Gutt
Eric J Faulkes         Eric Walklet           Fabio Michelin         Fabricio
Federico               Five Year Guy          Florian Raoult         Fred Rojas
GeorgeSpinner          GohanX                 Goolio                 Grant McNaught
Greg                   Greg Sargent           Gregory Val            GuitarJedi
HFSPlay                Handheld Obsession     Harmonica              Henry
Henry R                Hentai Joe             Hugo Pinto             ItsBobDudes
JOSE LUIS              JR                     JSwan                  Jack Sammons
Jacob Hoffman          Jacob Lawter           James Dingo            James Durden
James Kilgore          James Wilson           Jason Baker            Jeff Roberts
Jeremy Hasse           Jerry Suggs            Jesse Clark            Jim Knowler
JimLahey               Jimmy Dozier           Jimmy Richards         Jockel
Joe Giuliano           Joel Albino            Johan Smolinski        John Figueroa
John Fletcher          John Hood              John Silva             John T. Keen
John Wilson            Johnny harvick         Jonah Phillips         Jonathan
Jonathan Brochu        Jonathan Tuttle        JonathanValls          Joost Peters
Jootec from            Jorge                  Jork Sonkinfield       Jose L
Josep Barbie           Joseph Kulinski        Joseph Milazzo         Joseph Mogavero
Josh Emery             Josh Mayer             Josh Yates-Walker      Josiah Wilson
Justin D'Arcangelo     Justin Rudebaugh       Kai Cherry             Kai Luotojoki
Keith Gordon           Ken Scott              Kevin Dayton           Kike Alcor
KnC                    Kricys                 KrzysFR                Kyle Pedersen
L.Rapter               Lakeside               Laurent Cooper         Lee Grocott
Lee Osborne            Lucius Bono            Luis F Giron           M Reznor
MaDDoG                 Mack H                 Madox                  Magnus Kvevlander
Manuel Astudillo       Marco Cuevas           Marco Emparan          Mark Baffa
MarthSR                Martin Ansin           Matt Elder             Matt Evans
Matt Hargett           Matt Heinrich          Matt Lichtenberg       Matt McCarthy
Matt Postema           Matthew Pollard        Matthew Woodford       MechaGG
MiSTer Retro           Michael Anderson       Michael Bariszlovits   Michael Berger
Michael C              Michael Eggers         Michael Ferguson       Michael Rea
Michael_DKT            Mike Holzinger         Mike Jegenjan          Mike Olson
Mike Parks             Mottzilla              Nailbomb               Nathan Souris
Neil St Clair          NerdyNester            Nic Kaiman             Nick Daniels
Nick Delia             Nick Gudauskas         Nico Stamp             Nicolas Hurtado
Niko                   NonstopXiaowei         Obvious Fakename       OopsAllBerrys
Oriez                  Pascal Courtois        Patrick Roman          Paul
Paul Cunningham        Paul Hoggett           Paulo M.               Paweł Mandes
PeFClic                Pedro Santiago         Per Ole                Philip Lai
Philip Lawson          Pierre-Emmanuel Martin Rachael Netz           Rachel Schaeffer
Ralph Barbagallo       Ramon Gamaliel         RandomRetro            Raphael Melgar
Raul3D                 RayGun                 RetroPREZ              RetroRGB
Rex Kung               Rex Willer             Richard Eng            Richard Murillo
Richard Simpson        Rick Ochoa             Riyad Twair            Robert Daniel
Robert Hayes           Romain Dijoux          Ronald Dean            Ronan Amicel
Ruben                  Ryan                   Ryan O'Malley          SIDKidd64
Sam Hall               Samuel Warner          Sang Hee               Sascha Zupanek
Schnookums             Ser Erris              Shannon King           Spank Minister
SteelRush              Stefan Krueger         Stephen R Price        Steve Ikeguchi
Steve Lin              Steve Skrzyniarz       Steve Tack             Steven A
Steven Hansen          Steven Yedwab          Stry8993               Stuart Morton
Sunder Raj             SuperBabyHix           Synbios                Taehyun Kim
Tales Dilli            Terse                  The Collector          The Video
Thomas Attanasio       Thomas Irwin           Thorias                Timothy Bearup
Tobias Dossin          Tom Milner             Topher Campbell        Travis Brown
Trifle                 Tulio Adriano          Ty B                   VickiViperZabel
Victor Bly             Victor Fontanez        Will Abbott            William Clemens
William Tryon          Yunus Soğukkanlı       Zach Marquette         Zoltan Kovacs
albertprime            alejandro carlos       angel_killah           arcadebros
benedict lindley       blackwine              brian burney           cbab
chauviere benjamin     circletheory           crap I                 dARKrEIGn20xx
datajerk               deathr0w               deathwombat            derFunkenstein
genocide jr.           gunmakuma              jbrlll                 jim br
kamel rasennadja       kccheng                kernelchagi            keropi
liphy                  meng po                metal                  natalie
pacoarcade             patrick pejic          raoulvp                retrod00d
rsn8887                slayer213              starman_jr             taal.M
tim rogers             tonitellezb            troy coberly           turbochop3300
twilitezoner           type78                 yoaarond
```

Supporters for JTKARNOV:

```
3style                 8bits4ever             A Hernandez            A Murder
Aaron Ray              Adam Leslie            Adam Rykowski          Adam Small
Adrian Labastida       Alan Shurvinton        Alda Alesio            Alec Peden
Alex Baldwin           Alex Mandic            Alexander Facchini     Alexander Lash
Alexander Upton        Alfonso Clemente       Allen Tipper           Allister Fiend
Alvaro Paniagua        Andrea Chiavazza       Andreas Micklei        Andrew Boudreau
Andrew Hannan          Andrew P Gibson        Andrew Schmidt         Angel Aguinaga
Angelfred              Angelo Kanaris         AnotherJoe             Anselmo Moreno
Anthony Cheng          Anthony Monaco         Anton Gale             Arend Pronk
Arkadiusz              Armin Hierstetter      Arnulf Eide            Arthur Fung
Aurich Lawson          BRCDEvg                Banane                 Barley Cheezers
Bear S                 Ben Cullen             Ben Tiefert            Ben Toman
BigRedPimp             Bit2018                Bitmap Bureau          Bitmaps Retro
Bliz 452               Boris Pruessmann       Brad                   Brandon Lennie
Brandon Peach          Brandon Thomas         Brandoon               Brent Fraser
Brian Peek             Brian Shiver           Brianna Cluck          Bruce Fontaine
Bruno Meyere           Bruno Silva            Bryan Evans            Byshop303
Cameron Berkenpas      Cameron Tinker         Carlos Bailleres       Carlos Gruberman
Casey Hamann           Cedric Vioget          Cesar Sandoval         Chad Page
Charles                Chris                  Chris Angelini         Chris Babishoff
Chris Brentano         Chris D                Chris Davis            Chris Hauk
Chris Hoff             Chris King             Chris Maguire          Chris Mzhickteno
Chris O'Neil           Chris Petroni          Chris Scully           Chris Sewell
Chris Tuckwell         Chris W Miller         Chris smith            Christophe GARDES
Chuong Dang            Clayton Anderson       Clinton Cronin         Cobra Clips
Colin Colehour         Collidingforces        Cory Sizemore          Cosmic Savant
Craig McLaughlin       Damien D               Dan Kelley             Daniel
Daniel .               Daniel Dongil          Daniel Flowers         Daniel Fowler
Daniel Page            Daniele Pellegrini     Danny Garfield         Darren Wootton
Daryll David           Dave Bennett           Dave Douglas           Dave Nice
David                  David Drury            David Fleetwood        David Frost
David Jones            David Osborne          David Rey              David Stone
Denis Brækhus          Dennis Ranker          Denny Letourneau       Devon Meunier
Diana Carolina         Didier Touron          Dimitris Zongas        Douglas Alves
Dr Catjail             Dr. Octagon            DrMnike                Dubesinhower
Dward Venegas          Ed                     Edward Mallett         Epixjava
Eren Kotan             Eric                   Eric Gutt              Eric J Faulkes
Eric Schneider         Eric Sorensen          F34R                   FROELIGER
Fabio Michelin         Fabrice Odero          Fabricio               Federico
Five Year Guy          Florian Raoult         Franco Catrin          Frank Brevoort
Frank Schwab           Gareth Jones           Gavin C                GeorgeSpinner
GigaBoots              Girth305               Glenn Percival         Gord Allott
Grant McNaught         Gregory Val            Grummkol               Guillermo Tunon
Gutxi Haitz            Guy Taylor             HamsoloPlays           Hans Baier
Harmonica              Heinz Stampfli         Henry                  Henry R
Hentai Joe             Hugo Pinto             ItalianGrandma         ItsBobDudes
JAMES D BOOTH          JR                     JSwan                  Jack Sammons
Jacob Hoffman          Jakob Schmid           James Boone            James Dingo
James Mann             James Miller           James Nivin            James Trautner
James Wilson           Janne Heikkarainen     Jason Baker            Jason Dee
Javier Heredia         Jayson Larose          Jeremie Barnes         Jeremy Hopkins
Jerry Langwell         Jerry Suggs            Jerry Yuan             Jesse Clark
Jesse Rankin           Jesus Garcia           Jim Knowler            JimLahey
Jimmy Ecker            Jimmy Richards         Jockel                 Joe Dinges
Joe Giuliano           Joel Albino            John Dawson            John Figueroa
John Fletcher          John K                 John Silva             John T. Keen
John Torn              John Wilson            John Woods             Johnny harvick
Jon Prusik             Jonah Phillips         Jonathan               Jonathan Tuttle
JonathanValls          Joost Peters           Jork Sonkinfield       Jose Perez
Josep Barbie           Joseph Kulinski        Joseph Milazzo         Joseph Mogavero
Josh Hogan             Josh Yates-Walker      Juan Barriga           Juan Pablo
Justin D'Arcangelo     Kai Cherry             Kai Luotojoki          Kaiosten
Kaya Bear              Keith Gordon           Kellerkind             Ken B
Ken Scott              Kevin Dayton           Kevin Miller           Kike Alcor
Kimberley Fisher       Kingau                 KnC                    Konrad
Kricys                 Kristian.              KrzysFR                Kyle Pedersen
L.Rapter               Lakeside               Lars Vonhof-Hunold     Lee Grocott
Lee Osborne            LoBai Zen              Lucian                 Lucius Bono
Luis F Giron           M Reznor               MaDDoG                 Mack H
Madox                  Magnus Aspling         Magnus Kvevlander      Mane Function
Manksalot              Marcello Medini        Mark Floyd             Mark Jeffers
Mark Saunders          MarthSR                Matt Bouverie          Matt Evans
Matt Hargett           Matt Heinrich          Matt McCarthy          Matt Postema
Matt Simonds           Matt Vulcano           Matthew Compston       Matthew Woodford
Matthieu Marchione     Max                    Max Power              MechaGG
MiSTer Retro           Michael Anderson       Michael Bariszlovits   Michael Martin
Michael Rea            Michael_DKT            Mickaël Renou          Mike Holzinger
Mike Jegenjan          Mottzilla              Mysterious Benefactor  Nadir Shabazz
Nailbomb               Nathan Souris          NerdyNester            Nicholas Bold
Nick Daniels           Nick Delia             Nick Gudauskas         Nico Stamp
Niko                   NonstopXiaowei         Noyman29               Obvious Fakename
Oliver Heilmann        Omar                   Omar Najera            OopsAllBerrys
Oriez                  Oskar Maria            Oskar Sigvardsson      Oyvind Christiansen
Pablo Avila-Estevez    Patrick McCarron       Patrick Roman          Paul
Paul Cunningham        Paul Jr                Paulo M.               Paulo Nascimento
Paweł Mandes           PeFClic                Pedro Santiago         Per Ole
Peter Mehes            Philip Lai             Philip Lawson          Pierre-Emmanuel Martin
Potato                 ProfessorAnon          R Omar Leal            Rachel Schaeffer
Ralph Barbagallo       Ramon Gamaliel         RandomRetro            Raphael Melgar
Raul3D                 Rautz                  RayGun                 RetroRGB
Retro_Brewz            Rex Kung               Rex Willer             Richard Eng
Richard Murillo        Richard Simpson        Robert Hayes           Robin Hertzberg
Romain Dijoux          Romier Silvera         Ronald Dean            Ronan Amicel
Ronin Yojimbo          Roro                   RoryDropkick           Ruben
Rufo Sanchez           Rune P                 Russ Crandall          Ryan
Ryan Clark             Ryan Kasper            Ryne Weiss             SIDKidd64
Saiyan                 Sam Hall               Sang Hee               Sascha Zupanek
Sayit BELET            Schnookums             Scott Bender           Ser Erris
Seth Wickline          Shad Uttam             Shannon King           Shen mue
Sherwood Hachtman      Shon Garraway          Sigmund68k             Skeletex
Sonthaya Sonthaya      Spank Minister         Stadium ARTs           SteelRush
Stefan Krueger         Stephen                Stephen Pagenstecher   Stephen R Price
Steve Lin              Steve Skrzyniarz       Steve Tack             Steven A
Steven Hansen          Stoneman               Stuart Morton          Sunder Raj
SuperBabyHix           Synbios                TMoney                 Taehyun Kim
Tales Dilli            Terse                  The Collector          The Video
Thomas Attanasio       Thorias                Tim Inman              Timothy Bearup
Tobias Dossin          Tom Milner             Tony Shong             Trifle
Trucker 69             Two Bards              Ty B                   Tyson Hanes
VERHILLE Arnaud        VickiViperZabel        Victor Bly             Victor Emmanuel
Victor Fontanez        Vincent Lietart        Wesley Lyons           Will Abbott
William Clemens        William Roussin        William Tryon          XC-3730C
Xaxius                 Yoshi9288              Yunus Soğukkanlı       Zoltan Kovacs
aguijon                alejandro carlos       alexcom                amdrgn
angel_killah           arcadebros             blackwine              brian burney
cbab                   chauviere benjamin     dARKrEIGn              dECKARD
datajerk               deathwombat            eclipse                eltee
fbmg                   ill_deez               jbrlll                 jim br
jonathan capparelli    jose luis              kadybat                kccheng
kernelchagi            kerobaros              keropi                 liphy
mattcurrie             mattyhochs             meijin3                metal
myusernamewastaken     ogge_leander           ojwales                patrick woodburn
raoulvp                retroboi               rsn8887                slayer213
sourdille              thomas winfrey         tim rogers             tonitellezb
troy coberly           turbochop3300          twilitezoner           type78
vampsthevampyre        yoaarond               Δlain
```
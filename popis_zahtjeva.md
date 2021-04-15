
# Sažetak projekta
Cilj projekta je izrada sustava za rješavanje i generiranje sudoku zagonetki raznih vrsta. Osnovna pravila sudoku zagonetki su da svaki redak i stupac kvadratne mreže dimenzija x * x sadrže svaki od brojeva od 1 do x točno jednom. Dio mreže je zadan, a posao rješavača je da ju nadopuni prema navedenim pravilima. Sudoku zagonetke su u ovom dijelu svojih pravila iste kao i latinski kvadrat (negdje se naziva i čarobni kvadrat) ali uvode i dodatna ograničenja u odnosu na njega. Uz retke i stupce, postoje i kutije ili regije, odnosno iscrtani razgraničeni oblici unutar mreže (obično su označeni podebljanim rubom ćelija ili različitom bojom pozadine ćelija) koji također sadrže x ćelija i u kojima se svaki od brojeva od 1 do x mora pojavljivati točno jednom. Konvencija je da te kutije budu manji kvadrati koji se ne preklapaju i čija visina i širina odgovaraju korijenu visine i širine cjelokupne mreže. Zbog ovog ograničenja, sudoku zagonetke su najčešće dimenzija 9 * 9 s kutijama dimenzija 3 * 3, ali s vremenom su kreativniji engimatičari stvorili pregršt zagonetki s pravokutnim kutijama (poput sudoku zagonetke dimenzija 6 * 6 s kutijama dimenzija 2 * 3), kutijama nepravilnih rubova ili sa više slojeva kutija koje se preklapaju. U nekim varijantama zagonetki postavljaju se i dodatna ograničenja poput onog da vrijednost u jednoj ćeliji mora biti manja od vrijednosti u susjednoj ćeliji, ili pak veća od vrijednosti u susjednoj ćeliji, a u nekim sudoku zagonetkama uvodi se pravilo da se brojevi ne smiju ponavljati unutar niti jedne od dijagonala cjelokupne mreže. 

Prilikom zadavanja sudoku zagonetki, moramo biti posebno pažljivi da ne postavimo zagonetku koja ima više od jednog točnog rješenja prema prethodno postavljenim pravilima. Ovo se lako može dogoditi kada u mreži postoji premalen broj unaprijed zadanih ćelija. Posao provjere mogućih rješenja uvelike nam olakšavaju računalni alati, jer je svaka sudoku zagonetka kombinatorni problem. Otkriveno je da je najmanji mogući broj zadanih ćelija 17. Možemo si priuštiti i isprobavanje rješenja metodom grube sile (brute force) u nekim slučajevima, na primjer kada treba samo jednom pronaći rješenje zagonetke za koju znamo da ima jednoznačno rješenje. 

# Lista projektnih isporuka

## Algoritam za provjeru ispravnosti zadane zagonetke 

Algoritam prolazi kroz cjelokupnu mrežu i provjerava ponavljaju li se brojevi u retku, stupcu ili kutiji, što krši pravila sudoku zagonetke. Sve pronađene greške se na odgovarajući način prikazuju dizajneru zagonetke u obliku tekstualne obavijesti s detaljima svake greške, odnosno mjesto greške i pravilom koje je prekršeno, a sva polja gdje se krše pravila označavaju se i vizualno.

Provjera se vrši prilikom dizajniranja i prilikom rješavanja zagonetke.


## Algoritmi za generiranje nasumične mreže

Algoritam prolazi svaki redak u mreži od vrha do dna i pokušava postaviti na nasumični stupac unutar tog retka svaku od znamenki koje se koriste u zagonetki, redom od najmanje do najveće. Nasumični odabir se ponavlja ukoliko bi postavljanje znamenke na tu poziciju, odnosno u taj redak i stupac, uzrokovalo ponavljanje znamenki, odnosno ukoliko se znamenka već nalazi u navedenom stupcu ili kutiji. 

## Algoritam za nasumičnu nadopunu djelomično zadane mreže

Ovaj algoritam je sličan algoritmu za generiranje nasumične mreže, ali preskače upis u ona polja kojima je dizajner zagonetke u sučelju prethodno definirao zadanu vrijednost te ih uzima u obzir kada provjerava pravila zagonetke i kada ispunjava preostala prazna polja. Ukoliko je dizajner prilikom definiranja zadanih vrijednosti polja počinio pogrešku i prekršio pravila sudoku zagonetke o ponavljanju znamenki, algoritam neće niti pokušavati nadopuniti zagonetku te će obavijestiti dizajnera, a pri ovome postupku se koristi prije navedeni algoritam za provjeru ispravnosti zadane zagonetke. 

## Sučelje za učitavanje zagonetke

Sučelje za učitavanje zagonetke omogućuje korisniku da iz odabrane datoteke u datotečnom sustavu učita objekt koji odgovara nekoj sudoku zagonetci i izmjeni postavljene vrijednosti zagonetke ili ju pokuša riješiti. 

Spremaju se vrijednosti u poljima sudoku zagoentke, a tamo gdje su praznine upisuje se 0. Učitanoj zagonetki će se aksnije pridružiti rješenje kada to bude potrebno. Sprema se u i broj koji predstavlja boju svakog polja, odnsono broj kutije, da bismo mogli rekonstruirati oblik zagonetke. 

## Sučelje za spremanje zagonetke 

Sučelje za spremanje zagonetke omogućuje dizajneru da stvorenu zagonetku spremi u obliku datoteke da bi netko na njoj kasnije nastavio raditi ili da bi ju netko kasnije rješavao. 

## Alat za učitavanje i spremanje zagonetki 

Ovaj alat kombinira sučelja za spremanje i učitavanje zagonetki, te daje mogućnost odabira imena datoteke i pregleda datotečnog sustava za vlastitim zagonetkama ili pretrage baze podataka gdje se nalaze primjeri zagonetki. 


## Sučelje za zadavanje zagonetke

Sučelje za zadavanje zagonetke sadrži opcije za odabir znamenke koju upisujemo u mrežu, te odabir opcije za upis praznog polja. Zadnja odabrana znamenka ili prazno polje se postavlja u polje mreže koje odaberemo.  

Odabir znamenke vrši se klikom na odgovarajuće gumbe na ekranu ili pritiskom na tipke sa znamenkama na tipovnici.

Polja gdje je upisana 0 prilikom dizajniranja (prazno) označavaju se <span style="color: rgb(250,0,0);">crvenom</span> bojom. Polja gdje je korisnik upisao broj koji ne krši pravila označuju se <span style="color: rgb(0,255,0);">zelenom</span> bojom. Polja gdje je korisnik upisao broj koji krši pravila označuju se <span style="color: rgb(250,200,0);">narančastom</span> bojom.

Polje u koje smo zadnje upisali vrijednost, te polja u istom retku i stupcu, biti će označena <span style="color: pink;">ružičastom</span> bojom, a polja koja imaju istu vrijednost kao zadnja upisana vrijednost biti će **podebljana** i *ukošena*.

Možemo provjeriti je li naša dizajnirana zagonetka rješiva i pročitati upute za rješavanju sve odjednom ili pratiti ih korak po korak uz stisak na gumb za nastavak.

Možemo spremiti zagonetku na kojoj radimo da ju drugi rješavaju ili da mi ili netko drugi nastavimo rad kasnije, a možemo i učitati spremljene zagonetke. 

## Alat za dizajn zagonetki

Alat za dizajn zagonetki koristi alat za učitavanje i spremanje zagonetki da bi dizajner mogao neometano prekidati ili nastavljati rad, a prilikom rada na zagonetki koristi se sučelje za zadavanje zagonetki koje implementira algoritme za provjeru ispravnosti zadane zagonetke i nasumičnu nadopunu djelomično generirane mreže.


## [Algoritmi za primjenu različitih tehnika rješavanja](tehnike.md)

Osnovne tehnike rješavanja koje koriste ljudi oslanjaju se na promatranje mreže, a za složenije zagonetke potrebno je na neki način zabilježiti sve moguće vrijednosti za određeno polje i izvesti iz njih zaključke i generalizacije za redak, stupac ili kutije. Tehnika jedinog kandidata upisuje vrijednost u polje ako je to jedini broj koji bi mogao biti na tom mjestu bez da se prekrše pravila zagonetke, jer niti jedno polje ne smije ostati prazno. Tehnika jedine pozicije upisuje vrijednost u polje ako je to jedino polje u retku, stupcu ili kutiji gdje bi ta vrijednost mogla biti bez da se prekrše pravila zagonetke, jer se svaka vrijednost mora pojaviti u svakom retku, stupcu ili kutiji. Osim ove vrijednost, koristi se tehnika skrivenog para i skrivene trojke, i tako dalje. Skriveni par i skrivena trojka su složenije tehnike koje pomažu u eliminaciji mogućih vrijednosti za određeno polje. Tehnika vidljivog para nalaže da, ukoliko dvije ćelije u istom retku, stupcu ili kutiji sadrže dvije iste vrijednosti kao jedine mogućnosti za upisivanje, te dvije vrijednosti možemo ukloniti kao mogućnosti iz svih ostalih ćelija tog retka, stupca ili kutije jer će one obje sigurno biti iskorištene u te dvije ćelije. Tehnika vidljive trojke, na sličan način nalaže da, ukoliko tri ćelije u istom retku, stupcu ili kutiji sadrže tri iste vrijednosti kao jedine mogućnosti za upisivanje, te tri vrijednosti možemo ukloniti kao mogućnosti iz svih ostalih ćelija tog retka, stupca ili kutije jer će one obje sigurno biti iskorištene u te tri ćelije. Ovo bi se analogno moglo proširiti na sve skupove od 4 ili čak x ćelija u istom retku, stupcu ili kutiji koje sadrže istih x vrijednosti kao jedine mogućnosti za upisivanje, ali zbog uobičajenih dimenzija zagonetke od 9 * 9 nema potrebe razmišljati o većim skupovima. Tehnike skrivenog para ili skrivene trojke itd. odnose se na pronalazak x ćelija koje su jedine koje u nekom retku, stupcu ili kutiji sadrže skup nekih x vrijednosti kao mogućnosti za upisivanje, ali uz tih x vrijednosti mogu biti i različite dodatne mogućnosti. Te dodatne mogućnosti mogu se ukloniti jer će tih x ćelija sigurno biti zauzeto navedenim vrijednostima iz skupa. Postoji još mnogo tehnika osim ovih, a terminologija nije usklađena te različiti izvori navode različite tehnike, koriste različite nazive za istu tehniku, a ponekada čak i koriste isti naziv za različite tehnike.

## Algoritam za pronalazak jedinstvenog rješenja

Zagonetka nije rješiva samo ako tehnikama rješavanja koje primjenjujemo nije moguće jednoznačno odrediti vrijednost svake ćelije. Tehnike se u ovom algoritmu primjenjuju redom od jednostavnijih prema složenijima, te se napreduje na iduću tehniku kada nije bilo moguće odrediti vrijednost niti jednog dodatnog polja prilikom prethodnog prolaska mrežom. Svaki puta kada korisnik ukloni ili doda vrijednost u neko polje u mreži, rješivost se mijenja. Ako su preostala neka prazna polja kojima nismo odredili vrijednost nakon što smo isprobali sve raspoložive tehnike rješavanja, možemo proglasiti zagonetku nerješivom. 

## Alat za dizajn rješive zagonetke

Alat za dizajn rješive zagonetke proširuje alat za dizajn zagonetki dodatkom mogućnosti slanja zahtjeva za provjerom postoji li jedinstveno rješenje zagonetke pomoću algoritma za pronalazak jedinstvenog rješenja. Ako ne postoji jedinstveno rješenje, dizajnera se obaviještava o tome i prikazuju se sve moguće vrijednosti za svaku od ćelija da bi se moglo prilagoditi zagonetku. 

## Algoritam za ocjenu težine zagonetke 

Različite tehnike za rješavanje sudoku zagonetki nemaju istu razinu složenosti. Neke od tehnika su trivijalne i pokušati će ih primijeniti, bez da unaprijed zna za njih, svatko tko prvi put ugleda sudoku zagonetku. Takve tehnike nose manje bodova težine nego neke složenije tehnike koje se mora naučiti iz tuđih savjeta ili sam otkriti kroz veće iskustvo rješavanja. Također vrijedi da je lakše primijeniti složenije tehnike nakon što smo se prvi put susreli s njima i svladali ih, jer nam postaju rutinske. Zbog toga naredne primjene složene tehnike, nakon njihove prve pojave u određenoj zagonetki, nose manje bodova težine. 

Ovakav način razmišljanja razvio se prvi put na forumima entuzijasta sudoku zagonetki, ali otada su ga primijenili mnogi popularni softverski rješavači sudoku zagonetki u svom kodu, naravno svaki uz neke svoje izmjene.

## Algoritam za uklanjanje i ponovno dodavanje polja

Kada se polje uklanja, to se čini u cilju dizajna teže zagonetke s više praznina. Veća je mogućnost da će zagonetka biti rješiva ako se uvijek uklanjaju polja u paru, ali tako da se zadrži osna simetričnost, odnosno da se ukloni polje i njegov parnjak tako da zamijenimo retke i stupce. Jedina iznimka je središnje polje u mreži koje ima isti broj retka i stupca, pa se u tom slučaju uklanja samo jedno polje. Lokacija s koje smo uklonili vrijednost te vrijednost koja je tamo bila upisana upisuju se na vrh stoga. Prilikom ponovnog dodavanja vrijednosti čita se i potom uklanja zapise s vrha stoga. Ovo je poželjno činiti ako zagonetka ima više od jednog mogućeg rješenja ili je složenija nego što smo namjeravali.

## Alat za dizajn zagonetki zadane težine

Alat za dizajn zagonetki zadane težine dodaje alatu za dizajn rješivih zagonetki mogućnosti uklanjanja nasumičnih polja i dodaje procjenu težine u provjeru rješivosti. Ukoliko postoji jedinstveno rješenje dizajner će biti obaviješten o težini koju je prilikom rješavanja algoritam dodijelio zagonetci uz pomoć drugih algoritama koji su navedeni u ostatku teksta.  Ukoliko korisnik odabere da se generira nasumična zagonetka, mora zadati težinu zagonetke u odgovarajućem rasponu, a ukoliko sam ručno uklanja polja i ispunjava polja da bi stvorio zagonetku, biti će mu prikazana težina zagonetke koju je stvorio. Postoji i mogućnost da uklonimo ili dodamo nasumične dvije vrijednosti polja iz zagonetke, na način da se uvijek uklanja par vrijednosti tako da zagonetka zadrži osnu simetričnost, a da se uvijek vraćaju vrijednosti u zadnji izbrisani par polja.


## Sučelje za definiranje granica blokova 

Korisnik nakon odabira dimenzija mreže dobiva sučelje koje sadrži navedenu mrežu podijeljenu na pravilne kutije, ali bez upisanih vrijednosti. Na raspolaganju ima odabir između više boja pa naizmjeničnim bojanjem pozadine ćelija odredi kutije u mreži. Trenutačno su na raspolaganju korisniku četiri boje pozadine, svaka sa pripadajućim gumbom. Boje su  <span style="color: black;">crna</span>, <span style="color: rgb(128, 128, 128)
;">siva</span>, <span style="color: rgb(64, 64, 64);">tamno siva</span> i <span style="color: rgb(192, 192, 192)
;">svjetlo siva</span>. Ukoliko korisnik odredi premaleni ili preveliki broj kutija ili u nekoj kutiji ima premalo ili previše znamenki, bit će mu onemogućen nastavak dizajniranja zagonetke dok ne ispravi grešku. Novi prozor će se pojaviti i upozoriti ga na to. 

## Algoritam za provjeru pripadnosti bloku

Svaki put kada korisnik promijeni boju neke ćelije, odnosno njenu pripadnost bloku, pokreće se algoritam koji se širi mrežom na sve susjedne ćelije u smjeru gore, dolje, lijevo ili desno koje su iste boje. Kada se širenje zaustavi, algoritam je saznao veličinu kutije te provjerava ima li kutija više ili manje ćelija nego što ima mogućih različitih znamenki u zagonetki. Ako je tome slučaj, zagonetka nije ispravno zadana i upozorava se korisnika na grešku. 

## Alat za dizajn različitih oblika zagonetki

Navedeni alat omogućuje dizajneru odabir dimenzija kvadratne mreže u kojoj želi dizajnirati zagonetku, te definiranje granica manjih kutija unutar osnovne mreže, koje mogu biti i nepravilne. Sučelje koje dizajner koristi implementira algoritam za provjeru pripadnosti bloku da spriječi netočno definiranje zagonetke s blokovima koji nisu ispravne veličine. 


## Sučelje za rješavanje zagonetke

Sučelje za rješavanje zagonetke sadrži mrežu u koju korisnik može upisivati svoje konačno rješenje ili više vrijednosti koje bi se mogle pojaviti u određenom polju. Korisnik odabire znamenku koju želi upisati, te odabire način rada, odnosno je li rješenje konačno ili samo pretpostavka. Ukoliko korisnik pokušava dodati znamenku kao pretpostavku, a ona već postoji kao pretpostavka ili konačno rješenje u tom polju, pretpostavka ili konačno rješenje će se ukloniti. Konačno rješenje se može ukloniti i dodavanjem drugih pretpostavki u isto polje, a u tom slučaju se konačno rješenje pretvara u jednu od pretpostavki. Pretpostavke su označene <span style="color: rgb(255, 255, 0);">žutom</span> bojom.

Korisnik tijekom rješavanja u svakom trenutku vidi težinu zagonetke koju rješava, vrijeme koje je proteklo od početka rješavanja, broj grešaka koje je učinio i broj polja za koja je zatražio pomoć. Greške su označene <span style="color: rgb(250, 200, 0);">narančastom</span> bojom.

Korisnik može i zatražiti pomoć u obliku otkrivanja jednog nasumičnog dodatnog polja ili dodatnog polja koje korisnik označi. Polje za koje tražimo pomoć mora biti jedno od polja u koje korisnik još nije upisao svoje konačno rješenje i koje nije zadano u početnoj zagonetki. Ovo je moguće samo prilikom rješavanja zagoentke, a ne prilikom zadavanja. Polja za koja je iskorišena pomoć imati će <span style="color: rgb(0, 0, 255);">plavu</span> pozadinu, a <span style="color: rgb(0, 255, 255);">svjetlo plavi (cyan)</span> tekst. Polja koja su zadana u početnoj zagonetki također imaju <span style="color: rgb(0, 255, 255);">svjetlo plavi (cyan)</span> tekst. 

Moduli rada se mjenjaju pomoću gumba koji uključuje ili isključuje bilješke te gumba koji uključuje označavanje polja za koje tražimo pomoć. Svaki od modula ostaje aktivan dok ne uključimo drugi modul. 

Kada se korisnik odluči na završetak igre, može zatražiti da mu se korak po korak postepeno objasni točno rješenje uz metode rješavanja te potvrdu za nastavak na idući korak, ali u tom slučaju rješenje korisnika se ne boduje. Kada se ova mogućnost odabere, više se ne može vršiti daljnje ispunjavanje zagonetke.

Može i odabrati bodovanje svog rješenja, gdje mu se otkriju njegov rezultat, njegove greške i točno rješenje uz metode rješavanja, ali rješenje je vidljivo odjednom, a ne korak po korak. Polja koja su zadana imaju <span style="color: rgb(0, 255, 255);">svjetlo plavi (cyan)</span> tekst, polja koja su točno rješena imaju <span style="color: rgb(0,255,0);">zeleni</span> tekst, a polja koju neispravno rješena ili su ostala prazna (ako imaju sadržaj sadrže samo pretpostavke) imaju <span style="color: rgb(255, 0, 255);">mahentljubičastia (magenta)</span> tekst. Kao i rješenju korak po korak, više se ne može vršiti daljnje ispunjavanje zagonetke.

## Algoritam za provjeru ispravnosti riješene zagonetke

Ovaj algoritam se sastoji od dva dijela, jedan od kojih služi kao nit vodilja korisniku za provjeru drži li se pravila prilikom rješavanja, a drugi služi da bi se evaluiralo korisnikovo rješenje na samom kraju rješavanja.

Prvi dio algoritma provjerava je li korisnik nekim od svojih unosa u polja prekršio osnovna pravila zagonetke. Ovaj algoritam ne provjerava hoće li ga korisnikova pretpostavka u budućnosti prisiliti da počini grešku ili izbriše neka polja i krene ispočetka, već se promatraju samo ona polja koja su otkrivena u postavljenoj zagonetki ili za koja je korisnik do tog trenutka unio svoje konačno rješenje. Ova provjera vrši se svaki put kada korisnik unese ili izbriše konačno rješenje iz nekog od polja, ali ne kada korisnik doda ili ukloni broj s popisa mogućih vrijednosti za određeno polje. ~~Prema preferencijama korisnika, prikaz rezultata ove provjere, zajedno s detaljnim informacijama o mjestu i vrsti greške, može se uključiti ili isključiti. U slučaju da je prikaz isključen, korisnik će vidjeti jedino negativne bodove koje je dobio kada prekrši pravila, a neće biti obaviješten o detaljima niti tekstom niti vizualno.~~ Svaki put kada korisnik upiše novo konačno rješenje u neko polje, bez obzira je li točno ili ne, mogućnosti za tu znamenku se uklanjaju iz polja u istom retku, stupcu i kutiji.

Drugi dio uspoređuje korisnikov unos u neispunjena polja mreže s točnim rješenjem zagonetke nakon što korisnik pretpostavlja da je točno riješio zagonetku ili je odlučio odustati i pogledati rješenje. Ukoliko se točno rješenje i uneseno rješenje razlikuju, ta će polja označena drugačije nego ona polja u koja je korisnik unio točno rješenje, ali u svim će poljima biti prikazana točna vrijednost rješenja. 

Prilikom obje provjere, polja u koja je korisnik unio samo popis mogućih vrijednosti, a ne konačno rješenje, tretiraju se kao da su prazna. 

Napomena: Moram još implementirati isključivanje prikaza rezultata provjere ispravnosti prilikom rješavanja, prije predaje rješenja. Za sada je prikaz, odnosno ova vrsta pomoći, uključen.

## Algoritam za bodovanje rješenja

Algoritam za bodovanje rješenja pokreće se ukoliko je korisnik uspješno riješio sudoku zagonetku u potpunosti točno. Korisnik će postići bolji rezultat ukoliko mu je bilo potrebno manje vremena za rješavanje, a postići će lošiji rezultat sa svakim potezom koji uzrokuje novo kršenje pravila u zagonetki koje nije bilo prisutno prije nego je povukao taj potez. 

Korisnik ima mogućnost, u zamjenu za određeni iznos kaznenih bodova, otkriti jedno ili više polja u mreži među onim poljima kojima još nije upisao svoje konačno rješenje i koja nisu zadana u početnoj zagonetki. 

Ukoliko korisnik, na primjer, dvaput za redom stisne na isto polje i postavi vrijednost koja se već pojavljuje u tom retku, stupcu ili kutiji, samo jednom će dobiti kaznene bodove, ali ukoliko upiše vrijednost koja krši pravila, ukloni ju, pa ju ponovno upiše, više puta će dobiti kaznene bodove. 

Ovime se sprječava višestruko kažnjavanje nenamjernog dvostrukog upisa vrijednosti koja uzrokuje grešku, ali se višestruko kažnjava krivo razmišljanje ako korisnik dođe više puta do istog netočnog zaključka. 


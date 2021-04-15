# [Tehnike rješavanja](https://www.sudokuoftheday.com/about/techniques/ "Sudoku Techniques - Sudoku of the Day")

## Motivacija

Relativno je lagano, uz današnju računalnu moć, isprobati sve moguće vrijednosti za sva polja zagoetke. Međutim, ljudi ne rješavaju probleme na taj način, te je da bismo educirali korisnika potrebno naći algoritam koji će raditi zaključke o mogućim vrijednostima polja na isti način kao i čovjek. 

Drugi problem, osim rješavanja zagonetke, postavlja se i kada ocjenjujemo težinu. Entuzijasti enigmatike primjetili su da ustaljena praksa brojenja praznih polja od kojih svako povećava složenosti za istu fiksnu vrijednost ne odražava činjenicu da neka polja zahtjevaju mnogo više zaključivanja i vremena da bismo ih ispunili. 

Ukoliko implemenitramo algoritam koji rješava zagonetku logičkim tehnikama, a ne pogađanjem, istovremeno rješavamo i problem bodovanja složenosti, jer svakoj tehnici možemo dodjeliti različitu težinu. Neke tehnike ne upisuju izravno vrijednosti u ćelije, već samo eliminiraju mogućnosti, pa neke od ćelija traže primjenu većeg broja tehnika prije nego ih ispunimo. Na ovaj način zagonetke s istim brojem praznina ne moraju imati istu težinu niti svaka praznina nosi isti broj bodova za složenost.

Da bi složenost bila što manja, odnosno da bi se zagonetka rješila što jednostavnijim tehnikama, prvo se primjenjuju jednostavnije tehnike, a tek ako one ne daju rješenje prelazi se na složenije. Ako pomoću složenije tehnike pronađemo neko rješenje ili eliminiramo neku mogućnost, ponovno se vraćamo na najjednostavnije tehnike jer one sada možda imaju mogućnost naći neko novo rješenje.

```java
public int sequence() {
		if (singlePosition() == 1) {
			difficulty.setText(String.valueOf(difficultyScore) + " Postoji jedinstveno rješenje");
			return 1;
		}
		if (singleCandidate() == 1) {
			difficulty.setText(String.valueOf(difficultyScore) + " Postoji jedinstveno rješenje");
			return 1;
		}
		if (candidateLines() == 1) {
			difficulty.setText(String.valueOf(difficultyScore) + " Postoji jedinstveno rješenje");
			return 1;
		}
		if (multipleLines() == 1) {
			difficulty.setText(String.valueOf(difficultyScore) + " Postoji jedinstveno rješenje");
			return 1;
		}
		if (nakedSet() == 1) {
			difficulty.setText(String.valueOf(difficultyScore) + " Postoji jedinstveno rješenje");
			return 1;
		}
		if (hiddenSet() == 1) {
			difficulty.setText(String.valueOf(difficultyScore) + " Postoji jedinstveno rješenje");
			return 1;
		}
		return 0;
	}
```

## Tehnike rješavanja i složenost

| Tehnika | Kod | Trošak za prvo korištenje | Daljni trošak po korištenju |
|---|---|---|---| 
Jedina znamenka (Single Candidate) | sct | 100 | 100 |
Jedina pozicija (Single Position) | spt | 100 | 100 |
Linija kandidata (Candidate lines) | clt |350 | 200 |
~~Dvostruki par (Double pairs)~~ | dpt | 500 | 250|
Više linija (Multiple lines) | mlt | 700 | 400 |
Ogoljeni par (Naked Pair) | dj2 | 750 | 500 |
Skriveni par (Hidden Pair) |us2 |1500 | 1200 |	
Ogoljena trojka (Naked Triple) | dj3 | 2000 | 1400 |
Skrivena trojka (Hidden Triple)	| us3 | 2400 | 1600 | 
~~X krilo (X-Wing)~~ | xwg | 2800 | 1600 |
~~Forsiranje ulančavanjem (Forcing Chains)~~ | fct | 4200 | 2100 |
Ogoljena četvorka (Naked Quad) | dj4 | 5000 | 4000 |
Skrivena četvorka (Hidden Quad)	| us4 | 7000 | 5000 |
~~Sabljarka (Swordfish)~~ |	sf4 | 8000 | 6000 |
Ogoljeni skup >4 | / | 5000 | 4000 |
Skriveni skup >4 | / | 7000 | 5000 |

Napomena 1: ~~Prekrižene~~ tehnike nisu još implementirane.

Napomena 2: Ogoljenim i skrivenim kupovima većim od 4 nisu dodjeljeni kodovi i troškovi na izvoru koji smo koristili, ali mi smo im odlučili dodjeliti težinu koju imaju ogoljene odnosno skrivene četvorke i brojimo njihova pojavljivanja zajedno sa  ogoljenim odnosno skrivenim četvorkama prilikom određivanja prvog korištenja. Ovo je moguće jer jedan algoritam traži sve ogoljene, a drugi sve skrivene skupove, bez obzira na njihovu veličinu. Naša aplikacija podržava promjenu veličine zagonetke, pa raste mogućnost za pojavu većih ogoljenih i skrivenih skupova i nekako smo ih morali uključiti da održimo rješivost. 
## [Jednostavne tehnike](simple.md)
Jedina znamenka (Single Candidate) i Jedina pozicija (Single Position) 
## [Srednje složene tehnike](medium.md)
## [Napredne tehnike](advanced.md)

## Tehnike pogađanja

Tehnike pogađanja ljudima oduziamju mnogo vremena, iako su za računalo jednostavne. Korisne su da bismo vrlo brzo na računalu prošli sve mogućnosti, ali ljudi koji rješavaju zagonetku vrlo rijetko za njima posežu. Postoje dvije tehnike pogađanja, čisto pogađanje i nishio. Nishio se odnosi na tehniku gjde pokušamo postaviti jednu vrijednost od više mogućih u ćeliju i nastavljamo rješavanje prije navedenim tehnikama sve dok ne dođemo do kršenja pravila. Onda izbrišemo sve što napisali nakon što smo uveli pretpostavku te rekonstruiramo mogućnosti koje su vrijedile za sva polja prije pogađanja, uz iznimku polja u kojem smo pogađali jer tamo eliminiramo onu mogućnost koja nas je dovela do kršenja pravila. Čisto pogađanje ne uključuje ovaj korak vraćanja unatrag te rekontrukcije i eliminacije mogućnosti, već samo brišemo netočna polja.


```java
public int guessing() {
    int[] t2 = new int[rows * cols];
    int[][] v2 = new int[rows * cols][cols];
    int u1 = unset;
    int d1 = difficultyScore;
    String s = solvingInstructions;
    for (int ix = 0; ix < rows; ix++){
        for (int jx = 0; jx < cols; jx++) {
            t2[ix * cols + jx] =  temporary[ix * cols + jx];
            for (int v = 0; v < cols; v++) {
                v2[ix * cols + jx][v] = possibilities[ix * cols + jx][v];
            }
        }
    }
    for (int val = 0; val < rows; val++) {
        for (int i = 0; i < rows; i++){
            int possible = 0;
            for (int j = 0; j < cols; j++) {
                if (possibilities[i * cols + j][val] == 1 && temporary[i * cols + j] == 0) {
                    possible++;
                }
                if (temporary[i * cols + j] == val + 1) {
                    possible = -1;
                    break;
                }
            }
            if (possible == -1) {
                continue;
            }
            if (possible == 0) {
                for (int ix = 0; ix < rows; ix++){
                    for (int jx = 0; jx < cols; jx++) {
                        temporary[ix * cols + jx] = t2[ix * cols + jx];
                        for (int v = 0; v < cols; v++) {
                            possibilities[ix * cols + jx][v] = v2[ix * cols + jx][v];
                        }
                    }
                }
                difficultyScore = d1;
                solvingInstructions = s;
                unset = u1;
                return 1;
            }
            int randomCol = ThreadLocalRandom.current().nextInt(0, cols);
            while (possibilities[i * cols + randomCol][val] == 0 || temporary[i * cols + randomCol] != 0) {
                randomCol = ThreadLocalRandom.current().nextInt(0, cols);
            }
            solvingInstructions += "Trying " + String.valueOf(val + 1) + " in cell (" + String.valueOf(i + 1) + ", " + String.valueOf(randomCol + 1) + ").\n";
            temporary[i * cols + randomCol] = val + 1;
            for (int v = 0; v < cols; v++) {
                possibilities[i * cols + randomCol][v] = 0;
            }
            possibilities[i * cols + randomCol][val] = 1;
            unset--;
            if (sequence() == 1) {
                return 0;
            }
        }
    }
    return 0;
}
```

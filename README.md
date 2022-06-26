# Návod na lámání zaheslovaných souborů v Kali Linux

Linuxová distribuce Kali Linux disponuje mnoha předinstalovanými nástroji, mezi které patří i ty pro lámání hesel.

Pokud je soubor zaheslovaný, tak je nejprve nutné hash hesla ze souboru extrahovat.

> Hash hesla je řetězec znaků, který vznikne tím, že vstupní data (heslo v textové podobě, tzv. plain text) jsou předána hashovací funkci, která je na daný řetězec převede. Jde o jednosměrnou funkci, kdy z řetězce na vstupu *H* vygeneruje hashovací funkce vždy stejný hash *H'*, přičemž nelze pomocí této funkce zpětně převést hash *H'* na původní řětězec *H*.

Mějme soubor ```secret.zip```, který chceme rozbalit, ale při pokusu o to jsme vyzvání k zadání hesla.
Jelikož heslo neznáme, tak se ho můžeme pokusit prolomit hrubou silou nebo slovníkovým útokem.
Nejprve však musíme získat hash hesla, který je obsažen v souboru ```secret.zip```. 

Nejprve je třeba otevřít Kali Linux terminál:

![Krok 1](https://cyberarena.utko.feec.vutbr.cz/img/tutorial/step1.png)

K extrakci hashe je možné použít nástroj **zip2john** v následujícím formátu:
```
zip2john secret.zip > hash.txt
```

Ve výstupním souboru ```hash.txt``` se nachází všechny extrahované hashe.

![Krok 2](https://cyberarena.utko.feec.vutbr.cz/img/tutorial/step2.png)

V dalším kroce je na čase začít hashe lamát pomocí nástroje **john**. 
Jedna z možností je útok hrubou silou (tzv. brute force attack), který spočívá v postupném kombinování různých znaků do řětězce, který je hashován a porovnáván s extrahovanými hashy ze souboru ```hash.txt```.
Tento typ útoku je značně neefektivní a vyžaduje velké množství času a výpočetního výkonu.
Při použití silného hesla může prolomení hashe trvat miliony i více let. Pokud chcete rychle zjistit, jak dlouho by trvalo běžnému počítači prolomit vaše heslo útokem hrubou silou, můžete využít nástroj na adrese https://www.security.org/how-secure-is-my-password/. 

Efektivnějším způsobem je tzv. slovníkový útok (dictionary attack), který lze sestavit podle určitého vzorce (např. dostupné informace o vlastníkovi souboru) nebo lze slovník sestavit z databází uniklých hesel.
Pokud uživatel použil slabé (málo komplexní nebo již dříve odhalené heslo) a takové heslo se nachází ve slovníku, tak je nalezena shoda jeho otisku s extrahovaným hashem ze souboru ```hash.txt```.

Jedním z nejznámějších slovníků pro lámání hesel je **rockyou**, ve kterém se nachází přes 14 000 000 uživatelských hesel.

Slovníkový útok se provádí následujícím příkazem:
```
john secret.zip --wordlist=/usr/share/wordlists/rockyou.txt
```

Pokud bylo lámání hesel úspěšné, tak se nalezená hesla zobrazí ve výstupu přímo v terminálu:

![Krok 3](https://cyberarena.utko.feec.vutbr.cz/img/tutorial/step3.png)

Pro zobrazení již prolomených hesel podle hashů ze zadaného souboru lze využít následující příkaz: 
```
john hash.txt --show
```

![Krok 4](https://cyberarena.utko.feec.vutbr.cz/img/tutorial/step4.png)

Obsah zaheslovaného archivu ```secret.zip``` lze nyní se znalostí hesla extrahovat pravým kliknutím na soubor a výběrem možnosti **Extract Here**. Otevřít aktuální místo v terminálu ve správci souborů je možné příkazem:
```
xdg-open .
```

![Krok 5](https://cyberarena.utko.feec.vutbr.cz/img/tutorial/step5.png)

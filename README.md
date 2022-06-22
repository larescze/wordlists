# Návod na lámání zaheslovaných souborů v Kali Linux

Linuxová distribuce Kali Linux disponuje mnoha předinstalovanými nástroji, mezi které patří i ty pro lámání hesel.

Pokud je soubor zaheslovaný, tak je nejprve nutné hash hesla ze souboru extrahovat.

> Hash hesla je řetězec znaků, který vznikne tím, že vstupní data (heslo v textové podobě, tzv. plain text) jsou předána hashovací funkci, která je na daný řetězec převede. Jde o jednosměrnou funkci, kdy z řetězce na vstupu A vygeneruje hashovací funkce vždy stejný hash A', přičemž nelze pomocí této funkce zpětně převést hash na původní řětězec.

Mějme soubor ```secret.zip```, který chceme rozbalit, ale při pokusu o to jsme vyzvání k zadání hesla.
Jelikož heslo neznáme, tak se ho můžeme pokusit prolomit hrubou silou nebo slovníkovým útokem.
Nejprve však musíme získat hash hesla, který je obsažen v souboru ```secret.zip```. 

K extrakci hashe je možné použít nástroj **zip2john** v následujícím formátu:
```
zip2john secret.zip > hash.txt
```

Ve výstupním souboru ```hash.txt``` se nachází všechny extrahované hashe.
V dalším kroce je na čase začít hashe lamát pomocí nástroje **john**. 
Jedna z možností je útok hrubou silou (tzv. brute force attack), který spočívá v postupném kombinování různých znaků do řětězce, který je hashován a porovnáván s extrahovanými hashy ze souboru ```hash.txt```.
Tento typ útoku je značně neefektivní a vyžaduje velké množství času a výpočetního výkonu.
Při použití silného hesla může prolomení hashe trvat miliony i více let.

Efektivnějším způsobem je tzv. slovníkový útok (dictionary attack), který lze sestavit podle určitého vzorce (např. dostupné informace o vlastníkovi souboru) nebo lze slovník sestavit z databází uniklých hesel.
Pokud uživatel použil slabé (málo komplexní nebo již dříve odhalené heslo) a takové heslo se nachází ve slovníku, tak je nalezena shoda jeho otisku s extrahovaným hashem ze souboru ```hash.txt```.

Jedním z nejznámějších slovníků pro lámání hesel je **rockyou**, ve kterém se nachází přes 14 000 000 uživatelských hesel.

Slovníkový útok se provádí následujícím příkazem:
```
john secret.zip --wordlist=/usr/share/wordlists/rockyou.txt
```

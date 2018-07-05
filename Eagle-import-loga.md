# Jak importovat loga do desky v Eagle

## Instalace software
Je t�eba st�hnout a nainstalovat program [Inkscape](https://inkscape.org/cs/),

D�le samoz�ejm� program [Eagle](https://www.autodesk.com/products/eagle/overview) - n�vod d�l�n ve verzi 9.0.1

## P��prava dat
Loga, kter� chceme importovat je t�eba m�t p�evedeny do form�tu SVG
D�le doporu�uji, ale nen� to nutn�, vyexportovat si n�hled desky. Bude se n�m potom l�pe umis�ovat logo, nap�. n�m potom nehroz� um�st�n� loga na sou��stku apod.

### Export desky z Eagle a import do Inkscape
Pro export d�me `File` -> `Print (Ctrl+P)` a otev�e se n�m okno, kde mus�me nastavit n�sleduj�c� parametry:

![Print to PDF](img/eagle_logo/1.png)

Pot� importujeme vznikl� PDF do Inkscape, kde jsme si vytvo�ili nov� dokument, `File` -> `Import Ctrl+I` a vybereme soubor PDF, odklik�me v�echny dialogy a po�k�me ne� se vlo��

V�sledek by m�l vypadat n�jak takto:

![PDF import Inkscape](img/eagle_logo/2.png)

## �prava dokumentu a import loga

Abychom mohli nastavit spr�vnou velikost dokumentu, mus�me zjistit rozm�ry desky. To provedeme tak, �e si otev�eme desku v Eagle a vpravo klikneme na `MANAFACTURING` -> `Board` a rozklikneme roletku `Board`. Zde n�s zaj�maj� parametry Width (���ka) a Height (v��ka).

![Eagle velikost desky](img/eagle_logo/3.png)

Pot�ebujeme upravit velikost dokumentu, abychom m�li vzor desky a loga 1:1. Otev�eme vlastnosti dokumentu `File` -> `Document Properties (Shift+Ctrl+D)`. Zde zhruba uprost�ed najdeme nastaven� `Custom size`, zde nastav�me hodnoty, kter� jsme z�skali v p�edchoz�m kroce:

![Inkscape Document Properties](img/eagle_logo/4.png)

Jakmile m�me nastavenou stejnou velikost dokumentu, jak� je velikost na�� desky, tak mus�me desku posunout tak, aby jej� okraje spl�valy s okraji dokumentu. To provedeme tak, �e vybereme desku a nastav�me jej� pozici v sou�adnicov�m syst�mu na `X = 0  Y = 0`:

![Position setting](img/eagle_logo/5.png)

Nyn� m�me spr�vn� nastaven dokument, importovanou desku jako p�edlohu a m��eme p�istoupit k vlo�en� samotn�ch log.

Logo vlo��me kliknut�m na `File` -> `Import (Ctrl+I)` a vybereme SVG soubor s logem. Jakmile m�me vlo�eno, je t�eba nastavit vhodnou velikost loga a jeho um�st�n�.

![Logo Import](img/eagle_logo/6.png)

Jakmile m�me logo vhodn� um�st�n� je pot�eba odstranit desku, kterou jsme pou�ili jako �ablonu.

![Inkscape only logo](img/eagle_logo/7.png)

## P�evod loga do scriptu pro Eagle

Logo budeme vkl�dat jako script do Eaglu, kter� n�m vytvo�� body a n�sledn� polygony, kter� budou tvo�it v�sledn� obrazce. Pro generov�n� scriptu pou�ijeme [tuhle](https://gfwilliams.github.io/svgtoeagle/) str�nku:

![SVGToEagle](img/eagle_logo/8.png)

Zde je pot�eba vyplnit pole `Eagle CAD Layer`, do tohoto pole zadejte n�zev vrstvy v Eagle, do kter� chcete v�sledn� loga vlo�it, j� pou�iji `bDocu`

D�le je t�eba nastavit `SVG scale factor` na hodnotu `1`, ale je mo�n� pou��t i jin� hodnoty, z�le�� na tom, jak� jste si nastavili m���tko dokumentu v Inkscape, j� m�l 1:1 tud� vol�m 1

Je mo�n� zde v�sledn� obrazec horizont�ln� oto�it, ale nedoporu�uji to, mnohem lep�� je loga oto�it je�t� v Inkscapu. Pot�eba ot��et loga je, kdy� chceme loga vkl�dat na spodn� stranu desky.

No a nakonec vybereme soubor SVG, ve kter�m m�me n� dokument, kter� m� nastaveny spr�vn� rozm�ry a obsahuje pouze logo.

Takhle n�jak m��e vypadat v�sledek:

![Bad result](img/eagle_logo/9.png)

Vid�me ale, �e je barevn� vypln�na i p�vodn� b�l� plocha uprost�ed loga, kterou by Eagle bral jako sou��st loga a tud� by z cel�ho loga byl vid�t jen kruh.

Mus�me tedy opravit uzav�en� obrysy, kter� maj� obsahovat n�jakou "d�ru". Tohoto lze nejjednodu�eji dos�hnout tak, �e si v Inkscapu vytvo��me miniaturn� obd�ln�k, kter� n�m vytvo�� "pr�tok" do d�ry v uzav�en�m obrysu:

![Inkscape make rectangle](img/eagle_logo/10.png)

Jakmile m�me vytvo�eno, zvol�me n�stroj `Edit paths by nodes (F2)` a vybereme obrys, do kter�ho m� b�t ud�l�na d�ra a n�mi vytvo�en� obd�ln�k (v tomto p��pad� �erven�) a zvol�me n�stroj `Path` -> `Difference (Ctrl+-)`, tento postup samoz�ejm� opakujeme pro v�echny uzav�en� obrysy obsahuj�c� d�ru, kter� se zde vyskytuj�:

![Inkscape Difference](img/eagle_logo/11.png)

Nyn� m�me v obrysu vytvo�en mal� "pr�tok" d�ky kter�mu u� se n�m vytvo�� korektn� data pro Eagle:

![Inkscape succesful difference](img/eagle_logo/12.png)

Zkontrolujeme na n�hledu, �e se n�m v�echny loga korektn� p�evedly. Jestli je v�echno v po��dku, tak zkop�rujeme vygenerovan� text v poli nad n�hledem nap�. do pozn�mkov�ho bloku (j� pou�iju [PSPad](http://www.pspad.com/cz/)) a ulo�te jej jako soubor s koncovkou `.scr`.

![Succesful convert](img/eagle_logo/13.png)

Nyn� v�sledn� soubor importujeme do Eagle `File` -> `Execute Script...`, otev�e se n�m n�sleduj�c� okno:

![Eagle Execute Script](img/eagle_logo/14.png)

Zde vybereme n� soubor `.scr` a potvrd�me v�b�r. Nyn� ji� m�me na�e logo v Eaglu ve vrstv� �.52 s n�zvem bDocu jako polygon a nen� probl�m jej p�i v�rob� natisknout na desku jako popisek.

![Final logo](img/eagle_logo/15.png)
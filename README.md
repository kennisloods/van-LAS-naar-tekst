# van-LAS-naar-tekst
Vertaalt een selectie van LIDAR-gegevens in LAS-formaat naar een tekstformaat zodat deze in AutoCAD kunnen worden ingelezen.

## toelichting

R&W verwerkt vaak LIDAR-gegevens in LAS-formaat. Het LAS-formaat bevat een puntenwolk met daarin de gemeten waarnemingen.

Een aantal paketten, zoals AutoCAD, kan dit formaat niet, of niet optimaal verwerken. Daarom is een ETL-script geschreven dat de puntenwolk voor een bepaald aandachtsgebied omzet naar losse x,y,z-coördinaten met de daarbijbehorende kleurwaarde in R,G,B.

Het vertaalde bestand is een spatiegescheiden bestand met de volgdende velden:

```
<X> <Y> <Z> <R> <G> <B>
```

De coördinaten zijn in RD (EPSG:28992) en afgerond op cm.

## bronnen

  * ad-hoc las-bestand

## bewerking
1. Inlezen te vertalen puntenwolk in .las-formaat,
2. filteren van de puntenwolk op het interessegebied,
3. puntenwolk omzetten naar losse puntfeatures met bijbehorende kleurwaarde,
4. wegschrijven naar een textbestand.

## gebruik van het script

Het script staat hier: `K:\GEODATA\Kennisloods\Tools\VanLasnaarTekst\vanLASnaarTekst.fmw`.

De command line aanroep is:

```
"C:\Program Files (x86)\FME\fme.exe" K:\GEODATA\Kennisloods\Tools\VanLasnaarTekst\vanLASnaarTekst.fmw
--P_BRON_LAS "K:\SO\Algemeen\HansvanderBoor\Test Puntenwolk\Brienenoordbrug.las"
--P_DOEL_CSV "P:\Geo_Data\SO\RW\B_Stad\Kennisloods\Projectdata\Data\04_Output\maatwerk\vanLASnaarTekst"
--P_COORDSYS "EPSG:28992"
--P_MINX "96948"
--P_MINY "435006"
--P_MAXX "97048"
--P_MAXY "435235"
--P_CLIP_LAS_TO_ENVELOPE "YES"
--P_LOGBESTAND "K:\GEODATA\Kennisloods\Tools\VanLasnaarTekst\log\vanLASnaarTekst.log"
```

De betekenis van de parameters is:

| parameter | betekenis |
| --------- | --------- |
| `P_BRON_LAS`             | Locatie van het te vertalen LAS-bestand. |
| `P_DOEL_CSV`             | Folder waarnaar het tekstbestand moet worden weggeschreven <sup>1</sup>. |
| `P_COORDSYS`             | Te hanteren coordinaatstelsel |
| `P_MINX`                 | Meest westelijke ordinaat van het interessegebied in RD. |
| `P_MAXX`                 | Meest oostelijke ordinaat van het interessegebied in RD. |
| `P_MINY`                 | Meest zuidelijke ordinaat van het interessegebied in RD. |
| `P_MAXY`                 | Meest noordelijke ordinaat van het interessegebied in RD. |
| `P_CLIP_LAS_TO_ENVELOPE` | Indicatie of het bronbestand op het interessegebied moet worden afgesneden. Dat versnelt de extractie. |
| `P_LOGBESTAND`           | Locatie van het logbestand. |

<sup>1</sup> De bestandsnaam is: `<datum>_<invoernaam>_XYZ.txt`. Bijvoorbeeld: de gegevens uit `Brienenoordbrug_XYZ.las` werden op 29 mei 2018 vertaald naar `20180529_Brienenoordbrug_XYZ.txt`.

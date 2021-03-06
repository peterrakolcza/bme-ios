# `iOS` alapú szoftverfejlesztés - Labor `02`

## A labor témája
* [iCalculator](#icalculator)
    * [Alap nézetek használata](#alap-nezetek-hasznalata)
    * [`Outlet`ek és akció metódusok](#outletek-es-akcio-metodusok)
    * [Debugolás alapok](#debugolas-alapok)
    * [Stack View](#stack-view)
* [Önálló feladatok](#onallo)
    * [Több számológép művelet támogatása](#tobb-szamologep-muvelet)
    * [Alkalmazás ikon beállítása](#app_icon)
* [Swift Coding Challenge](#scc)
* [Szorgalmi feladatok](#szorgalmi)
    * [Korábbi számítások (*`History` nézet*)](#korabbi-szamitasok)


A labor során egy egyszerű számológép alkalmazást készítünk el, melyen keresztül megismerkedünk az `iOS`-es felhasználói felület készítésének alapjaival.

## iCalculator <a id="icalculator"></a>
> Hozzunk lére egy új `Single View App`ot, `iCalculator` névvel a `labor_03` könyvtárba!
 
> A `Target` beállítások alatt módosítsuk a `Devices` beállítást `iPhone`-ra.

<p align="center"> 
<img src="img/01_device_iphone.png" alt="01" style="width: 75%;"/>
</p>

*Egy `Storyboard` összefoglalja az alkalmazás felhasználói felületének több "jelenetét". Minden jelenethez tartozik egy nézet (`View`) hierarchia és egy `View Controller` (`UIViewController`), ami az adott nézet hierarchiát menedzseli (fogadja a nézetek eseményeit és konfigurálja a nézeteket).*

*Lényegében a `View Controller`hez tartozó egyedi osztály forráskódjában tudjuk "hozzátenni a logikát" az `Interface Builder`ben megtervezett nézet hierarchiához.*

*Egy alkalmazásban több `Storyboard` is lehet, a projekt beállítások között lehet megadni melyik legyen az, melyet megjelenít a program indításkor.*


> Első lépésként válasszuk ki a `Main.storyboard`ot, majd a `File Inspector`ban kapcsoljuk ki a `Use Trait Variations` és a `Use Safe Area Layout Guides` beállításokat!

<p align="center">
<img src="img/02_trait_variations_safe_area.png" alt="02" style="width: 40%; align: center;"/>
</p>

*Egy `Trait` leír egy környezeti tulajdonságot, mint például, hogy támogatott-e a `Force Touch` vagy sem. A `Trait Collection` egy `Dictionary`, ami az alkalmazás aktuális környezeti tulajdonságait `Trait`ek és a hozzájuk tartozó értékek formájában tárolja.*

*A `Layout Traits`ben találhatjuk meg többek között a `Size Classes` `Trait`et, ami lehetővé teszi, hogy különféle képernyőméretekre (készülék kategóriákra) és tájolásokra (álló/fekvő) egyetlen nagy közös UI tervet készítsünk (egy `Storyboard`ot).*

*Ez a `Trait Variations` felel meg az `Adaptive Layout`nak, amivel később fogunk megismerkedni. Ha kikapcsoljuk, akkor a `Storyboard` azt feltételezi, hogy a felületet csak egyetlen "eszköz típusra" definiáljuk (pl. `iPhone`-ra).*

*A `Safe Area Layout Guide`-ok a képernyő egy olyan területét határolják, melyet nem takar semmilyen rendszerhez tartozó elem, vagy egyéb tartalom. Az `iOS 11`-ben vezették be, a `Top` illetve `Bottom Layout Guide`-ok helyett.*


## Alap nézetek használata <a id="alap-nezetek-hasznalata"></a>

> Hozzunk létre a `Main.storyboard`ban, a `ViewController`en belül lévő `View`-ban **2 db** `UITextField`et, **2 db** `UILabel`t és **1 db** `UIButton`t. Ezeket az elemeket `Object Library`ből érhetjük el.

<p align="center">
<img src="img/26_object_library.png" alt="26" style="width: 40%;"/>
</p>

Valami ilyesmit kellene kapnunk

<p align="center">
<img src="img/03_starter_ui.png" alt="03" style="width: 50%;"/>
</p>

*A nézetek elrendezéséhez most még abszolút koordinátákat használunk, ami miatt elforgatott, vagy eltérő méretű kijelzőn a felület "helytelenül" fog megjelenni (nem középen lesz amit középre rakunk).  Ezt a problémát oldja meg az `Auto Layout`, melyről később fogunk tanulni.*

Lehetőség van rá, hogy értelmes neveket adjunk az egyes felületelemeknek. Az átnevezéshez válasszunk ki egy elemet a bal szélső listából, majd az `Enter` megnyomása után átnevezhetjük. 

<p align="center">
<img src="img/04_named_views.png" alt="04" style="width: 30%;"/>
</p>

> Rendezzük középre a két `UILabel` szövegét az *`Alignment`* property módosításával!

![](img/05_alignment.png)

> Állítsuk át az alsó `UILabel` háttérszínét szürkére a *`Background`* property módosításával!

<img src="img/05b_label_background.png" alt="05b" style="width: 50%;"/>

> Teszteljük a felületet a szimulátorral!

> Hozzunk létre egy `IBOutlet`et `ViewController`ben az egyik `Text Field`hez! 

```swift
@IBOutlet weak var inputTextFieldA: UITextField!
```


## `Outlet`ek és akció metódusok <a id="outletek-es-akcio-metodusok"></a>

**Mi az `Outlet`?**
*Property, amin keresztül hivatkozhatunk egy, a grafikus szerkesztőben (`Interface Builder`) létrehozott, felületi elemre.*

**Miért van itt `weak` property?**
*Mert az `Outlet`ekhez tartozó nézeteket a szülő nézeteik vagy a gyökérnézet esetén a `View Controller` birtokolja. Ha egy nézet kidobja a gyerekeit, akkor azt várjuk, hogy azok törlődjenek (szabaduljon fel az általuk foglalt memóriaterület), és ha `strong` referencia lenne rájuk az `Outlet`ek miatt, akkor ez nem történne meg.*

> Kössük be a `Text Field`et az `Outlet`re, a `Storyboard`ból a `Connections Inspector`t használva!

<p align="center">
<img src="img/06_outlet.png" alt="06"/>
</p>

> Módosítsuk kódból, a `viewDidLoad` metódusból a `Text Field` értékét!

```swift
override func viewDidLoad() {
  super.viewDidLoad()
  
  inputTextFieldA.text = "13"
}
```

<!-- Mutogassuk meg, hogy milyen szépen mutatja a kódban és a UI editorban, hogy melyik Outlethez melyik UI elem tartozik. Mind a sorok elején lévő kis jellel, mint a Connection inspectorban. -->
> Váltsunk `Assistant Editor` nézetbe (`⌘+⌥+Enter`), majd hozzunk létre `Outletek`et az egyes nézetekhez `Jobbklikk-Drag&Drop`-pal (kivéve a `+` jeles `UILabel`hez)!

![](img/07_create_outlet.png)

```swift
@IBOutlet weak var inputTextFieldA: UITextField!
@IBOutlet weak var inputTextFieldB: UITextField!
@IBOutlet weak var resultLabel: UILabel!
```

<!-- Meséljünk a generált eseményekről és az elkapásukra szolgáló akció metódusokról. -->
> Adjunk hozzá egy akciót is a gomb (`calculateButton`) `Touch Up Inside` akciójához (`calculateButtonTouchUpInside`) és valósítsuk meg a gomb lenyomásakor meghívódó metódust!

```swift
@IBAction func calculateButtonTouchUpInside(_ sender: Any) {
  let numberFormatter = NumberFormatter()

  if
    let textA = inputTextFieldA.text,
    let textB = inputTextFieldB.text,
    let a = numberFormatter.number(from: textA)?.doubleValue,
    let b = numberFormatter.number(from: textB)?.doubleValue {

    resultLabel.text = "\(a + b)"
  }
}
```

*A többsoros `if` valójában négy `optional binding` egymás után végrehajtva. `textA`, `textB`, `a` és `b` is mind új változók, melyek csak az `if`-hez tartozó blokkon belül láthatók. Az ilyen, vesszővel elválasztott, több tagú `if`-eknél az egyes feltételek sorban értékelődnek ki és ha valamelyik feltétel nem sikerül, a többit már nem ellenőrzi a fordító ("`short-circuit` kiértékelés”).*

> Próbáljuk ki a félkész számológépet!

> Állítsuk át a `Storyboard`ban, a `Text Field`ek `Keyboard` attribútumát `Decimal Pad`ra. Ezzel elérjük, hogy egy csak számokat tartalmazó billentyűzet jelenjen meg!

![](img/08_decimal_number_pad.png)

> Ezek után adjuk a `calculateButtonTouchUpInside` akció metódusunkhoz a következő két utasítás, melyek hatására el fog tűnni a billentyűzet a képernyőről (amennyiben éppen aktív/látható)!

```swift
inputTextFieldA.resignFirstResponder()
inputTextFieldB.resignFirstResponder()
```

*A `first responder` az az objektum (esetünkben nézet), mely éppen "fókuszban van" és először fogadja a felhasználótól érkező billentyű eseményeket. Ha egy `Text Field` nézet lesz a `first responder`, akkor automatikusan megjelenik a billentyűzet. Ha "lemondunk" a `first responder` státuszról a `resignFirstResponder()` metódus meghívásával, akkor eltűnik a billentyűzet is.
Ha nem tudjuk pontosan hogy épp ki a `first responder`, akkor elegánsabb megoldás a `view.endEditing(true)` hívás, mely végiglépked a nézet gyereknézetein és mindegyiknél lemond a `first responder` státuszról.*

> Írjuk át a kódot, hogy egyszerre mondjuk le az összes `Text Field` `first responder` státuszáról!

```swift
view.endEditing(true)
```

A probléma most már csak az, hogy a feljövő virtuális billentyűzetet csak úgy tudjuk eltüntetni, ha megnyomjuk az számológép gombját. Az elegáns megoldás `Decimal Pad` billentyűzet eltüntetésére, hogy a gyökér nézet hátterét bárhol megérintve eltűnjön a billentyűzet.

Ahhoz, hogy a gyökér nézet megérintését le tudjuk kezelni, le kell cserélnünk az osztályát `UIView`-ról, `UIControl`ra (hiszen csak `UIControl` és belőle származó osztályok tudnak előre definiált eseményeket generálni). 
> Az `Interface Builder`ben kiválasztva a `ViewController` gyökér nézetét, az `Identity Inspector`ban választhatjuk ki hozzá a konkrét osztályt, itt váltsunk `UIControl`ra!
<p align="center">
<img src="img/09_selected_view.png" alt="09" style="width: 30%;"/>
</p>

<p align="center">
<img src="img/10_identity_inspector.png" alt="09" style="width: 30%;"/>
</p>


> Ezek után a `Connections inspector`ban a `Touch Up Inside` eseményhez rendeljünk hozzá egy `backgroundTouchUpInside` nevű metódust!

```swift
@IBAction func backgroundTouchUpInside(_ sender: Any) {
  view.endEditing(true)
}
```

*He szeretnénk, hogy eltűnjön a már beírt szöveg a `Text Field` kiválasztásakor, akkor kapcsoljuk be a `Clear when editing begins` opciót az `Attributes Inspector`ban. Itt adhatjuk meg azt is, hogy a törlés (`Clear`) gomb mikor jelenjen meg.*

![](img/11_clear_text_field.png)


## Debugolás alapok <a id="debugolas-alapok"></a>
*Debug breakpointok* a kódsorok elé klikkelve hozhatók létre, illetve itt kapcsolhatók ki/be. 

![](img/29_add_breakpoint.png)

Próbáljuk ki a *breakpointot*. Futtassuk az alkalmazást és léptessük, hogy lássuk ahogy példányosodnak és értéket kapnak az objektumok. 

![](img/30_test_breakpoint.png)

Kapcsoljuk ki a hozzáadott *breakpointot* és vezessünk be egy hibát a kódba, adjuk hozzá a `UITextField`et saját magához (nyilván ez helytelen művelet).

```swift
override func viewDidLoad() {
super.viewDidLoad()

inputTextFieldA.addSubview(inputTextFieldA)
inputTextFieldA.text = "13"
}
```

> Indítsuk el az alkalmazást és figyeljük meg milyen, amikor egy `Exception` keletkezik.

Először a konzolt keressük meg és ezen belül görgessünk oda, ahol az exception leírása olvasható (ez mindig a stack trace előtt található, a konzol log vége felé).

`'2018-08-31 10:40:26.637447+0200 iCalculator[11108:797169] *** Terminating app due to uncaught exception 'NSInvalidArgumentException', reason: 'Can't add self as subview'`

A leírás itt elég jó, de azért próbáljuk megnézni pontosan melyik kódsor is dobta hibát. Nyissuk meg a `Debug Navigator`t, ha nem lenne nyitva (`⌘+7`). A probléma, hogy az `AppDelegate` van megjelölve mint az utolsó lefutó kódot tartalmazó osztály. Ez azért van, mert alapesetben, kivételek keletkezése után, ha azt a kivételt nem kapja el semmilyen köztes kód, végül itt, az `AppDelegate` egy (általunk nem látható) kódrészlete lövi ki a szálat.

![](img/31_breakpoint.png)

Szerencsére van lehetőség rá, hogy az exception keletkezéséhez legközelebbi kódrészletnél álljon meg a futás, ehhez Egy `Exception Breakpoint`ot kell létrehozni (ezt minden projektben egyetlen egyszer kell csak megtenni).

![](img/32_new_breakpoint.png)

> Futtassuk újra az alkalmazást és most már láthatjuk, hogy hol is keletkezett a hiba. Azonban érdemes azt is megfigyelni, hogy ilyenkor még nem jelenik meg a konzolon az exception leírása. Ahhoz, hogy azt megkapjuk "tovább kell engedni" a debuggert a `Continue Program Execution` gomb néhányszori megnyomásával (gyakran nem elég egyszer megnyomni).

![](img/33_xcode_continue_program_execution.png)


## Stack View <a id="stack-view"></a>

Ha most kipróbáljuk az alkalmazást egy, a grafikus tervezőfelülettel nem megegyező méretű szimulátoron, azt fogjuk tapasztalni, hogy a nézetek nem középre rendezve jelennek meg. Ennek oka, hogy jelenleg abszolút koordinátákkal adtuk meg a nézetek méretét és elhelyezkedését, ami nem változik ha eltérő méretű kijelzőt használunk.

A legegyszerűbb megoldás az elemek dinamikus elrendezéséhez az `iOS 9`-ben debütáló `Stack View`. 

> Válasszuk ki az összes nézetet, majd nyomjuk meg a `Stack` gombot a szerkesztő nézet jobb alsó sarkában! 

<p align="center">
<img src="img/12_stack_button.png" alt="12" style="width: 25%;"/>
</p>

Először valami hasonló, nem túl jól kinéző felületet fogunk látni.

<p align="center">
<img src="img/13_initial_stack_view.png" alt="13" style="width: 75%;"/>
</p>

> Ahhoz, hogy a nézetek egyenletesen helyezkedjenek el, válasszuk ki a `Stack View`-t és állítsuk be a *`Distribution`* paraméterét __*`Equal Spacing`*__-re az `Attributes Inspector`ban!

![](img/14_stack_view_distibution.png)

> Továbbra is a `Stack View` beállításai között állítsuk az *`Alignment`* paramétert __*`Fill`*__-re! Ezzel azt érjük el, hogy a `Stack View`-ban lévő nézetek kitőltik a rendelkezésre álló szélességet.

![](img/15_stack_view_alignment.png)

Utolsó lépésként még be kell állítanunk, hogy maga a `Stack View` dinamikusan legyen elrendezve a képernyőn. Ehhez az `Auto Layout`ot fogjuk használni, amiről a következő laboron még bőven lesz szó.
> Most egyelőre csak válasszuk ki a `Stack View`-t és a `Pin` opciót, majd széleit csatoljuk hozzá a szülő nézetéhez, tetejét a `Top Layout Guide`-hoz és magasságát állítsuk fixen `300`-ra!

<p align="center">
<img src="img/16_auto_layout.png" alt="16" style="width: 40%;"/>
</p>

# Önálló feladatok <a id="onallo"></a>

## Több számológép művelet támogatása <a id="tobb-szamologep-muvelet"></a>
> Töröljük ki a `+` jelet megjelenítő `UILabel`t és a helyére húzzunk be egy `Segmented Control`t (`UISegmentedControl`)!

<p align="center">
<img src="img/20_segmented_control.png" alt="20" style="width: 30%;"/>
</p>

> Módosítsuk a `Segmented Control`t, hogy **`3`** szegmensből álljon (`Attributes Inspector`ban a *`Segments`* attribútum), majd írjuk át ezek szövegét (dupla klikk a szerkesztőben, vagy `Attributes Inspector`ban az adott `Segment` `Title` property-jének módosításával) a szorzás, osztás és összeadás müveletknek megfelelő jelekre!

<p align="center">
<img src="img/21_segmented_control_segment_title.png" alt="21" style="width: 30%;"/>
</p>

<p align="center">
<img src="img/22_segmented_control_operators.png" alt="22" style="width: 50%;"/>
</p>

> Az `Assistant Editor` nézetre váltva, kössük be a `Segmented Control` `Value Changed` eseményét egy új `operationSelectorValueChanged:` nevű akció metódusra a `ViewController` osztályban!

```swift
@IBAction func operationSelectorValueChanged(_ sender: Any)
```

> Vegyünk fel egy új enumerációt a `ViewController` osztályon belül, mely az éppen kiválasztott számológép műveletet jelöli!

```swift
enum OperationType {
  case add
  case multiply
  case divide
}
```

> Továbbá vegyünk fel egy privát tagváltozót az osztály implementációs blokkjába!

```swift
private var operationType = OperationType.add
```

Érdemes a `Segmented Control` kezdeti értékét is megadni (bár esetünkben ez pont helyesen `0`-ra van inicializálva, de nem árt rászokni, hogy mindig inicializáljunk). 
> Ehhez fel kell vennünk egy `Outlet`et a `Segmented Control`hoz (pl. `operationSelector`), majd a `View Controller` `viewDidLoad` metódusában az `Outlet` `selectedSegmentIndex` property-jét `0`-ra állítani.

```swift
operationSelector.selectedSegmentIndex = 0
```

> Majd implementáljuk az `onOperationSelectorValueChanged` metódust!

```swift
@IBAction func operationSelectorValueChanged(_ sender: AnyObject) {
  switch operationSelector.selectedSegmentIndex {
  case 0:
    operationType = .add
  case 1:
    operationType = .multiply
  case 2:
    operationType = .divide
  default:
    return
  }
}
```

Ezek után már csak annyi dolgunk van, hogy a kiszámítást elindító gomb megnyomásakor meghívódó akció metódusban `operationType` tartalmának megfelelő műveletet végezzünk (itt is érdemes egy állapotgépet használni).

## Alkalmazás ikon beállítása <a id="app_icon"></a>

> Töltsük le a `res` mappában lévő [képet az alkalmazás ikonjáról](res/app-icon.png).  

> Válasszuk ki a projekt fájlai közül az `Assets.xcassets` nevű asset katalógust.

![](img/27_assets_xcasset.png)

> Nyissuk meg az [alábbi linket](https://appicon.co), töltsük fel az ikont majd generálást követően töltsük le a generált ikonokat.A kicsomagolást követően az `Assets katalógusban` lévő `AppIcon` nevű asset-etet írjuk felül a letöltött `Assets.xcassets` mappában lévő `AppIcon.appiconset`-tel.

![](img/28_assets_appicon.png)

---

*Az asset katalógusok az alkalmazás képfájljainak csoportosítására szolgálnak. Egy `iOS` alkalmazásban egy képfájlból (ikonból) gyakran több különféle felbontású verzió is kell, ezeket az összetartozó képeket tudjuk hatékonyan együtt kezelni az asset katalógusok segítségével. 
Pl. az `AppIcon` azonosítóhoz hozzárendelhetjük a menüben (Springboard) megjelenő `120x120` pixeles változatot, illetve a keresésnél (Spotlight) megjelenő kisebb változatokat is.
Ha nem adunk meg az egyik típushoz ikont, akkor a rendszer megpróbálja azt a megadott ikonból legenerálni (átméretezéssel), de ez a legtöbb esetben nem fog hibátlan eredménnyel járni.*



*Érdemes megjegyezni, hogy `iPhone`-on és `iPad`en eltérő méretű az alkalmazások ikonja.*


> A szimulátorban ellenőrizzük, hogy megjelenik-e az új alkalmazás ikon!

# Swift Coding Challenge <a id="scc"></a>

A labor következő részében egy kódolási feladatot fogunk megoldani a Swift nyelv használatával Xcode Playgroundban. A következő laborok végén is szerepelni fog egy-egy ilyen feladat, hogy megismerhessünk több fontos nyelvi elemet is. A megoldások során törekedjünk a legrövidebb megoldásokra. Ehhez hasonló programozási fejtörők sokszor szerepelnek állásinterjúkon is, ezért érdemes gyakorolni őket.

A megvalósított feladatok után írjunk mindegyikre egy-egy példát, a labor ellenőrzéséhez.

> Hozzunk lére egy új Playgroundot, SwiftChallenges névvel a labor_03 könyvtárba!

### Palindróm <a id="palindrome"></a>

Írj egy `function`t, ami egy `String`et vár paraméterül és `true`-val tér vissza ha a szó visszafele olvasva megegyezik önmagával.

```Swift
func challenge1(input: String) -> Bool {
   return true
}
```

# Szorgalmi feladatok <a id="szorgalmi"></a>

## Korábbi számítások (*`History` nézet*) <a id="korabbi-szamitasok"></a>

> Adjunk hozzá egy `Text View`-t a `Stack View` aljához, majd hozzunk létre hozzá egy `IBOutlet`et a `View Controller` interfészében (pl. `historyView` névvel)! A `Text View` csak olvasható legyen.

<p align="center">
<img src="img/24_history_view.png" alt="24" style="width: 50%;"/>
</p>

> A már korábban látott módon állítsuk be a `Text View` magasságát `100`-ra `Auto Layout` segítségével!

*A `Text View` legfőbb különbségei `UILabel`hez képest, hogy szerkeszthető és a kilógó szöveg görgethető.*

> Módosítsuk az eredményeket kiszámító kódot oly módon, hogy az aktuális számításról bekerüljön egy sor a `Text View`-ba, pl. `13.00 + 13.00 = 26.00`!

<p align="center">
<img src="img/25_history_view.png" alt="25" style="width: 30%;"/>
</p>
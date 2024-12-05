---
title: startTransition
---

<Intro>

`startTransition` તમને UI ને અવરોધિત કર્યા વિના state અપડેટ કરવા દે છે.

```js
startTransition(scope)
```

</Intro>

<InlineToc />

---

## સંદર્ભ {/*reference*/}

### `startTransition(scope)` {/*starttransitionscope*/}

`startTransition` ફંક્શન તમને state અપડેટને ટ્રાન્ઝિશન તરીકે ચિહ્નિત કરવા દે છે.

```js {7,9}
import { startTransition } from 'react';

function TabContainer() {
  const [tab, setTab] = useState('about');

  function selectTab(nextTab) {
    startTransition(() => {
      setTab(nextTab);
    });
  }
  // ...
}
```

[નીચે વધુ ઉદાહરણો જુઓ.](#usage)

#### પેરામીટર્સ {/*parameters*/}

* `scope`: એક ફંક્શન જે એક અથવા વધુ [`set` ફંક્શન્સ](/reference/react/useState#setstate) ને કૉલ કરીને કેટલીક state અપડેટ કરે છે. રિએક્ટ તરત જ `scope` ને કોઈ આર્ગ્યુમેન્ટ્સ વિના કૉલ કરે છે અને `scope` ફંક્શન કૉલ દરમિયાન સિંક્રોનસલી શેડ્યૂલ કરેલા બધા state અપડેટ્સને ટ્રાન્ઝિશન્સ તરીકે ચિહ્નિત કરે છે. તેઓ [અવરોધક નથી](/reference/react/useTransition#marking-a-state-update-as-a-non-blocking-transition) અને [અવાંછિત લોડિંગ સૂચકો બતાવશે નહીં.](/reference/react/useTransition#preventing-unwanted-loading-indicators)

#### પરત આપે છે {/*returns*/}

`startTransition` કંઈ પણ પરત આપતું નથી.

#### ચેતવણીઓ {/*caveats*/}

* `startTransition` એ ટ્રાન્ઝિશન પેન્ડિંગ છે કે નહીં તે ટ્રેક કરવાની રીત પૂરી પાડતું નથી. ટ્રાન્ઝિશન ચાલુ હોય ત્યારે પેન્ડિંગ સૂચક બતાવવા માટે, તમને [`useTransition`](/reference/react/useTransition) ની જરૂર પડશે.

* તમે કોઈ stateના `set` ફંક્શનની ઍક્સેસ ધરાવો છો તો જ તમે અપડેટને ટ્રાન્ઝિશનમાં લપેટી શકો છો. જો તમે કોઈ પ્રોપ અથવા કસ્ટમ હુક રિટર્ન વેલ્યુના પ્રતિસાદમાં ટ્રાન્ઝિશન શરૂ કરવા માંગો છો, તો [`useDeferredValue`](/reference/react/useDeferredValue) અજમાવો.

* તમે `startTransition` ને પાસ કરેલું ફંક્શન સિંક્રોનસ હોવું જોઈએ. રિએક્ટ તરત જ આ ફંક્શનને એક્ઝિક્યુટ કરે છે, જે દરમિયાન થતા બધા state અપડેટ્સને ટ્રાન્ઝિશન્સ તરીકે ચિહ્નિત કરે છે. જો તમે પછીથી વધુ state અપડેટ્સ કરવાનો પ્રયાસ કરો (ઉદાહરણ તરીકે, ટાઈમઆઉટમાં), તેઓ ટ્રાન્ઝિશન્સ તરીકે ચિહ્નિત નહીં થાય.

* ટ્રાન્ઝિશન તરીકે ચિહ્નિત state અપડેટને અન્ય state અપડેટ્સ દ્વારા વિચ્છેદિત કરી શકાય છે. ઉદાહરણ તરીકે, જો તમે ટ્રાન્ઝિશનમાં ચાર્ટ કમ્પોનેન્ટને અપડેટ કરો, પરંતુ પછી ચાર્ટનું રી-રેન્ડર ચાલુ હોય ત્યારે ઇનપુટમાં ટાઈપ કરવા માંડો, તો રિએક્ટ ઇનપુટ state અપડેટને સંભાળ્યા પછી ચાર્ટ કમ્પોનેન્ટ પર રેન્ડરિંગ કામને ફરીથી શરૂ કરશે.

* ટ્રાન્ઝિશન અપડેટ્સનો ઉપયોગ ટેક્સ્ટ ઇનપુટ્સને નિયંત્રિત કરવા માટે ન કરી શકાય.

* જો એકથી વધુ ચાલુ ટ્રાન્ઝિશન્સ હોય, તો રિએક્ટ હાલમાં તેમને એકસાથે બેચ કરે છે. આ એક મર્યાદા છે જેને ભવિષ્યના રિલીઝમાં દૂર કરવામાં આવશે.

---

## ઉપયોગ {/*usage*/}

### state અપડેટને અવરોધક ન બનાવતું ટ્રાન્ઝિશન તરીકે ચિહ્નિત કરવું {/*marking-a-state-update-as-a-non-blocking-transition*/}

તમે state અપડેટને *ટ્રાન્ઝિશન* તરીકે ચિહ્નિત કરી શકો છો જેને `startTransition` કૉલમાં લપેટીને:

```js {7,9}
import { startTransition } from 'react';

function TabContainer() {
  const [tab, setTab] = useState('about');

  function selectTab(nextTab) {
    startTransition(() => {
      setTab(nextTab);
    });
  }
  // ...
}
```

ટ્રાન્ઝિશન્સ ધીમી ડિવાઇસીસ પર પણ તમારા યુઝર ઇન્ટરફેસ અપડેટ્સને પ્રતિસાદી રાખવા દે છે.

ટ્રાન્ઝિશન સાથે, તમારું UI રી-રેન્ડરની વચ્ચે પ્રતિસાદી રહે છે. ઉદાહરણ તરીકે, જો યુઝર કોઈ ટેબ પર ક્લિક કરે પણ પછી તેમનું મન બદલાઈ જાય અને બીજી ટેબ પર ક્લિક કરે, તો તેઓ પ્રથમ રી-રેન્ડર પૂરું થવાની રાહ જોયા વિના તે કરી શકે છે.

<Note>

`startTransition` [`useTransition`](/reference/react/useTransition) સાથે ખૂબ જ સમાન છે, સિવાય કે તે ટ્રાન્ઝિશન ચાલુ છે કે નહીં તે ટ્રેક કરવા માટે `isPending` ધ્વજ પૂરી પાડતું નથી. જ્યારે `useTransition` ઉપલબ્ધ ન હોય ત્યારે તમે `startTransition` ને કૉલ કરી શકો છો. ઉદાહરણ તરીકે, `startTransition` કમ્પોનેન્ટ્સની બહાર કામ કરે છે, જેમ કે ડેટા ફેચિંગ લાઇબ્રેરીઝ અથવા અન્ય એસિન્ક્રોનસ ઓપરેશન્સ માટે.

[ટ્રાન્ઝિશન્સ વિશે શીખો અને `useTransition` પેજ પર ઉદાહરણો જુઓ.](/reference/react/useTransition)

</Note>

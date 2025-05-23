---
title: State ને સાચવી રાખવું અને ફરીથી સેટ કરવું
---

<Intro>

State દરેક કોમ્પોનેન્ટ માટે અલગ હોય છે. React એ UI માં કોમ્પોનેન્ટ ક્યા સ્થાન પર છે તે મુજબ state ને જાળવી રાખે છે. તમે નક્કી કરી શકો છો કે ક્યારે state ને સાચવવું અને ક્યારે તે ફરીથી શરૂ કરવું.

</Intro>

<YouWillLearn>

* React ક્યારે state ને સાચવવા અથવા ફરીથી સેટ કરવાનું પસંદ કરે છે.
* React ને કેવી રીતે state ફરીથી સેટ કરવા માટે બળજબરી કરવી.
* keys અને types state જાળવવામાં કેવી રીતે અસર કરે છે.

</YouWillLearn>

## State render tree માં એક નિશ્ચિત સ્થાન સાથે સંકળાયેલું હોય છે. {/*state-is-tied-to-a-position-in-the-tree*/}

React તમારા UI માં કોમ્પોનેન્ટ માળખું માટે [render trees](learn/understanding-your-ui-as-a-tree#the-render-tree) બનાવે છે.

જ્યારે તમે કોમ્પોનેન્ટને state આપો છો, ત્યારે તમને એવું લાગશે કે state "કોમ્પોનેન્ટમાં" રહે છે. પરંતુ વાસ્તવમાં, state React દ્વારા જાળવવામાં આવે છે. React દરેક state ના ટુકડાને તે કયા કોમ્પોનેન્ટ સાથે સંકળાયેલું છે તે render tree માં તેના સ્થાન આધારે યોગ્ય કોમ્પોનેન્ટ સાથે જોડે છે.

અહીં, ફક્ત એક `<Counter />` JSX ટૅગ છે, પરંતુ તે બે અલગ અલગ સ્થાનોએ રેન્ડર કરવામાં આવે છે:

<Sandpack>

```js
import { useState } from 'react';

export default function App() {
  const counter = <Counter />;
  return (
    <div>
      {counter}
      {counter}
    </div>
  );
}

function Counter() {
  const [score, setScore] = useState(0);
  const [hover, setHover] = useState(false);

  let className = 'counter';
  if (hover) {
    className += ' hover';
  }

  return (
    <div
      className={className}
      onPointerEnter={() => setHover(true)}
      onPointerLeave={() => setHover(false)}
    >
      <h1>{score}</h1>
      <button onClick={() => setScore(score + 1)}>
        એક ઉમેરો
      </button>
    </div>
  );
}
```

```css
label {
  display: block;
  clear: both;
}

.counter {
  width: 100px;
  text-align: center;
  border: 1px solid gray;
  border-radius: 4px;
  padding: 20px;
  margin: 0 20px 20px 0;
  float: left;
}

.hover {
  background: #ffffd8;
}
```

</Sandpack>

આ રીતે Tree બતાવે છે:   

<DiagramGroup>

<Diagram name="preserving_state_tree" height={248} width={395} alt="Root node તરીકે 'div' છે, જેમાં બે children છે. દરેક child નું લેબલ 'Counter' છે અને બંનેમાં એક state bubble છે જેનું નામ 'count' છે અને તેની value 0 છે.">

React tree

</Diagram>

</DiagramGroup>

**આ બે અલગ-અલગ counters છે કારણ કે દરેક પોતપોતાની સ્થિતી પર Tree માં દર્શાવવામાં આવે છે.** તમારે સામાન્ય રીતે React નો ઉપયોગ કરતા સમયે આ સ્થાનો અંગે વિચારવાની જરૂર નથી, પરંતુ તે કેવી રીતે કાર્ય કરે છે તે સમજવું ફાયદાકારક થઈ શકે છે.

React માં, સ્ક્રીન પર દરેક કોમ્પોનેન્ટનું પોતાનું અલગ state હોય છે. ઉદાહરણ તરીકે, જો તમે બાજુ બાજુ ૨ Counter કોમ્પોનેન્ટ્સ રેન્ડર કરો, તો દરેકને તેનું પોતાનું, સ્વતંત્ર, `score` અને `hover` state મળશે.

બન્ને counters પર ક્લિક કરવાનો પ્રયાસ કરો અને જુઓ કે તેઓ એકબીજાને અસર નથી કરતાં:

<Sandpack>

```js
import { useState } from 'react';

export default function App() {
  return (
    <div>
      <Counter />
      <Counter />
    </div>
  );
}

function Counter() {
  const [score, setScore] = useState(0);
  const [hover, setHover] = useState(false);

  let className = 'counter';
  if (hover) {
    className += ' hover';
  }

  return (
    <div
      className={className}
      onPointerEnter={() => setHover(true)}
      onPointerLeave={() => setHover(false)}
    >
      <h1>{score}</h1>
      <button onClick={() => setScore(score + 1)}>
        એક ઉમેરો
      </button>
    </div>
  );
}
```

```css
.counter {
  width: 100px;
  text-align: center;
  border: 1px solid gray;
  border-radius: 4px;
  padding: 20px;
  margin: 0 20px 20px 0;
  float: left;
}

.hover {
  background: #ffffd8;
}
```

</Sandpack>

જેમ કે તમે જોઈ શકો છો, જ્યારે એક counter અપડેટ થાય છે, ત્યારે ફક્ત તે કોમ્પોનેન્ટ માટેનું state અપડેટ થાય છે:


<DiagramGroup>

<Diagram name="preserving_state_increment" height={248} width={441} alt="React ઘટકોના વૃક્ષનો આલેખ. Root Node 'div' તરીકે લેબલ કરેલું છે અને તેના બે સંતાનો છે. ડાબી બાજુનો સંતાન 'Counter' તરીકે લેબલ કરેલું છે અને તેમાં 'count' નામનું state બબલ છે જેની કિંમત 0 છે. જમણી બાજુનો સંતાન પણ 'Counter' તરીકે લેબલ કરેલું છે અને તેમાં 'count' નામનું state બબલ છે જેની કિંમત 1 છે. જમણું state બબલ પીળા રંગમાં હાઇલાઇટ કરેલું છે, જેથી તેની કિંમતમાં ફેરફાર દર્શાવવામાં આવે છે.">

State અપડેટ કરવું.

</Diagram>

</DiagramGroup>


React state ને તે સમય સુધી જાળવે છે, જયાં સુધી તમે એ જ કોમ્પોનેન્ટને એ જ સ્થાન પર render કરતા રહો. આ જોવા માટે, બન્ને counters વધારવા, પછી "બીજું counter રેન્ડર કરો" checkbox ને અનચેક કરી બીજું કોમ્પોનેન્ટ દૂર કરો, અને પછી તેને ફરીથી ટિક કરીને ઉમેરો:

<Sandpack>

```js
import { useState } from 'react';

export default function App() {
  const [showB, setShowB] = useState(true);
  return (
    <div>
      <Counter />
      {showB && <Counter />} 
      <label>
        <input
          type="checkbox"
          checked={showB}
          onChange={e => {
            setShowB(e.target.checked)
          }}
        />
        બીજું counter રેન્ડર કરો
      </label>
    </div>
  );
}

function Counter() {
  const [score, setScore] = useState(0);
  const [hover, setHover] = useState(false);

  let className = 'counter';
  if (hover) {
    className += ' hover';
  }

  return (
    <div
      className={className}
      onPointerEnter={() => setHover(true)}
      onPointerLeave={() => setHover(false)}
    >
      <h1>{score}</h1>
      <button onClick={() => setScore(score + 1)}>
       એક ઉમેરો
      </button>
    </div>
  );
}
```

```css
label {
  display: block;
  clear: both;
}

.counter {
  width: 100px;
  text-align: center;
  border: 1px solid gray;
  border-radius: 4px;
  padding: 20px;
  margin: 0 20px 20px 0;
  float: left;
}

.hover {
  background: #ffffd8;
}
```

</Sandpack>

ધ્યાન દો, જ્યારે તમે બીજું counter render કરવાનું બંધ કરો છો, ત્યારે તેનો state પૂરેપૂરી રીતે નષ્ટ થઈ જાય છે. આ માટે કારણ એ છે કે જ્યારે React એક કોમ્પોનેન્ટને દૂર કરે છે, ત્યારે તેની state નષ્ટ થઈ જાય છે.

<DiagramGroup>

<Diagram name="preserving_state_remove_component" height={253} width={422} alt="React components નું એક tree નું diagram. Root node 'div' થી labeled છે અને તેના બે children છે. ડાબા બચ્ચું 'Counter' થી labeled છે અને તેમાં એક state bubble છે જે 'count' થી labeled છે અને value 0 છે. દાયમું બચ્ચું ગાયબ છે, અને તેના સ્થાને પીળો 'poof' ચિત્ર છે, જે React component ના tree માંથી કાઢવામાં આવતા છબી બતાવે છે.">
કોમ્પોનેન્ટ દૂર કરી રહ્યા છે

</Diagram>

</DiagramGroup>

જ્યારે તમે "બીજું counter રેન્ડર કરો" ટીક કરો છો, ત્યારે બીજું `Counter` અને તેનું state નવી શરૃઆતથી (`score = 0`) આરંભ થાય છે અને DOM માં ઉમેરાય છે.

<DiagramGroup>

<Diagram name="preserving_state_add_component" height={258} width={500} alt="React components નું એક tree નું diagram. Root node 'div' થી labeled છે અને તેના બે children છે. ડાબા બચ્ચું 'Counter' થી labeled છે અને તેમાં એક state bubble છે જે 'count' થી labeled છે અને value 0 છે. દાયમું બચ્ચું પણ 'Counter' થી labeled છે અને તેમાં એક state bubble છે જે 'count' થી labeled છે અને value 0 છે. આખા દાયમું બચ્ચા ના node ને પીળા રંગમાં હાઈલાઇટ કરવામાં આવ્યું છે, જે દર્શાવે છે કે તે તાજા એડ કરાયું છે.">

કોમ્પોનેન્ટ ઉમેરવું

</Diagram>

</DiagramGroup>

**React કંપોનેન્ટનું state ત્યાં સુધી રહે છે જ્યાં સુધી એ UI tree માં એક જ જગ્યા પર દેખાતું રહે છે.** જો તે દૂર કરવામાં આવે છે, અથવા એ જ સ્થાન પર બીજું કોમ્પોનેન્ટ રેન્ડર થાય છે, તો React તેની state ને નષ્ટ કરી દે છે.

## એજ કોમ્પોનેન્ટ એજ સ્થાન પર state જાળવે છે. {/*same-component-at-the-same-position-preserves-state*/}

આ ઉદાહરણમાં, બે અલગ અલગ `<Counter />` ટૅગ્સ છે:

<Sandpack>

```js
import { useState } from 'react';

export default function App() {
  const [isFancy, setIsFancy] = useState(false);
  return (
    <div>
      {isFancy ? (
        <Counter isFancy={true} /> 
      ) : (
        <Counter isFancy={false} /> 
      )}
      <label>
        <input
          type="checkbox"
          checked={isFancy}
          onChange={e => {
            setIsFancy(e.target.checked)
          }}
        />
        fancy સ્ટાઈલિંગ ઉપયોગ કરો
      </label>
    </div>
  );
}

function Counter({ isFancy }) {
  const [score, setScore] = useState(0);
  const [hover, setHover] = useState(false);

  let className = 'counter';
  if (hover) {
    className += ' hover';
  }
  if (isFancy) {
    className += ' fancy';
  }

  return (
    <div
      className={className}
      onPointerEnter={() => setHover(true)}
      onPointerLeave={() => setHover(false)}
    >
      <h1>{score}</h1>
      <button onClick={() => setScore(score + 1)}>
        એક ઉમેરો
      </button>
    </div>
  );
}
```

```css
label {
  display: block;
  clear: both;
}

.counter {
  width: 100px;
  text-align: center;
  border: 1px solid gray;
  border-radius: 4px;
  padding: 20px;
  margin: 0 20px 20px 0;
  float: left;
}

.fancy {
  border: 5px solid gold;
  color: #ff6767;
}

.hover {
  background: #ffffd8;
}
```

</Sandpack>

તમે checkbox ટીક કરો કે અનટીક, ત્યારે counter નું state રિસેટ થતું નથી. કેમ કે `isFancy` `true` હોય કે `false`, દર વખતે `<Counter />` `div` ના પહેલા ચાઈલ્ડ તરીકે જ હોય છે, જે root `App` કોમ્પોનેન્ટમાંથી પાછું અપાય છે:

<DiagramGroup>

<Diagram name="preserving_state_same_component" height={461} width={600} alt="આ ડાયાગ્રામમાં બે વિભાગ છે, જેમને વચ્ચે તીરથી અલગ કરવામાં આવ્યા છે જે પરિવર્તન દર્શાવે છે. દરેક વિભાગમાં કોમ્પોનેન્ટ્સનો લેઆઉટ છે. પેરેન્ટ તરીકે 'App' લેબલ કરાયેલ છે જેમાં state બબલ છે – 'isFancy'. આ કોમ્પોનેન્ટમાં એક ચાઈલ્ડ છે 'div', જે prop બબલ તરફ જાય છે જેમાં 'isFancy' છે (જમણી બાજુના વિભાગમાં પર્પલ રંગથી હાઇલાઇટ થયેલું છે), અને જે નીચેના એકમાત્ર ચાઈલ્ડને આપવામાં આવ્યું છે. છેલ્લો ચાઈલ્ડ 'Counter' છે, જેમાં 'count' નામનું state બબલ છે અને બંને વિભાગોમાં તેની value 3 છે. ડાબા વિભાગમાં કંઈHighlighted નથી અને 'isFancy' ની value false છે. જમણા વિભાગમાં 'isFancy' ની value true થઈ છે અને તે પીળા રંગથી હાઇલાઇટ છે, તેમજ તે props બબલ પણ હાઇલાઇટ છે જેમાં હવે 'isFancy' true છે.">

`App` નું state અપડેટ કરવાથી `Counter` રિસેટ થતો નથી કારણ કે `Counter` એ જ સ્થાન પર જ રહે છે."

</Diagram>

</DiagramGroup>


આ જ કોમ્પોનેન્ટ એ જ સ્થાન પર છે, એટલે React માટે એ જ counter ગણાય છે.

<Pitfall>

યાદ રાખો કે **React માટે મહત્વપૂર્ણ એ એ સ્થાન છે જે UI ટ્રીમાં છે -- JSX માર્કઅપમાં નહીં!** આ કોમ્પોનેન્ટમાં બે `return` વિભાગો છે, જેમાં અલગ અલગ `<Counter />` JSX ટૅગ્સ છે, એક `if`ની અંદર અને એક બહાર:

<Sandpack>

```js
import { useState } from 'react';

export default function App() {
  const [isFancy, setIsFancy] = useState(false);
  if (isFancy) {
    return (
      <div>
        <Counter isFancy={true} />
        <label>
          <input
            type="checkbox"
            checked={isFancy}
            onChange={e => {
              setIsFancy(e.target.checked)
            }}
          />
          fancy સ્ટાઈલિંગ ઉપયોગ કરો
        </label>
      </div>
    );
  }
  return (
    <div>
      <Counter isFancy={false} />
      <label>
        <input
          type="checkbox"
          checked={isFancy}
          onChange={e => {
            setIsFancy(e.target.checked)
          }}
        />
        fancy સ્ટાઈલિંગ ઉપયોગ કરો
      </label>
    </div>
  );
}

function Counter({ isFancy }) {
  const [score, setScore] = useState(0);
  const [hover, setHover] = useState(false);

  let className = 'counter';
  if (hover) {
    className += ' hover';
  }
  if (isFancy) {
    className += ' fancy';
  }

  return (
    <div
      className={className}
      onPointerEnter={() => setHover(true)}
      onPointerLeave={() => setHover(false)}
    >
      <h1>{score}</h1>
      <button onClick={() => setScore(score + 1)}>
        એક ઉમેરો
      </button>
    </div>
  );
}
```

```css
label {
  display: block;
  clear: both;
}

.counter {
  width: 100px;
  text-align: center;
  border: 1px solid gray;
  border-radius: 4px;
  padding: 20px;
  margin: 0 20px 20px 0;
  float: left;
}

.fancy {
  border: 5px solid gold;
  color: #ff6767;
}

.hover {
  background: #ffffd8;
}
```

</Sandpack>

તમને આશા હોઈ શકે છે કે state checkbox ટીક કરતા રિસેટ થશે, પરંતુ એ નહિ થાય! આ માટેનું કારણ એ છે કે ** બન્ને `<Counter />` ટૅગ્સ એ જ સ્થાન પર રેન્ડર થાય છે.** React એ જોઈ શકતું નથી કે તમે તમારી ફંક્શનમાં કયા સ્થાન પર શરતો મૂકો છો. તે ફક્ત તે વૃક્ષને "દેખે" છે જે તમે રિટર્ન કરો.

બન્ને પરિસ્થિતિઓમાં, `App` કોમ્પોનેન્ટ `<div>` રિટર્ન કરે છે, જેમાં `<Counter />` પ્રથમ ચાઈલ્ડ તરીકે છે. React માટે, આ બંને counters એ જ "એડ્રેસ" ધરાવે છે: રુટના પ્રથમ ચાઈલ્ડના પ્રથમ ચાઈલ્ડ. આ રીતે React પેહલા અને બીજાં રેન્ડરો વચ્ચે તેમને મેળવે છે, ભલે તમે તમારી લોજિકને કેવી રીતે રચો.

</Pitfall>

## વિભિન્ન કોમ્પોનેન્ટ્સ એ જ સ્થાન પર state રિસેટ કરે છે. {/*different-components-at-the-same-position-reset-state*/}

આ ઉદાહરણમાં, checkbox ટીક કરતા `<Counter>` ને `<p>` દ્વારા બદલી દેવામાં આવશે:

<Sandpack>

```js
import { useState } from 'react';

export default function App() {
  const [isPaused, setIsPaused] = useState(false);
  return (
    <div>
      {isPaused ? (
        <p>પાછા મળશું</p> 
      ) : (
        <Counter /> 
      )}
      <label>
        <input
          type="checkbox"
          checked={isPaused}
          onChange={e => {
            setIsPaused(e.target.checked)
          }}
        />
        થોડો વિરામ લ્યો
      </label>
    </div>
  );
}

function Counter() {
  const [score, setScore] = useState(0);
  const [hover, setHover] = useState(false);

  let className = 'counter';
  if (hover) {
    className += ' hover';
  }

  return (
    <div
      className={className}
      onPointerEnter={() => setHover(true)}
      onPointerLeave={() => setHover(false)}
    >
      <h1>{score}</h1>
      <button onClick={() => setScore(score + 1)}>
        એક ઉમેરો
      </button>
    </div>
  );
}
```

```css
label {
  display: block;
  clear: both;
}

.counter {
  width: 100px;
  text-align: center;
  border: 1px solid gray;
  border-radius: 4px;
  padding: 20px;
  margin: 0 20px 20px 0;
  float: left;
}

.hover {
  background: #ffffd8;
}
```

</Sandpack>

અહીં, તમે એ જ સ્થાન પર _વિભિન્ન_ કોમ્પોનેન્ટ પ્રકારો વચ્ચે સ્વિચ કરો છો. પ્રારંભમાં, `<div>`ના પ્રથમ ચાઈલ્ડમાં `Counter` હતો. પરંતુ જ્યારે તમે `p` સ્વેપ કર્યું, ત્યારે React એ UI ટ્રીમાંથી `Counter`ને હટાવી દીધી અને તેનું state નષ્ટ કરી દીધું.

<DiagramGroup>

<Diagram name="preserving_state_diff_pt1" height={290} width={753} alt="ત્રણ વિભાગો સાથેનો ડાયાગ્રામ, દરેક વિભાગ વચ્ચે તીરથી પરિવર્તન દર્શાવતો છે. પહેલા વિભાગમાં એક React કોમ્પોનેન્ટ છે જેમાં 'div' લેબલ છે અને એક જ ચાઈલ્ડ છે જે 'Counter' તરીકે લેબલ છે, જેમાં 'count' નામક state બબલ છે અને તેની value 3 છે. મધ્યમ વિભાગમાં એ જ 'div' પેરેન્ટ છે, પરંતુ ચાઈલ્ડ કોમ્પોનેન્ટ હવે હટાવી દેવાયું છે, જે પીળા 'પ્રૂફ' ચિત્રથી દર્શાવાયું છે. ત્રીજા વિભાગમાં એ જ 'div' પેરેન્ટ છે, હવે નવી ચાઈલ્ડ છે જે 'p' તરીકે લેબલ છે, જે પીળા રંગથી હાઇલાઇટ થયેલું છે.">

જ્યારે `Counter` ને `p` માં બદલી દેવામાં આવે છે, ત્યારે `Counter` હટાવી દેવામાં આવે છે અને `p` ઉમેરવામાં આવે છે.

</Diagram>

</DiagramGroup>

<DiagramGroup>

<Diagram name="preserving_state_diff_pt2" height={290} width={753} alt="ત્રણ વિભાગો સાથેનો ડાયાગ્રામ, દરેક વિભાગ વચ્ચે તીરથી પરિવર્તન દર્શાવતો છે. પહેલા વિભાગમાં એક React કોમ્પોનેન્ટ છે જે 'p' તરીકે લેબલ છે. મધ્યમ વિભાગમાં એ જ 'div' પેરેન્ટ છે, પરંતુ ચાઈલ્ડ કોમ્પોનેન્ટ હવે હટાવી દેવામાં આવ્યું છે, જે પીળા 'પ્રૂફ' ચિત્રથી દર્શાવાયું છે. ત્રીજા વિભાગમાં એ જ 'div' પેરેન્ટ છે, હવે નવી ચાઈલ્ડ છે જે 'Counter' તરીકે લેબલ છે, જેમાં 'count' નામક state બબલ છે અને તેની value 0 છે, જે પીળા રંગથી હાઇલાઇટ થયેલું છે.">

જ્યારે પાછો સ્વિચ કરવામાં આવે છે, ત્યારે `p` હટાવી દેવામાં આવે છે અને `Counter` ઉમેરવામાં આવે છે.

</Diagram>

</DiagramGroup>

અને, **જ્યારે તમે એ જ સ્થાન પર અલગ કોમ્પોનેન્ટ રેન્ડર કરો છો, ત્યારે તે તેની સમગ્ર સબટ્રીનો state રિસેટ કરી નાખે છે.** આ કેવી રીતે કાર્ય કરે છે તે જોવા માટે, કાઉન્ટર_increment કરો અને પછી checkbox ટીક કરો:

<Sandpack>

```js
import { useState } from 'react';

export default function App() {
  const [isFancy, setIsFancy] = useState(false);
  return (
    <div>
      {isFancy ? (
        <div>
          <Counter isFancy={true} /> 
        </div>
      ) : (
        <section>
          <Counter isFancy={false} />
        </section>
      )}
      <label>
        <input
          type="checkbox"
          checked={isFancy}
          onChange={e => {
            setIsFancy(e.target.checked)
          }}
        />
        fancy સ્ટાઈલિંગ ઉપયોગ કરો
      </label>
    </div>
  );
}

function Counter({ isFancy }) {
  const [score, setScore] = useState(0);
  const [hover, setHover] = useState(false);

  let className = 'counter';
  if (hover) {
    className += ' hover';
  }
  if (isFancy) {
    className += ' fancy';
  }

  return (
    <div
      className={className}
      onPointerEnter={() => setHover(true)}
      onPointerLeave={() => setHover(false)}
    >
      <h1>{score}</h1>
      <button onClick={() => setScore(score + 1)}>
        એક ઉમેરો
      </button>
    </div>
  );
}
```

```css
label {
  display: block;
  clear: both;
}

.counter {
  width: 100px;
  text-align: center;
  border: 1px solid gray;
  border-radius: 4px;
  padding: 20px;
  margin: 0 20px 20px 0;
  float: left;
}

.fancy {
  border: 5px solid gold;
  color: #ff6767;
}

.hover {
  background: #ffffd8;
}
```

</Sandpack>

જ્યારે તમે checkbox ક્લિક કરો છો, ત્યારે કાઉન્ટર state રિસેટ થાય છે. હાલમાં તમે `Counter` રેન્ડર કરો છો, પરંતુ `div`ના પ્રથમ ચાઈલ્ડમાંથી `div` ને `section`માં બદલી દેવામાં આવે છે. જ્યારે ચાઈલ્ડ `div` DOM માંથી હટાવાયું, ત્યારે તેની નીચેનો સંપૂર્ણ ટ્રી (જેમાં `Counter` અને તેનો state પણ શામેલ છે) નષ્ટ થઈ ગયો.

<DiagramGroup>

<Diagram name="preserving_state_diff_same_pt1" height={350} width={794} alt="ત્રણ વિભાગો સાથેનો ડાયાગ્રામ, દરેક વિભાગ વચ્ચે તીરથી પરિવર્તન દર્શાવતો છે. પહેલા વિભાગમાં એક React કોમ્પોનેન્ટ છે જે 'div' તરીકે લેબલ છે, જેમાં એક જ ચાઈલ્ડ છે જે 'section' તરીકે લેબલ છે, અને આ 'section'માં એક જ ચાઈલ્ડ છે જે 'Counter' તરીકે લેબલ છે, જેમાં 'count' નામક state બબલ છે અને તેની value 3 છે. મધ્યમ વિભાગમાં એ જ 'div' પેરેન્ટ છે, પરંતુ ચાઈલ્ડ કોમ્પોનેન્ટ હવે હટાવી દેવાયા છે, જે પીળા 'પ્રૂફ' ચિત્રથી દર્શાવાયું છે. ત્રીજા વિભાગમાં એ જ 'div' પેરેન્ટ છે, હવે નવી ચાઈલ્ડ છે જે 'div' તરીકે લેબલ છે, જે પીળા રંગથી હાઇલાઇટ થયેલું છે, અને તેમાં નવી ચાઈલ્ડ છે જે 'Counter' તરીકે લેબલ છે, જેમાં 'count' નામક state બબલ છે અને તેની value 0 છે, તમામ પીળા રંગથી હાઇલાઇટ થયેલા છે.">

જ્યારે `section` ને `div` માં બદલી દેવામાં આવે છે, ત્યારે `section` હટાવી દેવામાં આવે છે અને નવી `div` ઉમેરવામાં આવે છે.

</Diagram>

</DiagramGroup>

<DiagramGroup>

<Diagram name="preserving_state_diff_same_pt2" height={350} width={794} alt="ત્રણ વિભાગો સાથેનો ડાયાગ્રામ, દરેક વિભાગ વચ્ચે તીરથી પરિવર્તન દર્શાવતો છે. પહેલા વિભાગમાં એક React કોમ્પોનેન્ટ છે જે 'div' તરીકે લેબલ છે, જેમાં એક જ ચાઈલ્ડ છે જે 'div' તરીકે લેબલ છે, અને આ 'div'માં એક જ ચાઈલ્ડ છે જે 'Counter' તરીકે લેબલ છે, જેમાં 'count' નામક state બબલ છે અને તેની value 0 છે. મધ્યમ વિભાગમાં એ જ 'div' પેરેન્ટ છે, પરંતુ ચાઈલ્ડ કોમ્પોનેન્ટ હવે હટાવી દેવાયા છે, જે પીળા 'પ્રૂફ' ચિત્રથી દર્શાવાયું છે. ત્રીજા વિભાગમાં એ જ 'div' પેરેન્ટ છે, હવે નવી ચાઈલ્ડ છે જે 'section' તરીકે લેબલ છે, જે પીળા રંગથી હાઇલાઇટ થયેલું છે, અને તેમાં નવી ચાઈલ્ડ છે જે 'Counter' તરીકે લેબલ છે, જેમાં 'count' નામક state બબલ છે અને તેની value 0 છે, તમામ પીળા રંગથી હાઇલાઇટ થયેલા છે.">

જ્યારે પાછો સ્વિચ કરવામાં આવે છે, ત્યારે `div` હટાવી દેવામાં આવે છે અને નવું `section` ઉમેરવામાં આવે છે.

</Diagram>

</DiagramGroup>

સામાન્ય રીતે, **જો તમે રેન્ડર વચ્ચે state સાચવવા માંગતા હો, તો તમારા ટ્રીની માળખું એક રેન્ડરથી બીજામાં "મેચ" કરવું જોઈએ.** જો માળખું અલગ છે, તો state નષ્ટ થઈ જાય છે કારણકે React એ કોમ્પોનેન્ટને ટ્રીમાંથી હટાવતી વખતે state નષ્ટ કરે છે.

<Pitfall>

આ વાતનો અર્થ છે કે તમે કોમ્પોનેન્ટ ફંક્શનને અંદર ન લખો.

અહીં, `MyTextField` કોમ્પોનેન્ટ ફંક્શન *MyComponent* ની અંદર લખાયું છે:

<Sandpack>

```js
import { useState } from 'react';

export default function MyComponent() {
  const [counter, setCounter] = useState(0);

  function MyTextField() {
    const [text, setText] = useState('');

    return (
      <input
        value={text}
        onChange={e => setText(e.target.value)}
      />
    );
  }

  return (
    <>
      <MyTextField />
      <button onClick={() => {
        setCounter(counter + 1)
      }}>{counter} વખત ક્લિક કરેલું</button>
    </>
  );
}
```

</Sandpack>


દરેક વખત તમે બટન પર ક્લિક કરો છો, ત્યારે ઇનપુટ state ગાયબ થઈ જાય છે! આ એ માટે છે કે દરેક રેન્ડર પર `MyTextField` ફંક્શન અલગ બનાવાયું છે. તમે એક જ જગ્યાએ *વિભિન્ન* કોમ્પોનેન્ટ રેન્ડર કરી રહ્યા છો, એટલે React બધા stateને રિસેટ કરે છે. આથી બગ્સ અને પર્ફોર્મન્સની સમસ્યાઓ આવે છે. આથી, **હંમેશા કોમ્પોનેન્ટ ફંક્શનને ઉપરના સ્તરે લખો અને તેને નેસ્ટ ન કરો.**

</Pitfall>

## એજ સ્થાન પર state રિસેટ કરવો {/*resetting-state-at-the-same-position*/}

આપમેળે, React કોઈ component એજ સ્થાન પર રહે ત્યારે તેનું state સાચવી રાખે છે. સામાન્ય રીતે આપણે એવું જ ઇચ્છીએ છીએ, એટલે આ વર્તન યોગ્ય ગણાય છે. પણ ક્યારેક તમારે component નું state ફરીથી શરૂ કરવું હોય છે. આ ઉદાહરણ જુઓ જેમાં બે ખેલાડીઓ દરેક વારમાં પોતાનું સ્કોર નોંધે છે:

<Sandpack>

```js
import { useState } from 'react';

export default function Scoreboard() {
  const [isPlayerA, setIsPlayerA] = useState(true);
  return (
    <div>
      {isPlayerA ? (
        <Counter person="Taylor" />
      ) : (
        <Counter person="Sarah" />
      )}
      <button onClick={() => {
        setIsPlayerA(!isPlayerA);
      }}>
        આગલો ખેલાડી!
      </button>
    </div>
  );
}

function Counter({ person }) {
  const [score, setScore] = useState(0);
  const [hover, setHover] = useState(false);

  let className = 'counter';
  if (hover) {
    className += ' hover';
  }

  return (
    <div
      className={className}
      onPointerEnter={() => setHover(true)}
      onPointerLeave={() => setHover(false)}
    >
      <h1>{person}'s score: {score}</h1>
      <button onClick={() => setScore(score + 1)}>
        એક ઉમેરો
      </button>
    </div>
  );
}
```

```css
h1 {
  font-size: 18px;
}

.counter {
  width: 100px;
  text-align: center;
  border: 1px solid gray;
  border-radius: 4px;
  padding: 20px;
  margin: 0 20px 20px 0;
}

.hover {
  background: #ffffd8;
}
```

</Sandpack>

હાલમાં, જયારે તમે ખેલાડી બદલો છો, ત્યારે સ્કોર જાળવાય છે. બંને `Counter` એ સમાન સ્થાન પર દર્શાય છે, એટલે React તેને *એક જ* `Counter` સમજે છે, જેના `person` પ્રોપમાં ફેરફાર થયો છે.

પરંતુ આ એપ્લિકેશનમાં, ખ્યાલમાં, તે બે અલગ-અલગ કાઉન્ટર હોવા જોઈએ. તે બંને UI માં એક જ જગ્યાએ દેખાય છે, પરંતુ એક ટેલર માટેનું કાઉન્ટર છે, અને બીજું સારાહ માટેનું કાઉન્ટર છે.

જ્યારે તમે બંને વચ્ચે બદલી કરો છો, ત્યારે state ને રીસેટ કરવા માટે બે રીતો છે:

1. કમ્પોનેન્ટ્સને અલગ સ્થાનોમાં રેન્ડર કરો
2. પ્રત્યેક કોમ્પોનેન્ટને `key` સાથે સ્પષ્ટ ઓળખ આપો


### વિકલ્પ 1: વિભિન્ન સ્થાનોએ કમ્પોનન્ટને રેન્ડર કરવું {/*option-1-rendering-a-component-in-different-positions*/}

જો તમે આ બે `Counter`s ને સ્વતંત્ર રાખવા માંગતા હો, તો તમે તેમને બે અલગ અલગ સ્થાનોએ રેન્ડર કરી શકો છો:

<Sandpack>

```js
import { useState } from 'react';

export default function Scoreboard() {
  const [isPlayerA, setIsPlayerA] = useState(true);
  return (
    <div>
      {isPlayerA &&
        <Counter person="Taylor" />
      }
      {!isPlayerA &&
        <Counter person="Sarah" />
      }
      <button onClick={() => {
        setIsPlayerA(!isPlayerA);
      }}>
        આગલો ખેલાડી!
      </button>
    </div>
  );
}

function Counter({ person }) {
  const [score, setScore] = useState(0);
  const [hover, setHover] = useState(false);

  let className = 'counter';
  if (hover) {
    className += ' hover';
  }

  return (
    <div
      className={className}
      onPointerEnter={() => setHover(true)}
      onPointerLeave={() => setHover(false)}
    >
      <h1>{person}'s score: {score}</h1>
      <button onClick={() => setScore(score + 1)}>
        એક ઉમેરો
      </button>
    </div>
  );
}
```

```css
h1 {
  font-size: 18px;
}

.counter {
  width: 100px;
  text-align: center;
  border: 1px solid gray;
  border-radius: 4px;
  padding: 20px;
  margin: 0 20px 20px 0;
}

.hover {
  background: #ffffd8;
}
```

</Sandpack>

* પ્રારંભિક રૂપે, `isPlayerA` એ `true` છે. તેથી પ્રથમ સ્થાનમાં `Counter` state છે, અને બીજું ખાલી છે.
* Next player" બટન પર ક્લિક કરવાથી પ્રથમ પોઝિશન ખાલી થાય છે, પરંતુ બીજું પોઝિશન હવે Counter ધરાવશે.

<DiagramGroup>

<Diagram name="preserving_state_diff_position_p1" height={375} width={504} alt="React ઘટકોનો આકાર (ડાયાગ્રામ) બનાવો. પેરેન્ટને 'Scoreboard' તરીકે લેબલ કરો, જેમાં state બબલ 'isPlayerA' અને તેનું મૂલ્ય 'true' છે. એકમાત્ર ચાઈલ્ડ, જે ડાબી બાજુ પર છે, તે 'Counter' તરીકે લેબલ કરો, જેમાં state બબલ 'count' અને તેનું મૂલ્ય 0 છે. ડાબી બાજુના ચાઈલ્ડને પીળા રંગમાં હાઇલાઈટ કરો, આને એડ કરવામાં આવ્યું છે.">

પ્રારંભિક state

</Diagram>

<Diagram name="preserving_state_diff_position_p2" height={375} width={504} alt="React ઘટકોનો આકાર (ડાયાગ્રામ) બનાવો. પેરેન્ટને 'Scoreboard' તરીકે લેબલ કરો, જેમાં state બબલ 'isPlayerA' અને તેનો મૂલ્ય 'false' છે. state બબલને પીળા રંગમાં હાઇલાઈટ કરો, જે બતાવે છે કે તે બદલાયું છે. ડાબી બાજુનો ચાઈલ્ડ 'poof' ચિત્ર સાથે પીળા રંગમાં બદલાય છે, જે બતાવે છે કે તેને હટાવવામાં આવી છે, અને દાબી બાજુની જગ્યાએ નવા ચાઈલ્ડને ઉમેરવામાં આવ્યો છે, જે પીળા રંગમાં હાઇલાઈટ કરેલો છે. નવો ચાઈલ્ડ 'Counter' તરીકે લેબલ છે અને તેમાં state બબલ 'count' અને તેનું મૂલ્ય 0 છે.">

"નેક્સ્ટ" પર ક્લિક કરવું

</Diagram>

<Diagram name="preserving_state_diff_position_p3" height={375} width={504} alt="React ઘટકોનું એક આકૃતિ જેમાં વૃક્ષ છે. પેરેન્ટને 'સ્કોરબોર્ડ' તરીકે લેબલ કરવામા આવે છે, જેમાં 'isPlayerA' નામક સ્ટેટ બબલ છે જેનો મૂલ્ય 'true' છે. આ સ્ટેટ બબલને પીળા રંગમાં હાઇલાઇટ કરવામા આવે છે, જે બતાવે છે કે તે બદલાયું છે. ડાબી બાજુ પર એક નવું ચાઇલ ઉમેરવામાં આવે છે, જે પીળા રંગમાં હાઇલાઇટ કરવામા આવે છે, જે બતાવે છે કે તે ઉમેરાયું છે. આ નવું ચાઇલ 'કાઉન્ટર' તરીકે લેબલ કરવામા આવે છે અને તેમાં 'count' નામક સ્ટેટ બબલ છે, જેનો મૂલ્ય 0 છે. જમણી બાજુનો ચાઇલ પીળા 'poof' ચિત્ર સાથે બદલાવ માટે પ્રદર્શિત થાય છે, જે દર્શાવે છે કે તે ડિલીટ કરાયો છે.">

ફરીથી "નેક્સ્ટ" પર ક્લિક કરવું

</Diagram>

</DiagramGroup>

દરેક `Counter` ની state ત્યારે નષ્ટ થાય છે જ્યારે તેને DOM માંથી દૂર કરવામાં આવે છે. એટલે દરેક વખતે જ્યારે તમે બટન ક્લિક કરો છો, ત્યારે તે રિસેટ થઈ જાય છે.

જ્યારે તમારાં પાસે થોડાં જ સ્વતંત્ર component હોય અને તેઓને એ જ જગ્યાએ રેન્ડર કરવાનાં હોય, ત્યારે આ ઉકેલ અનુકૂળ છે. આ ઉદાહરણમાં, તમારાં પાસે ફક્ત બે છે, એટલે JSX માં બંનેને અલગથી રેન્ડર કરવું મુશ્કેલ નથી.

### વિકલ્પ 2: key દ્વારા state રિસેટ કરવી {/*option-2-resetting-state-with-a-key*/}

કોઈ component ની state રિસેટ કરવાની એક બીજી, વધુ સામાન્ય રીત પણ છે.

તમે કદાચ ત્યારે `key`s જોયા હશે જ્યારે [સૂચિ રેન્ડર](https://react.dev/learn/rendering-lists#keeping-list-items-in-order-with-key) કરતાં હોય. Keys ફક્ત લિસ્ટ માટે જ નથી! તમે React ને કોઇપણ component વચ્ચે ફરક પાડવામાં મદદ કરવા માટે પણ keys નો ઉપયોગ કરી શકો છો. મૂળરૂપે, React પેરેન્ટની અંદરનો ઓર્ડર ("પ્રથમ કાઉન્ટર", "બીજું કાઉન્ટર") પરથી component વચ્ચે ભેદ કરતું હોય છે. પરંતુ keys દ્વારા તમે React ને કહી શકો છો કે આ ફક્ત *પ્રથમ* કે બીજું* કાઉન્ટર નથી, પણ એક નિર્ધારિત કાઉન્ટર છે—જેમ કે *ટેલરનું* કાઉન્ટર. આ રીતે, React ને જ્યાં પણ *ટેલરનું* કાઉન્ટર ટ્રીમાં દેખાશે ત્યાં ખબર પડશે કે એ કાઉન્ટર એનું છે!

આ ઉદાહરણમાં, બંને `<Counter />`s એ જ જગ્યાએ JSX માં દેખાતા હોવા છતાં, તેઓ state શેર નથી કરતા:

<Sandpack>

```js
import { useState } from 'react';

export default function Scoreboard() {
  const [isPlayerA, setIsPlayerA] = useState(true);
  return (
    <div>
      {isPlayerA ? (
        <Counter key="Taylor" person="Taylor" />
      ) : (
        <Counter key="Sarah" person="Sarah" />
      )}
      <button onClick={() => {
        setIsPlayerA(!isPlayerA);
      }}>
        આગલો ખેલાડી!
      </button>
    </div>
  );
}

function Counter({ person }) {
  const [score, setScore] = useState(0);
  const [hover, setHover] = useState(false);

  let className = 'counter';
  if (hover) {
    className += ' hover';
  }

  return (
    <div
      className={className}
      onPointerEnter={() => setHover(true)}
      onPointerLeave={() => setHover(false)}
    >
      <h1>{person}'s score: {score}</h1>
      <button onClick={() => setScore(score + 1)}>
        એક ઉમેરો
      </button>
    </div>
  );
}
```

```css
h1 {
  font-size: 18px;
}

.counter {
  width: 100px;
  text-align: center;
  border: 1px solid gray;
  border-radius: 4px;
  padding: 20px;
  margin: 0 20px 20px 0;
}

.hover {
  background: #ffffd8;
}
```

</Sandpack>

ટેલર અને સારાહ વચ્ચે બદલવાથી state સંગ્રહિત રહેતી નથી. કારણ: તમે બંનેને અલગ-અલગ key અપાવ્યા છે.

```js
{isPlayerA ? (
  <Counter key="Taylor" person="Taylor" />
) : (
  <Counter key="Sarah" person="Sarah" />
)}
```

`key` સ્પષ્ટ કરવાથી React ને જણાવવામાં આવે છે કે તે પોઝિશન માટે તેમના ઓર્ડરની બદલે `key` જાતે જ વાપરે. એટલે કે, તમે JSX માં એ જ જગ્યાએ તેમને રેન્ડર કરો છતાં પણ, React તેમને બે જુદા કાઉન્ટર તરીકે જુએ છે, તેથી તેઓ ક્યારેય state શેર નથી કરતા. દરેક વખતે જ્યારે કાઉન્ટર સ્ક્રીન પર આવે છે, ત્યારે તેની state બનાવવામાં આવે છે. જ્યારે તે દૂર થાય છે, ત્યારે તેની state નષ્ટ થાય છે. તેમનાં વચ્ચે ટૉગલ કરવાથી state વારંવાર રિસેટ થાય છે.

<Note>

ધ્યાન રાખો કે keys વૈશ્વિક રીતે અનન્ય હોતી નથી. તે માત્ર _પેરેન્ટની અંદર પોઝિશન_ દર્શાવે છે.

</Note>

### ફોર્મને key દ્વારા રિસેટ કરવું {/*resetting-a-form-with-a-key*/}

ફોર્મ સાથે કામ કરતાં વખતે key દ્વારા state રિસેટ કરવું ખાસ કરીને ઉપયોગી છે.

આ ચેટ એપમાં, `<Chat>` component માં ટેક્સ્ટ ઇનપુટ state સામેલ છે:

<Sandpack>

```js src/App.js
import { useState } from 'react';
import Chat from './Chat.js';
import ContactList from './ContactList.js';

export default function Messenger() {
  const [to, setTo] = useState(contacts[0]);
  return (
    <div>
      <ContactList
        contacts={contacts}
        selectedContact={to}
        onSelect={contact => setTo(contact)}
      />
      <Chat contact={to} />
    </div>
  )
}

const contacts = [
  { id: 0, name: 'Taylor', email: 'taylor@mail.com' },
  { id: 1, name: 'Alice', email: 'alice@mail.com' },
  { id: 2, name: 'Bob', email: 'bob@mail.com' }
];
```

```js src/ContactList.js
export default function ContactList({
  selectedContact,
  contacts,
  onSelect
}) {
  return (
    <section className="contact-list">
      <ul>
        {contacts.map(contact =>
          <li key={contact.id}>
            <button onClick={() => {
              onSelect(contact);
            }}>
              {contact.name}
            </button>
          </li>
        )}
      </ul>
    </section>
  );
}
```

```js src/Chat.js
import { useState } from 'react';

export default function Chat({ contact }) {
  const [text, setText] = useState('');
  return (
    <section className="chat">
      <textarea
        value={text}
        placeholder={'Chat to ' + contact.name}
        onChange={e => setText(e.target.value)}
      />
      <br />
      <button>{contact.email} ને મોકલવું</button>
    </section>
  );
}
```

```css
.chat, .contact-list {
  float: left;
  margin-bottom: 20px;
}
ul, li {
  list-style: none;
  margin: 0;
  padding: 0;
}
li button {
  width: 100px;
  padding: 10px;
  margin-right: 10px;
}
textarea {
  height: 150px;
}
```

</Sandpack>

ઇનપુટમાં કંઈક દાખલ કરવાનો પ્રયાસ કરો, અને પછી "Alice" અથવા "Bob" પર ક્લિક કરો કે બીજું રિસિપિઅન્ટ પસંદ કરો. તમે નોટિસ કરશો કે ઇનપુટ state સચવાય રાખવામાં આવે છે કારણ કે `<Chat>` એ ટ્રીમાં એ જ સ્થાન પર રેન્ડર થાય છે.

**ઘણા એપ્સમાં, આ યોગ્ય વર્તન હોઈ શકે છે, પરંતુ ચેટ એપમાં નહીં!** તમે નહીં ઈચ્છો કે વપરાશકર્તાએ ખોટા વ્યક્તિને પહેલેથી લખેલો સંદેશો ભૂલથી મોકલવો. આને ઠીક કરવા માટે, એક `key` ઉમેરો:

```js
<Chat key={to.id} contact={to} />
```

આ સુનિશ્ચિત કરે છે કે જ્યારે તમે અલગ રિસિપિઅન્ટ પસંદ કરો, ત્યારે `Chat` component નવી રીતે ફરીથી બનાવાશે, જેમાં નીચેના તમામ state સહિત. React DOM elements ને ફરીથી બનાવશે, reused કરવાની જગ્યાએ.

હવે, recipient બદલતા જ ટેક્સ્ટ ફિલ્ડ સાફ થઈ જાય છે:

<Sandpack>

```js src/App.js
import { useState } from 'react';
import Chat from './Chat.js';
import ContactList from './ContactList.js';

export default function Messenger() {
  const [to, setTo] = useState(contacts[0]);
  return (
    <div>
      <ContactList
        contacts={contacts}
        selectedContact={to}
        onSelect={contact => setTo(contact)}
      />
      <Chat key={to.id} contact={to} />
    </div>
  )
}

const contacts = [
  { id: 0, name: 'Taylor', email: 'taylor@mail.com' },
  { id: 1, name: 'Alice', email: 'alice@mail.com' },
  { id: 2, name: 'Bob', email: 'bob@mail.com' }
];
```

```js src/ContactList.js
export default function ContactList({
  selectedContact,
  contacts,
  onSelect
}) {
  return (
    <section className="contact-list">
      <ul>
        {contacts.map(contact =>
          <li key={contact.id}>
            <button onClick={() => {
              onSelect(contact);
            }}>
              {contact.name}
            </button>
          </li>
        )}
      </ul>
    </section>
  );
}
```

```js src/Chat.js
import { useState } from 'react';

export default function Chat({ contact }) {
  const [text, setText] = useState('');
  return (
    <section className="chat">
      <textarea
        value={text}
        placeholder={'Chat to ' + contact.name}
        onChange={e => setText(e.target.value)}
      />
      <br />
      <button>{contact.email} ને મોકલવું</button>
    </section>
  );
}
```

```css
.chat, .contact-list {
  float: left;
  margin-bottom: 20px;
}
ul, li {
  list-style: none;
  margin: 0;
  padding: 0;
}
li button {
  width: 100px;
  padding: 10px;
  margin-right: 10px;
}
textarea {
  height: 150px;
}
```

</Sandpack>

<DeepDive>

#### હટાવેલા components માટે state જાળવવું {/*preserving-state-for-removed-components*/}

એક વાસ્તવિક ચેટ એપમાં, તમે કદાચ ઇચ્છો કે જ્યારે વપરાશકર્તા પાછો અગાઉનો રિસિપિઅન્ટ પસંદ કરે, ત્યારે ઇનપુટ state પુનઃપ્રાપ્તિ થાય. એક component જે હવે દેખાતું નથી, તેની state "જીવંત" રાખવા માટે કેટલાક રસ્તા છે:

- તમે ફક્ત વર્તમાન ચેટની જગ્યાએ _બધા_ ચેટ રેન્ડર કરી શકો છો, પરંતુ બાકીના બધા ચેટને CSS દ્વારા છુપાવી શકો છો. આ રીતે, ચેટ ટ્રીમાંથી દૂર નહીં થાય, એટલે તેમની સ્થાનિક state સચવાઈ રહેશે. આ ઉકેલ સરળ UIs માટે ઉત્તમ છે. પરંતુ, જો છુપાયેલા ટ્રી મોટા હોય અને તેમાં ઘણા DOM નોડ્સ હોય, તો આ ધીમી હોઈ શકે છે.
- તમે [state ને ઉપર લાવી શકો છો](/learn/sharing-state-between-components) અને દરેક રિસિપિઅન્ટ માટે પેન્ડિંગ સંદેશો પેરેન્ટ component માં રાખી શકો છો. આ રીતે, જ્યારે ચાઈલ્ડ components દૂર થાય છે, ત્યારે કંઈક ફેરફાર નહીં આવે, કેમ કે મહત્વપૂર્ણ માહિતી પેરેન્ટ પાસે રહેશે. આ સૌથી સામાન્ય ઉકેલ છે.
- તમે React state સિવાય બીજી સંસ્થા પણ ઉપયોગ કરી શકો છો. ઉદાહરણ તરીકે, તમે કદાચ ઇચ્છો કે સંદેશોનો ડ્રાફ્ટ વપરાશકર્તા ભૂલથી પાનું બંધ કરે ત્યારે પણ જાળવાતો રહે. આને અમલમાં મૂકવા માટે, તમે `Chat` component ના state ને [`localStorage`](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage) માંથી વાંચીને આરંભ કરી શકો છો, અને ડ્રાફ્ટ્સને ત્યાં પણ સંગ્રહ કરી શકો છો.

જેવી રીતે તમે કોઈ પણ રીત પસંદ કરો, _Alice સાથે_ ચેટ અને _Bob સાથે_ ચેટ પરિપ્રેક્ષ્યમાં અલગ છે, એટલે આ પ્રમાણે પસંદગીના recipient પર આધાર રાખીને `<Chat>` ટ્રીને `key` આપવી અર્થપૂર્ણ છે.

</DeepDive>

<Recap>

- React state એજ પ્રકારના component ને સચવી રાખે છે, જ્યાં તે એ જ સ્થાન પર રેન્ડર થાય છે.
- State JSX ટૅગ્સમાં સચવાતી નથી. તે તે ટ્રી પોઝિશન સાથે જોડાયેલું છે જ્યાં તમે JSX મૂક્યા છે.
- તમે subtree ને નવા `key` આપીને તેની state ફરીથી સેટ કરાવી શકો છો.
- component નિર્ધારણને nesting ન કરો, નહીંતર તમે ભૂલથી state રિસેટ કરી શકશો.


</Recap>



<Challenges>

#### ગાયબ થતા ઇનપુટ ટેક્સ્ટને ઠીક કરો {/*fix-disappearing-input-text*/}

આ ઉદાહરણમાં, બટન પર ક્લિક કરવાથી એક સંદેશો બતાવે છે. પરંતુ, બટન પર ક્લિક કરવાથી ઇનપુટ પણ ભૂલથી રિસેટ થઈ જાય છે. આ કેમ થાય છે? આને ઠીક કરો જેથી બટન પર ક્લિક કરવાથી ઇનપુટ ટેક્સ્ટ રિસેટ ન થાય.

<Sandpack>

```js src/App.js
import { useState } from 'react';

export default function App() {
  const [showHint, setShowHint] = useState(false);
  if (showHint) {
    return (
      <div>
        <p><i>સંકેત: તમારું પ્રિય શહેર કયું છે?</i></p>
        <Form />
        <button onClick={() => {
          setShowHint(false);
        }}>સંકેત છુપાવો</button>
      </div>
    );
  }
  return (
    <div>
      <Form />
      <button onClick={() => {
        setShowHint(true);
      }}>સંકેત બતાવો</button>
    </div>
  );
}

function Form() {
  const [text, setText] = useState('');
  return (
    <textarea
      value={text}
      onChange={e => setText(e.target.value)}
    />
  );
}
```

```css
textarea { display: block; margin: 10px 0; }
```

</Sandpack>

<Solution>

સમસ્યા એ છે કે `Form` અલગ-અલગ સ્થાનો પર રેન્ડર થાય છે. `if` શાખામાં, તે `<div>`નો બીજો ચાઈલ્ડ છે, પરંતુ `else` શાખામાં, તે પ્રથમ ચાઈલ્ડ છે. તેથી, દરેક પોઝિશનમાં component નો પ્રકાર બદલાય છે. પ્રથમ પોઝિશન `p` અને `Form` વચ્ચે બદલાય છે, જ્યારે બીજું પોઝિશન `Form` અને `button` વચ્ચે બદલાય છે. React દરેક વખતે component નો પ્રકાર બદલાતા જ state રિસેટ કરી દે છે.

સૌથી સરળ ઉકેલ એ છે કે શાખાઓને એકરૂપ બનાવવી, જેથી `Form` હંમેશા એ જ સ્થાન પર રેન્ડર થાય છે:

<Sandpack>

```js src/App.js
import { useState } from 'react';

export default function App() {
  const [showHint, setShowHint] = useState(false);
  return (
    <div>
      {showHint &&
        <p><i>સંકેત: તમારું પ્રિય શહેર કયું છે?</i></p>
      }
      <Form />
      {showHint ? (
        <button onClick={() => {
          setShowHint(false);
        }}>સંકેત છુપાવો</button>
      ) : (
        <button onClick={() => {
          setShowHint(true);
        }}>સંકેત બતાવો</button>
      )}
    </div>
  );
}

function Form() {
  const [text, setText] = useState('');
  return (
    <textarea
      value={text}
      onChange={e => setText(e.target.value)}
    />
  );
}
```

```css
textarea { display: block; margin: 10px 0; }
```

</Sandpack>


ટેકનિકલી, તમે `else` શાખામાં `<Form />` પહેલા `null` પણ ઉમેરીને `if` શાખાની રચના સાથે મેળ કરી શકો છો:

<Sandpack>

```js src/App.js
import { useState } from 'react';

export default function App() {
  const [showHint, setShowHint] = useState(false);
  if (showHint) {
    return (
      <div>
        <p><i>સંકેત: તમારું પ્રિય શહેર કયું છે?</i></p>
        <Form />
        <button onClick={() => {
          setShowHint(false);
        }}>સંકેત છુપાવો</button>
      </div>
    );
  }
  return (
    <div>
      {null}
      <Form />
      <button onClick={() => {
        setShowHint(true);
      }}>સંકેત બતાવો</button>
    </div>
  );
}

function Form() {
  const [text, setText] = useState('');
  return (
    <textarea
      value={text}
      onChange={e => setText(e.target.value)}
    />
  );
}
```

```css
textarea { display: block; margin: 10px 0; }
```

</Sandpack>

આ રીતે, `Form` હંમેશા બીજો ચાઈલ્ડ રહે છે, તેથી તે એ જ સ્થાન પર રહે છે અને તેની state જાળવે છે. પરંતુ આ પદ્ધતિ વધારે સ્પષ્ટ નથી અને એમાં એ ખતરો છે કે બીજું વ્યક્તિ એ `null` હટાવી શકે.

</Solution>

#### બે ફોર્મ ફીલ્ડસનું સ્થાન બદલો {/*swap-two-form-fields*/}

આ ફોર્મ તમને પ્રથમ અને છેલ્લું નામ દાખલ કરવા દે છે. તેમાં એક ચેકબોક્સ પણ છે જે નક્કી કરે છે કયું ફીલ્ડ પહેલું આવશે. જ્યારે તમે ચેકબોક્સ પર ટિક્ક કરે છો, ત્યારે "છેલ્લું નામ" ફીલ્ડ "પ્રથમ નામ" ફીલ્ડ કરતાં પહેલા દેખાવા લાગે છે.

આ લગભગ કામ કરે છે, પરંતુ એક બગ છે. જો તમે "પ્રથમ નામ" ઇનપુટ ભરો અને ચેકબોક્સ પર ટિક કરો, તો લખાણ "પ્રથમ નામ" ઇનપુટમાં જ રહી જાય છે (જે હવે "છેલ્લું નામ" છે). તેને ઠીક કરો જેથી લખાણ પણ ખસે જ્યારે તમે ક્રમ બદલો.

<Hint>

એવું લાગે છે કે આ ફીલ્ડ્સ માટે, પેરેન્ટમાં તેમની પોઝિશન પૂરતી નથી. શું કોઈ એવી રીત છે જે React ને બતાવે છે કે રિ-રેન્ડર વખતે state ને કેવી રીતે મેળ પાડવું?

</Hint>

<Sandpack>

```js src/App.js
import { useState } from 'react';

export default function App() {
  const [reverse, setReverse] = useState(false);
  let checkbox = (
    <label>
      <input
        type="checkbox"
        checked={reverse}
        onChange={e => setReverse(e.target.checked)}
      />
      ક્રમ બદલવો

    </label>
  );
  if (reverse) {
    return (
      <>
        <Field label="Last name" /> 
        <Field label="First name" />
        {checkbox}
      </>
    );
  } else {
    return (
      <>
        <Field label="First name" /> 
        <Field label="Last name" />
        {checkbox}
      </>
    );    
  }
}

function Field({ label }) {
  const [text, setText] = useState('');
  return (
    <label>
      {label}:{' '}
      <input
        type="text"
        value={text}
        placeholder={label}
        onChange={e => setText(e.target.value)}
      />
    </label>
  );
}
```

```css
label { display: block; margin: 10px 0; }
```

</Sandpack>

<Solution>

દોસ્તો `<Field>` components ને બંને `if` અને `else` શાખાઓમાં `key` આપો. આ React ને બતાવે છે કે પેરેન્ટમાં તેમનો ક્રમ બદલાતા હોવા છતાં, કેવી રીતે યોગ્ય state "મેળ પાડવી".

<Sandpack>

```js src/App.js
import { useState } from 'react';

export default function App() {
  const [reverse, setReverse] = useState(false);
  let checkbox = (
    <label>
      <input
        type="checkbox"
        checked={reverse}
        onChange={e => setReverse(e.target.checked)}
      />
      ક્રમ બદલવો

    </label>
  );
  if (reverse) {
    return (
      <>
        <Field key="lastName" label="Last name" /> 
        <Field key="firstName" label="First name" />
        {checkbox}
      </>
    );
  } else {
    return (
      <>
        <Field key="firstName" label="First name" /> 
        <Field key="lastName" label="Last name" />
        {checkbox}
      </>
    );    
  }
}

function Field({ label }) {
  const [text, setText] = useState('');
  return (
    <label>
      {label}:{' '}
      <input
        type="text"
        value={text}
        placeholder={label}
        onChange={e => setText(e.target.value)}
      />
    </label>
  );
}
```

```css
label { display: block; margin: 10px 0; }
```

</Sandpack>

</Solution>

#### વિગત ફોર્મ પુનઃસેટ કરો {/*reset-a-detail-form*/}

આ એડિટ કરી શકાય તેવુ કોન્ટેક્ટ લિસ્ટ છે. તમે પસંદ કરેલા કોન્ટેક્ટની વિગત એડિટ કરી શકો છો અને પછી "સેવ" દબાવીને તેને અપડેટ કરી શકો છો, અથવા "રિસેટ" દબાવીને તમારા ફેરફારો કરી શકો છો.

જ્યારે તમે બીજું કોન્ટેક્ટ પસંદ કરો (ઉદાહરણ તરીકે, એલીસ), ત્યારે સ્ટેટ અપડેટ થાય છે પરંતુ ફોર્મ અગાઉના કોન્ટેક્ટની વિગત બતાવતું રહે છે. આને ઠીક કરો જેથી જ્યારે પસંદ કરેલું કોન્ટેક્ટ બદલાય, ત્યારે ફોર્મ પુનઃસેટ થાય.

<Sandpack>

```js src/App.js
import { useState } from 'react';
import ContactList from './ContactList.js';
import EditContact from './EditContact.js';

export default function ContactManager() {
  const [
    contacts,
    setContacts
  ] = useState(initialContacts);
  const [
    selectedId,
    setSelectedId
  ] = useState(0);
  const selectedContact = contacts.find(c =>
    c.id === selectedId
  );

  function handleSave(updatedData) {
    const nextContacts = contacts.map(c => {
      if (c.id === updatedData.id) {
        return updatedData;
      } else {
        return c;
      }
    });
    setContacts(nextContacts);
  }

  return (
    <div>
      <ContactList
        contacts={contacts}
        selectedId={selectedId}
        onSelect={id => setSelectedId(id)}
      />
      <hr />
      <EditContact
        initialData={selectedContact}
        onSave={handleSave}
      />
    </div>
  )
}

const initialContacts = [
  { id: 0, name: 'Taylor', email: 'taylor@mail.com' },
  { id: 1, name: 'Alice', email: 'alice@mail.com' },
  { id: 2, name: 'Bob', email: 'bob@mail.com' }
];
```

```js src/ContactList.js
export default function ContactList({
  contacts,
  selectedId,
  onSelect
}) {
  return (
    <section>
      <ul>
        {contacts.map(contact =>
          <li key={contact.id}>
            <button onClick={() => {
              onSelect(contact.id);
            }}>
              {contact.id === selectedId ?
                <b>{contact.name}</b> :
                contact.name
              }
            </button>
          </li>
        )}
      </ul>
    </section>
  );
}
```

```js src/EditContact.js
import { useState } from 'react';

export default function EditContact({ initialData, onSave }) {
  const [name, setName] = useState(initialData.name);
  const [email, setEmail] = useState(initialData.email);
  return (
    <section>
      <label>
        Name:{' '}
        <input
          type="text"
          value={name}
          onChange={e => setName(e.target.value)}
        />
      </label>
      <label>
        Email:{' '}
        <input
          type="email"
          value={email}
          onChange={e => setEmail(e.target.value)}
        />
      </label>
      <button onClick={() => {
        const updatedData = {
          id: initialData.id,
          name: name,
          email: email
        };
        onSave(updatedData);
      }}>
        સેવ
      </button>
      <button onClick={() => {
        setName(initialData.name);
        setEmail(initialData.email);
      }}>
        રિસેટ
      </button>
    </section>
  );
}
```

```css
ul, li {
  list-style: none;
  margin: 0;
  padding: 0;
}
li { display: inline-block; }
li button {
  padding: 10px;
}
label {
  display: block;
  margin: 10px 0;
}
button {
  margin-right: 10px;
  margin-bottom: 10px;
}
```

</Sandpack>

<Solution>

`EditContact` કોમ્પોનેન્ટને `key={selectedId}` આપો. આ રીતે, વિવિધ કોન્ટેક્ટ્સ વચ્ચે સ્વિચ કરવાથી ફોર્મ પુનઃસેટ થશે.

<Sandpack>

```js src/App.js
import { useState } from 'react';
import ContactList from './ContactList.js';
import EditContact from './EditContact.js';

export default function ContactManager() {
  const [
    contacts,
    setContacts
  ] = useState(initialContacts);
  const [
    selectedId,
    setSelectedId
  ] = useState(0);
  const selectedContact = contacts.find(c =>
    c.id === selectedId
  );

  function handleSave(updatedData) {
    const nextContacts = contacts.map(c => {
      if (c.id === updatedData.id) {
        return updatedData;
      } else {
        return c;
      }
    });
    setContacts(nextContacts);
  }

  return (
    <div>
      <ContactList
        contacts={contacts}
        selectedId={selectedId}
        onSelect={id => setSelectedId(id)}
      />
      <hr />
      <EditContact
        key={selectedId}
        initialData={selectedContact}
        onSave={handleSave}
      />
    </div>
  )
}

const initialContacts = [
  { id: 0, name: 'Taylor', email: 'taylor@mail.com' },
  { id: 1, name: 'Alice', email: 'alice@mail.com' },
  { id: 2, name: 'Bob', email: 'bob@mail.com' }
];
```

```js src/ContactList.js
export default function ContactList({
  contacts,
  selectedId,
  onSelect
}) {
  return (
    <section>
      <ul>
        {contacts.map(contact =>
          <li key={contact.id}>
            <button onClick={() => {
              onSelect(contact.id);
            }}>
              {contact.id === selectedId ?
                <b>{contact.name}</b> :
                contact.name
              }
            </button>
          </li>
        )}
      </ul>
    </section>
  );
}
```

```js src/EditContact.js
import { useState } from 'react';

export default function EditContact({ initialData, onSave }) {
  const [name, setName] = useState(initialData.name);
  const [email, setEmail] = useState(initialData.email);
  return (
    <section>
      <label>
        Name:{' '}
        <input
          type="text"
          value={name}
          onChange={e => setName(e.target.value)}
        />
      </label>
      <label>
        Email:{' '}
        <input
          type="email"
          value={email}
          onChange={e => setEmail(e.target.value)}
        />
      </label>
      <button onClick={() => {
        const updatedData = {
          id: initialData.id,
          name: name,
          email: email
        };
        onSave(updatedData);
      }}>
        સેવ
      </button>
      <button onClick={() => {
        setName(initialData.name);
        setEmail(initialData.email);
      }}>
        રિસેટ
      </button>
    </section>
  );
}
```

```css
ul, li {
  list-style: none;
  margin: 0;
  padding: 0;
}
li { display: inline-block; }
li button {
  padding: 10px;
}
label {
  display: block;
  margin: 10px 0;
}
button {
  margin-right: 10px;
  margin-bottom: 10px;
}
```

</Sandpack>

</Solution>

#### ઇમેજ લોડ થતી વખતે તેને હટાવો {/*clear-an-image-while-its-loading*/}

જ્યારે તમે "નેક્સ્ટ" પર ક્લિક કરો, બ્રાઉઝર નવી છબી લોડ કરવાનું શરૂ કરે છે. પરંતુ, કારણ કે તે એ જ `<img>` ટેગમાં બતાવવામાં આવે છે, પહેલાંની છબી નવા લોડ થવા સુધી દેખાતી રહે છે. જો છબી અને ટેક્સ્ટ એકજ રહેવું જરૂરી છે, તો આ નકળી હોઈ શકે છે. આને એ રીતે બદલો કે "નેક્સ્ટ" પર ક્લિક કરતાં પહેલાં, જૂની છબી તરત જ દૂર થઈ જાય.

<Hint>

શું એવી કોઈ રીત છે જેને React ને DOM ને ફરીથી બનાવવા માટે કહીએ, એના બદલે કે તે તેનો ઉપયોગ ફરીથી કરે?

</Hint>

<Sandpack>

```js
import { useState } from 'react';

export default function Gallery() {
  const [index, setIndex] = useState(0);
  const hasNext = index < images.length - 1;

  function handleClick() {
    if (hasNext) {
      setIndex(index + 1);
    } else {
      setIndex(0);
    }
  }

  let image = images[index];
  return (
    <>
      <button onClick={handleClick}>
        આગળ
      </button>
      <h3>
        છબી {index + 1} of {images.length}
      </h3>
      <img src={image.src} />
      <p>
        {image.place}
      </p>
    </>
  );
}

let images = [{
  place: 'Penang, Malaysia',
  src: 'https://i.imgur.com/FJeJR8M.jpg'
}, {
  place: 'Lisbon, Portugal',
  src: 'https://i.imgur.com/dB2LRbj.jpg'
}, {
  place: 'Bilbao, Spain',
  src: 'https://i.imgur.com/z08o2TS.jpg'
}, {
  place: 'Valparaíso, Chile',
  src: 'https://i.imgur.com/Y3utgTi.jpg'
}, {
  place: 'Schwyz, Switzerland',
  src: 'https://i.imgur.com/JBbMpWY.jpg'
}, {
  place: 'Prague, Czechia',
  src: 'https://i.imgur.com/QwUKKmF.jpg'
}, {
  place: 'Ljubljana, Slovenia',
  src: 'https://i.imgur.com/3aIiwfm.jpg'
}];
```

```css
img { width: 150px; height: 150px; }
```

</Sandpack>

<Solution>

તમે `<img>` ટેગને `key` આપી શકો છો. જયારે તે `key` બદલાય છે, React એ `<img>` DOM નોડને ફરીથી બનાવશે. આ દરેક છબી લોડ થાય ત્યારે એક સંક્ષિપ્ત ફ્લેશ ઊભો કરે છે, તેથી આ તમારા એપમાં દરેક છબી માટે કરવું યોગ્ય નહીં હોય. પરંતુ આ ત્યારે યોગ્ય છે જ્યારે તમે ખાતરી કરવા માંગતા હો કે છબી હંમેશા ટેક્સ્ટ સાથે મેળ ખાય છે.

<Sandpack>

```js
import { useState } from 'react';

export default function Gallery() {
  const [index, setIndex] = useState(0);
  const hasNext = index < images.length - 1;

  function handleClick() {
    if (hasNext) {
      setIndex(index + 1);
    } else {
      setIndex(0);
    }
  }

  let image = images[index];
  return (
    <>
      <button onClick={handleClick}>
        આગળ
      </button>
      <h3>
        છબી {index + 1} of {images.length}
      </h3>
      <img key={image.src} src={image.src} />
      <p>
        {image.place}
      </p>
    </>
  );
}

let images = [{
  place: 'Penang, Malaysia',
  src: 'https://i.imgur.com/FJeJR8M.jpg'
}, {
  place: 'Lisbon, Portugal',
  src: 'https://i.imgur.com/dB2LRbj.jpg'
}, {
  place: 'Bilbao, Spain',
  src: 'https://i.imgur.com/z08o2TS.jpg'
}, {
  place: 'Valparaíso, Chile',
  src: 'https://i.imgur.com/Y3utgTi.jpg'
}, {
  place: 'Schwyz, Switzerland',
  src: 'https://i.imgur.com/JBbMpWY.jpg'
}, {
  place: 'Prague, Czechia',
  src: 'https://i.imgur.com/QwUKKmF.jpg'
}, {
  place: 'Ljubljana, Slovenia',
  src: 'https://i.imgur.com/3aIiwfm.jpg'
}];
```

```css
img { width: 150px; height: 150px; }
```

</Sandpack>

</Solution>

#### લિસ્ટમાં ખોટી જગ્યાએ રહેલી state ઠીક કરો {/*fix-misplaced-state-in-the-list*/}

આ લિસ્ટમાં દરેક Contact પાસે તેની state છે, જે બતાવે છે કે તેની માટે "ઈમેઇલ બતાવો" બટન દબાયું છે કે નહીં.  
તમે જો Alice માટે "ઈમેઇલ બતાવો" બટન દબાવો, અને પછી "ક્રમ બદલવો" ચેકબોક્સ પર ટિક્ક કરો,  
તો તમે જોશો કે હવે _Taylor_ નું ઈમેઇલ ખુલેલું દેખાય છે, જ્યારે Alice — જે હવે નીચે ખસે ગઈ છે — તેનું ઈમેઇલ બંધ દેખાય છે.

આને ઠીક કરો જેથી દરેક contact સાથે તેનો ખુલેલો state જોડાય, અને ક્રમ બદલવા પર પણ તેનો અસર નો થાય.

<Sandpack>

```js src/App.js
import { useState } from 'react';
import Contact from './Contact.js';

export default function ContactList() {
  const [reverse, setReverse] = useState(false);

  const displayedContacts = [...contacts];
  if (reverse) {
    displayedContacts.reverse();
  }

  return (
    <>
      <label>
        <input
          type="checkbox"
          value={reverse}
          onChange={e => {
            setReverse(e.target.checked)
          }}
        />{' '}
        વિપરીત ક્રમમાં બતાવો

      </label>
      <ul>
        {displayedContacts.map((contact, i) =>
          <li key={i}>
            <Contact contact={contact} />
          </li>
        )}
      </ul>
    </>
  );
}

const contacts = [
  { id: 0, name: 'Alice', email: 'alice@mail.com' },
  { id: 1, name: 'Bob', email: 'bob@mail.com' },
  { id: 2, name: 'Taylor', email: 'taylor@mail.com' }
];
```

```js src/Contact.js
import { useState } from 'react';

export default function Contact({ contact }) {
  const [expanded, setExpanded] = useState(false);
  return (
    <>
      <p><b>{contact.name}</b></p>
      {expanded &&
        <p><i>{contact.email}</i></p>
      }
      <button onClick={() => {
        setExpanded(!expanded);
      }}>
        {expanded ? 'Hide' : 'Show'} email
      </button>
    </>
  );
}
```

```css
ul, li {
  list-style: none;
  margin: 0;
  padding: 0;
}
li {
  margin-bottom: 20px;
}
label {
  display: block;
  margin: 10px 0;
}
button {
  margin-right: 10px;
  margin-bottom: 10px;
}
```

</Sandpack>

<Solution>

સમસ્યા એ છે કે આ ઉદાહરણમાં `key` તરીકે ઇન્ડેક્સનો ઉપયોગ કરવામાં આવી રહ્યો હતો:

```js
{displayedContacts.map((contact, i) =>
  <li key={i}>
```

તેથી, તમે ઈચ્છો છો કે state _પ્રતિસાદ તરીકે દરેક વિશિષ્ટ contact_ સાથે જોડાય.

સામાન્ય રીતે, contact ID ને `key` તરીકે ઉપયોગ કરવાથી આ સમસ્યા ઠીક થઈ જાય છે:

<Sandpack>

```js src/App.js
import { useState } from 'react';
import Contact from './Contact.js';

export default function ContactList() {
  const [reverse, setReverse] = useState(false);

  const displayedContacts = [...contacts];
  if (reverse) {
    displayedContacts.reverse();
  }

  return (
    <>
      <label>
        <input
          type="checkbox"
          value={reverse}
          onChange={e => {
            setReverse(e.target.checked)
          }}
        />{' '}
        Show in ક્રમ બદલવો

      </label>
      <ul>
        {displayedContacts.map(contact =>
          <li key={contact.id}>
            <Contact contact={contact} />
          </li>
        )}
      </ul>
    </>
  );
}

const contacts = [
  { id: 0, name: 'Alice', email: 'alice@mail.com' },
  { id: 1, name: 'Bob', email: 'bob@mail.com' },
  { id: 2, name: 'Taylor', email: 'taylor@mail.com' }
];
```

```js src/Contact.js
import { useState } from 'react';

export default function Contact({ contact }) {
  const [expanded, setExpanded] = useState(false);
  return (
    <>
      <p><b>{contact.name}</b></p>
      {expanded &&
        <p><i>{contact.email}</i></p>
      }
      <button onClick={() => {
        setExpanded(!expanded);
      }}>
        {expanded ? 'Hide' : 'Show'} email
      </button>
    </>
  );
}
```

```css
ul, li {
  list-style: none;
  margin: 0;
  padding: 0;
}
li {
  margin-bottom: 20px;
}
label {
  display: block;
  margin: 10px 0;
}
button {
  margin-right: 10px;
  margin-bottom: 10px;
}
```

</Sandpack>

State શ્રેણીની સ્થાન સાથે જોડાય છે. `key` તમને ક્રમ પર આધાર રાખ્યા વગર એક નામાંકિત સ્થાન નિર્ધારિત કરવાની સુવિધા આપે છે.

</Solution>

</Challenges>

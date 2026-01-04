---
title: "Guide: Latin-based and Alphanumeric Tashlhiyt Scripting with Translations"
date: 2018-11-18T12:33:46+10:00
weight: 1
show_in_listing: false
---


<div style="text-align: justify;">
  <p>
    In the Common Voice project, some of the questions (prompts) are written using the
    <strong>standard Latin-based script</strong> commonly used to write Tachelhit today.
    While this writing system is widely adopted in academic, educational, and digital contexts,
    it may not be familiar or comfortable for everyone. Some contributors may have learned
    Tachelhit orally, or through other informal or alternative writing conventions, which can
    make reading or answering these questions more challenging.
  </p>
</div>

## Guide

<div style="text-align: justify;">
    <p>
        To support contributors who may struggle with the standard Latin-based script, we provide
        the table below as a practical reference. This table shows how specific Tachelhit sounds
        (phonemes) are represented across different writing conventions: the extended Latin script,
        more simplified or regional Latin usages, and alternative numeric-based representations.
        Each sound is also illustrated with an example word to make the correspondence clearer.
        The goal is not to impose a single way of writing, but to help readers recognize familiar
        sounds across different systems.
    </p>
</div>

| Phoneme (IPA) | Standard | Reduced | Alphanumeric | Example |
|---------------|----------------------------|-----------------------------|---------------------------------|---------|
| /ʔ/ | **ʼ** | ae | 2 | mu'xaran |
| /ʕ/ | **ɛ** | a / aa | 3 | ɛli (Ali) |
| /ħ/ | **ḥ** | h | 7 | ḥmad (Ahmad) |
| /ɣ/ | **ɣ** | gh / r | gh / r | iɣ (if) |
| /χ/ | **x** | kh / x | 5 | ixsi (turned off) |
| /ḍ/ | **ḍ** | d | d | aḍu (wind) |
| /ṣ/ | **ṣ** | s | s | ṣbr (wait) |
| /ṭ/ | **ṭ** | t | t | iṭṭas (slept) |
| /ṛ/ | **ṛ** | r | r | aṛam (camel) |
| /ẓ/ | **ẓ** | z | z | iẓi (argued) |
| /q/ | **q** | q | 9 | aqllal (head) |
| /ʃ/ | **c** | ch | ch | kcm (go) |
| /w/ | **.ᵂ** | ∅ | ∅ | asggᵂas (year) |

## Translated Sentences

<div style="text-align: justify;">
  <p>
    In addition to the phoneme table, the questions themselves are provided with translations
    to make comprehension easier. These translations are included to reduce reading difficulty
    and to allow contributors to focus on meaning rather than decoding unfamiliar spelling.
    They are meant as a support tool, helping users confidently understand the questions before
    answering them in their own words.
  </p>
</div>

<style>
  .question-block {
    border: 1px solid red;
    padding: 12px;
    margin: 12px 0;
  }
  .question-block .en {
    font-weight: bold;
    margin: 0 0 6px 0;
  }
  .question-block .sh {
    margin: 0;
  }
  .question-with-audio {
    display: flex;
    align-items: center;
    gap: 12px;
  }

  .question-text {
    flex: 1;
  }

  .question-audio audio {
    /* background-color: blue;  */
    padding-left: 100px;
    width: 160px;
  }
</style>

<div id="questions"></div>

<script>
// CSV parser that handles quoted fields
function parseCSV(text) {
  const rows = [];
  let currentRow = [];
  let currentField = '';
  let insideQuotes = false;

  for (let i = 0; i < text.length; i++) {
    const char = text[i];
    const nextChar = text[i + 1];

    if (char === '"') {
      if (insideQuotes && nextChar === '"') {
        currentField += '"';
        i++;
      } else {
        insideQuotes = !insideQuotes;
      }
    } else if (char === ',' && !insideQuotes) {
      currentRow.push(currentField.trim());
      currentField = '';
    } else if ((char === '\n' || char === '\r') && !insideQuotes) {
      if (currentField || currentRow.length > 0) {
        currentRow.push(currentField.trim());
        if (currentRow.some(f => f)) {
          rows.push(currentRow);
        }
        currentRow = [];
        currentField = '';
      }
      if (char === '\r' && nextChar === '\n') {
        i++;
      }
    } else {
      currentField += char;
    }
  }

  if (currentField || currentRow.length > 0) {
    currentRow.push(currentField.trim());
    if (currentRow.some(f => f)) {
      rows.push(currentRow);
    }
  }

  return rows;
}

fetch('/assets/csv/questions.csv')
  .then(response => response.text())
  .then(text => {
    const rows = parseCSV(text);
    const headers = rows.shift(); // Remove header row

    // Transform each row into an object
    const data = rows.map(row => {
      const [id, en, sh, audio] = row;
      return { id, en, sh, audio };
    });

    // Use all questions
    const selected = data;

    const container = document.getElementById('questions');

    selected.forEach(q => {
      const div = document.createElement('div');
      div.className = 'question-block question-with-audio';

      div.innerHTML = `
        <div class="question-text">
          <p class="en">${q.en}</p>
          <p class="sh">${q.sh}</p>
        </div>
        <div class="question-audio">
          <audio controls preload="none">
            <source src="/assets/audios/${q.audio}" type="audio/mpeg">
          </audio>
        </div>
      `;

      container.appendChild(div);
    });
  });
</script>



<!-- 
<div class="question-block question-with-audio">
  <div class="question-text">
    <p class="en">What traditional dishes do you prepare during major celebrations?</p>
    <p class="sh">Man tirmt ad tsujadt ɣ laɛyad mqqurnin ?</p>
  </div>

  <div class="question-audio">
    <audio controls preload="none">
      <source src="/assets/audio/question-01.mp3" type="audio/mpeg">
      Your browser does not support the audio element.
    </audio>
  </div>
</div>

<div class="question-block">
  <p class="en">What are the most commonly used musical instruments in your region?</p>
  <p class="sh">Mad gan alat n lmusiqa lli bahra ittustɛmaln ɣ tamazirt nnk ?</p>
</div>

<div class="question-block">
  <p class="en">Could you tell me a proverb in Tachelhit and explain its meaning?</p>
  <p class="sh">Is tufit ad iyi tbdrt kra n lmaqula s tclḥit d ad iyi tcrḥrt lmɛna nns ?</p>
</div>

<div class="question-block">
  <p class="en">Can you describe a traditional costume worn during celebrations?</p>
  <p class="sh">Is tufit ad iyi tuṣft kra n lksut tabldit lli lssan mddn ɣ lficṭat ?</p>
</div>

<div class="question-block">
  <p class="en">How is traditional bread prepared?</p>
  <p class="sh">Manik a s ar nssnwa aɣrum abldi ?</p>
</div>

<div class="question-block">
  <p class="en">Which herbs are used to treat illnesses?</p>
  <p class="sh">Man lɛcub ad n stɛmal bac ad nsjji timuḍan ?</p>
</div>

<div class="question-block">
  <p class="en">Can you describe a traditional song that everyone knows?</p>
  <p class="sh">Is tufit ad iyi tuṣft kra n luɣnya tabldit lli kullu ssnn mddn ?</p>
</div>

<div class="question-block">
  <p class="en">Can you describe the differences between life in the city and in the countryside?</p>
  <p class="sh">Is tufit ad iyi tuṣft lfrq gr lmɛict n lmdint d tin tmazirt ?</p>
</div>

<div class="question-block">
  <p class="en">What is the relationship between people and their domestic animals?</p>
  <p class="sh">Mad igan lɛalaqa gr mddn d lḥayawanat n tgmmi ?</p>
</div>

<div class="question-block">
  <p class="en">What do you like to do when you have free time?</p>
  <p class="sh">Mad dark iɛzzan ad tskart iɣ dark txwa luqt ?</p>
</div>

<div class="question-block">
  <p class="en">What is the first thing you do in the morning?</p>
  <p class="sh">Mad tga lḥajt izwarn lli tskart zik sbaḥ ?</p>
</div>

<div class="question-block">
  <p class="en">What do you like to eat when you’re in a good mood?</p>
  <p class="sh">Mad dark iɛzzan ad tcttat iɣ k iɛjb lḥal ?</p>
</div>

<div class="question-block">
  <p class="en">What annoys you the most in daily life?</p>
  <p class="sh">Mad k bahra isɛṣṣabn ɣ tudrt nnk n kay gat ass ?</p>
</div>

<div class="question-block">
  <p class="en">What do you do when you can’t sleep?</p>
  <p class="sh">Mad tskart iɣ k ur yiwi yiḍṣ ?</p>
</div>

<div class="question-block">
  <p class="en">What do you think is the most important thing to pass on to children?</p>
  <p class="sh">Mad tga lḥajt lli bahra dark istawhmman is ixssa a s tnzzri i tarwa ?</p>
</div>

<div class="question-block">
  <p class="en">What makes the Chleuh people special or different from others?</p>
  <p class="sh">Mad ittajan iclḥiyn tmyzn nɣ d xtalfn f wiyyad ?</p>
</div>

<div class="question-block">
  <p class="en">How do young people spend their free time in your village or city?</p>
  <p class="sh">Manik a s ar zzrayn ccabab luqt lli darsn ixwan ɣ tmazirt nnk nɣ d lmdint nnk ?</p>
</div>

<div class="question-block">
  <p class="en">How do you prepare mint tea?</p>
  <p class="sh">Manik a s ar nn tggat atay n nnɛnaɛ ?</p>
</div>

<div class="question-block">
  <p class="en">Can you talk about the differences between religious celebrations in the city and in the countryside?</p>
  <p class="sh">Tẓḍaṛt ad tsawlt f lfrq lli illan gr lḥafalat ddinya ɣ lmdint d tmazirt ?</p>
</div>

<div class="question-block">
  <p class="en">How do young people see the future of the Tachelhit language?</p>
  <p class="sh">Manik a s ar smuqquln ccabab lmustaqbal n tclḥit ?</p>
</div>

<div class="question-block">
  <p class="en">What are the main challenges that people in your city or village face today?</p>
  <p class="sh">Mad gan imukrisn mqqurnin lli s ar ttɛanayn middn n lmdint nnk nɣ d tamazirt nnk ɣ ass ad ?</p>
</div>

<div class="question-block">
  <p class="en">How do you think technologies in Tachelhit could help the Chleuh people in their lives?</p>
  <p class="sh">Manik a s ar ak ttbayan rad tɛawn ttiknulujya n tclḥit iclḥiyn ɣ tudrt nnsn ?</p>
</div>

<div class="question-block">
  <p class="en">What is something you’ve learned recently that made you think deeply?</p>
  <p class="sh">Mad tga taɣawsa lli tɛllmt mu'xaran lli k yujjan ad tswingmt</p>
</div>

<div class="question-block">
  <p class="en">How do you think young people can help improve their region?</p>
  <p class="sh">Manik a s ar ak ittbayan ẓḍaṛn ccabab ad sis ɛawenn bac ad ṭuwwṛn ljiha nnsn ?</p>
</div>

<div class="question-block">
  <p class="en">In your opinion, what are the advantages that smartphones have brought to our lives?</p>
  <p class="sh">Ɣ ṛṛa'i nnk, mad gan tiɣawsiwin fulkinin lli d iwin smatphones s tudrt nnɣ ?</p>
</div>

<div class="question-block">
  <p class="en">What was your favorite subject at school, and why?</p>
  <p class="sh">Mad yadlli tga lmadda lli kullu dark iɛzzan ɣ lmdrasa, d maxx ?</p>
</div>

<div class="question-block">
  <p class="en">What is your dream job, and why?</p>
  <p class="sh">Mad tga lxdmt lli s bdda ttwargat, d maxx ?</p>
</div>

<div class="question-block">
  <p class="en">What do you do to stay fit and healthy?</p>
  <p class="sh">Mad tskart bac ad sti tawit ɣ lforma nnk d ṣṣaḥt nnk ?</p>
</div>

<div class="question-block">
  <p class="en">Can you describe your favorite place in your region and why you love it?</p>
  <p class="sh">Tẓḍaṛt ad iyi tuṣft lblast lli k bahra ittɛjabn ɣ ljiha nnk d max alliɣ tt tḥmlt ?</p>
</div>

<div class="question-block">
  <p class="en">Can you talk about the most important thing your parents taught you?</p>
  <p class="sh">Tẓḍaṛt ad iyi tsawlt f tɣawsa lli kullu ihmman lli ak mlan ayt dark ?</p>
</div>

<div class="question-block">
  <p class="en">What are the values you respect the most in your community?</p>
  <p class="sh">Mad gant lqiyyam lli bahra ttḥtaramt ɣ ayt tmazirt nnk ?</p>
</div>

<div class="question-block">
  <p class="en">What is the one thing you would like the whole world to know about the Chleuh people?</p>
  <p class="sh">Mad tga lḥajt lli trit ad issn lɛalam kullu t f iclḥiyn ?</p>
</div>

<div class="question-block">
  <p class="en">Can you describe how your village or city has changed in recent years?</p>
  <p class="sh">Tẓḍaṛt ad tuṣft manik s tbadl tamazirt nnk nɣ d lmdint nnk ɣ isggʷasn ad ggʷranin ?</p>
</div>

<div class="question-block">
  <p class="en">If you won a lot of money, what would you do?</p>
  <p class="sh">Iɣ trbḥt agudi n lflus, mad rad tskrt ?</p>
</div>

<div class="question-block">
  <p class="en">What do you like to do when you are alone?</p>
  <p class="sh">Mad ttirit ad tskart iɣ tgit waḥdu k ?</p>
</div>

<div class="question-block">
  <p class="en">What is your favorite dish, and how do you prepare it?</p>
  <p class="sh">Man tirmt ad dark iɛzzan d manmk a s ar tt tɛdalt ?</p>
</div>

<div class="question-block">
  <p class="en">If you could travel anywhere, where would you go and why?</p>
  <p class="sh">Iɣ tufit ad tsafart, mani s trit ad tddut d maxx ?</p>
</div>

<div class="question-block">
  <p class="en">What always makes you laugh?</p>
  <p class="sh">Mad k bdda issḍṣan ?</p>
</div>

<div class="question-block">
  <p class="en">If you could change just one thing in the world, what would it be?</p>
  <p class="sh">Iɣ tẓḍaṛt ad tsbadlt kra ɣ ddunit ad, mad rad tg ?</p>
</div>

<div class="question-block">
  <p class="en">If you had to give advice to a 15-year-old, what would you tell them?</p>
  <p class="sh">Iɣ ixssa ad tfkt kra n nnaṣiḥa i kra n wazzan n 15 n usggʷas, mad rad as tinit ?</p>
</div>

<div class="question-block">
  <p class="en">Can you tell me about a dream you had that you found strange?</p>
  <p class="sh">Is tẓḍaṛt ad iyi tɛawdt kra n twargit lli tskrt tg ak d lɛjb ?</p>
</div>

<div class="question-block">
  <p class="en">What would you do if you could go back in time for one day?</p>
  <p class="sh">Mad rad tskrt iɣ tẓḍaṛt ad tssurrit luqt kra n wass ?</p>
</div>

<div class="question-block">
  <p class="en">What is the most beautiful lesson life has taught you so far?</p>
  <p class="sh">Mad iga ddaṛṣ lli kullu ifulkin ad ak tmla tudrt ar ɣid ?</p>
</div>

<div class="question-block">
  <p class="en">If Tachelhit disappeared tomorrow, how would you feel?</p>
  <p class="sh">Iɣ laḥḥ taclḥit askka, mad s rad tḥussut ?</p>
</div>

<div class="question-block">
  <p class="en">Which song always makes you feel sad, and why?</p>
  <p class="sh">Man lmusiqa ad k bdda isqllaqn d max ?</p>
</div>

<div class="question-block">
  <p class="en">How do you see the future of your region in twenty years?</p>
  <p class="sh">Mannik a s ar ak ittbayan lmustaqbal n ljiha nnk ɣid s yat 20 usggʷas ?</p>
</div>

<div class="question-block">
  <p class="en">If you could change one habit among the Chleuh people, which one would you choose?</p>
  <p class="sh">Iɣ tufit ad tsbadlt kra n lɛada ɣ iclḥiyn, mad rad tg ?</p>
</div>

<div class="question-block">
  <p class="en">If you had the chance to teach something to everyone, what would you choose?</p>
  <p class="sh">Iɣ tufit bac ad tsɛlmt kra n tɣawsa i lɛalam kullu t, mad rad tg ?</p>
</div>

<div class="question-block">
  <p class="en">What do you think about the fact that many young people no longer speak Tachelhit well?</p>
  <p class="sh">Mad ak ibayn ɣ kigan n ccabab lli ur sul ssnn ad sawaln taclḥit mzyan ?</p>
</div>

<div class="question-block">
  <p class="en">How do you imagine life without the Internet?</p>
  <p class="sh">Manik a s ar tzrrat tudrt bla lantirnit ?</p>
</div>

<div class="question-block">
  <p class="en">What do you think of people who forget their traditions?</p>
  <p class="sh">Mad ak ittbayann ɣ mddn lli itettun lɛadat nnsn ?</p>
</div>

<div class="question-block">
  <p class="en">If you could speak to the government, what would you say to help your region?</p>
  <p class="sh">Iɣ tufit ad tsawlt s lḥukuma, mad rad asn tinit bac ad tɛawnt ljiha nnk ?</p>
</div>

<div class="question-block">
  <p class="en">If you had to choose between living by the sea or in the mountains, which would you choose and why?</p>
  <p class="sh">Iɣ d yiwi rbbi rad tstit gr ad tɛict ɣ tama n lbḥṛ nɣ d ɣ udrar, mad rad tstit d maxx ?</p>
</div>

<div class="question-block">
  <p class="en">What do you think of movies or series in Tachelhit?</p>
  <p class="sh">Mad ak ittbayann ɣ laflam d lmusalsalat n tclḥit ?</p>
</div>

<div class="question-block">
  <p class="en">If you could learn something from the elders, what would you want to learn?</p>
  <p class="sh">Iɣ tẓḍaṛt ad tɛllmt kra zɣ iqqdimn, mad rad tirit ad tɛllmt ?</p>
</div>

<div class="question-block">
  <p class="en">Do you think Tachelhit should be taught in schools, and why?</p>
  <p class="sh">Is ak ibayn is ixṣṣa ad sɣrn tclḥit ɣ lmdrasa, d maxx ?</p>
</div>

<div class="question-block">
  <p class="en">If you could meet a famous person, who would you choose?</p>
  <p class="sh">Iɣ rad tmiqqirt kra n yan ittussann, matta wad a rad tstit ?</p>
</div>

<div class="question-block">
  <p class="en">What do you think of customs that people keep without really understanding them?</p>
  <p class="sh">Mad ak ittbayann ɣ lɛadat lli nttajja bla tnt nit nfhm ?</p>
</div>

<div class="question-block">
  <p class="en">What do you think about those who go live abroad?</p>
  <p class="sh">Mad ak ittbayann ɣ willi ifttun ad ɛicn ɣ lxarij ?</p>
</div>

<div class="question-block">
  <p class="en">How do you choose your friends?</p>
  <p class="sh">Manik ad tskart bac ad tstit imddukal nnk ?</p>
</div>
 -->

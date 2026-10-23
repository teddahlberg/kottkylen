<script setup lang="ts">
import { ref, computed } from 'vue';

/**
 * Story Data Structure
 * Each stage contains:
 * - image: The image to display
 * - text: The story text to show
 * - choices: Array of options, each leading to another stage ID
 */
const story = {
  start: {
    image: '/start.jpg',
    text: 'Vinden piskar genom Sundsvalls gator. Tre legender, återförenade. På Navet står de nu sida vid sida. De har räddat världen från total undergång mer gånger än de kan räkna, men nu står de inför sitt hittills svåraste uppdrag: After Work. En enda kritisk fråga återstår för att säkra operationens framgång... var ska den första ölen intas?',
    choices: [
      { text: 'Bishops Arms', next: 'tavern' },
      { text: 'Daltons', next: 'daltons_tavern' },
      { text: 'Estreet', next: 'streets' },
      { text: 'Olearys', next: 'tavern' },
      { text: 'Ställ in, far hem', next: 'forced_game_over' },
    ],
  },
  transit: {
    image: '/transit.jpg',
    text: 'Hjältarna beger sig mot sitt mål...',
    choices: [
      { text: 'Fortsätt', next: '__PENDING__' },
    ],
  },
  tavern: {
    image: '/bishops.jpg',
    text: 'Inne på Bishops Arms flyter ölen i floder och skålarna ekar mellan väggarna. Men mitt i den episka festligheten dånar marken. Ett vrål som får blodet att frysa till is hörs från stadens centrum. Något är på gång på torget, och det ser inte ut som en vanlig fredagskväll.',
    choices: [
      { text: 'Ta en öl till först', next: 'beer_refill' },
      { text: 'Ut på torget!', next: 'square' },
    ],
  },
  daltons_tavern: {
    image: '/daltons.jpg',
    text: 'Inne på Daltons är luften tung av gammal tobak och golvet klibbar om fötterna. Det är ett riktigt sunk, men ölen är kall och stämningen rå. Plötsligt skälver väggarna och ett avgrundsvrål skär genom ljudet av jukeboxen. Något händer på torget, och det låter som om världen håller på att gå under.',
    choices: [
      { text: 'Ta en öl till först', next: 'beer_refill' },
      { text: 'Ut på torget!', next: 'square' },
    ],
  },
  streets: {
    image: 'https://picsum.photos/800/450?random=3',
    text: 'Vid Estreet njuter hjältarna av den friska luften och en välförtjänt drink. Men lugnet är bedrägligt. Plötsligt skakar marken under deras fötter och ett avgrundsvrål ekar mellan husfasaderna. Något har anlänt till torget, och det är inte för att fira AW.',
    choices: [
      { text: 'Ta en öl till först', next: 'beer_refill' },
      { text: 'Ut på torget!', next: 'square' },
    ],
  },
  home: {
    image: 'https://picsum.photos/800/450?random=4',
    text: 'Your home is quiet and dusty. The hearth is cold, but the bed is inviting.',
    choices: [
      { text: 'Sleep', next: 'sleep' },
      { text: 'Read a Book', next: 'book' },
      { text: 'Clean the House', next: 'clean' },
      { text: 'Go Outside', next: 'start' },
      { text: 'Exit Game', next: 'quit' },
    ],
  },
  square: {
    image: '/torget.jpg',
    text: 'Hjältarna rusar ut på torget och stannar tvärt. Himlen är mörk av ryska drönare som surrar hotfullt över Sundsvall. I mitten av torget, på en tron av skrot, sitter Putin och skrattar hånfullt. "Detta är nu mitt Sundsvall!" vrålar han. Staden är i kaos, och det är upp till er att sätta stopp för detta vansinne!',
    choices: [
      { text: 'Full frontal attack: Storma drönarna!', next: 'full_frontal_attack' },
      { text: 'Taktisk manöver: Flankera från skuggorna!', next: 'flank' },
      { text: 'Diplomatisk lösning: Utmana Putin på en öl!', next: 'putin_challenge' },
      { text: 'Ställ in, far hem', next: 'forced_game_over' },
    ],
  },
  flank: {
    image: '/flank.jpg',
    text: 'Hjältarna glider ljudlöst genom gränderna, precis utom synhåll för drönarnas sensorer. Genom ett perfekt samspel flankerar de skrothögen och överraskar Putins vakter i en blixtsnabb attack. Överraskningsmomentet är totalt, och vägen till diktatorn ligger nu öppen!',
    choices: [
      { text: 'Avfyra en rökgranat och storma!', next: 'smoke_grenade_attack' },
      { text: 'Smyga fram till tronen', next: 'slink_to_throne' },
      { text: 'Hacka drönarsystemet', next: 'hack_drones' },
      { text: 'Tveka i sista sekunden', next: 'forced_game_over' },
    ],
  },
  putin_challenge: {
    image: '/putin.jpg',
    text: 'Hjältarna kliver fram mot skrothögen. Putin ser ner på er med en blick av totalt förakt. "En öl? Ni tror att ni kan dricka mig under bordet?" hånar han. Atmosfären är elektrisk. En enda felaktig kommentar och drönarna kommer att förvandla er till aska.',
    choices: [
      { text: 'Drick honom av tronen', next: 'fight' },
      { text: 'Håna hans smak och kalla ölen "ljummen"', next: 'fight' },
      { text: 'Häll ut hans öl på marken', next: 'forced_game_over' },
    ],
  },
  full_frontal: {
    image: '/full_frontal.jpg',
    text: 'Med fullt mod och maximal öl i blodet stormar hjältarna rakt fram! Drönarna hinner inte ens reagera innan de slits i stycken. Putin ser förskräckt på när hans armé förvandlas till metallskrot. Ni är ostoppbara!',
    choices: [{ text: 'Gå till slutstriden!', next: 'fight' }],
  },
  full_frontal_death: {
    image: '/full_frontal_death.jpg',
    text: 'Hjältarna stormar fram, men utan fulla öl-depåer saknas den nödvändiga skärpan. Drönarna reagerar blixtsnabbt och öppnar eld. Det blir en våldsam och kort upplevelse. Sundsvall faller, och hjältarna faller med dem.',
    choices: [{ text: 'Avsluta lidandet', next: 'forced_game_over' }],
  },
  putin_drunk_death: {
    image: '/full_frontal_death.jpg',
    text: 'Hjältarna utmanar Putin, men de är så proppfulla med öl att de knappt kan stå upp. Putin skrattar hånfullt åt deras tillstånd och ger order om att utplåna dem på stället. Ni var för förfriskade för sitt eget bästa.',
    choices: [{ text: 'Avsluta lidandet', next: 'forced_game_over' }],
  },
  hack_drunk_death: {
    image: '/full_frontal_death.jpg',
    text: 'Hjältarna försöker hacka drönarsystemet, men fingrarna är för tunga av öl och synen är suddig. Ett avgörande tangentfel avslöjar deras position och drönarna slår till med kirurgisk precision. Ni var för berusade för att vara diskreta.',
    choices: [{ text: 'Avsluta lidandet', next: 'forced_game_over' }],
  },

  // Special Refill Stage
  beer_refill: {
    image: '/beer.jpg',
    text: 'En iskall, frostig bägare öl återställer både modet och törsten!',
    choices: [
      { text: 'Ah, uppfriskande! Fortsätt...', next: '__BACK__' },
    ],
  },
  // Fallback/End stages for the demo
  drink: { image: 'https://picsum.photos/800/450?random=5', text: 'The ale tastes of hops and history.', choices: [{ text: 'Back', next: 'tavern' }, { text: 'Back', next: 'tavern' }, { text: 'Back', next: 'tavern' }, { text: 'Back', next: 'tavern' }, { text: 'Exit', next: 'quit' }] },
  barkeep: { image: 'https://picsum.photos/800/450?random=6', text: 'The barkeep grunts and tells you the town is dangerous tonight.', choices: [{ text: 'Back', next: 'tavern' }, { text: 'Back', next: 'tavern' }, { text: 'Back', next: 'tavern' }, { text: 'Back', next: 'tavern' }, { text: 'Exit', next: 'quit' }] },
  fight: { 
    image: '/victory_torget.jpg', 
    text: 'Striden rasar! Hjältarna kastar sig in i kampen. Drönare exploderar i ett regn av gnistor när Ted, Marcus och Samuel slåss sida vid sida. Putin försöker fly, men er beslutsamhet är starkare än hans armé. Med en sista gemensam ansträngning krossar ni drönarsvärmen och skickar diktatorn på flykt!', 
    choices: [{ text: 'Segern är vår!', next: 'quit' }] 
  },
  stranger: { image: 'https://picsum.photos/800/450?random=8', text: 'The stranger whispers a secret about a hidden treasure before vanishing.', choices: [{ text: 'Back', next: 'streets' }, { text: 'Back', next: 'streets' }, { text: 'Back', next: 'streets' }, { text: 'Back', next: 'streets' }, { text: 'Exit', next: 'quit' }] },
  market: { image: 'https://picsum.photos/800/450?random=9', text: 'The market is filled with exotic spices and strange artifacts.', choices: [{ text: 'Back', next: 'streets' }, { text: 'Back', next: 'streets' }, { text: 'Back', next: 'streets' }, { text: 'Back', next: 'streets' }, { text: 'Exit', next: 'quit' }] },
  nap: { image: 'https://picsum.photos/800/450?random=10', text: 'You take a short nap on a bench. You wake up feeling slightly refreshed.', choices: [{ text: 'Back', next: 'streets' }, { text: 'Back', next: 'streets' }, { text: 'Back', next: 'streets' }, { text: 'Back', next: 'streets' }, { text: 'Exit', next: 'quit' }] },
  sleep: { image: 'https://picsum.photos/800/450?random=11', text: 'A deep, dreamless sleep washes over you.', choices: [{ text: 'Back', next: 'home' }, { text: 'Back', next: 'home' }, { text: 'Back', next: 'home' }, { text: 'Back', next: 'home' }, { text: 'Exit', next: 'quit' }] },
  book: { image: 'https://picsum.photos/800/450?random=12', text: 'The book tells tales of ancient kings and forgotten wars.', choices: [{ text: 'Back', next: 'home' }, { text: 'Back', next: 'home' }, { text: 'Back', next: 'home' }, { text: 'Back', next: 'home' }, { text: 'Exit', next: 'quit' }] },
  clean: { image: 'https://picsum.photos/800/450?random=13', text: 'Your house is now sparkling clean, but you are exhausted.', choices: [{ text: 'Back', next: 'home' }, { text: 'Back', next: 'home' }, { text: 'Back', next: 'home' }, { text: 'Back', next: 'home' }, { text: 'Exit', next: 'quit' }] },
  quit: { 
    image: '/end_act1.jpg', 
    text: 'Solsidan återvänder till Sundsvall. Putin är besegrad, drönarna är skrot och staden firar sina räddare. Hjältarna hittar äntligen till RÅ och Köttkylen för en välförtjänt öl. Slutet på akt 1.', 
    choices: [{ text: 'Börja om äventyret', next: 'start' }] 
  },
  forced_game_over: { image: 'https://picsum.photos/800/450?random=0', text: 'You decided to give up and go home. Game Over.', choices: [] },
};

const characters = ['Ted', 'Marcus', 'Samuel'];

const currentStageId = ref<string>('start');
const previousStageId = ref<string>('start');
const pendingStageId = ref<string>('');
const beerGracePeriod = ref(0);
const health = ref<Record<string, number>>({
  Ted: 100,
  Marcus: 100,
  Samuel: 100,
});
const beer = ref<Record<string, number>>({
  Ted: 100,
  Marcus: 100,
  Samuel: 100,
});

const currentStage = computed(() => story[currentStageId.value]);

const isGameOver = computed(() => {
  return characters.some(name => beer.value[name] <= 0);
});

const availableChoices = computed(() => {
  if (isGameOver.value) {
    return [{ text: 'Börja om', next: 'start' }];
  }
  const choices = [...currentStage.value.choices];
  const anyBelow50 = characters.some(name => beer.value[name] < 50);
  const hasEndSuffering = currentStage.value.choices.some(choice => choice.text === 'Avsluta lidandet');
  if (anyBelow50 && currentStageId.value !== 'beer_refill' && !hasEndSuffering) {
    choices.unshift({ text: '🍺 Take a Beer', next: 'beer_refill' });
  }
  return choices;
});

const handleChoice = (nextStageId: string) => {
  let targetId = nextStageId;
  if (nextStageId === '__BACK__') {
    targetId = previousStageId.value;
  }

  if (nextStageId === '__PENDING__') {
    targetId = pendingStageId.value;
  }

  if (targetId === 'full_frontal_attack') {
    const allFull = characters.every(name => beer.value[name] === 100);
    targetId = allFull ? 'full_frontal' : 'full_frontal_death';
  }

  if (targetId === 'slink_to_throne') {
    const allFull = characters.every(name => beer.value[name] === 100);
    targetId = allFull ? 'full_frontal_death' : 'fight';
  }

  if (targetId === 'smoke_grenade_attack') {
    const allFull = characters.every(name => beer.value[name] === 100);
    targetId = allFull ? 'fight' : 'full_frontal_death';
  }

  if (targetId === 'putin_challenge') {
    const minBeer = Math.min(...characters.map(name => beer.value[name]));
    const roll = Math.floor(Math.random() * 100);
    if (roll < minBeer) {
      targetId = 'putin_drunk_death';
    }
  }

  if (targetId === 'hack_drones') {
    const minBeer = Math.min(...characters.map(name => beer.value[name]));
    const roll = Math.floor(Math.random() * 100);
    if (roll < minBeer) {
      targetId = 'hack_drunk_death';
    } else {
      targetId = 'fight';
    }
  }

  if (currentStageId.value === 'start' && targetId !== 'forced_game_over') {
    pendingStageId.value = targetId;
    targetId = 'transit';
  }

  if (targetId === 'beer_refill') {
    beerGracePeriod.value = 2;
    characters.forEach(name => {
      beer.value[name] = 100;
      health.value[name] = Math.min(100, health.value[name] + 10);
    });
  } else if (targetId === 'forced_game_over') {
    characters.forEach(name => {
      beer.value[name] = 0;
      health.value[name] = 0;
    });
  } else if (targetId !== 'beer_refill') {
    if (beerGracePeriod.value > 0) {
      beerGracePeriod.value--;
    } else if (currentStageId.value !== 'beer_refill') {
      characters.forEach(name => {
        const loss = Math.floor(Math.random() * (25 - 5 + 1)) + 5;
        beer.value[name] = Math.max(0, beer.value[name] - loss);
        
        if (beer.value[name] < 50) {
          health.value[name] = Math.max(0, health.value[name] - loss);
        }
      });

      if (characters.some(name => beer.value[name] <= 0)) {
        characters.forEach(name => {
          health.value[name] = 0;
        });
      }
    }
  }
  
  if (targetId === 'start') {
    characters.forEach(name => {
      health.value[name] = 100;
      beer.value[name] = 100;
    });
  }

  previousStageId.value = currentStageId.value;
  currentStageId.value = targetId;
};
</script>

<template>
  <div class="game-viewport">
    <div class="game-layout-wrapper">
      <!-- Wrapper to align characters perfectly with the image container height -->
      <div class="ui-side-panel">
        <div class="ui-slots">
          <div class="character-unit" v-for="name in characters" :key="name">
            <div class="char-name">{{ name }}</div>
            <div class="ui-slot" :style="{ backgroundImage: 'url(/' + name.toLowerCase() + '.jpg)', backgroundSize: 'cover', backgroundPosition: 'center' }"></div>
            <div class="status-bars">
              <div class="status-row">
                <span class="status-label">Health</span>
                <div class="bar-bg health-bg">
                  <div class="bar-fill health-fill" :style="{ width: health[name] + '%' }"></div>
                </div>
              </div>
              <div class="status-row">
                <span class="status-label">Beer</span>
                <div class="bar-bg beer-bg">
                  <div class="bar-fill beer-fill" :style="{ width: beer[name] + '%' }"></div>
                </div>
              </div>
            </div>
          </div>
        </div>
      </div>

      <!-- Right side content -->
      <div class="logo-container">
        <img 
          src="/logo.jpg" 
          alt="Köttkylen Logo" 
          class="logo-img" 
        />
      </div>

      <div class="game-container">
        <template v-if="!isGameOver">
          <img 
            :src="currentStage.image" 
            alt="Story Image" 
            class="game-image" 
          />
        </template>
        <div v-else class="game-over-screen">
          GAME OVER
        </div>
      </div>

      <!-- Bottom UI: Menu and Dialogue -->
      <div class="bottom-ui-row">
        <div class="menu-container">
          <button 
            v-for="choice in availableChoices" 
            :key="choice.text" 
            class="menu-button"
            @click="handleChoice(choice.next)"
          >
            {{ choice.text }}
          </button>
        </div>
        <div class="dialogue-container">
          {{ currentStage.text }}
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>

.game-viewport {
  display: flex;
  justify-content: center;
  align-items: center;
  width: 100vw;
  height: 100vh;
  background-color: black;
  font-family: -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, Helvetica, Arial, sans-serif, "Apple Color Emoji", "Segoe UI Emoji", "Segoe UI Symbol";
}

.game-layout-wrapper {
  display: grid;
  grid-template-columns: auto 800px;
  grid-template-rows: auto 450px auto;
  column-gap: 0.2rem;
  row-gap: 1rem;
  justify-content: center;
  padding-top: 1rem;
}

.ui-side-panel {
  grid-column: 1;
  grid-row: 2;
  height: 450px; /* Match the image container height perfectly */
  display: flex;
  flex-direction: column;
  justify-content: space-between; /* Distribute characters evenly top to bottom */
  justify-self: end;
}

.ui-slots {
  display: flex;
  flex-direction: column;
  height: 100%;
  justify-content: space-between;
  z-index: 10;
}

.character-unit {
  display: flex;
  flex-direction: column;
  gap: 0.4rem;
  align-items: center;
}

.char-name {
  color: white;
  font-family: inherit;
  font-size: 16px;
  font-weight: bold;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.ui-slot {
  width: 90px;
  height: 90px;
  border: 2px solid white;
  background-color: rgba(255, 255, 255, 0.1);
  box-sizing: border-box;
}

.status-bars {
  display: flex;
  flex-direction: column;
  gap: 0.2rem;
}

.status-row {
  display: grid;
  grid-template-columns: 40px 90px 40px;
  align-items: center;
  justify-items: center;
}

.status-label {
  color: white;
  font-size: 0.6rem;
  text-align: right;
  text-transform: uppercase;
  padding-right: 8px;
}

.bar-bg {
  width: 90px;
  height: 6px;
  background-color: rgba(0, 0, 0, 0.6);
  border: 1px solid rgba(255, 255, 255, 0.3);
  box-sizing: border-box;
  overflow: hidden;
}

.bar-fill {
  height: 100%;
  transition: width 0.3s ease;
}

.health-fill {
  background-color: #ff4d4d;
}

.beer-fill {
  background-color: #ffcc00;
}

.logo-container {
  grid-column: 2;
  grid-row: 1;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100px;
  justify-self: center;
}

.logo-img {
  height: 100%;
  width: auto;
  display: block;
  object-fit: contain;
}

.game-container {
  grid-column: 2;
  grid-row: 2;
  width: 800px;
  max-width: calc(100vw - 400px); 
  height: 450px; 
  
  position: relative;
  overflow: hidden;
  box-shadow: 0 0 20px rgba(255, 255, 255, 0.1);
  background-color: #111;
}

.game-over-screen {
  width: 100%;
  height: 100%;
  background-color: black;
  display: flex;
  justify-content: center;
  align-items: center;
  color: white;
  font-size: 3rem;
  text-align: center;
}

.game-image {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  pointer-events: none;
}

.bottom-ui-row {
  grid-column: 2;
  grid-row: 3;
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  width: 800px;
  gap: 1rem;
}

.menu-container {
  display: flex;
  flex-direction: column;
  border: 2px solid white;
  background-color: rgba(0, 0, 0, 0.5);
  box-sizing: border-box;
  overflow: hidden;
}

.dialogue-container {
  flex: 1;
  border: 2px solid white;
  background-color: rgba(0, 0, 0, 0.5);
  color: white;
  padding: 1rem;
  box-sizing: border-box;
  font-size: 16px;
  line-height: 1.4;
  text-align: left;
  max-height: 164px;
  overflow-y: auto;
}

.menu-button {
  width: auto;
  min-width: 120px;
  height: 32px;
  background-color: transparent;
  border: none;
  border-bottom: 1px solid rgba(255, 255, 255, 0.3);
  color: white;
  font-family: inherit;
  font-size: 16px;
  cursor: pointer;
  transition: all 0.2s ease;
  text-align: left;
  padding: 0 1rem;
  box-sizing: border-box;
}

.menu-button:last-child {
  border-bottom: none;
}

.menu-button:hover {
  background-color: rgba(255, 255, 255, 0.2);
  transform: translateX(5px);
}
</style>
